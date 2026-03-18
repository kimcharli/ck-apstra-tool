# ck-apstra-tool

> Apstra automation tooling — built with SDD discipline.

## Development Workflow

This project uses **Spec-Driven Development**. Before writing any code:

1. Update `specs/requirements.md` → get approval
2. Update `specs/design.md` → get approval
3. Update `specs/tasks.md` → execute tasks in order, commit after each

See `AGENTS.md` for full project constitution and Claude Code guidance.

## Setup

```bash
uv sync
uv run ck-apstra-tool --help
```
