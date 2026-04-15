# Claude Code Configuration for site

You are working in the **site** project.

## Essential Files to Read

1. [`AGENTS.md`](AGENTS.md) -- project commands, conventions, and operational patterns
2. [`.tickets/`](.tickets/) -- active task tickets managed by [`tk`](https://github.com/wedow/ticket) CLI
3. [`openspec/specs/`](openspec/specs/) -- living system documentation (source of truth for behavior)
4. [`IMPLEMENTATION_PLAN.md`](IMPLEMENTATION_PLAN.md) -- high-level vision from interview
5. [`LESSONS.md`](LESSONS.md) -- cumulative learnings from prior iterations
6. [`.claude/skills/`](.claude/skills/) -- agent skills (read relevant ones per ticket)
7. [`.rl/config`](.rl/config) -- Ralph Loop configuration

## Context

You may be running as part of the **Ralph Loop** -- an autonomous development pattern where each iteration spawns a fresh Claude Code instance. Your context is clean each time; continuity comes from:

- Git history (recent commits show what was done)
- [`.tickets/`](.tickets/) via [`tk`](https://github.com/wedow/ticket) `ready` / `ls` (tracks remaining tasks)
- [`openspec/`](openspec/) (specs and active change proposals via [OpenSpec](https://github.com/fission-ai/openspec))
- [`AGENTS.md`](AGENTS.md) (operational guide)
- [`LESSONS.md`](LESSONS.md) (cumulative learnings from prior iterations)
- [`.claude/skills/`](.claude/skills/) (reusable agent knowledge)

## Key Commands

```bash
# Backpressure (run before every commit)
echo 'No backpressure command configured -- set BACKPRESSURE_CMD in .rl/config'
```

## Rules

- Write tests for every implementation -- code without tests is not complete
- Run backpressure before committing
- Use **[conventional commits](https://www.conventionalcommits.org/)**: `<type>(<scope>): <description>` (see [`AGENTS.md`](AGENTS.md) for types/scopes)
- Use [`tk`](https://github.com/wedow/ticket) to manage tickets: `tk start`, `tk close`, `tk add-note`
- Append learnings to [`LESSONS.md`](LESSONS.md) when you discover something non-obvious
- Read relevant skills from [`.claude/skills/`](.claude/skills/) before starting a task
