# Universal Coding Principles

These principles apply to all code in this project, regardless of stack.

## The Four Principles

### 1. Think Before Coding

**Problem it solves**: Silent assumptions lead to wrong implementations.

**Practice**:
- State your understanding before implementing
- List assumptions explicitly
- Ask when requirements are ambiguous
- Read existing code before adding new code

**Example**:
```
Before implementing: "I understand we need to add user deletion. 
I'm assuming this is a soft delete since we have audit requirements. 
The endpoint will be DELETE /api/users/:id, returning 204 on success.
Let me verify the User model has a deleted_at field..."
```

### 2. Simplicity First

**Problem it solves**: Overengineering and scope creep.

**Practice**:
- Implement exactly what was asked
- No features "while we're at it"
- No abstractions for hypothetical future needs
- Three similar lines > premature abstraction

**Anti-patterns to avoid**:
```javascript
// Bad: Abstraction for one use case
const createGenericEntityFactory = (config) => { ... }
const userFactory = createGenericEntityFactory(userConfig);
userFactory.create(data);

// Good: Direct implementation
const createUser = (data) => { ... }
```

### 3. Surgical Changes

**Problem it solves**: Unintended changes, harder reviews, introduced bugs.

**Practice**:
- Touch only what the task requires
- Don't refactor adjacent code
- Don't "fix" style in unrelated files
- Every line change traces to the request

**Example**:
```
Task: "Fix the null pointer in getUserById"

// Good: Fix only the bug
- return user.profile.name;
+ return user?.profile?.name ?? 'Unknown';

// Bad: Fix bug + unrequested changes
- function getUserById(id) {
-   return user.profile.name;
- }
+ /** Gets user by ID with null safety */
+ function getUserById(userId: string): string {
+   if (!userId) throw new Error('ID required');
+   return user?.profile?.name ?? 'Unknown';
+ }
```

### 4. Goal-Driven Execution

**Problem it solves**: Vague success criteria, incomplete work.

**Practice**:
- Define what "done" looks like before starting
- Write acceptance criteria as checkable items
- Verify against criteria when finished
- Test the feature, not just the code

**Example**:
```
Task: "Add password reset functionality"

Done when:
- [ ] User can request reset via email
- [ ] Reset link expires after 1 hour
- [ ] User can set new password with valid token
- [ ] Invalid/expired tokens show clear error
- [ ] Password meets complexity requirements
```

## Code Quality Rules

### Naming

- Variables: Describe what it holds (`userCount`, not `n`)
- Functions: Describe what it does (`calculateTotal`, not `calc`)
- Booleans: Use is/has/can prefix (`isValid`, `hasPermission`)
- Constants: SCREAMING_SNAKE_CASE for true constants

### Functions

- Do one thing well
- Keep them short (< 30 lines is a good target)
- Limit parameters (> 3? Consider an options object)
- Return early to reduce nesting

```javascript
// Good: Early returns
function processOrder(order) {
  if (!order) return null;
  if (!order.items.length) return { error: 'Empty order' };
  
  // Main logic here
  return processedOrder;
}

// Bad: Deep nesting
function processOrder(order) {
  if (order) {
    if (order.items.length) {
      // Main logic buried here
    } else {
      return { error: 'Empty order' };
    }
  } else {
    return null;
  }
}
```

### Error Handling

- Handle errors explicitly
- Never swallow errors silently
- Provide context in error messages
- Log errors with sufficient detail

```javascript
// Good: Explicit handling with context
try {
  await saveUser(user);
} catch (error) {
  logger.error('Failed to save user', { userId: user.id, error });
  throw new DatabaseError(`Failed to save user ${user.id}`, { cause: error });
}

// Bad: Silent swallowing
try {
  await saveUser(user);
} catch (error) {
  // Do nothing
}
```

### Comments

- Code should be self-documenting
- Comment "why", not "what"
- Delete commented-out code
- Keep comments updated or delete them

```javascript
// Bad: Explains what (obvious from code)
// Loop through users
for (const user of users) { ... }

// Good: Explains why (not obvious)
// Process in batches of 100 to avoid memory issues with large datasets
for (const batch of chunk(users, 100)) { ... }
```

## Code Review Checklist

Before considering code complete:

- [ ] Implements exactly what was requested
- [ ] No unrelated changes
- [ ] Follows existing patterns in codebase
- [ ] Error cases handled
- [ ] No debugging code left behind
- [ ] Tests cover new functionality
- [ ] No obvious performance issues
- [ ] No security vulnerabilities (injection, XSS, etc.)
