# Claude Workflow Rules

Opinionated rules for AI-assisted development with Claude Code. Drop into any project to get consistent, high-quality output from AI coding assistants.

## Usage

1. Copy `CLAUDE.md` into your project root:

```bash
curl -o CLAUDE.md https://raw.githubusercontent.com/rolandfarkas/claude-workflow-rules/main/CLAUDE.md
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
- **Testing** — language-appropriate test frameworks, CI setup, what to test
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

Two layers of memory work together:

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

## License

MIT
