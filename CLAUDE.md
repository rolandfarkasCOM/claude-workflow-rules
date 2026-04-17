# Claude Workflow Rules

Opinionated rules for AI-assisted development with Claude Code. Drop into any project root as `CLAUDE.md`.

---

## First-Time Setup

When this file is placed in a new project for the first time, ask the user:

1. **Git workflow** — "Do you want me to commit changes, or will you handle commits yourself?"
2. **PR/review workflow** — "Should I create PRs and run code reviews, or is this a direct-to-main workflow?"
3. **Testing approach** — "What testing framework does this project use, or should I set one up?"

Record the answers in `.claude/memory/context.md` so future sessions respect the same preferences.

---

## How to Work

**Always plan first.** Any task with 3+ steps or architectural impact starts in plan mode. Spec it out, confirm the approach, then build. If the plan breaks mid-execution, stop and re-plan — don't push through a bad approach.

**Use subagents aggressively.** Keep the main context window focused. Offload research, file exploration, and parallel analysis to subagents. One clear task per subagent. For complex problems, spawn several in parallel.

**Prove it works before calling it done.** Run type checks, builds, and tests. Diff against the expected behavior. Ask yourself: would a senior engineer sign off on this?

**Prefer elegance, but don't over-engineer.** For non-trivial changes, pause and consider if there's a cleaner approach. If a fix feels hacky, step back and implement the right solution. For simple, obvious fixes — just do them.

**Fix bugs autonomously.** When given a bug report, diagnose and fix it. Read logs, trace errors, resolve failing tests. Don't ask for hand-holding or wait for instructions. If CI is red, go fix it.

**Learn from every correction.** When the user corrects your approach, capture the lesson in `tasks/lessons.md`. Write a rule that prevents the same mistake. Review lessons at the start of each session.

---

## Project Memory

Claude-mem handles automatic session memory. These files capture **deliberate** project knowledge that persists across sessions and contributors.

### Memory Files

Read these at session start. Update them when you make significant decisions or discover important context.

| File | What goes in it | When to update |
|---|---|---|
| `.claude/memory/decisions.md` | Architectural choices, rationale, alternatives considered | When choosing libraries, patterns, structure, or deprecating existing solutions |
| `.claude/memory/patterns.md` | Established code patterns, naming conventions, component structures | When creating reusable patterns, utilities, or hooks |
| `.claude/memory/context.md` | Domain knowledge, business rules, user preferences, workflow choices | When learning requirements, terminology, or user preferences for this project |
| `.claude/memory/session.md` | Rolling log of recent work (keep last 10-15 entries) | At the end of each significant task or discovery |

### Format

**decisions.md:**
```
## 2026-04-17 — Use Hono over Express
Chose Hono for Cloudflare Workers compatibility. Express doesn't run on Workers.
Alternatives: itty-router (too minimal), Elysia (Bun-only).
```

**session.md:**
```
## 2026-04-17
- Fixed passkey origin mismatch by reading Origin header in getRelyingParty()
- Added OAuth account linking endpoints (POST /auth/me/link/github)
- Updated Profile page with Link GitHub/Google buttons
```

---

## Testing

**Write tests appropriate to the codebase.** Detect the language and framework, then use the right tool:

| Stack | Testing approach |
|---|---|
| TypeScript/JavaScript | Vitest, Jest, or Playwright for E2E |
| Python | pytest |
| PHP | PHPUnit |
| Go | Built-in `go test` |
| Rust | Built-in `cargo test` |

**CI is mandatory for any project using git.** Set up GitHub Actions (or equivalent) with:
- Type checking / linting for the language
- Build verification
- Test suite execution
- Security audit (`npm audit`, `pip audit`, etc.)

**Don't test everything — test what matters.** Focus on:
- Business logic and edge cases
- Auth and security paths
- API contracts and response shapes
- Things that have broken before

---

## Error Handling

**Search for solutions before guessing.** When encountering unfamiliar errors, use web search or documentation before attempting fixes. Don't guess at solutions — look them up.

**Use the error handling pattern appropriate to the codebase:**
- TypeScript: typed errors, try/catch at boundaries, never swallow errors silently
- React: Error Boundaries for UI, try/catch for async
- API routes: consistent error response shape, appropriate HTTP status codes
- Database: handle constraint violations, connection failures, timeouts

**Fail loudly in development, gracefully in production.** Log full error details server-side. Return generic messages to users. Never expose stack traces, file paths, or internal state to clients.

**When debugging:** read the actual error message, check the stack trace, reproduce the issue, then fix the root cause. Don't add random try/catches hoping something sticks.

---

## Task Tracking

1. Write the plan to `tasks/todo.md` with checkable items
2. Confirm the plan before starting implementation
3. Mark items complete as you go — don't batch updates
4. Summarize changes at each milestone
5. After corrections, update `tasks/lessons.md`

---

## Tools

### GSD (Get Shit Done)

Structured project execution with phases, verification gates, and project memory.

```
Install: npx get-shit-done-cc@latest
```

**Core flow:** `/gsd-new-project` → `/gsd-plan-phase N` → `/gsd-execute-phase N` → `/gsd-verify-work` → `/gsd-ship`

