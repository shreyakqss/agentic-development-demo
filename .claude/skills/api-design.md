---
name: API Design
description: Design and document RESTful API endpoints with proper contracts
agents: [backend-dev]
---

# API Design Skill

Design RESTful API endpoints following best practices.

## Process

1. **Understand the requirement**: What data/actions does the client need?
2. **Define resources**: Identify nouns (users, orders, products)
3. **Design endpoints**: Map CRUD operations to HTTP methods
4. **Specify contracts**: Define request/response schemas
5. **Document**: Create clear API documentation

## REST Conventions

| Action | HTTP Method | Endpoint Pattern | Success Code |
|--------|-------------|------------------|--------------|
| List | GET | /resources | 200 |
| Create | POST | /resources | 201 |
| Read | GET | /resources/:id | 200 |
| Update | PUT/PATCH | /resources/:id | 200 |
| Delete | DELETE | /resources/:id | 204 |

## Request/Response Schema Template

```json
{
  "endpoint": "POST /api/v1/resources",
  "request": {
    "headers": { "Content-Type": "application/json" },
    "body": { "field": "type and constraints" }
  },
  "response": {
    "success": { "status": 201, "body": {} },
    "errors": [
      { "status": 400, "condition": "validation failed" },
      { "status": 401, "condition": "unauthorized" }
    ]
  }
}
```

## Checklist

- [ ] Resource names are plural nouns
- [ ] Consistent naming convention (kebab-case or snake_case)
- [ ] Proper HTTP status codes
- [ ] Error responses include error code and message
- [ ] Pagination for list endpoints
- [ ] Versioning strategy defined
