---
marp: true
theme: default
paginate: true
style: |
  section {
    font-family: 'Inter', sans-serif;
    background: #ffffff;
    color: #1a1a2e;
  }
  h1, h2 {
    color: #7c3aed;
  }
  code {
    background: #f3f4f6;
    border-radius: 4px;
    color: #1f2937;
  }
  pre {
    background: #f8fafc !important;
    border: 1px solid #e2e8f0;
  }
  a {
    color: #2563eb;
  }
  table {
    font-size: 0.85em;
  }
  th {
    background: #f1f5f9;
  }
  section.lead {
    background: linear-gradient(135deg, #7c3aed 0%, #2563eb 100%);
    color: white;
  }
  section.lead h1 {
    color: white;
  }
---

# Agentic Development with Claude Code

Shreyak Chakraborty
QSS Technosoft 

**Skills, Sub-Agents & Efficient Workflows**

---

## Agenda

1. **Skills & Sub-Agents** — Extending Claude Code capabilities
2. **Efficient Workflows** — Patterns for productive AI-assisted development
3. **Tool Calling** — Built-in tools & MCP integration
4. **Best Practices & Security** — Safe AI development
5. **Project Knowledge Bases** — Context that scales
6. **Live Demo** — Building a feature with agents

---

<!-- _class: lead -->

# Part 1: Skills & Sub-Agents

---

## What Are Skills?

**Skills** = Reusable prompts/workflows that extend Claude Code

```
.claude/skills/
├── api-design.md         # Backend: REST API contracts
├── component-design.md   # Frontend: UI components
├── test-strategy.md      # QA: Test planning
└── architecture-planning.md  # Tech Lead: System design
```

**Invoke with**: `/skill-name` or let Claude auto-select

---

## Skill Anatomy

```markdown
---
name: API Design
description: Design RESTful API endpoints with proper contracts
agents: [backend-dev]        # Which agent can use this
user_invocable: true         # Can user call directly?
---

# API Design Skill

## Process
1. Understand the requirement
2. Define resources
3. Design endpoints
4. Specify contracts
5. Document
```

---

## What Are Sub-Agents?

**Sub-Agents** = Specialized Claude instances for specific domains

| Agent | Role | Owns |
|-------|------|------|
| **Tech Lead** | Architecture, coordination | `docs/`, contracts |
| **Backend Dev** | APIs, database, logic | `backend/**` |
| **Frontend Dev** | UI, components, styling | `frontend/**` |
| **QA Engineer** | Testing, validation | Test files |

---

## Agent Definition

```markdown
---
name: Backend Developer
description: APIs, databases, server-side logic
model: sonnet
tools:
  - Bash
  - Edit
  - Glob
  - Grep
  - Read
  - Write
  - Agent   # Can spawn sub-agents
---

# Backend Developer Agent

## Core Responsibilities
- Design and implement RESTful APIs
- Write database schemas and migrations
- Implement business logic
```

---

## Agent Orchestration Pattern

```
┌─────────────────────────────────────────────┐
│              Tech Lead (Opus)               │
│         Architecture & Coordination         │
└──────────────┬──────────────┬───────────────┘
               │              │
    ┌──────────▼──────┐  ┌────▼──────────┐
    │  Backend Dev    │  │  Frontend Dev │
    │   (Sonnet)      │  │   (Sonnet)    │
    └────────┬────────┘  └───────┬───────┘
             │                   │
             └─────────┬─────────┘
                  ┌────▼────┐
                  │   QA    │
                  │(Sonnet) │
                  └─────────┘
```

---

## When to Use Agents vs Skills

| Use Case | Use |
|----------|-----|
| Domain-specific implementation | **Agent** |
| Reusable workflow/template | **Skill** |
| Parallel work streams | **Multiple Agents** |
| Single focused task | **Skill within Agent** |
| Complex coordination | **Tech Lead Agent** |

---

<!-- _class: lead -->

# Part 2: Efficient Workflows

---

## The Four Principles

```
┌─────────────────┐  ┌─────────────────┐
│  Think Before   │  │   Simplicity    │
│     Coding      │  │     First       │
│                 │  │                 │
│ State assumptions│  │ No extras beyond│
│ Ask when unclear │  │ what was asked  │
└─────────────────┘  └─────────────────┘

┌─────────────────┐  ┌─────────────────┐
│    Surgical     │  │   Goal-Driven   │
│    Changes      │  │   Execution     │
│                 │  │                 │
│ Touch only what │  │ Define "done"   │
│ you must        │  │ before starting │
└─────────────────┘  └─────────────────┘
```

---

## Workflow: Feature Implementation

```bash
# 1. Tech Lead plans architecture
/architecture-planning "User authentication with OAuth"

# 2. Parallel implementation
# Claude spawns Backend + Frontend agents

# 3. Backend implements API
# Agent: backend-dev
POST /api/auth/login
POST /api/auth/callback
GET  /api/auth/me

# 4. Frontend implements UI
# Agent: frontend-dev
LoginPage, AuthCallback, useAuth hook

# 5. QA validates
# Agent: qa-engineer
Integration tests, security review
```

---

## Context Management

**Problem**: Claude has limited context window

**Solutions**:

| Technique | When |
|-----------|------|
| `CLAUDE.md` | Project-wide rules, always loaded |
| `general-context/` | Domain guidelines, loaded on demand |
| Agent boundaries | Scope context to domain |
| Targeted file reads | Read only what's needed |

---

## Project Structure for AI

```
.
├── CLAUDE.md              # Always read first
├── general-context/       # Shared guidelines
│   ├── coding-principles.md
│   ├── backend-guidelines.md
│   └── frontend-guidelines.md
├── .claude/
│   ├── agents/           # Agent definitions
│   └── skills/           # Skill definitions
├── backend/              # Backend code
└── frontend/             # Frontend code
```

---

<!-- _class: lead -->

# Part 3: Tool Calling

---

## Built-in Tools

| Tool | Purpose | Example |
|------|---------|---------|
| `Read` | Read files | View source code |
| `Edit` | Modify files | Change specific lines |
| `Write` | Create files | New components |
| `Glob` | Find files | `**/*.tsx` |
| `Grep` | Search content | Find function usage |
| `Bash` | Run commands | `npm test`, `git status` |
| `Agent` | Spawn sub-agent | Delegate to specialist |

---

## Tool Usage Best Practices

```bash
# Good: Use dedicated tools
Read file.ts          # Not: cat file.ts
Grep "pattern"        # Not: rg "pattern"
Edit file.ts          # Not: sed -i 's/...'
Glob "**/*.tsx"       # Not: find . -name "*.tsx"

# Good: Parallel independent calls
Read file1.ts  ─┬─►  [results]
Read file2.ts  ─┘

# Bad: Sequential when not needed
Read file1.ts → Read file2.ts → [slower]
```

---

## MCP (Model Context Protocol)

**MCP** = Standard protocol to connect Claude to external tools/data

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   Claude    │────▶│  MCP Server │────▶│  External   │
│    Code     │◀────│             │◀────│  Service    │
└─────────────┘     └─────────────┘     └─────────────┘
```

**Examples**: GitHub, Slack, databases, APIs

---

## MCP Configuration

```json
// ~/.claude/settings.json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@anthropic/mcp-server-github"],
      "env": {
        "GITHUB_TOKEN": "ghp_..."
      }
    },
    "postgres": {
      "command": "npx",
      "args": ["-y", "@anthropic/mcp-server-postgres"],
      "env": {
        "DATABASE_URL": "postgresql://..."
      }
    }
  }
}
```

---

## Context7 MCP Integration

**Context7** = Up-to-date documentation for any library

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   Claude    │────▶│  Context7   │────▶│  Library    │
│    Code     │◀────│  MCP Server │◀────│    Docs     │
└─────────────┘     └─────────────┘     └─────────────┘
```

