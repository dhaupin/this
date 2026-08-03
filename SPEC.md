# .this Specification

**Version**: 0.2.0 (Draft)

## Overview

`.this` is a **universal project-context container** for AI agents and humans.

It provides a standardized place for:
- WHO is involved (humans, agents)
- HOW to work together
- WHAT is in scope / out of scope
- WHERE to chain-load other contexts
- **ANY tool/agent/runtime specific files**

## Quick Start

```bash
mkdir .this
echo "# Project Identity" > .this/identity.md
echo "# How to work" > .this/guidance.md
echo "# What's in scope" > .this/scope.md
```

That's it. 3 files = working `.this`.

## The Philosophy: Contain the Chaos

The agent landscape is messy. Every tool has its own conventions:
- `.cursorrules`, `.cursor/rules/`
- `.clinerules`, `.clinerules/`
- `.windsurfrules`
- `CLAUDE.md`, `AGENTS.md`
- `memory-bank/`
- And hundreds more...

**.this doesn't try to fix this. Instead, it CONTAINS it.**

The mess happens INSIDE `.this/`, not scattered across the project root.

```
.this/                    # Your container
├── identity.md           # Core identity
├── guidance.md          # How to work
├── scope.md            # In/out of scope
├── axolotl/            # Your agent context (vant, horcrux, etc.)
├── cursorrules/        # Cursor-specific rules
├── clinerules/         # Cline-specific rules
├── memory-bank/        # Cline memory
├── claude.md           # Claude-specific
└── [anything-else]/   # Whatever new tools emerge
```

## Core Philosophy

1. **Minimal defaults** - Only 3 required files
2. **Opt-in** - If it doesn't exist, nothing breaks
3. **Chainable** - Pull contexts from anywhere
4. **Agent-agnostic** - Readable by any system
5. **Multi-tenant** - Supports multiple humans
6. **Contain the mess** - Chaos goes INSIDE .this, not everywhere
7. **Public by default** - Anyone can read .this contents
8. **Framework responsibility** - Encryption/access control handled by frameworks

## Public vs Private

**.this is public by default.**

The folder and its contents are designed to be readable by:
- Any agent
- Any framework
- Any human

**Sensitive data is the framework's responsibility.**

Frameworks can handle sensitive data in whatever way they choose:
- Encrypted blobs (e.g., Vant horcrux files)
- Separate encrypted folders
- Access control mechanisms

Example:
```
.this/
├── identity.md         # PUBLIC - readable by everyone
├── guidance.md        # PUBLIC - readable by everyone
├── scope.md           # PUBLIC - readable by everyone
├── axolotl/           # Agent-specific (can be encrypted)
│   ├── identity.md    # Who this agent is
│   └── brain.svg      # ENCRYPTED horcrux - framework handles decryption
├── sensitive/        # Framework-specific encrypted data
└── cursorrules/      # PUBLIC - tool-specific rules
```

**The contract:**
- `.this` provides a standardized location
- Frameworks decide how to handle sensitive data
- Users can put public stuff in `.this` with confidence

## Folder Structure

```
.this/
├── identity.md        # WHO - humans + agents (REQUIRED)
├── guidance.md       # HOW - how to work together (REQUIRED)
├── scope.md         # WHAT - in/out of scope (REQUIRED)
├── chain.md         # WHERE - chain-load other .this (OPTIONAL)
├── bounds.md        # LIMITS - hard boundaries (OPTIONAL)
└── [agent_name]/   # Agent-specific context (OPTIONAL)
    ├── identity.md
    ├── guidance.md
    ├── scope.md
    └── [any-files]  # Horcrux, configs, etc.
```

## The "Anything Goes" Layer

Beyond the 3 core files, `.this` is a free-form container:

```
.this/                          # Project root container
├── identity.md                 # Project-level identity (PUBLIC)
├── guidance.md                # Project-level guidance (PUBLIC)
├── scope.md                   # Project-level scope (PUBLIC)
│
├── axolotl/                   # Agent-specific context
│   ├── identity.md            # Who this agent is (PUBLIC)
│   ├── guidance.md            # How to work with this agent (PUBLIC)
│   ├── scope.md               # What this agent does (PUBLIC)
│   ├── brain.svg              # Vant horcrux (ENCRYPTED - framework handles)
│   └── .this/                 # Nested .this (chained)
│
├── cursorrules/               # Cursor rules (PUBLIC)
├── clinerules/                # Cline rules (PUBLIC)
├── vant/                      # Vant config (PUBLIC or ENCRYPTED)
└── [any-folder]/             # Whatever else emerges
```

**The only rule:** Contain the chaos inside `.this/`, not the project root.

**Note:** Contents are public unless the framework explicitly encrypts them.

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

## Hierarchy: Root vs Subfolder

`.this` can exist at project root AND in subfolders. This creates a **hierarchy of context**.

### The Rule: Layered Extension

By default:
- **Root `.this/`** provides the BASE context
- **Subfolder `.this/`** EXTENDS the base context
- More specific contexts ADD to general ones

Example:
```
project/
├── .this/                      # BASE LAYER
│   ├── identity.md            # Project identity
│   ├── guidance.md            # "Follow coding standards"
│   └── scope.md               # "Frontend only"
│
├── backend/
│   └── .this/                 # ADDS TO base
│       ├── guidance.md        # "Backend uses Python"
│       └── scope.md           # "API endpoints only"
│
├── frontend/
│   └── .this/                 # ALSO adds to base
│       ├── guidance.md        # "Frontend uses React"
│       └── scope.md           # "Components only"
│
└── agents/
    └── .this/                 # Agent-specific additions
        └── guidance.md        # "Use Vant for memory"
```

