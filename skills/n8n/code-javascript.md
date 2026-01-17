# JavaScript Code Node

Expert guidance for writing JavaScript code in n8n Code nodes.

---

## Quick Start

```javascript
// Basic template for Code nodes
const items = $input.all();

// Process data
const processed = items.map(item => ({
  json: {
    ...item.json,
    processed: true,
    timestamp: new Date().toISOString()
  }
}));

return processed;
```

### Essential Rules

1. **Choose "Run Once for All Items" mode** (recommended for most use cases)
2. **Access data**: `$input.all()`, `$input.first()`, or `$input.item`
3. **CRITICAL**: Must return `[{json: {...}}]` format
4. **CRITICAL**: Webhook data is under `$json.body` (not `$json` directly)
5. **Built-ins available**: $helpers.httpRequest(), DateTime (Luxon), $jmespath()

---

## Mode Selection Guide

### Run Once for All Items (Recommended - Default)

**Use this mode for:** 95% of use cases

- **How it works**: Code executes **once** regardless of input count
- **Data access**: `$input.all()` or `items` array
- **Best for**: Aggregation, filtering, batch processing, transformations, API calls with all data
- **Performance**: Faster for multiple items (single execution)

```javascript
// Example: Calculate total from all items
const allItems = $input.all();
const total = allItems.reduce((sum, item) => sum + (item.json.amount || 0), 0);

return [{
  json: {
    total,
    count: allItems.length,
    average: total / allItems.length
  }
}];
```

**When to use:**
- Comparing items across the dataset
- Calculating totals, averages, or statistics
- Sorting or ranking items
- Deduplication
- Building aggregated reports
- Combining data from multiple items

### Run Once for Each Item

**Use this mode for:** Specialized cases only

- **How it works**: Code executes **separately** for each input item
- **Data access**: `$input.item` or `$item`
- **Best for**: Item-specific logic, independent operations, per-item validation
- **Performance**: Slower for large datasets (multiple executions)

```javascript
// Example: Add processing timestamp to each item
const item = $input.item;

return [{
  json: {
    ...item.json,
    processed: true,
    processedAt: new Date().toISOString()
  }
}];
```

**Decision Shortcut:**
- **Need to look at multiple items?** -> Use "All Items" mode
- **Each item completely independent?** -> Use "Each Item" mode
- **Not sure?** -> Use "All Items" mode (you can always loop inside)

---

## Data Access Patterns

### Pattern 1: $input.all() - Most Common

**Use when**: Processing arrays, batch operations, aggregations

```javascript
// Get all items from previous node
const allItems = $input.all();

// Filter, map, reduce as needed
const valid = allItems.filter(item => item.json.status === 'active');
const mapped = valid.map(item => ({
  json: {
    id: item.json.id,
    name: item.json.name
  }
}));

return mapped;
```

### Pattern 2: $input.first() - Very Common

**Use when**: Working with single objects, API responses, first-in-first-out

```javascript
// Get first item only
const firstItem = $input.first();
const data = firstItem.json;

return [{
  json: {
    result: processData(data),
    processedAt: new Date().toISOString()
  }
}];
```

### Pattern 3: $input.item - Each Item Mode Only

**Use when**: In "Run Once for Each Item" mode

```javascript
// Current item in loop (Each Item mode only)
const currentItem = $input.item;

return [{
  json: {
    ...currentItem.json,
    itemProcessed: true
  }
}];
```

### Pattern 4: $node - Reference Other Nodes

**Use when**: Need data from specific nodes in workflow

```javascript
// Get output from specific node
const webhookData = $node["Webhook"].json;
const httpData = $node["HTTP Request"].json;

return [{
  json: {
    combined: {
      webhook: webhookData,
      api: httpData
    }
  }
}];
```

---

## Critical: Webhook Data Structure

**MOST COMMON MISTAKE**: Webhook data is nested under `.body`

```javascript
// WRONG - Will return undefined
const name = $json.name;
const email = $json.email;

// CORRECT - Webhook data is under .body
const name = $json.body.name;
const email = $json.body.email;

// Or with $input
const webhookData = $input.first().json.body;
const name = webhookData.name;
```

**Why**: Webhook node wraps all request data under `body` property.

---

## Return Format Requirements

**CRITICAL RULE**: Always return array of objects with `json` property

### Correct Return Formats

```javascript
// Single result
return [{
  json: {
    field1: value1,
    field2: value2
  }
}];

// Multiple results
return [
  {json: {id: 1, data: 'first'}},
  {json: {id: 2, data: 'second'}}
];

// Empty result (when no data to return)
return [];
```

### Incorrect Return Formats