**Why?** Claude's training data can be outdated for fast-moving libraries

---

## Context7 Setup

```bash
# Add to Claude Code settings
claude mcp add context7 -- npx -y @upstash/context7-mcp
```

Or manually in `~/.claude/settings.json`:

```json
{
  "mcpServers": {
    "context7": {
      "command": "npx",
      "args": ["-y", "@upstash/context7-mcp"]
    }
  }
}
```

---

## Context7 Tools

| Tool | Purpose |
|------|---------|
| `resolve-library-id` | Find library ID from name |
| `get-library-docs` | Fetch current documentation |

**Flow**:
1. Resolve library name → ID
2. Fetch docs with topic filter
3. Claude uses fresh docs in response

---

## Context7 Demo

```
User: "How do I set up React Query v5 with suspense?"

Claude (without Context7):
→ May use outdated v4 patterns

Claude (with Context7):
1. resolve-library-id("tanstack-react-query")
2. get-library-docs(id, topic="suspense")
3. Returns current v5 suspense API
→ Uses useSuspenseQuery (not useQuery with suspense: true)
```

---

## Context7 Practical Example

```typescript
// Context7 fetches latest Hono docs
// Correct for Hono v4:

import { Hono } from 'hono'
import { cors } from 'hono/cors'

const app = new Hono()

app.use('/*', cors())

app.get('/api/users', (c) => {
  return c.json({ users: [] })
})

export default app
```

Without Context7, might generate outdated patterns

---

## When to Use Context7

| Use Case | Context7? |
|----------|-----------|
| React/Vue/Angular basics | No (stable) |
| New library version (v5, v4) | **Yes** |
| Fast-moving libs (Bun, Hono, tRPC) | **Yes** |
| Internal/private libraries | No (not indexed) |
| Breaking change migrations | **Yes** |

---

## With vs Without MCP

