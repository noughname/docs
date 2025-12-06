---
title: Update Task
layout: default
parent: API Documentation
grand_parent: Reference
nav_order: 4
---

# PUT /tasks/:id

Update an existing task.

{: .fs-6 .fw-300 }

---

## Endpoint

```
PUT /tasks/:id
```

## Authentication

Required. Include Bearer token in Authorization header.

---

## Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `id` | integer | Yes | Unique task identifier |

---

## Request Body

All fields are optional. Only include fields you want to update.

| Field | Type | Description |
|-------|------|-------------|
| `title` | string | Task title |
| `description` | string | Detailed description |
| `status` | string | Task status |
| `priority` | string | Priority level |
| `assigned_to` | integer | User ID to assign |
| `due_date` | string | Due date (ISO 8601) |
| `tags` | array | Array of tag strings |

---

## Request Examples

### Update Status

```bash
curl -X PUT "https://api.example.com/v1/tasks/123" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "status": "completed"
  }'
```

### Update Multiple Fields

```bash
curl -X PUT "https://api.example.com/v1/tasks/123" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "priority": "critical",
    "assigned_to": 43,
    "due_date": "2024-01-18T17:00:00Z"
  }'
```

### JavaScript

```javascript
const response = await fetch('https://api.example.com/v1/tasks/123', {
  method: 'PUT',
  headers: {
    'Authorization': 'Bearer YOUR_TOKEN',
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    status: 'in_progress',
    priority: 'high'
  })
});
const task = await response.json();
```

### Python

```python
import requests

url = 'https://api.example.com/v1/tasks/123'
headers = {
    'Authorization': 'Bearer YOUR_TOKEN',
    'Content-Type': 'application/json'
}
payload = {
    'status': 'in_progress',
    'priority': 'high'
}

response = requests.put(url, headers=headers, json=payload)
task = response.json()
```

---

## Response

### Success Response (200 OK)

```json
{
  "data": {
    "id": 123,
    "title": "Implement new feature",
    "description": "Add user profile customization",
    "status": "in_progress",
    "priority": "high",
    "assigned_to": {
      "id": 42,
      "name": "John Doe",
      "email": "john@example.com"
    },
    "due_date": "2024-01-25T17:00:00Z",
    "created_at": "2024-01-16T10:30:00Z",
    "updated_at": "2024-01-17T09:15:00Z",
    "tags": ["feature", "frontend"],
    "links": {
      "self": "/tasks/123",
      "comments": "/tasks/123/comments",
      "attachments": "/tasks/123/attachments"
    }
  },
  "meta": {
    "timestamp": "2024-01-17T09:15:00Z"
  }
}
```

---

## Error Responses

### 404 Not Found

```json
{
  "error": {
    "code": "TASK_NOT_FOUND",
    "message": "Task with ID 999 does not exist"
  }
}
```

### 400 Bad Request

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Validation failed",
    "details": [
      {
        "field": "status",
        "message": "Invalid status value",
        "allowed_values": ["pending", "in_progress", "completed", "cancelled"]
      }
    ]
  }
}
```

### 403 Forbidden

```json
{
  "error": {
    "code": "FORBIDDEN",
    "message": "You don't have permission to update this task"
  }
}
```

---

## Partial Updates

This endpoint supports partial updates. You can update one or more fields without affecting others:

```json
// Only update priority
{"priority": "high"}

// Update status and assigned user
{
  "status": "in_progress",
  "assigned_to": 42
}
```

---

## Related Endpoints

- [List Tasks]({% link docs/reference/api-docs/tasks-list.md %})
- [Create Task]({% link docs/reference/api-docs/tasks-create.md %})
- [Get Task]({% link docs/reference/api-docs/tasks-get.md %})
- [Delete Task]({% link docs/reference/api-docs/tasks-delete.md %})
