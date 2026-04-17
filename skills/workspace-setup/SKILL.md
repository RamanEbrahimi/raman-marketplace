---
name: workspace-setup
description: Locate the active project, validate workspace state, and load required artifacts before any command executes.
---

# Skill — Workspace Setup

Run this skill at the start of **every** command invocation, before reading
any project artifacts or dispatching any agent.

## Step 1 — Locate the active project

1. List the contents of `projects/active/`.
2. If the directory is **empty** → stop. Tell the user:
   > "No active project found under `projects/active/`. Create a project
   > folder first (e.g. `projects/active/my-topic/`) with at least a
   > `notes/overview.md` and a `project.yaml`."
3. If there are **two or more** subdirectories → stop. Tell the user which
   slugs are present and ask them to archive all but one.
4. If there is **exactly one** subdirectory → capture `$SLUG` and proceed.

## Step 2 — Read project state

Load these files in order. If any is missing, note it and continue (do not
abort unless the command strictly requires it):

| File | Required for |
|------|-------------|
| `projects/active/$SLUG/project.yaml` | All commands |
| `projects/active/$SLUG/notes/overview.md` | All commands |
| `projects/active/$SLUG/literature/map.md` | Literature, Ideation, Writer |
| `projects/active/$SLUG/conjectures/conjectures.md` | Modeling, Proof, Writer |
| `projects/active/$SLUG/models/model_v<current>.md` | Proof, Simulation, Writer |

The value of `current_model_version` is in `project.yaml`; use it to
resolve `model_v<current>.md`.

If present, also read these optional venue fields from `project.yaml`:

- `target_venue`
- `backup_venues`
- `venue_family`
- `publication_bar`
- `loop_history`

## Step 3 — Validate mode

Read `mode` from `project.yaml` (values: `discovery` | `project`).

- If `mode: discovery` and the command is `proof-pass` or
  `experiment-pass` → refuse immediately:
  > "The project is in discovery mode. Proof and simulation passes require
  > the project to be in 'project' mode. Run a modeling pass and promote
  > to project mode first."

## Step 4 — Validate the `inbox/`

Check `projects/active/$SLUG/inbox/` for any PDF files.
- If PDFs are present and the running command is `literature-review` or
  `ideate`, note them in the context you pass to the Literature agent.
  The `inbox-parser` skill handles the actual extraction.

## Step 5 — Report to the running command

Surface a one-line preamble before proceeding:

> "Active project: `$SLUG` | mode: `$MODE` | model: `$MODEL_VERSION` |
> target venue: `$TARGET_VENUE` | open claims: `$OPEN_COUNT` | inbox PDFs:
> `$PDF_COUNT`"

Replace unavailable values with `—`.

## Failure modes

| Situation | Action |
|-----------|--------|
| `project.yaml` missing | Warn, assume `mode: discovery`, proceed |
| `notes/overview.md` missing | Ask the user for a brief topic before dispatching |
| Active project folder unreadable | Abort, report the path |
