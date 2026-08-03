# .this Specification

## Overview

`.this` is a universal project-context folder specification for AI agents and humans.

It provides a standardized way to define:
- WHO is involved (humans, agents)
- HOW to work together
- WHAT is in scope / out of scope
- WHERE to chain-load other contexts

## Core Philosophy

1. **Minimal defaults** - Only 3 required files
2. **Opt-in** - If it doesn't exist, nothing breaks
3. **Chainable** - Pull contexts from anywhere
4. **Agent-agnostic** - Readable by any system
5. **Multi-tenant** - Supports multiple humans

## Folder Structure

```
.this/
├── identity.md    # WHO - humans + agents (REQUIRED)
├── guidance.md   # HOW - how to work together (REQUIRED)
├── scope.md     # WHAT - in/out of scope (REQUIRED)
├── chain.md     # WHERE - chain-load other .this (OPTIONAL)
└── bounds.md    # LIMITS - hard boundaries (OPTIONAL)
```

## File Specifications

### identity.md

WHO is involved in this project.

```markdown
# Project Identity

## Humans
- name: D
  role: Creator
  contact: dhaupin

## Agents
- name: Axolotl
  role: Developer
  source: vant
```

### guidance.md

HOW to work together.

```markdown
# Guidance

## Communication
- Prefix notes with [note] for non-blocking context
- Ask before destructive operations

## Workflow
1. Understand before modifying
2. Verify build after changes
3. Keep scope focused
```

### scope.md

WHAT is in scope / out of scope.

```markdown
# Scope

## In
- Frontend React components
- Build configuration

## Out
- Backend services
- Infrastructure changes
```

### chain.md

WHERE to chain-load other contexts.

```markdown
# Chain

## Load
- ~/my-soul/.this/          # My soul/identity
- ../shared-standards/.this/ # Team standards
```

### bounds.md

LIMITS - hard boundaries.

```markdown
# Bounds

## Never
- Push to main branch
- Delete production resources
- Access secrets without permission
```

## Chaining

The `chain.md` file allows pulling context from other `.this` folders:
- Relative paths: `../project/.this/`
- Absolute paths: `~/my-context/.this/`
- URLs: `https://example.com/context/.this/` (future)

## Principles

1. **Opt-in** - No .this folder = no context loaded
2. **Fallback** - Agent can work without .this
3. **Composable** - Multiple .this can be chained
4. **Portable** - Works with any agent/runtime
5. **Minimal** - Keep it small, reference externally if needed

## Version

Current: 0.1.0 (Draft)

---

*This spec is agent-agnostic. Implementations may vary.*
