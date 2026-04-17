---
description: Promote a candidate from the idea funnel to the active research question (human-in-the-loop)
argument-hint: "<gap-brief-slug>"
---

# /agentic-research:promote-idea

Human-in-the-loop promotion of a gap brief into the active model line.

## Inputs

- `$ARGUMENTS` — the gap brief slug. Must match an existing
  `notes/gap_briefs/<slug>.md`. If empty, list the candidates from
  `notes/idea_funnel.md` and ask the user to pick.

## What to do

Follow `agents/playbooks/promote-idea.md` exactly. In short:

1. Verify the gap brief exists and has not been demoted to `_dead/`.
2. Append a "Promotion request" block to the gap brief with recommended
   model family, target venue, backup venues, first three milestones, and
   risks.
3. **Stop and surface** to the user:
   - "Ready to promote `<slug>`. This will rewrite `notes/overview.md`,
     switch `project.yaml` mode to `project` if needed, copy the target
     venue / backup venues / publication bar into `project.yaml`, and kick
     off the `refine-model` playbook. Approve? (yes/no)"
4. On approval:
   - Patch `notes/overview.md` with the new objective.
   - Patch `project.yaml`: set `mode: project`, `target_venue`,
     `backup_venues`, `venue_family`, and `publication_bar`.
   - Dispatch `/agentic-research:refine-model`.
5. Log both events to `logs/agent-history.md`.

## Forbidden

- Do **not** patch `project.yaml` or `notes/overview.md` until the user
  approves in chat.
- Do not start `refine-model` before approval.
