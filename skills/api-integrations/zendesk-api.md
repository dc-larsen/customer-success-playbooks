# Zendesk API Integration

## Authentication
- Base URL: `https://{your-subdomain}.zendesk.com/api/v2`
- Auth: Basic Auth with email/token format
- Format: `{email}/token:{api_token}`
- Environment variables: `ZENDESK_EMAIL`, `ZENDESK_API_TOKEN`, `ZENDESK_SUBDOMAIN`

## Common Endpoints

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/tickets` | GET | List tickets |
| `/tickets/{id}` | GET | Get specific ticket |
| `/tickets/{id}/comments` | GET | Get ticket comments |
| `/users` | GET | List users |
| `/organizations` | GET | List organizations |
| `/search.json?query={query}` | GET | Search tickets |

## Python Client Pattern

```python
import os
import requests
from requests.auth import HTTPBasicAuth
from typing import Dict, Optional

class ZendeskClient:
    def __init__(self):
        email = os.getenv("ZENDESK_EMAIL")
        token = os.getenv("ZENDESK_API_TOKEN")
        subdomain = os.getenv("ZENDESK_SUBDOMAIN")

        if not all([email, token, subdomain]):
            raise ValueError("ZENDESK_EMAIL, ZENDESK_API_TOKEN, and ZENDESK_SUBDOMAIN required")

        self.base_url = f"https://{subdomain}.zendesk.com/api/v2"
        self.auth = HTTPBasicAuth(f"{email}/token", token)

    def _request(self, method: str, endpoint: str, params: Optional[Dict] = None) -> Dict:
        url = f"{self.base_url}{endpoint}"
        response = requests.request(method, url, auth=self.auth, params=params)
        response.raise_for_status()
        return response.json()

    def get_ticket(self, ticket_id: int) -> Dict:
        return self._request("GET", f"/tickets/{ticket_id}")

    def get_ticket_comments(self, ticket_id: int) -> Dict:
        return self._request("GET", f"/tickets/{ticket_id}/comments")

    def search_tickets(self, query: str) -> Dict:
        return self._request("GET", "/search.json", {"query": f"type:ticket {query}"})

    def list_organizations(self) -> Dict:
        return self._request("GET", "/organizations")
```

## Environment Variables
```bash
ZENDESK_EMAIL=your-email@company.com
ZENDESK_API_TOKEN=your-api-token
ZENDESK_SUBDOMAIN=your-subdomain
```

## Search Query Examples
```python
# Find tickets by requester email
client.search_tickets("requester:user@example.com")

# Find tickets by status
client.search_tickets("status:open")

# Find tickets by organization
client.search_tickets("organization:acme")

# Combine filters
client.search_tickets("status:open requester:user@example.com created>2024-01-01")
```

## Setup
1. Go to Zendesk Admin > Apps and Integrations > APIs > Zendesk API
2. Enable Token Access
3. Add a new API token
4. Copy the token to your environment variables
