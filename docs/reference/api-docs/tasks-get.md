---
title: Get Task
layout: default
parent: API Documentation
grand_parent: Reference
nav_order: 3
---

# GET /tasks/:id

Retrieve details of a specific task.

{: .fs-6 .fw-300 }

---

## Endpoint

```
GET /tasks/:id
```

## Authentication

Required. Include Bearer token in Authorization header.

---

## Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `id` | integer | Yes | Unique task identifier |

---

## Request Examples

### cURL

```bash
curl -X GET "https://api.example.com/v1/tasks/123" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json"
```

### JavaScript

```javascript
const response = await fetch('https://api.example.com/v1/tasks/123', {
  headers: {
    'Authorization': 'Bearer YOUR_TOKEN',
    'Content-Type': 'application/json'
  }
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

response = requests.get(url, headers=headers)
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
    "description": "Add user profile customization with avatar upload and theme selection",
    "status": "in_progress",
    "priority": "high",
    "assigned_to": {
      "id": 42,
      "name": "John Doe",
      "email": "john@example.com",
      "avatar_url": "https://example.com/avatars/42.jpg"
    },
    "created_by": {
      "id": 15,
      "name": "Jane Smith",
      "email": "jane@example.com"
    },
    "due_date": "2024-01-25T17:00:00Z",
    "created_at": "2024-01-16T10:30:00Z",
    "updated_at": "2024-01-17T14:20:00Z",
    "completed_at": null,
    "tags": ["feature", "frontend", "ui"],
    "attachments": [
      {
        "id": 1,
        "filename": "mockup.png",
        "url": "/tasks/123/attachments/1",
        "size": 245760,
        "uploaded_at": "2024-01-16T11:00:00Z"
      }
    ],
    "comments_count": 5,
    "subtasks_count": 3,
    "subtasks_completed": 1,
    "links": {
      "self": "/tasks/123",
      "comments": "/tasks/123/comments",
      "attachments": "/tasks/123/attachments",
      "history": "/tasks/123/history",
      "subtasks": "/tasks/123/subtasks"
    }
  },
  "meta": {
    "timestamp": "2024-01-17T15:00:00Z"
  }
}
```

---

## Response Fields

| Field | Type | Description |
|-------|------|-------------|
| `id` | integer | Unique task identifier |
| `title` | string | Task title |
| `description` | string | Detailed description (supports Markdown) |
| `status` | string | Current status |
| `priority` | string | Priority level |
| `assigned_to` | object | User object of assignee |
| `created_by` | object | User object of creator |
| `due_date` | string | Due date (ISO 8601) |
| `created_at` | string | Creation timestamp |
| `updated_at` | string | Last update timestamp |
| `completed_at` | string\|null | Completion timestamp |
| `tags` | array | Array of tag strings |
| `attachments` | array | Array of attachment objects |
| `comments_count` | integer | Number of comments |
| `subtasks_count` | integer | Total number of subtasks |
| `subtasks_completed` | integer | Number of completed subtasks |
| `links` | object | Related resource links |

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

### 401 Unauthorized

```json
{
  "error": {
    "code": "UNAUTHORIZED",
    "message": "Invalid or missing authentication token"
  }
}
```

### 403 Forbidden

```json
{
  "error": {
    "code": "FORBIDDEN",
    "message": "You don't have permission to view this task"
  }
}
```

---

## Related Endpoints

- [List Tasks]({% link docs/reference/api-docs/tasks-list.md %})
- [Create Task]({% link docs/reference/api-docs/tasks-create.md %})
- [Update Task]({% link docs/reference/api-docs/tasks-update.md %})
- [Delete Task]({% link docs/reference/api-docs/tasks-delete.md %})
