# Claude Workflow Rules — CLAUDE.md Template for Claude Code

A drop-in **`CLAUDE.md`** template with opinionated rules and workflows for [Claude Code](https://claude.com/claude-code), Anthropic's AI coding assistant. Copy one file into your project root to get consistent, high-quality AI output: planning discipline, persistent project memory, language-aware testing, security defaults, and atomic git practices.

## Quick Start

1. Copy `CLAUDE.md` into your project root.

   **macOS / Linux:**

   ```bash
   curl -o CLAUDE.md https://raw.githubusercontent.com/rolandfarkasCOM/claude-workflow-rules/main/CLAUDE.md
   ```

   **Windows (PowerShell):**

   ```powershell
   Invoke-WebRequest -Uri https://raw.githubusercontent.com/rolandfarkasCOM/claude-workflow-rules/main/CLAUDE.md -OutFile CLAUDE.md
   ```

   Or clone this repo and copy the file manually.

2. Open Claude Code in your project and tell it to set everything up:

```
Read CLAUDE.md and set up the project — create the memory directory and task files as described.
```

Claude will create `.claude/memory/` (with `decisions.md`, `patterns.md`, `context.md`, `session.md`) and `tasks/` (with `todo.md`, `lessons.md`). On every future session, Claude Code loads `CLAUDE.md` automatically at startup.

## What's Included

- **First-Time Setup** — Claude asks about your git, PR, and testing preferences on first run
- **How to Work** — planning, subagents, verification, elegance, autonomous bug fixing, self-improvement loop
- **Project Memory** — structured files for architectural decisions, code patterns, domain context, and session logs
- **Testing** — language-appropriate test frameworks (Vitest, Jest, Playwright, pytest, PHPUnit, `go test`, `cargo test`), CI setup, what to test
- **Error Handling** — search-first debugging, language-appropriate patterns, fail loudly in dev / gracefully in prod
- **Task Tracking** — structured approach using `tasks/todo.md` and `tasks/lessons.md`
- **Tool Integration** — [GSD](https://github.com/gsd-build/get-shit-done) for structured execution, [Claude-Mem](https://github.com/thedotmack/claude-mem) for automatic session memory
- **Code Standards** — read before write, minimal changes, root cause fixes, edge case handling
- **Security** — input validation, parameterized queries, rate limiting, audit logging
- **Dependencies** — minimize packages, audit before adding, record decisions
- **Documentation** — update when context changes, not for its own sake
- **Git Discipline** — atomic commits, no force push, user controls commit workflow
- **Checklists** — new project setup and session start protocols

## Memory System

Two layers of memory work together to keep Claude Code aware of your project across sessions:

| Layer | How it works | What it captures |
|---|---|---|
| **Claude-Mem** (automatic) | Plugin captures every session silently | Code changes, errors, decisions — everything |
| **`.claude/memory/`** (deliberate) | You update these files intentionally | Architectural decisions, patterns, domain knowledge, session notes |

Claude-mem is great for "what did I do last week?" — the memory files are for "why did we choose Hono over Express?"

## Optional Tools

Referenced in the rules but not required:

```bash
# GSD — structured project execution with phases and verification gates
npx get-shit-done-cc@latest

# Claude-Mem — persistent session memory with semantic search (requires Bun)
npx claude-mem install
```

## Customization

The rules are a starting point. Add project-specific sections to `CLAUDE.md` for:

- Build commands (`npm run dev`, `npm run build`, `npm run typecheck`)
- Tech stack details (framework, database, hosting)
- Project-specific conventions (naming, file structure, component patterns)
- Key directories and their purpose
- Performance requirements or constraints

## FAQ

### What is CLAUDE.md?

`CLAUDE.md` is the convention Claude Code uses for project-specific instructions. When you open a project, Claude Code automatically loads `CLAUDE.md` from the repo root and treats it as system prompt context for the entire session — so any rules, conventions, or commands you put there shape every response.

### How is this different from a regular README?

A README documents the project for humans. `CLAUDE.md` documents it for the AI coding assistant — preferred workflow, testing approach, security rules, things to avoid. The two complement each other.

### Can I use these rules with Cursor, Cody, or other AI coding tools?

The underlying rules (planning, testing, security, git discipline) apply to any AI assistant. The Claude Code–specific bits — the `CLAUDE.md` auto-loading convention, the `/gsd-*` slash commands, and claude-mem integration — only work with Claude Code itself.

### Where do I put project-specific overrides?

Edit `CLAUDE.md` directly after copying it. The file lives in your repo and is yours to customize — add a section at the bottom for your stack, build commands, and conventions.

## License

MIT
