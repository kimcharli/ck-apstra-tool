# How to Apply Spec-Driven Development in This Project

This document is the human reference for the SDD workflow used in `ck-apstra-tool`.
For AI agent behavioral rules, see `AGENTS.md`. For active specs, see `specs/`.

---

## Why SDD

AI coding tools are most effective when given complete context upfront. Without
a spec, the AI discovers requirements incrementally — leading to rework, drift,
and code that's hard to explain later. A spec written before coding:

- Forces clarity on *what* and *why* before *how*
- Gives the AI a single source of truth to validate against
- Becomes living documentation that outlasts any one session
- Makes switching tools (Claude → Gemini → Copilot) frictionless

---

## The Three Documents

All active specs live in `specs/`. Each has a `Status` field that drives the gates.

### `specs/requirements.md` — What & Why
Written before anything else. Answers:
- What problem are we solving?
- Who is affected?
- What does success look like? (acceptance criteria)
- What is explicitly out of scope?

**Status flow:** DRAFT → APPROVED → IN-PROGRESS → DONE

### `specs/design.md` — How
Written after requirements are APPROVED. Answers:
- What is the architecture?
- What are the key data models and interfaces?
- What tradeoffs were considered and why?

**Status flow:** DRAFT → APPROVED → IN-PROGRESS → DONE

### `specs/tasks.md` — Ordered steps
Written after design is APPROVED. Contains:
- Atomic, independently committable tasks
- Explicit dependencies between tasks
- A completed section tasks move into when done

**Status flow:** DRAFT → APPROVED → IN-PROGRESS → DONE

---

## The Gates

The AI enforces three gates before writing any code:

```
requirements APPROVED → design APPROVED → task exists in tasks.md
```

If any gate fails, the AI stops and tells you which gate failed and what to do
next. This is intentional friction — it keeps the specs honest.

---

## The Daily Loop

### Starting a new feature
1. Open `specs/requirements.md`, describe the feature under a new User Story
2. Ask Claude: *"Read AGENTS.md and specs/requirements.md. Help me complete
   requirements for [feature]."*
3. Review, approve (change status to `APPROVED`)
4. Ask Claude: *"Requirements are approved. Let's write the design."*
5. Review, approve
6. Ask Claude: *"Design is approved. Break this into tasks."*
7. Review tasks, approve
8. Ask Claude: *"Execute T01."* — Claude implements, commits, marks `[x]`
9. Repeat for each task

### Mid-feature discovery
If something new surfaces during implementation:
1. Stop Claude: *"Hold on, this changes the requirements."*
2. Update `specs/requirements.md` (back to DRAFT if needed)
3. Cascade the change through design and tasks before resuming

### Per-feature specs
For larger or parallel features, create a dedicated file in `specs/features/`
using `specs/features/_template.md`. This keeps the top-level docs clean while
preserving full traceability per feature.

---

## Model Selection

| Phase | Model |
|---|---|
| Requirements, Design | Claude Opus (deeper reasoning) |
| Tasks, Implementation | Claude Sonnet (faster, cost-effective) |

---

## What Belongs Where

| Content | Location |
|---|---|
| AI behavioral rules, gates | `AGENTS.md` |
| Active requirements / design / tasks | `specs/*.md` |
| Per-feature breakdown | `specs/features/*.md` |
| This guide and stable reference docs | `docs/` |
| Source code | `src/ck_apstra_tool/` |
| Tests | `tests/` |

---

## Common Mistakes

**Skipping to design before requirements are approved**
The AI will stop you, but if you override it, you'll pay in rework. The
requirements gate exists because vague requirements produce vague designs.

**Treating tasks.md as a scratchpad**
Tasks should be atomic and committable. If a task takes more than ~1 hour,
it's too big — break it down.

**Not updating specs when requirements change**
The spec is the source of truth. If the code diverges from the spec without
a spec update, the next session starts with wrong context.

**Editing code directly without a task entry**
Small "quick fixes" that bypass tasks.md accumulate into undocumented drift.
Always add the task, even if it's one line.
