# Backend Development Guidelines

Best practices for backend development in this project.

## API Design

### REST Conventions

| Action | Method | Path | Success | Error |
|--------|--------|------|---------|-------|
| List | GET | /resources | 200 | 404 |
| Create | POST | /resources | 201 | 400, 409 |
| Read | GET | /resources/:id | 200 | 404 |
| Update | PUT | /resources/:id | 200 | 400, 404 |
| Partial | PATCH | /resources/:id | 200 | 400, 404 |
| Delete | DELETE | /resources/:id | 204 | 404 |

### Request/Response Format

```json
// Success response
{
  "data": { ... },
  "meta": { "page": 1, "total": 100 }
}

// Error response
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Email is required",
    "details": [{ "field": "email", "message": "Required" }]
  }
}
```

### Pagination

Use cursor-based pagination for large datasets:
```
GET /resources?cursor=abc123&limit=20
```

## Database

### Schema Design

- Use UUIDs for public-facing IDs
- Add `created_at` and `updated_at` timestamps
- Use soft deletes (`deleted_at`) for recoverable data
- Index foreign keys and frequently queried columns

### Query Patterns

```sql
-- Good: Parameterized query
SELECT * FROM users WHERE id = $1

-- Bad: String concatenation (SQL injection risk)
SELECT * FROM users WHERE id = ' + userId + '
```

### Migrations

- Always provide up AND down migrations
- Test rollback before deploying
- Never modify deployed migrations, create new ones

## Error Handling

### Error Categories

| HTTP Code | Category | When to Use |
|-----------|----------|-------------|
| 400 | Bad Request | Invalid input |
| 401 | Unauthorized | Missing/invalid auth |
| 403 | Forbidden | Valid auth, no permission |
| 404 | Not Found | Resource doesn't exist |
| 409 | Conflict | Duplicate/state conflict |
| 422 | Unprocessable | Valid syntax, invalid semantics |
| 500 | Server Error | Unexpected failures |

### Error Handling Pattern

```javascript
// Explicit error handling
try {
  const user = await findUser(id);
  if (!user) {
    throw new NotFoundError('User not found');
  }
  return user;
} catch (error) {
  if (error instanceof NotFoundError) {
    throw error; // Re-throw known errors
  }
  logger.error('Unexpected error finding user', { id, error });
  throw new InternalError('Failed to fetch user');
}
```

## Security

### Input Validation

- Validate ALL inputs at API boundary
- Use allowlists, not denylists
- Sanitize before database operations
- Limit string lengths and array sizes

### Authentication

- Use secure session tokens or JWTs
- Implement token refresh for long sessions
- Log authentication events
- Rate limit login attempts

### Authorization

- Check permissions on every request
- Use principle of least privilege
- Validate resource ownership

## Logging

### Log Levels

| Level | When to Use |
|-------|-------------|
| ERROR | Failures requiring attention |
| WARN | Unusual but handled situations |
| INFO | Significant business events |
| DEBUG | Detailed troubleshooting (dev only) |

### What to Log

```javascript
// Good: Structured, contextual
logger.info('Order created', { 
  orderId: order.id, 
  userId: user.id, 
  amount: order.total 
});

// Bad: Unstructured, no context
console.log('order created');
```

## Testing

### Unit Tests

- Test business logic in isolation
- Mock external dependencies
- Cover happy path + edge cases + errors

### Integration Tests

- Test against real database (use test DB)
- Test full request/response cycle
- Verify error responses

### Test Structure

```javascript
describe('UserService', () => {
  describe('createUser', () => {
    it('creates user with valid input', async () => {
      // Arrange
      const input = { email: 'test@example.com' };
      
      // Act
      const user = await userService.createUser(input);
      
      // Assert
      expect(user.email).toBe(input.email);
    });

    it('throws on duplicate email', async () => {
      // Test error case
    });
  });
});
```