| Command | When to use |
|---|---|
| `/gsd-new-project` | Starting a new project from scratch |
| `/gsd-map-codebase` | Understanding an existing codebase before planning |
| `/gsd-discuss-phase N` | Talking through how a phase should work |
| `/gsd-plan-phase N` | Creating a detailed execution plan |
| `/gsd-execute-phase N` | Building with verification gates |
| `/gsd-verify-work` | Checking completed work meets criteria |
| `/gsd-ship` | Final review before shipping |
| `/gsd-quick` | Small task, no full planning needed |
| `/gsd-do` | Just do a simple thing |
| `/gsd-debug` | Structured debugging |
| `/gsd-code-review` | Review code quality |
| `/gsd-autonomous` | Let Claude run autonomously |
| `/gsd-secure-phase N` | Security-focused phase |
| `/gsd-add-tests` | Add test coverage |
| `/gsd-audit-fix` | Fix security audit findings |
| `/gsd-cleanup` | Code cleanup pass |
| `/gsd-pause-work` / `/gsd-resume-work` | Save and restore work state |
| `/gsd-progress` | Check project status |
| `/gsd-help` | Full command reference |

Creates `.planning/` with `PROJECT.md`, `REQUIREMENTS.md`, `ROADMAP.md`, `STATE.md`, and phase plans.

### Claude-Mem

Persistent memory across sessions with semantic search. Runs automatically as a Claude Code plugin — captures decisions, code changes, and errors from every session. Injects relevant context into future sessions.

```
Install: npx claude-mem install (requires Bun)
```

| Command | Purpose |
|---|---|
| `/mem-search <query>` | Find relevant context from past sessions |
| `npx claude-mem start` | Start the memory worker |
| `http://localhost:37777` | Memory dashboard |

Claude-mem handles the **automatic** memory. The `.claude/memory/` files above are for **deliberate** knowledge you want to preserve regardless of what claude-mem captures.

---

## Code Standards

**Read before you write.** Always understand existing code before modifying it. Don't guess at patterns — read the file.

**Do what was asked, nothing more.** Don't add features, refactor surrounding code, add docstrings to unchanged functions, or design for hypothetical future requirements. If three similar lines work, don't abstract them.

**Fix root causes.** Don't patch symptoms. Don't retry the same failed approach — diagnose why it failed first. If blocked, explain what you tried and what didn't work.

**Handle edge cases.** Empty strings, null, NaN, negative numbers, oversized inputs, concurrent requests. Test with adversarial inputs, not just the happy path.

---

## Security

- Validate inputs at system boundaries — never trust client data
- Parameterized queries only — no string interpolation for SQL
- Timing-safe comparisons for all authentication checks
- Rate limit every public-facing endpoint
- Audit log security-critical events with IP and user-agent
- Never commit secrets, `.env` files, or credentials
- Sanitize all outputs — XSS, header injection, path traversal
- Principle of least privilege for all access controls
- Think about attack vectors before writing the code, not after

---

## Dependencies

**Minimize new dependencies.** Before adding a package:
- Check if the functionality can be done in a few lines of code
- Check the package size, maintenance status, and download count
- Check for known vulnerabilities (`npm audit`, `pip audit`)
- Prefer well-maintained packages with minimal transitive dependencies
- Record the decision and rationale in `.claude/memory/decisions.md`

**Keep dependencies updated.** When working on a project, check for outdated or vulnerable packages and flag them.

---

## Documentation

**Update docs when the context changes:**
- README — when setup steps, dev workflow, or deployment process changes
- API docs — when endpoints are added, changed, or removed
- Inline comments — only when the logic isn't self-evident from the code
- AGENTS.md — when the codebase structure changes in ways AI needs to know

**Don't create documentation for its own sake.** No docstrings on obvious functions. No README updates for internal refactors. No changelog entries unless the project uses one.

---

## Git

- One logical change per commit
- Never force push to main
- Never skip hooks (`--no-verify`)
- New commits, not amends (unless asked)
- Stage specific files — never `git add -A`
- Commit messages explain why, not what
- Only commit when the user asks, unless user has opted into auto-commits (recorded in `.claude/memory/context.md`)

---

## Starting a New Project

```
1. [ ] Create CLAUDE.md (copy this file)
2. [ ] Create .claude/memory/ with decisions.md, patterns.md, context.md, session.md
3. [ ] Create tasks/ with todo.md and lessons.md
4. [ ] Ask user about git/PR/testing preferences → record in context.md
5. [ ] Run /gsd-new-project for structured planning
6. [ ] Set up .gitignore
7. [ ] Set up CI with type checking, build, tests, and security audit
8. [ ] README with setup and deploy instructions
9. [ ] Confirm claude-mem is active
```

## Starting a Session

1. Read `.claude/memory/` files — decisions, patterns, context, recent session log
2. Search memory: `/mem-search` for relevant past context
3. Read `STATE.md` and `.planning/` artifacts (if using GSD)
4. `git status` — understand current state
5. Check CI status and open issues
6. Confirm the task before starting
