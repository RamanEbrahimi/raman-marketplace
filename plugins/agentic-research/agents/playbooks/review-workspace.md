# Playbook — Review Workspace

**Trigger phrases:** "review the workspace", "audit the project", "check
for inconsistencies", "verifier pass".

**Supervisor action:** `review workspace`

**Agent:** Verifier

**Mode:** autonomous QC.

## Inputs

The full project tree, read-only:

- `paper/paper.md`, `paper/notation.md`
- `models/model_v<current>.md`
- `conjectures/conjectures.md`
- `proofs/`
- `experiments/*/summary.md`
- `literature/map.md`
- `project.yaml`

## Steps

1. **Load context.** `agents/verifier/AGENT.md` + skills.
2. **Overclaim check.** For every numbered theorem in `paper.md`, find
   its claim ID and check `project.yaml`:
   - `proved` → must have a `proofs/<claim-id>.md` with status `proved`
     and zero gaps.
   - `partial-proof` → paper must say "partial proof", not "we prove".
   - `simulation-supported` → paper must say "we observe empirically",
     not "we prove".
3. **Notation consistency.** Every symbol used in `paper.md` must have
   an entry in `paper/notation.md`. Symbols defined in
   `paper/notation.md` but never used → P2.
4. **Citation consistency.** Every paper cited in `paper.md` must have
   a BibTeX entry in `literature/bibliography.bib` and a corresponding
   note in `literature/papers/`.
5. **Proof rigor.** For each `proofs/<claim-id>.md`, run
   `adversarial-proof-review` on the latest attempt. Any new gaps → P1
   or P0 depending on whether the claim is currently `proved`.
6. **Reproducibility.** For each `experiments/<exp-id>/summary.md`,
   verify the existence of `code/run.py`, `config.yaml`, and
   `results/`. Verify the figures it references exist under
   `figures/`. Missing files → P1.
7. **Novelty drift.** Re-read the latest `literature/map.md` novelty
   assessment. If a new cluster has appeared since the project's
   current framing was set, flag a "novelty drift" P1.
8. **Venue fit.** Re-read `project.yaml`'s `target_venue`,
   `backup_venues`, and `publication_bar`. Flag when the current artifact
   bundle no longer matches the target venue's expected contribution shape.
   Use P0 if the manuscript overclaims venue-level readiness; otherwise P1.
9. **Append findings** to `reviews/suggestions.md` using the format in
   `agents/verifier/AGENT.md`.
10. **Log** to `logs/agent-history.md`. Set `Approval needed: yes` if
   any P0 findings exist or if the recommended next action is `retarget
   venue`.

## Quality contract

- A new dated section in `reviews/suggestions.md`.
- Every finding has a file pointer.
- Venue fit is assessed when `target_venue` exists.
- P0 findings surfaced to the user.
