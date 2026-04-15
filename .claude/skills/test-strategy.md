---
name: Test Strategy
description: Design comprehensive test plans covering unit, integration, and e2e testing
agents: [qa-engineer]
---

# Test Strategy Skill

Design comprehensive test strategies that balance coverage with maintainability.

## Process

1. **Analyze feature**: What are the critical paths?
2. **Identify risks**: What could go wrong?
3. **Plan test levels**: Unit, integration, e2e allocation
4. **Define test cases**: Specific scenarios to cover
5. **Estimate effort**: Prioritize by value/cost
6. **Document**: Test plan with acceptance criteria

## Testing Pyramid

```
        /\
       /  \    E2E Tests (few)
      /----\   - Critical user journeys
     /      \  - Cross-system validation
    /--------\ Integration Tests (some)
   /          \ - API contracts
  /            \ - Database operations
 /--------------\ Unit Tests (many)
                  - Business logic
                  - Pure functions
                  - Component behavior
```

## Test Case Template

```markdown
## Test Case: [Feature] - [Scenario]

**Preconditions:**
- User is logged in
- Feature flag enabled

**Steps:**
1. Navigate to /feature
2. Click "Create" button
3. Fill form with valid data
4. Submit form

**Expected Result:**
- Success message displayed
- Item appears in list
- Database record created

**Edge Cases:**
- Empty form submission
- Duplicate entry
- Network failure mid-submit
```

## Risk-Based Prioritization

| Risk Level | Test Coverage | Example |
|------------|---------------|---------|
| Critical | 100% automated | Payment processing |
| High | 80%+ automated | Authentication |
| Medium | Key paths automated | CRUD operations |
| Low | Manual or skip | Admin-only features |

## Test Strategy Checklist

- [ ] Critical paths identified
- [ ] Test levels appropriate for each scenario
- [ ] Edge cases documented
- [ ] Error scenarios covered
- [ ] Performance criteria defined
- [ ] Test data strategy planned
