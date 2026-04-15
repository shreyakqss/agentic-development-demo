# Agentic Development Demo

A starter repository for running software work with a multi-agent setup (Tech Lead, Backend, Frontend, QA) using Claude Code-style agent and skill definitions.

## What This Repo Is

This project is intentionally lightweight. It provides:

- A shared engineering playbook in `general-context/`
- Role-specific agent definitions in `.claude/agents/`
- Reusable skill definitions in `.claude/skills/`

It does not yet include an implemented application in `backend/` or `frontend/`.

## Repository Structure

```text
.
├── .claude/
│   ├── agents/              # Team role definitions
│   │   ├── tech-lead.md
│   │   ├── backend-dev.md
│   │   ├── frontend-dev.md
│   │   └── qa-engineer.md
│   └── skills/              # Task-specific playbooks
│       ├── architecture-planning.md
│       ├── api-design.md
│       ├── database-schema.md
│       ├── component-design.md
│       ├── style-system.md
│       ├── test-strategy.md
│       └── bug-triage.md
├── backend/                 # Backend implementation space
├── frontend/                # Frontend implementation space
├── general-context/
│   ├── coding-principles.md
│   ├── backend-guidelines.md
│   └── frontend-guidelines.md
└── CLAUDE.md                # Project-level operating guide
```

## Team Model

This repo is organized around four roles:

- `Tech Lead`: architecture, planning, API contracts, delegation
- `Backend Dev`: APIs, domain logic, data layer, server concerns
- `Frontend Dev`: UI, state, accessibility, client integration
- `QA Engineer`: test strategy, automation, quality gates

Typical flow:

1. Tech Lead defines plan and acceptance criteria.
2. Backend and Frontend implement in parallel.
3. QA validates behavior and regression risk.

## How To Use This Repo

1. Start with the shared principles:
   - `general-context/coding-principles.md`
   - `general-context/backend-guidelines.md`
   - `general-context/frontend-guidelines.md`
2. Pick the right role prompt from `.claude/agents/`.
3. Apply one or more skills from `.claude/skills/` for the specific task.
4. Build implementation code under `backend/` and `frontend/`.

## Example Task Kickoff

Use prompts like:

- "As Tech Lead, propose architecture and task breakdown for user authentication."
- "As Backend Dev, implement `POST /auth/login` with validation and tests."
- "As Frontend Dev, build the login page with loading/error/empty states."
- "As QA Engineer, define and implement test strategy for auth flows."

## Engineering Standards

Core standards emphasized in this repository:

- Think before coding: state assumptions and clarify unknowns.
- Simplicity first: implement only what was requested.
- Surgical changes: keep edits focused and traceable to requirements.
- Goal-driven execution: define "done" criteria and verify against them.

## Current Status

- `backend/`: scaffold only
- `frontend/`: scaffold only
- Documentation and team operating model: in place

If you want, the next step can be scaffolding a concrete stack (for example: FastAPI + React, or Express + Next.js) inside the existing structure.