### Override Behavior

To explicitly OVERRIDE (not extend) a parent value, add a header:

```markdown
---
override: true
---

# Backend Guidance

## Workflow
- Use FastAPI
- All endpoints must have type hints
```

### Precedence

When loading context, frameworks should apply:

1. **Root `.this/`** - Base layer (loaded first)
2. **Subfolder `.this/`** - Extends base (loaded after)
3. **More specific paths** - Override when marked

This is similar to:
- CSS specificity (more specific selectors win)
- Environment variables (local overrides global)
- Vant islands (extends brain)

## Principles

1. **Opt-in** - No .this folder = no context loaded
2. **Fallback** - Agent can work without .this
3. **Composable** - Multiple .this can be chained
4. **Portable** - Works with any agent/runtime
5. **Minimal** - Keep it small, reference externally if needed
6. **Human-readable** - Markdown/text preferred over binary
7. **Version-aware** - Include spec version in metadata

## File Formats

**.this prioritizes human readability:**

- **Recommended**: Markdown (`.md`) for all core files
- **Supported**: JSON, YAML for structured data
- **Framework files**: Any format the framework requires (e.g., `.svg` for horcrux)

Core files (`identity.md`, `guidance.md`, `scope.md`, `chain.md`, `bounds.md`) should be Markdown for interoperability.

## Reserved Names

**These folder names have special meaning:**

| Folder | Purpose |
|--------|---------|
| `.this/` | The container itself |
| `.this/.this/` | Nested chained context |

**These filenames are reserved:**

| File | Purpose |
|------|---------|
| `identity.md` | WHO - required core file |
| `guidance.md` | HOW - required core file |
| `scope.md` | WHAT - required core file |
| `chain.md` | WHERE - optional chaining |
| `bounds.md` | LIMITS - optional boundaries |

All other names are free-form.

## Size Recommendations

**Keep `.this` lean:**

- **Core files**: < 10KB each recommended
- **Total .this folder**: < 1MB recommended
- **Rationale**: Context is loaded into agent prompts; huge files = truncation

For large data, use external references:
```markdown
See: ./docs/full-architecture.md
```

## Anti-Patterns

**Don't:**

1. **Duplicate everything** - `.this` should reference, not copy
2. **Make it huge** - Agents have context limits
3. **Hide critical info** - Public by default; use framework encryption for secrets
4. **Ignore updates** - Keep `.this` current or it's worse than nothing
5. **Nest too deep** - Depth > 3 becomes hard to follow

**Do:**

1. **Start small** - Even 3 files is better than none
2. **Link externally** - Large docs belong in project, not `.this`
3. **Be specific** - "Use FastAPI" > "Follow best practices"
4. **Update regularly** - Stale context hurts more than no context

## Adoption Guidance

### Getting Started

1. **Create `.this/` folder** in project root
2. **Add `identity.md`** - Who's involved
3. **Add `guidance.md`** - How to work together
4. **Add `scope.md`** - What's in/out of scope
5. **Optional: Add `chain.md`** - Link to other contexts

### Minimal Example

```
.this/
├── identity.md    # "We build a trading bot"
├── guidance.md    # "Verify before commit"
└── scope.md       # "Backend only"
```

### Growth Path

```
.this/
├── identity.md         # Always
├── guidance.md         # Always
├── scope.md           # Always
├── chain.md           # When linking contexts
├── bounds.md          # When need hard limits
├── axolotl/          # When agent context needed
│   └── brain.svg     # Encrypted horcrux
├── cursorrules/      # When using Cursor
└── docs/             # When docs needed
    └── architecture.md
```

## Framework Integration

**For framework authors:**

### Basic Implementation

```javascript
// Pseudocode for .this loader
async function loadThis(projectPath) {
    const thisPath = path.join(projectPath, '.this');
    if (!fs.existsSync(thisPath)) return null;
    
    const context = {};
    
    // Load core files (Markdown)
    for (const file of ['identity.md', 'guidance.md', 'scope.md']) {
        const content = fs.readFileSync(path.join(thisPath, file), 'utf8');
        if (content) context[file.replace('.md', '')] = content;
    }
    
    // Load optional files
    if (fs.existsSync(path.join(thisPath, 'chain.md'))) {
        context.chain = await loadChain(fs.readFileSync(...));
    }
    
    // Load subfolders as modules
    context.modules = await loadModules(thisPath);
    
    return context;
}
```

### Extended Implementation

```javascript
// With hierarchy and chaining
async function loadThisHierarchy(projectPath) {
    const contexts = [];
    
    // Walk from root to current directory
    const parts = path.relative(projectRoot, projectPath).split(path.sep);
    let currentPath = projectRoot;
    
    for (const part of parts) {
        currentPath = path.join(currentPath, part);
        const thisPath = path.join(currentPath, '.this');
        
        if (fs.existsSync(thisPath)) {
            const ctx = await loadThis(thisPath);
            contexts.push({ path: thisPath, context: ctx });
        }
    }
    
    // Merge with precedence (later = higher priority)
    return mergeContexts(contexts);
}
```

## Version

Current: 0.2.0 (Draft)

### Changelog

- **0.2.0**: Added hierarchy, adoption guidance, anti-patterns
- **0.1.0**: Initial spec with core philosophy

## Collision Considerations

This specification uses the folder name `.this` in good faith.

If conflicts arise with established projects or trademarks, we will work in good faith to resolve them.

See [LEGAL.md](./LEGAL.md) for full disclaimer.

---

*This spec is agent-agnostic. Implementations may vary.*
