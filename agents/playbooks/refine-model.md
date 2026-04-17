# Playbook — Refine Model (human-in-the-loop)

**Trigger phrases:** "refine the model", "bump the model", "promote
candidate <slug> to the model".

**Supervisor action:** `refine model`

**Agent:** Modeling

**Mode:** human-in-the-loop. The Supervisor must surface a checkpoint to
the user before changing `current_model_version`.

## Inputs

- The active gap brief (`notes/gap_briefs/<slug>.md`) or a user-supplied
  diff
- `models/model_v<current>.md`
- `paper/notation.md`
- `conjectures/conjectures.md`
- The relevant section of `literature/map.md`
- `project.yaml` venue fields, or the promoted gap brief if the venue has
  not yet been copied into `project.yaml`

## Steps

1. **Load context.** Read `agents/modeling/AGENT.md` and skills.
2. **Diff the model.** State precisely what is changing: new primitive,
   relaxed assumption, new payoff, new solution concept, etc. State how
   the change is supposed to help clear the active target venue's bar.
3. **Draft `models/model_v<N+1>.md`** using the format in
   `agents/modeling/AGENT.md`. Include a "Differences from v<N>" section
   that links to the motivation and a "Venue-fit check" section.
4. **Update notation.** Append any new symbols to `paper/notation.md`.
   Never rename existing ones.
5. **Add candidate statements** to `conjectures/conjectures.md` with
   status `hypothesis`. Use stable IDs (`C<n>`).
6. **Sanity-check** each new candidate with the `conjecture-testing`
   skill on a small case.
7. **Stop at the checkpoint.** Do not edit `project.yaml`. Report to the
   Supervisor:
   - Files written: `models/model_v<N+1>.md`, `paper/notation.md`,
     `conjectures/conjectures.md`
   - Diff summary
   - Venue-fit judgment (`meets bar`, `close-needs-model`,
     `strong-but-wrong-venue`, ...)
   - Recommended next action ("Run proof pass on C1, C2")
   - "Awaiting user approval to set
     `current_model_version: model_v<N+1>` in `project.yaml`."
8. **Log** the run summary to `logs/agent-history.md` with
   `Approval needed: yes`.

## Quality contract

- New model file exists with full primitives, payoffs, assumptions, and
  candidate statements.
- Notation file updated.
- Conjectures file updated.
- `project.yaml` is unchanged.
- Run summary in history.
