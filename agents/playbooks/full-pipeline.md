# Playbook — Full Pipeline (compound)

**Trigger phrases:** "run the full pipeline on X", "do everything for
topic X", "go end-to-end".

**Supervisor action:** `full pipeline`

**Agents:** Literature → Ideation → (user checkpoint) → Modeling → (user
 checkpoint) → Proof + Simulation in parallel → venue gate with loops →
 Writer → Verifier

**Mode:** compound, with **two mandatory human checkpoints**. This is
the longest-running action; do not start it without confirming with the
user.

## Inputs

- Topic = `$TOPIC`
- Active project slug
- `notes/overview.md`

## Steps

1. **Confirm with the user.** Before doing anything, surface:
   - "I'm about to run the full pipeline on `$TOPIC`. This will run a
     literature pass, an ideation pass, and stop at a checkpoint to ask
     which candidate to promote. After promotion it will run a modeling
     pass, then proof and experiment passes in parallel, then a venue gate
     that may loop back to literature / modeling / proof / experiment if
     the target publication bar is not met, then update the draft, then a
     Verifier pass. Estimated wall-clock: long. Proceed?"
2. **Step A — Literature pass.**
   `agents/playbooks/literature-review.md` on `$TOPIC`.
3. **Step B — Ideation pass.** Step 2 of
   `agents/playbooks/ideate.md` (just the ideation, not the full
   compound — we will do verification + report differently here).
4. **Checkpoint 1.** Surface the ranked candidates from
   `notes/idea_funnel.md`, including target venue + backup venues for each
   surviving candidate, and ask: "Which candidate to promote? Or abort?"
5. **Step C — Promote.** On user approval, run
   `agents/playbooks/promote-idea.md` on the chosen candidate.
6. **Step D — Modeling pass.** Run
   `agents/playbooks/refine-model.md`.
7. **Checkpoint 2.** Surface the new `models/model_v<N+1>.md` and ask:
   "Approve the model bump? Or abort?"
8. **Step E — Proof pass.** Run
   `agents/playbooks/proof-pass.md` on the new candidate statements.
9. **Step F — Experiment pass.** Run
   `agents/playbooks/experiment-pass.md` targeting the same statements
   for empirical evidence.
10. **Step G — Venue gate.** Use `skills/venue-targeting/SKILL.md` to
    assess the active `target_venue` after Proof + Experiment:
    - `meets bar` → continue to Step H.
    - `close-needs-proof` → loop back to Step E once.
    - `close-needs-experiment` → loop back to Step F once.
    - `close-needs-model` → loop back to Step D.
    - `close-needs-literature` → loop back to Step A.
    - `strong-but-wrong-venue` → stop and surface a retarget-venue
      checkpoint to the user.
    - `not-publishable-yet` after repeated loops → stop and surface a
      resumable state to the user.
    Record each loop in `project.yaml` `loop_history`.
11. **Step H — Update draft.** Run
    `agents/playbooks/update-draft.md`. This auto-chains a Verifier
    pass.
12. **Final report.** Surface to the user a one-paragraph summary of
    every artifact touched, every status change, and the recommended
    next action.

## Failure handling

Any sub-step that fails its quality contract aborts the pipeline unless
the venue gate explicitly routes a recovery loop. The Supervisor logs the
abort and surfaces it to the user with a clear "resumable from step <X>"
message.

## Quality contract

The compound run is "done" when:

- A new literature map section exists.
- A promoted gap brief exists.
- A new model version file exists.
- At least one proof file has been updated.
- At least one experiment summary has been written.
- A target venue is selected and used in the venue gate.
- `paper/paper.md` has been updated and validated.
- A Verifier section has been appended to `reviews/suggestions.md`.
- A compound entry exists in `logs/agent-history.md`.
- The user has been surfaced a final report.