```javascript
// WRONG: Object without array wrapper
return {
  json: {field: value}
};

// WRONG: Array without json wrapper
return [{field: value}];

// WRONG: Incomplete structure
return [{data: value}];  // Should be {json: value}
```

---

## Common Patterns

### 1. Data Transformation & Enrichment

```javascript
const items = $input.all();

return items.map(item => {
  const data = item.json;
  const nameParts = data.name.split(' ');

  return {
    json: {
      first_name: nameParts[0],
      last_name: nameParts.slice(1).join(' '),
      email: data.email,
      created_at: new Date().toISOString()
    }
  };
});
```

### 2. Filtering with Regex

```javascript
const pattern = /\b([A-Z]{2,5})\b/g;
const matches = {};

for (const item of $input.all()) {
  const text = item.json.text;
  const found = text.match(pattern);

  if (found) {
    found.forEach(match => {
      matches[match] = (matches[match] || 0) + 1;
    });
  }
}

return [{json: {matches}}];
```

### 3. Aggregation & Reporting

```javascript
const items = $input.all();
const total = items.reduce((sum, item) => sum + (item.json.amount || 0), 0);

return [{
  json: {
    total,
    count: items.length,
    average: total / items.length,
    timestamp: new Date().toISOString()
  }
}];
```

---

## Built-in Functions & Helpers

### $helpers.httpRequest()

```javascript
const response = await $helpers.httpRequest({
  method: 'GET',
  url: 'https://api.example.com/data',
  headers: {
    'Authorization': 'Bearer token',
    'Content-Type': 'application/json'
  }
});

return [{json: {data: response}}];
```

### DateTime (Luxon)

```javascript
// Current time
const now = DateTime.now();

// Format dates
const formatted = now.toFormat('yyyy-MM-dd');
const iso = now.toISO();

// Date arithmetic
const tomorrow = now.plus({days: 1});
const lastWeek = now.minus({weeks: 1});

return [{
  json: {
    today: formatted,
    tomorrow: tomorrow.toFormat('yyyy-MM-dd')
  }
}];
```

---

## Error Prevention - Top 5 Mistakes

### #1: Empty Code or Missing Return

```javascript
// WRONG: No return statement
const items = $input.all();
// ... processing code ...
// Forgot to return!

// CORRECT: Always return data
const items = $input.all();
// ... processing ...
return items.map(item => ({json: item.json}));
```

### #2: Expression Syntax Confusion

```javascript
// WRONG: Using n8n expression syntax in code
const value = "{{ $json.field }}";

// CORRECT: Use JavaScript directly
const value = $input.first().json.field;
```

### #3: Incorrect Return Wrapper

```javascript
// WRONG: Returning object instead of array
return {json: {result: 'success'}};

// CORRECT: Array wrapper required
return [{json: {result: 'success'}}];
```

### #4: Missing Null Checks

```javascript
// WRONG: Crashes if field doesn't exist
const value = item.json.user.email;

// CORRECT: Safe access with optional chaining
const value = item.json?.user?.email || 'no-email@example.com';
```

### #5: Webhook Body Nesting

```javascript
// WRONG: Direct access to webhook data
const email = $json.email;

// CORRECT: Webhook data under .body
const email = $json.body.email;
```

---

## Best Practices

1. **Always Validate Input Data**
2. **Use Try-Catch for Error Handling**
3. **Prefer Array Methods Over Loops**
4. **Filter Early, Process Late**
5. **Use Descriptive Variable Names**
6. **Debug with console.log()**

---

## When to Use Code Node

Use Code node when:
- Complex transformations requiring multiple steps
- Custom calculations or business logic
- Recursive operations
- API response parsing with complex structure
- Data aggregation across items

Consider other nodes when:
- Simple field mapping -> Use **Set** node
- Basic filtering -> Use **Filter** node
- Simple conditionals -> Use **IF** or **Switch** node
- HTTP requests only -> Use **HTTP Request** node

---

## Quick Reference Checklist

Before deploying Code nodes, verify:

- [ ] **Code is not empty** - Must have meaningful logic
- [ ] **Return statement exists** - Must return array of objects
- [ ] **Proper return format** - Each item: `{json: {...}}`
- [ ] **Data access correct** - Using `$input.all()`, `$input.first()`, or `$input.item`
- [ ] **No n8n expressions** - Use JavaScript template literals: `` `${value}` ``
- [ ] **Error handling** - Guard clauses for null/undefined inputs
- [ ] **Webhook data** - Access via `.body` if from webhook
- [ ] **Mode selection** - "All Items" for most cases

---

## Additional Resources

- Code Node Guide: https://docs.n8n.io/code/code-node/
- Built-in Methods: https://docs.n8n.io/code-examples/methods-variables-reference/
- Luxon Documentation: https://moment.github.io/luxon/
