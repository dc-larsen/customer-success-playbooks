# Vitally API Integration

## Authentication
- Base URL: `https://{your-subdomain}.rest.vitally.io`
- Auth: Basic Auth with API token (base64 encoded)
- Header: `Authorization: Basic {base64_encoded_token}`
- Environment variables: `VITALLY_API_TOKEN`, `VITALLY_SUBDOMAIN`

## Common Endpoints

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/resources/accounts` | GET | List customer accounts |
| `/resources/accounts/{id}` | GET | Get specific account |
| `/resources/users` | GET | List users |
| `/resources/notes` | GET/POST | Account notes |
| `/resources/tasks` | GET/POST | Tasks |
| `/resources/conversations` | GET | Conversations |
| `/resources/nps_responses` | GET | NPS responses |

## Python Client Pattern

```python
import os
import base64
import requests
from typing import Dict, Optional, List

class VitallyClient:
    def __init__(self):
        token = os.getenv("VITALLY_API_TOKEN")
        subdomain = os.getenv("VITALLY_SUBDOMAIN")

        if not all([token, subdomain]):
            raise ValueError("VITALLY_API_TOKEN and VITALLY_SUBDOMAIN required")

        self.base_url = f"https://{subdomain}.rest.vitally.io"
        encoded_token = base64.b64encode(f"{token}:".encode()).decode()
        self.headers = {"Authorization": f"Basic {encoded_token}"}

    def _request(self, method: str, endpoint: str, params: Optional[Dict] = None, json: Optional[Dict] = None) -> Dict:
        url = f"{self.base_url}{endpoint}"
        response = requests.request(method, url, headers=self.headers, params=params, json=json)
        response.raise_for_status()
        return response.json()

    def get_accounts(self, limit: int = 100) -> Dict:
        return self._request("GET", "/resources/accounts", {"limit": limit})

    def get_account(self, account_id: str) -> Dict:
        return self._request("GET", f"/resources/accounts/{account_id}")

    def get_account_by_external_id(self, external_id: str) -> Dict:
        return self._request("GET", "/resources/accounts", {"externalId": external_id})

    def create_note(self, account_id: str, note: str, category: str = "general") -> Dict:
        return self._request("POST", "/resources/notes", json={
            "accountId": account_id,
            "note": note,
            "category": category
        })

    def get_users_for_account(self, account_id: str) -> Dict:
        return self._request("GET", "/resources/users", {"accountId": account_id})
```

## Environment Variables
```bash
VITALLY_API_TOKEN=your-api-token
VITALLY_SUBDOMAIN=your-subdomain
```

## Pagination
Vitally uses cursor-based pagination:

```python
def get_all_accounts(self) -> List[Dict]:
    all_accounts = []
    cursor = None

    while True:
        params = {"limit": 100}
        if cursor:
            params["cursor"] = cursor

        response = self._request("GET", "/resources/accounts", params)
        all_accounts.extend(response.get("results", []))

        cursor = response.get("next")
        if not cursor:
            break

    return all_accounts
```

## Filtering
Use query parameters for filtering:

```python
# Filter by trait
client._request("GET", "/resources/accounts", {
    "traits.plan": "enterprise"
})

# Filter by health score
client._request("GET", "/resources/accounts", {
    "healthScore.gte": 70
})
```

## Setup
1. Go to Vitally Settings > Integrations > API
2. Generate a new API token
3. Note your subdomain from your Vitally URL
4. Copy both to your environment variables
