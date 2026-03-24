# ck-apstra-tool — Project Constitution

## Purpose
>
> (Fill in: one paragraph on what this tool does and why it exists)

## Stack & Environment

- **Language**: Python (managed via `uv`)
- **Target**: Apstra controllers / blueprints
- **Auth**: Apstra REST API (token-based)
- **Runtime**: macOS / Linux CLI

## Project Structure

```text
ck-apstra-tool/
├── AGENTS.md                    # This file — read first, always
├── README.md
├── specs/
│   ├── requirements.md          # What + Why  (edit before any feature)
│   ├── design.md                # How         (edit after requirements approved)
│   ├── tasks.md                 # Ordered tasks (edit after design approved)
│   └── features/                # Per-feature spec files
│       └── _template.md
├── src/
│   └── ck_apstra_tool/
├── tests/
└── pyproject.toml
```

## Conventions

- All code lives under `src/ck_apstra_tool/`
- Tests mirror the source tree under `tests/`
- Use `uv` for dependency management — never `pip install` directly
- Commit after every completed task in `specs/tasks.md`
- Mark tasks `[x]` in `specs/tasks.md` before committing

## SDD Gates — enforce before every code change

Read `specs/requirements.md`, `specs/design.md`, and `specs/tasks.md` at the start
of every session. Then enforce these gates in order:

1. **requirements gate** — `specs/requirements.md` status must be `APPROVED`
   → If DRAFT: stop, tell the user, help them complete requirements first
2. **design gate** — `specs/design.md` status must be `APPROVED`
   → If DRAFT: stop, tell the user, help them complete design first
3. **task gate** — the work must map to an open `[ ]` item in `specs/tasks.md`
   → If missing: add the task entry before writing any code

If a gate fails, refuse to write code and name the gate that failed.
If a requirement changes mid-implementation, stop and update specs first.

## Apstra Context

- Controllers managed: (fill in IPs / environments)
- Blueprint naming convention: (fill in)
- Key SDK / MCP: `ck-apstra-mcp-exec` at `~/Projects/ck-apstra-mcp-exec`

## Claude Code Workflow

- Use Opus for spec phases (requirements, design)
- Use Sonnet for implementation phases (tasks, code)
- Spawn subagents per task in `specs/tasks.md` for parallel execution


## Pre-commit (run before every commit)
```bash
uv run ruff check --fix && uv run ruff format
uv run pytest
```

## Shell: Multi-line Content
NEVER pass multi-line strings inline to the shell. Write to `work/tmp/<name>.txt` first.

```bash
# ✅ git commit -F work/tmp/commit_msg.txt
# ✅ bash work/tmp/run_script.sh
# ✅ python work/tmp/patch.py
```

