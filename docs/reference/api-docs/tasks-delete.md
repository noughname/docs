---
title: Delete Task
layout: default
parent: API Documentation
grand_parent: Reference
nav_order: 5
---

# DELETE /tasks/:id

Delete a task permanently.

{: .fs-6 .fw-300 }

---

## Endpoint

```
DELETE /tasks/:id
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
curl -X DELETE "https://api.example.com/v1/tasks/123" \
  -H "Authorization: Bearer YOUR_TOKEN"
```

### JavaScript

```javascript
const response = await fetch('https://api.example.com/v1/tasks/123', {
  method: 'DELETE',
  headers: {
    'Authorization': 'Bearer YOUR_TOKEN'
  }
});

if (response.status === 204) {
  console.log('Task deleted successfully');
}
```

### Python

```python
import requests

url = 'https://api.example.com/v1/tasks/123'
headers = {
    'Authorization': 'Bearer YOUR_TOKEN'
}

response = requests.delete(url, headers=headers)

if response.status_code == 204:
    print('Task deleted successfully')
```

---

## Response

### Success Response (204 No Content)

```
HTTP/1.1 204 No Content
```

No response body is returned for successful deletions.

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
    "message": "You don't have permission to delete this task"
  }
}
```

### 409 Conflict

```json
{
  "error": {
    "code": "TASK_HAS_DEPENDENCIES",
    "message": "Cannot delete task with active subtasks",
    "details": {
      "subtasks_count": 3,
      "subtasks_completed": 1
    }
  }
}
```

---

## Important Notes

{: .warning }
**Warning**: Deletion is permanent and cannot be undone. All associated data (comments, attachments, history) will also be deleted.

### What Gets Deleted

When you delete a task, the following are also removed:
- All comments on the task
- All file attachments
- Change history
- Subtasks (if any)
- Task associations and links

### Soft Delete Alternative

If you need to preserve data, consider updating the task status to `cancelled` instead:

```bash
curl -X PUT "https://api.example.com/v1/tasks/123" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"status": "cancelled"}'
```

---

## Permissions

Only users with the following roles can delete tasks:
- Task creator
- Task assignee
- Project admin
- System admin

---

## Rate Limiting

This endpoint is subject to rate limiting:
- **Limit**: 100 requests per minute
- **Headers**: Check `X-RateLimit-*` headers in response

---

## Bulk Delete

To delete multiple tasks, use the bulk delete endpoint:

```bash
POST /tasks/bulk-delete
{
  "task_ids": [123, 124, 125]
}
```

---

## Related Endpoints

- [List Tasks]({% link docs/reference/api-docs/tasks-list.md %})
- [Create Task]({% link docs/reference/api-docs/tasks-create.md %})
- [Get Task]({% link docs/reference/api-docs/tasks-get.md %})
- [Update Task]({% link docs/reference/api-docs/tasks-update.md %})
