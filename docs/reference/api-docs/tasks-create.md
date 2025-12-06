---
title: Create Task
layout: default
parent: API Documentation
grand_parent: Reference
nav_order: 2
---

# POST /tasks

Create a new task.

{: .fs-6 .fw-300 }

---

## Endpoint

```
POST /tasks
```

## Authentication

Required. Include Bearer token in Authorization header.

---

## Request Body

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `title` | string | Yes | Task title (max 255 characters) |
| `description` | string | No | Detailed description (max 5000 characters) |
| `status` | string | No | Initial status (default: `pending`) |
| `priority` | string | No | Priority level (default: `medium`) |
| `assigned_to` | integer | No | User ID to assign task to |
| `due_date` | string | No | Due date (ISO 8601 format) |
| `tags` | array | No | Array of tag strings |

---

## Request Examples

### cURL

```bash
curl -X POST "https://api.example.com/v1/tasks" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Implement new feature",
    "description": "Add user profile customization",
    "priority": "high",
    "assigned_to": 42,
    "due_date": "2024-01-25T17:00:00Z",
    "tags": ["feature", "frontend"]
  }'
```

### JavaScript

```javascript
const response = await fetch('https://api.example.com/v1/tasks', {
  method: 'POST',
  headers: {
    'Authorization': 'Bearer YOUR_TOKEN',
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    title: 'Implement new feature',
    description: 'Add user profile customization',
    priority: 'high',
    assigned_to: 42,
    due_date: '2024-01-25T17:00:00Z',
    tags: ['feature', 'frontend']
  })
});
const task = await response.json();
```

### Python

```python
import requests
import json

url = 'https://api.example.com/v1/tasks'
headers = {
    'Authorization': 'Bearer YOUR_TOKEN',
    'Content-Type': 'application/json'
}
payload = {
    'title': 'Implement new feature',
    'description': 'Add user profile customization',
    'priority': 'high',
    'assigned_to': 42,
    'due_date': '2024-01-25T17:00:00Z',
    'tags': ['feature', 'frontend']
}

response = requests.post(url, headers=headers, json=payload)
task = response.json()
```

---

## Response

### Success Response (201 Created)

```json
{
  "data": {
    "id": 123,
    "title": "Implement new feature",
    "description": "Add user profile customization",
    "status": "pending",
    "priority": "high",
    "assigned_to": {
      "id": 42,
      "name": "John Doe",
      "email": "john@example.com"
    },
    "due_date": "2024-01-25T17:00:00Z",
    "created_at": "2024-01-16T10:30:00Z",
    "updated_at": "2024-01-16T10:30:00Z",
    "tags": ["feature", "frontend"],
    "links": {
      "self": "/tasks/123",
      "comments": "/tasks/123/comments",
      "attachments": "/tasks/123/attachments"
    }
  },
  "meta": {
    "timestamp": "2024-01-16T10:30:00Z"
  }
}
```

**Response Headers:**
```
HTTP/1.1 201 Created
Location: /tasks/123
Content-Type: application/json
```

---

## Error Responses

### 400 Bad Request - Missing Required Field

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Validation failed",
    "details": [
      {
        "field": "title",
        "message": "Title is required"
      }
    ]
  }
}
```

### 400 Bad Request - Invalid Field Value

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Validation failed",
    "details": [
      {
        "field": "priority",
        "message": "Invalid priority value",
        "allowed_values": ["low", "medium", "high", "critical"]
      }
    ]
  }
}
```

### 401 Unauthorized

```json
{
  "error": {
    "code": "UNAUTHORIZED",
    "message": "Invalid or missing authentication token"
  }
}
```

### 404 Not Found - User Not Found

```json
{
  "error": {
    "code": "USER_NOT_FOUND",
    "message": "User with ID 999 does not exist",
    "details": {
      "field": "assigned_to",
      "value": 999
    }
  }
}
```

---

## Validation Rules

### Title

- **Required**: Yes
- **Type**: String
- **Min length**: 1 character
- **Max length**: 255 characters
- **Pattern**: Any Unicode characters

### Description

- **Required**: No
- **Type**: String
- **Max length**: 5000 characters

### Status

- **Required**: No
- **Type**: String
- **Default**: `pending`
- **Allowed values**: `pending`, `in_progress`, `completed`, `cancelled`

### Priority

- **Required**: No
- **Type**: String
- **Default**: `medium`
- **Allowed values**: `low`, `medium`, `high`, `critical`

### Due Date

- **Required**: No
- **Type**: String (ISO 8601 format)
- **Example**: `2024-01-25T17:00:00Z`
- **Validation**: Must be a future date

### Tags

- **Required**: No
- **Type**: Array of strings
- **Max items**: 10
- **Max length per tag**: 50 characters

---

## Examples

### Minimal Task

```json
{
  "title": "Simple task"
}
```

### Task with All Fields

```json
{
  "title": "Complete project milestone",
  "description": "Finish all remaining items for Q1 milestone:\n- Update documentation\n- Run final tests\n- Deploy to production",
  "status": "pending",
  "priority": "high",
  "assigned_to": 42,
  "due_date": "2024-03-31T23:59:59Z",
  "tags": ["milestone", "Q1", "important"]
}
```

### Task with Markdown Description

```json
{
  "title": "Review pull request",
  "description": "# Code Review\n\n## Changes\n- Added user authentication\n- Fixed memory leak\n\n## Checklist\n- [ ] Tests pass\n- [ ] Documentation updated\n- [ ] No security issues",
  "priority": "medium"
}
```

---

## Best Practices

1. **Provide clear titles**: Make titles descriptive and actionable
2. **Use appropriate priority**: Don't mark everything as high priority
3. **Set realistic due dates**: Account for dependencies and workload
4. **Tag consistently**: Use a standard set of tags across your organization
5. **Assign ownership**: Assign tasks to specific users when possible
6. **Include detailed descriptions**: Provide context and requirements

---

## Idempotency

This endpoint is **not idempotent**. Multiple identical POST requests will create multiple tasks.

If you need idempotency, include a unique `idempotency_key` in the request headers:

```bash
curl -X POST "https://api.example.com/v1/tasks" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -H "Idempotency-Key: unique-key-123" \
  -d '{"title": "Task title"}'
```

---

## Rate Limiting

This endpoint is subject to rate limiting:
- **Limit**: 100 requests per minute
- **Headers**: Check `X-RateLimit-*` headers in response

---

## Related Endpoints

- [List Tasks]({% link docs/reference/api-docs/tasks-list.md %})
- [Get Task]({% link docs/reference/api-docs/tasks-get.md %})
- [Update Task]({% link docs/reference/api-docs/tasks-update.md %})
- [Delete Task]({% link docs/reference/api-docs/tasks-delete.md %})
