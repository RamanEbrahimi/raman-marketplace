---
description: End-to-end compound pipeline (literature → ideation → modeling → proof + experiment → draft → verifier) with two human checkpoints
argument-hint: "<topic>"
---

# /agentic-research:full-pipeline

The longest-running compound action. Two mandatory human checkpoints.

## What to do

Follow `agents/playbooks/full-pipeline.md` exactly. In short:

1. **Confirm with the user before doing anything.** Surface the full
   plan, including venue selection + venue-aware loop behavior, and
   estimated wall-clock. Ask "Proceed?". Do not start without explicit
   approval.
2. **Step A — Literature pass** on `$ARGUMENTS`
   (`agents/playbooks/literature-review.md`).
3. **Step B — Ideation pass** (Step 2 of `agents/playbooks/ideate.md`).
4. **Checkpoint 1.** Surface the ranked candidates from
   `notes/idea_funnel.md` with target venue + backup venues. Ask:
   "Which candidate to promote? Or abort?"
5. **Step C — Promote.** On approval, run
   `/agentic-research:promote-idea <slug>`.
6. **Step D — Modeling pass.** `/agentic-research:refine-model`.
7. **Checkpoint 2.** Surface the new `models/model_v<N+1>.md`. Ask:
   "Approve the model bump? Or abort?"
8. **Step E — Proof pass.** `/agentic-research:proof-pass` on the new
   candidate statements.
9. **Step F — Experiment pass.** `/agentic-research:experiment-pass`
   targeting the same statements.
10. **Step G — Venue gate.** Judge whether the current proof +
    experiment bundle clears the active target venue's publication bar.
    If not, route to the right loop:
    - proof gap only → proof again
    - experiment gap only → experiment again
    - model mismatch → modeling
    - novelty / framing mismatch → literature
    - strong result but wrong venue → stop and surface a retarget-venue
      checkpoint
11. **Step H — Update draft.** `/agentic-research:update-draft` (which
    auto-chains a Verifier pass).
12. **Final report.** One paragraph summarizing every artifact touched,
    every status change, and the recommended next action.

## Failure handling

Any sub-step that fails its quality contract aborts the pipeline unless
the venue gate sends the run into an explicit recovery loop. Log the
abort and surface a clear "resumable from step <X>" message.

## Forbidden

- Do not skip the two human checkpoints.
- Do not start without an explicit "proceed" from the user.
