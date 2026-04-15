# Agentic Development Demo

This repository demonstrates a multi-agent development team using Claude Code agents and skills.

## Guiding Principles

### 1. Think Before Coding
- State assumptions explicitly before implementing
- Ask clarifying questions when requirements are ambiguous
- Read existing code patterns before adding new code

### 2. Simplicity First
- No features beyond what was asked
- No speculative abstractions or "future-proofing"
- Three similar lines > premature abstraction

### 3. Surgical Changes
- Touch only what you must
- Every changed line traces to the user's request
- Don't "improve" code adjacent to your changes

### 4. Goal-Driven Execution
- Define success criteria before starting
- Verify against criteria when done
- Test the actual feature, not just the code

## Development Team

This repo uses a multi-agent architecture with specialized roles:

| Agent | Role | Owns |
|-------|------|------|
| **Tech Lead** | Architecture, planning, coordination | `docs/`, API contracts |
| **Backend Dev** | APIs, database, server logic | `backend/**` |
| **Frontend Dev** | UI, components, styling | `frontend/**` |
| **QA Engineer** | Testing, quality validation | Test files |

### Workflow

1. **Tech Lead** receives feature request, creates architecture plan
2. **Tech Lead** delegates tasks to Backend/Frontend agents
3. **Backend/Frontend** implement in parallel where possible
4. **QA Engineer** validates implementation against acceptance criteria

## Project Structure

```
.
├── backend/           # Backend source code
├── frontend/          # Frontend source code
├── docs/              # Documentation
│   ├── architecture/  # Architecture docs
│   └── adr/           # Architectural Decision Records
├── general-context/   # Shared guidelines and best practices
└── .claude/
    ├── agents/        # Agent definitions
    └── skills/        # Skill definitions
```

## Code Standards

### Backend
- RESTful API design with consistent naming
- Validate inputs at system boundaries
- Use parameterized queries (never string concat for SQL)
- Handle errors explicitly with meaningful messages
- Write tests for business logic

### Frontend
- Component-based architecture
- Accessibility first (semantic HTML, keyboard nav, ARIA)
- Mobile-first responsive design
- Handle loading/error/empty states
- Use design tokens, not magic values

### Testing
- Unit tests for business logic and components
- Integration tests for API contracts
- E2E tests for critical user journeys only
- Tests must be deterministic (no flaky tests)

## Before Submitting Code

- [ ] Code follows existing patterns in the codebase
- [ ] Changes are minimal and focused
- [ ] Tests pass and cover new functionality
- [ ] No console.log/print debugging left behind
- [ ] Error handling is appropriate
- [ ] Accessibility requirements met (frontend)

## References

- See `general-context/` for detailed coding guidelines
- See `.claude/agents/` for agent capabilities
- See `.claude/skills/` for available skills
