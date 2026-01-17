# Python Code Node (Beta)

Expert guidance for writing Python code in n8n Code nodes.

---

## Important: JavaScript First

**Recommendation**: Use **JavaScript for 95% of use cases**. Only use Python when:
- You need specific Python standard library functions
- You're significantly more comfortable with Python syntax
- You're doing data transformations better suited to Python

**Why JavaScript is preferred:**
- Full n8n helper functions ($helpers.httpRequest, etc.)
- Luxon DateTime library for advanced date/time operations
- No external library limitations
- Better n8n documentation and community support

---

## Quick Start

```python
# Basic template for Python Code nodes
items = _input.all()

# Process data
processed = []
for item in items:
    processed.append({
        "json": {
            **item["json"],
            "processed": True,
            "timestamp": datetime.now().isoformat()
        }
    })

return processed
```

### Essential Rules

1. **Consider JavaScript first** - Use Python only when necessary
2. **Access data**: `_input.all()`, `_input.first()`, or `_input.item`
3. **CRITICAL**: Must return `[{"json": {...}}]` format
4. **CRITICAL**: Webhook data is under `_json["body"]` (not `_json` directly)
5. **CRITICAL LIMITATION**: **No external libraries** (no requests, pandas, numpy)
6. **Standard library only**: json, datetime, re, base64, hashlib, urllib.parse, math, random, statistics

---

## Mode Selection Guide

### Run Once for All Items (Recommended - Default)

```python
# Example: Calculate total from all items
all_items = _input.all()
total = sum(item["json"].get("amount", 0) for item in all_items)

return [{
    "json": {
        "total": total,
        "count": len(all_items),
        "average": total / len(all_items) if all_items else 0
    }
}]
```

### Run Once for Each Item

```python
# Example: Add processing timestamp to each item
item = _input.item

return [{
    "json": {
        **item["json"],
        "processed": True,
        "processed_at": datetime.now().isoformat()
    }
}]
```

---

## Critical Limitation: No External Libraries

**MOST IMPORTANT PYTHON LIMITATION**: Cannot import external packages

### What's NOT Available

```python
# NOT AVAILABLE - Will raise ModuleNotFoundError
import requests  # No
import pandas  # No
import numpy  # No
import scipy  # No
from bs4 import BeautifulSoup  # No
```

### What IS Available (Standard Library)

```python
# AVAILABLE - Standard library only
import json  # JSON parsing
import datetime  # Date/time operations
import re  # Regular expressions
import base64  # Base64 encoding/decoding
import hashlib  # Hashing functions
import urllib.parse  # URL parsing
import math  # Math functions
import random  # Random numbers
import statistics  # Statistical functions
```

### Workarounds

**Need HTTP requests?**
- Use **HTTP Request node** before Code node
- Or switch to **JavaScript** and use `$helpers.httpRequest()`

**Need data analysis (pandas/numpy)?**
- Use Python **statistics** module for basic stats
- Or switch to **JavaScript** for most operations

---

## Data Access Patterns

### Pattern 1: _input.all() - Most Common

```python
# Get all items from previous node
all_items = _input.all()

# Filter, transform as needed
valid = [item for item in all_items if item["json"].get("status") == "active"]

processed = []
for item in valid:
    processed.append({
        "json": {
            "id": item["json"]["id"],
            "name": item["json"]["name"]
        }
    })

return processed
```

### Pattern 2: _input.first() - Very Common

```python
# Get first item only
first_item = _input.first()
data = first_item["json"]

return [{
    "json": {
        "result": process_data(data),
        "processed_at": datetime.now().isoformat()
    }
}]
```

---

## Critical: Webhook Data Structure

**MOST COMMON MISTAKE**: Webhook data is nested under `["body"]`

```python
# WRONG - Will raise KeyError
name = _json["name"]
email = _json["email"]

# CORRECT - Webhook data is under ["body"]
name = _json["body"]["name"]
email = _json["body"]["email"]

# SAFER - Use .get() for safe access
webhook_data = _json.get("body", {})
name = webhook_data.get("name")
```

---

## Return Format Requirements

### Correct Return Formats

