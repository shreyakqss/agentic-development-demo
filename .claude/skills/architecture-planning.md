---
name: Architecture Planning
description: Plan system architecture with component design, data flow, and implementation strategy
agents: [tech-lead]
user_invocable: true 
---

# Architecture Planning Skill

Design system architecture with clear component boundaries and implementation strategy.

## Process

1. **Gather requirements**: Functional and non-functional
2. **Identify components**: What are the major pieces?
3. **Define boundaries**: Where does each component start/end?
4. **Design interactions**: How do components communicate?
5. **Plan implementation**: What order? What dependencies?
6. **Document**: Architecture diagrams and ADRs

## Architecture Document Template

```markdown
# Architecture: [Feature/System Name]

## Overview
[1-2 sentence description]

## Requirements
### Functional
- FR1: [requirement]

### Non-Functional
- NFR1: Performance - [target]
- NFR2: Security - [requirement]

## Component Diagram
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   Client    │────▶│   API       │────▶│  Database   │
│  (React)    │◀────│  (Node.js)  │◀────│  (Postgres) │
└─────────────┘     └─────────────┘     └─────────────┘

## Data Flow
1. [Step 1]
2. [Step 2]

## Implementation Plan
### Phase 1: Foundation
- [ ] Task 1 (Backend)
- [ ] Task 2 (Backend)

### Phase 2: Core Features  
- [ ] Task 3 (Frontend)
- [ ] Task 4 (Backend)

## Risks & Mitigations
| Risk | Impact | Mitigation |
|------|--------|------------|
| [risk] | High | [mitigation] |
```

## Architecture Checklist

- [ ] Requirements clearly defined
- [ ] Component responsibilities documented
- [ ] API contracts specified
- [ ] Database schema designed
- [ ] Security considerations addressed
- [ ] Implementation phases planned
- [ ] Risks identified with mitigations
