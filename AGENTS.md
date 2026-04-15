# Project Overview

This is the **site** project.

## Build & Validation

### Backpressure (run before every commit)

```bash
echo 'No backpressure command configured -- set BACKPRESSURE_CMD in .rl/config'
```

## Commit Conventions

All commits use [Conventional Commits](https://www.conventionalcommits.org/):

```
<type>(<scope>): <short description>
```

### Types

| Type | When to use |
|------|-------------|
| `feat` | New feature or capability |
| `fix` | Bug fix |
| `refactor` | Code restructuring, no behavior change |
| `docs` | Documentation only |
| `test` | Adding or updating tests only |
| `chore` | Tooling, config, dependencies, tickets |
| `perf` | Performance improvement |

### Scope

- **Build mode**: Use the ticket ID as scope: `feat(xx-a1b2): implement feature`
- **Interview**: `docs(plan): create proposal for <change-id>`
- **Bootstrap**: `chore(tickets): bootstrap <change-id>`
- **Review**: `fix(review): address PR feedback`
- **Archive**: `docs(specs): archive <change-id>`
- **E2E**: `fix(e2e): <test-name>`

## Ralph Loop Integration

This repository uses the **Ralph Loop** (`rl`) for AI-assisted development with [Claude Code](https://docs.anthropic.com/en/docs/claude-code).

### State Management

| File/Dir | Purpose |
|----------|---------|
| [`.rl/config`](.rl/config) | Ralph Loop configuration |
| [`.tickets/`](.tickets/) | Task tracking via [`tk`](https://github.com/wedow/ticket) |
| [`openspec/specs/`](openspec/specs/) | Living system documentation |
| [`openspec/changes/`](openspec/changes/) | Active proposals with delta specs |
| [`IMPLEMENTATION_PLAN.md`](IMPLEMENTATION_PLAN.md) | High-level vision from interview |
| [`LESSONS.md`](LESSONS.md) | Cumulative learnings |
| [`.claude/skills/`](.claude/skills/) | Agent skills |

### Running the Loop

```bash
rl loop interview            # Claude interviews you
rl loop bootstrap            # Create tickets from proposal
rl loop                      # Build one ticket
rl loop --auto --pr          # Autonomous: bootstrap -> build all -> archive -> PR
rl loop amend                # Amend specs/design to fix gaps
rl loop review               # Address PR review feedback
rl loop e2e                  # Fix E2E test failures
```

### Ticket Workflow

```bash
tk ready                     # Show tickets ready for work
tk ls                        # List all tickets
tk start <id>                # Mark in_progress
tk close <id>                # Mark closed
tk add-note <id> "text"      # Add a note
```