```python
# Single result
return [{
    "json": {
        "field1": value1,
        "field2": value2
    }
}]

# Multiple results
return [
    {"json": {"id": 1, "data": "first"}},
    {"json": {"id": 2, "data": "second"}}
]

# List comprehension
transformed = [
    {"json": {"id": item["json"]["id"], "processed": True}}
    for item in _input.all()
    if item["json"].get("valid")
]
return transformed

# Empty result (when no data to return)
return []
```

---

## Common Patterns

### 1. Data Transformation

```python
items = _input.all()

return [
    {
        "json": {
            "id": item["json"].get("id"),
            "name": item["json"].get("name", "Unknown").upper(),
            "processed": True
        }
    }
    for item in items
]
```

### 2. Filtering & Aggregation

```python
items = _input.all()
total = sum(item["json"].get("amount", 0) for item in items)
valid_items = [item for item in items if item["json"].get("amount", 0) > 0]

return [{
    "json": {
        "total": total,
        "count": len(valid_items)
    }
}]
```

### 3. String Processing with Regex

```python
import re

items = _input.all()
email_pattern = r'\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b'

all_emails = []
for item in items:
    text = item["json"].get("text", "")
    emails = re.findall(email_pattern, text)
    all_emails.extend(emails)

# Remove duplicates
unique_emails = list(set(all_emails))

return [{
    "json": {
        "emails": unique_emails,
        "count": len(unique_emails)
    }
}]
```

### 4. Statistical Analysis

```python
from statistics import mean, median, stdev

items = _input.all()
values = [item["json"].get("value", 0) for item in items if "value" in item["json"]]

if values:
    return [{
        "json": {
            "mean": mean(values),
            "median": median(values),
            "stdev": stdev(values) if len(values) > 1 else 0,
            "min": min(values),
            "max": max(values),
            "count": len(values)
        }
    }]
else:
    return [{"json": {"error": "No values found"}}]
```

---

## Error Prevention - Top 5 Mistakes

### #1: Importing External Libraries

```python
# WRONG: Trying to import external library
import requests  # ModuleNotFoundError!

# CORRECT: Use HTTP Request node or JavaScript
```

### #2: Empty Code or Missing Return

```python
# WRONG: No return statement
items = _input.all()
# Processing...
# Forgot to return!

# CORRECT: Always return data
items = _input.all()
# Processing...
return [{"json": item["json"]} for item in items]
```

### #3: Incorrect Return Format

```python
# WRONG: Returning dict instead of list
return {"json": {"result": "success"}}

# CORRECT: List wrapper required
return [{"json": {"result": "success"}}]
```

### #4: KeyError on Dictionary Access

```python
# WRONG: Direct access crashes if missing
name = _json["user"]["name"]  # KeyError!

# CORRECT: Use .get() for safe access
name = _json.get("user", {}).get("name", "Unknown")
```

### #5: Webhook Body Nesting

```python
# WRONG: Direct access to webhook data
email = _json["email"]  # KeyError!

# CORRECT: Webhook data under ["body"]
email = _json["body"]["email"]
```

---

## Best Practices

1. **Always Use .get() for Dictionary Access**
2. **Handle None/Null Values Explicitly**
3. **Use List Comprehensions for Filtering**
4. **Return Consistent Structure**
5. **Debug with print() Statements**

---

## When to Use Python vs JavaScript

### Use Python When:
- You need `statistics` module for statistical operations
- You're significantly more comfortable with Python syntax
- Your logic maps well to list comprehensions
- You need specific standard library functions

### Use JavaScript When:
- You need HTTP requests ($helpers.httpRequest())
- You need advanced date/time (DateTime/Luxon)
- You want better n8n integration
- **For 95% of use cases** (recommended)

---

## Quick Reference Checklist

Before deploying Python Code nodes, verify:

- [ ] **Considered JavaScript first** - Using Python only when necessary
- [ ] **Code is not empty** - Must have meaningful logic
- [ ] **Return statement exists** - Must return list of dictionaries
- [ ] **Proper return format** - Each item: `{"json": {...}}`
- [ ] **Data access correct** - Using `_input.all()`, `_input.first()`, or `_input.item`
- [ ] **No external imports** - Only standard library (json, datetime, re, etc.)
- [ ] **Safe dictionary access** - Using `.get()` to avoid KeyError
- [ ] **Webhook data** - Access via `["body"]` if from webhook

---

## Additional Resources

- Code Node Guide: https://docs.n8n.io/code/code-node/
- Python in n8n: https://docs.n8n.io/code/builtin/python-modules/
