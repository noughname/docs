---
title: List Tasks
layout: default
parent: API Documentation
grand_parent: Reference
nav_order: 1
---

# GET /tasks

Retrieve a list of all tasks.

{: .fs-6 .fw-300 }

---

## Endpoint

```
GET /tasks
```

## Authentication

Required. Include Bearer token in Authorization header.

---

## Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `page` | integer | No | Page number (default: 1) |
| `per_page` | integer | No | Results per page (default: 20, max: 100) |
| `status` | string | No | Filter by status: `pending`, `in_progress`, `completed` |
| `priority` | string | No | Filter by priority: `low`, `medium`, `high` |
| `assigned_to` | integer | No | Filter by user ID |
| `sort` | string | No | Sort field: `created_at`, `updated_at`, `priority`, `due_date` |
| `order` | string | No | Sort order: `asc`, `desc` (default: `desc`) |

---

## Request Example

```bash
curl -X GET "https://api.example.com/v1/tasks?status=in_progress&per_page=10" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json"
```

### JavaScript

```javascript
const response = await fetch('https://api.example.com/v1/tasks?status=in_progress', {
  headers: {
    'Authorization': 'Bearer YOUR_TOKEN',
    'Content-Type': 'application/json'
  }
});
const data = await response.json();
```

### Python

```python
import requests

url = 'https://api.example.com/v1/tasks'
headers = {
    'Authorization': 'Bearer YOUR_TOKEN',
    'Content-Type': 'application/json'
}
params = {'status': 'in_progress', 'per_page': 10}

response = requests.get(url, headers=headers, params=params)
tasks = response.json()
```

---

## Response

### Success Response (200 OK)

```json
{
  "data": [
    {
      "id": 1,
      "title": "Complete project documentation",
      "description": "Write comprehensive docs for the API",
      "status": "in_progress",
      "priority": "high",
      "assigned_to": {
        "id": 42,
        "name": "John Doe",
        "email": "john@example.com"
      },
      "due_date": "2024-01-20T17:00:00Z",
      "created_at": "2024-01-15T10:30:00Z",
      "updated_at": "2024-01-16T14:20:00Z",
      "tags": ["documentation", "api"],
      "links": {
        "self": "/tasks/1",
        "comments": "/tasks/1/comments",
        "attachments": "/tasks/1/attachments"
      }
    },
    {
      "id": 2,
      "title": "Fix authentication bug",
      "description": "Users cannot login with SSO",
      "status": "in_progress",
      "priority": "critical",
      "assigned_to": {
        "id": 43,
        "name": "Jane Smith",
        "email": "jane@example.com"
      },
      "due_date": "2024-01-18T12:00:00Z",
      "created_at": "2024-01-16T09:15:00Z",
      "updated_at": "2024-01-16T15:45:00Z",
      "tags": ["bug", "authentication"],
      "links": {
        "self": "/tasks/2",
        "comments": "/tasks/2/comments",
        "attachments": "/tasks/2/attachments"
      }
    }
  ],
  "meta": {
    "page": 1,
    "per_page": 10,
    "total": 45,
    "total_pages": 5,
    "timestamp": "2024-01-16T16:00:00Z"
  },
  "links": {
    "first": "/tasks?page=1&per_page=10",
    "prev": null,
    "next": "/tasks?page=2&per_page=10",
    "last": "/tasks?page=5&per_page=10"
  }
}
```

---

## Response Fields

### Task Object

| Field | Type | Description |
|-------|------|-------------|
| `id` | integer | Unique task identifier |
| `title` | string | Task title |
| `description` | string | Detailed description |
| `status` | string | Task status: `pending`, `in_progress`, `completed`, `cancelled` |
| `priority` | string | Priority level: `low`, `medium`, `high`, `critical` |
| `assigned_to` | object | User assigned to the task |
| `due_date` | string | Due date (ISO 8601 format) |
| `created_at` | string | Creation timestamp |
| `updated_at` | string | Last update timestamp |
| `tags` | array | Array of tag strings |
| `links` | object | Related resource links |

---

## Error Responses

### 401 Unauthorized

```json
{
  "error": {
    "code": "UNAUTHORIZED",
    "message": "Invalid or missing authentication token"
  }
}
```

### 400 Bad Request

```json
{
  "error": {
    "code": "INVALID_PARAMETER",
    "message": "Invalid value for parameter 'status'",
    "details": {
      "parameter": "status",
      "value": "invalid_status",
      "allowed_values": ["pending", "in_progress", "completed", "cancelled"]
    }
  }
}
```

### 429 Too Many Requests

```json
{
  "error": {
    "code": "RATE_LIMIT_EXCEEDED",
    "message": "Rate limit exceeded. Try again in 42 seconds",
    "retry_after": 42
  }
}
```

---

## Filtering Examples

### By Status

```bash
GET /tasks?status=completed
```

### By Priority

```bash
GET /tasks?priority=high
```

### By Multiple Parameters

```bash
GET /tasks?status=in_progress&priority=high&assigned_to=42
```

### With Sorting

```bash
GET /tasks?sort=due_date&order=asc
```

---

## Best Practices

1. **Use pagination**: Don't request all tasks at once
2. **Filter efficiently**: Use status and priority filters to reduce results
3. **Cache responses**: Cache for appropriate duration
4. **Handle rate limits**: Implement exponential backoff
5. **Check total count**: Use `meta.total` to understand dataset size

---

## Rate Limiting

This endpoint is subject to rate limiting:
- **Limit**: 100 requests per minute
- **Headers**: Check `X-RateLimit-*` headers in response

---

## Related Endpoints

- [Create Task]({% link docs/reference/api-docs/tasks-create.md %})
- [Get Task]({% link docs/reference/api-docs/tasks-get.md %})
- [Update Task]({% link docs/reference/api-docs/tasks-update.md %})
- [Delete Task]({% link docs/reference/api-docs/tasks-delete.md %})