| Without MCP | With MCP |
|-------------|----------|
| `gh pr list` via Bash | Direct GitHub tool |
| Manual API calls | Integrated tool calls |
| Parse CLI output | Structured responses |
| Auth in shell env | Auth in MCP config |

**Rule**: MCP when available, Bash as fallback

---

<!-- _class: lead -->

# Part 4: Best Practices & Security

---

## Security Principles

```
┌────────────────────────────────────────────┐
│           🔒 Security First                │
├────────────────────────────────────────────┤
│ ✓ Validate inputs at system boundaries    │
│ ✓ Parameterized queries (never concat)    │
│ ✓ No secrets in code or prompts           │
│ ✓ Review AI-generated code for vulns      │
│ ✓ Use permission modes appropriately      │
└────────────────────────────────────────────┘
```

---

## Permission Modes

| Mode | Behavior |
|------|----------|
| **Ask** | Prompt for every tool |
| **Auto-edit** | Allow edits, ask for bash |
| **YOLO** | Allow everything ⚠️ |

```bash
# Start with appropriate mode
claude --permission-mode ask       # Cautious
claude --permission-mode auto-edit # Balanced
```

---

## Code Review Checklist

Before accepting AI-generated code:

- [ ] No hardcoded secrets or credentials
- [ ] Input validation present
- [ ] SQL uses parameterized queries
- [ ] No XSS vulnerabilities (escaped output)
- [ ] Error messages don't leak internals
- [ ] Follows existing code patterns
- [ ] No unnecessary dependencies added

---

## Common AI Code Pitfalls

```javascript
// ❌ SQL Injection
const query = `SELECT * FROM users WHERE id = ${id}`;

// ✅ Parameterized
const query = 'SELECT * FROM users WHERE id = $1';
db.query(query, [id]);

// ❌ XSS Vulnerability
element.innerHTML = userInput;

// ✅ Safe
element.textContent = userInput;
```

---

<!-- _class: lead -->

# Part 5: Knowledge Bases

---

## CLAUDE.md Hierarchy

```
~/.claude/CLAUDE.md           # Global (all projects)
    │
    ▼
./CLAUDE.md                   # Project root
    │
    ▼
./backend/CLAUDE.md           # Directory-specific
./frontend/CLAUDE.md
```

**Loaded**: All applicable levels, merged

---

## What Goes in CLAUDE.md

```markdown
# Project Name

## Quick Start
- How to run: `npm run dev`
- How to test: `npm test`

## Architecture
- Backend: Node.js + Express
- Frontend: React + TypeScript

## Code Standards
- Use TypeScript strict mode
- Follow existing patterns

## Do NOT
- Add new dependencies without discussion
- Modify database schema directly
```

---

## Specialized Context Files

```
general-context/
├── coding-principles.md   # Universal rules
├── backend-guidelines.md  # API, DB, security
└── frontend-guidelines.md # Components, a11y

# In CLAUDE.md, reference:
## References
- See `general-context/` for detailed guidelines
```

**Benefit**: Load only what's relevant

---

## Context Anti-Patterns

| ❌ Don't | ✅ Do |
|----------|-------|
| Dump entire codebase | Targeted file reads |
| Repeat info in multiple places | Single source of truth |
| Include obvious conventions | Document non-obvious decisions |
| Write novels | Keep it scannable |

---

<!-- _class: lead -->

# Part 6: Live Demo

---

## Demo: Building a Feature

**Scenario**: Add user profile feature

```
1. Tech Lead plans architecture
2. Backend agent implements API
3. Frontend agent implements UI
4. QA agent validates

We'll use:
- Agent orchestration
- Skills for guidance
- Tool calling
- Our knowledge base
```

---

## Demo Structure

```
backend/
├── routes/
│   └── profile.js      # API routes
├── services/
│   └── profileService.js
└── models/
    └── Profile.js

frontend/
├── pages/
│   └── ProfilePage.tsx
├── components/
│   └── ProfileCard.tsx
└── hooks/
    └── useProfile.ts
```

---

## Commands We'll Use

```bash
# Invoke architecture planning
/architecture-planning

# Let Tech Lead delegate to agents
# (automatic with agent spawning)

# Run tests
npm test

# Check our work
git diff
```

---

## Key Takeaways

1. **Skills** = Reusable workflows
2. **Agents** = Domain specialists that work in parallel
3. **Tools** = Use dedicated tools, not bash equivalents
4. **MCP + Context7** = Fresh library docs, external integrations
5. **Security** = Always review AI-generated code
6. **Context** = Structure for efficient loading

---

## Resources

- **Claude Code Docs**: https://docs.anthropic.com/claude-code
- **MCP Spec**: https://modelcontextprotocol.io
- **Context7**: https://context7.com
- **This Repo**: Demo agents, skills, and guidelines

---

<!-- _class: lead -->

# Questions?

**Let's build something!**

