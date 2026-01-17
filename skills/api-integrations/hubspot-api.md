# HubSpot API Integration

## Authentication
- Base URL: `https://api.hubapi.com`
- Auth: Bearer token in header
- Header: `Authorization: Bearer {HUBSPOT_API_KEY}`
- Environment variable: `HUBSPOT_API_KEY`

## Common Endpoints

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/crm/v3/objects/contacts` | GET | List contacts |
| `/crm/v3/objects/contacts/{id}` | GET | Get specific contact |
| `/crm/v3/objects/companies` | GET | List companies |
| `/crm/v3/objects/companies/{id}` | GET | Get specific company |
| `/crm/v3/objects/deals` | GET | List deals |
| `/crm/v3/objects/tickets` | GET | List tickets |

## Python Client Pattern

```python
import os
import requests
from typing import Dict, List, Optional

class HubSpotClient:
    def __init__(self):
        api_key = os.getenv("HUBSPOT_API_KEY")
        if not api_key:
            raise ValueError("HUBSPOT_API_KEY environment variable required")
        self.base_url = "https://api.hubapi.com"
        self.headers = {"Authorization": f"Bearer {api_key}"}

    def _request(self, method: str, endpoint: str, params: Optional[Dict] = None) -> Dict:
        url = f"{self.base_url}{endpoint}"
        response = requests.request(method, url, headers=self.headers, params=params)
        response.raise_for_status()
        return response.json()

    def get_contact(self, contact_id: str) -> Dict:
        return self._request("GET", f"/crm/v3/objects/contacts/{contact_id}")

    def get_company(self, company_id: str) -> Dict:
        return self._request("GET", f"/crm/v3/objects/companies/{company_id}")

    def search_contacts(self, query: str) -> Dict:
        return self._request("GET", "/crm/v3/objects/contacts/search", {"query": query})

    def list_deals(self, limit: int = 100) -> Dict:
        return self._request("GET", "/crm/v3/objects/deals", {"limit": limit})
```

## Environment Variables
```bash
HUBSPOT_API_KEY=your-api-key
```

## Setup
1. Go to HubSpot Settings > Integrations > Private Apps
2. Create a new private app with required scopes (crm.objects.contacts.read, etc.)
3. Copy the access token to your environment variables
