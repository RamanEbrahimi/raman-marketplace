---
description: Bump the active model version (human-in-the-loop)
argument-hint: "[brief description of the change]"
---

# /agentic-research:refine-model

Dispatch the Modeling agent to draft `models/model_v<N+1>.md` from the
active gap brief.

## What to do

Follow `agents/playbooks/refine-model.md`. In short:

1. Read `agents/modeling/AGENT.md`, the active gap brief, the current
   `models/model_v<current>.md`, `paper/notation.md`, and
   `conjectures/conjectures.md`.
2. Draft `models/model_v<N+1>.md` using the format in
   `agents/modeling/AGENT.md`, including a "Differences from v<N>"
   section and a "Venue-fit check" section.
3. Append new symbols to `paper/notation.md`. Never rename existing
   ones.
4. Append candidate theorem statements to `conjectures/conjectures.md`
   with stable IDs and status `hypothesis`.
5. Sanity-check each new candidate with `conjecture-testing` on a small
   case. Record the outcome inside the model file.
6. **Stop and surface** to the user with a diff summary:
   - "New model file: `models/model_v<N+1>.md`"
   - "New candidate statements: C<n>, C<n+1>, ..."
   - "Venue-fit judgment: <...>"
   - "Awaiting approval to set `current_model_version: model_v<N+1>` in
     `project.yaml`."
7. Log to `logs/agent-history.md` with `Approval needed: yes`.

## Forbidden

- Do not edit `project.yaml` until the user approves in chat.
- Do not silently overwrite an existing `model_v<N>.md`.
