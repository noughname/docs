---
title: API Documentation
layout: default
parent: Reference
nav_order: 6
has_children: true
---

# API Documentation

Example API reference documentation structure.

{: .fs-6 .fw-300 }

This section demonstrates how to structure API documentation in your knowledge base. It shows a three-level hierarchy: API Documentation → Endpoints → Individual Methods.

---

## Overview

This is an example REST API for a fictional task management application. Use this as a template for documenting your own APIs.

### Base URL

```
https://api.example.com/v1
```

### Authentication

All API requests require authentication using a Bearer token:

```bash
curl -H "Authorization: Bearer YOUR_TOKEN" \
  https://api.example.com/v1/tasks
```

### Response Format

All responses are in JSON format:

```json
{
  "data": {},
  "meta": {
    "timestamp": "2024-01-15T10:30:00Z"
  }
}
```

### Error Handling

Error responses include details:

```json
{
  "error": {
    "code": "INVALID_REQUEST",
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

### HTTP Status Codes

| Code | Description |
|------|-------------|
| 200 | Success |
| 201 | Created |
| 204 | No Content |
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 429 | Too Many Requests |
| 500 | Internal Server Error |

---

## Rate Limiting

- **Limit**: 100 requests per minute per token
- **Headers**:
  - `X-RateLimit-Limit`: Maximum requests
  - `X-RateLimit-Remaining`: Remaining requests
  - `X-RateLimit-Reset`: Reset time (Unix timestamp)

---

## Pagination

List endpoints support pagination:

```bash
GET /tasks?page=1&per_page=20
```

**Response includes pagination metadata**:

```json
{
  "data": [...],
  "meta": {
    "page": 1,
    "per_page": 20,
    "total": 150,
    "total_pages": 8
  },
  "links": {
    "first": "/tasks?page=1",
    "prev": null,
    "next": "/tasks?page=2",
    "last": "/tasks?page=8"
  }
}
```

---

## Endpoints Overview

### Tasks

- [GET /tasks]({% link docs/reference/api-docs/tasks-list.md %}) - List all tasks
- [POST /tasks]({% link docs/reference/api-docs/tasks-create.md %}) - Create a task
- [GET /tasks/:id]({% link docs/reference/api-docs/tasks-get.md %}) - Get task details
- [PUT /tasks/:id]({% link docs/reference/api-docs/tasks-update.md %}) - Update a task
- [DELETE /tasks/:id]({% link docs/reference/api-docs/tasks-delete.md %}) - Delete a task

### Users

- GET /users - List users
- POST /users - Create user
- GET /users/:id - Get user details
- PUT /users/:id - Update user
- DELETE /users/:id - Delete user

---

## SDKs and Libraries

### JavaScript

```bash
npm install @example/api-client
```

```javascript
import { TasksAPI } from '@example/api-client';

const api = new TasksAPI('YOUR_TOKEN');
const tasks = await api.tasks.list();
```

### Python

```bash
pip install example-api-client
```

```python
from example_api import TasksAPI

api = TasksAPI(token='YOUR_TOKEN')
tasks = api.tasks.list()
```

---

## Changelog

### Version 1.0.0 (2024-01-15)

- Initial API release
- Tasks CRUD operations
- User management
- Authentication support

---

## Support

- **Documentation**: [https://docs.example.com](https://docs.example.com)
- **Status**: [https://status.example.com](https://status.example.com)
- **Contact**: [api-support@example.com](mailto:api-support@example.com)
