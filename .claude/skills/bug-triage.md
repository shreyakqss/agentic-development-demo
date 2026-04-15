---
name: Bug Triage
description: Investigate, document, and prioritize bugs with clear reproduction steps
agents: [qa-engineer]
---

# Bug Triage Skill

Investigate and document bugs with clear, actionable information.

## Process

1. **Reproduce**: Confirm the bug exists
2. **Isolate**: Find minimal reproduction steps
3. **Investigate**: Identify root cause area
4. **Document**: Write clear bug report
5. **Prioritize**: Assess severity and impact
6. **Assign**: Route to appropriate team member

## Bug Report Template

```markdown
## Bug: [Brief description]

**Severity:** Critical / High / Medium / Low
**Component:** Backend API / Frontend UI / Database

### Environment
- Browser/OS: Chrome 120 / macOS
- Version: v1.2.3
- User role: Admin

### Reproduction Steps
1. Login as admin user
2. Navigate to /settings
3. Click "Save" without changes
4. Observe error

### Expected Behavior
Settings page saves successfully with "No changes" message.

### Actual Behavior
500 error displayed. Console shows: `TypeError: Cannot read property 'id' of undefined`

### Evidence
- Screenshot: [attached]
- Console logs: [attached]
- Network request: [attached]

### Investigation Notes
- Error occurs in `SettingsController.save()` line 45
- `user.preferences` is undefined when no changes made
- Likely missing null check

### Suggested Fix
Add null check before accessing `user.preferences.id`
```

## Severity Definitions

| Severity | Definition | Response Time |
|----------|------------|---------------|
| Critical | System down, data loss, security breach | Immediate |
| High | Major feature broken, no workaround | Same day |
| Medium | Feature impaired, workaround exists | This sprint |
| Low | Minor issue, cosmetic | Backlog |

## Triage Questions

1. Can users complete their task? (workaround?)
2. How many users affected? (scope?)
3. Is data at risk? (integrity?)
4. Is it getting worse? (trend?)

## Bug Triage Checklist

- [ ] Bug reproduced consistently
- [ ] Minimal reproduction steps documented
- [ ] Environment details captured
- [ ] Screenshots/logs attached
- [ ] Severity assessed
- [ ] Root cause area identified
- [ ] Assigned to appropriate owner
