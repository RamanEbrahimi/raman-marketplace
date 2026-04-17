# Playbook — Promote Idea (human-in-the-loop)

**Trigger phrases:** "promote idea <slug>", "make this the active
question", "let's pursue candidate N from the funnel".

**Supervisor action:** `promote idea`

**Agent:** Ideation, then Modeling

**Mode:** human-in-the-loop. The user must explicitly approve before any
state change to `project.yaml` or `notes/overview.md`.

## Inputs

- The candidate slug (`$SLUG`) — must point to an existing
  `notes/gap_briefs/<slug>.md`
- `notes/idea_funnel.md`
- `literature/map.md`

## Steps

1. **Load context.** `agents/ideation/AGENT.md`.
2. **Re-read the gap brief.** Verify it has not been marked dead by a
   prior Verifier pass.
3. **Append a "Promotion request" block** to the gap brief with:
   - Recommended model family (free text — Modeling will formalize).
   - Target venue and backup venues.
   - Minimum publishable bar and loop triggers.
   - First three milestones (model draft, conjecture set, experiment
     plan).
   - Risks the user should accept before promoting.
4. **Stop and surface** to the user:
   - "Ready to promote `<slug>`. This will:"
     - "rewrite `notes/overview.md` to focus on this question"
     - "switch `project.yaml` mode to `project` (if currently
       `discovery`)"
     - "copy the target venue and backup venues into `project.yaml`"
     - "kick off the `refine model` playbook"
   - "Approve? (yes/no)"
5. **On approval:** the Supervisor patches `notes/overview.md` and
   `project.yaml` (`mode`, `target_venue`, `backup_venues`,
   `venue_family`, `publication_bar`), then dispatches
   `agents/playbooks/refine-model.md`.
6. **Log** with `Approval needed: yes` until the user approves; then a
   second log entry on approval.

## Quality contract

- Gap brief has a Promotion request block.
- `project.yaml` is **not** changed until the user approves in chat.
- On approval, `notes/overview.md` reflects the new objective and the
  Modeling playbook starts.
