---
description: Run a Verifier pass over the workspace and append findings to reviews/suggestions.md
argument-hint: ""
---

# /agentic-research:review-workspace

Dispatch the Verifier for an adversarial QC pass.

## What to do

Follow `agents/playbooks/review-workspace.md`. In short:

1. Read `agents/verifier/AGENT.md`.
2. **Overclaim check.** Every numbered theorem in `paper.md` must match
   a claim ID in `project.yaml`, with the right verb for the status.
3. **Notation consistency.** Every symbol in `paper.md` exists in
   `paper/notation.md`. Unused entries → P2.
4. **Citation consistency.** Every paper cited in `paper.md` has a
   BibTeX entry and a `literature/papers/` note.
5. **Proof rigor.** Run `adversarial-proof-review` on the latest
   attempt of every `proofs/<claim-id>.md`.
6. **Reproducibility.** Each `experiments/<exp-id>/` has `code/`,
   `config.yaml`, `results/`, and the figures it references exist.
7. **Novelty drift.** Re-read the latest `literature/map.md` novelty
   assessment; flag drift from the project's current framing.
8. **Venue fit.** Re-read `project.yaml`'s `target_venue`,
   `backup_venues`, and `publication_bar`; flag when the artifact bundle
   no longer supports that venue.
9. Append findings to `reviews/suggestions.md` using the format in
   `agents/verifier/AGENT.md`. Severity P0 / P1 / P2.
10. Log to `logs/agent-history.md`. Set `Approval needed: yes` if any
    P0 findings exist or the recommended route is `retarget venue`.

## Forbidden

- Do not edit any file outside `reviews/suggestions.md`.
- Do not propose specific revised text — that is the Writer's job.
- Do not silently downgrade severity.
