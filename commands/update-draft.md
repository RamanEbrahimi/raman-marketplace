---
description: Sync the manuscript with the latest project artifacts (Writer + Verifier)
argument-hint: "[section name; default: all stale sections]"
---

# /agentic-research:update-draft

Dispatch the Writer to update `paper/paper.md` from current artifacts,
then auto-chain a Verifier pass.

## What to do

Follow `agents/playbooks/update-draft.md`. In short:

### Writer phase

1. Read `agents/writer/AGENT.md`.
2. Diff the manuscript against the source artifacts: model file,
   conjectures, proofs, experiment summaries, literature map.
3. Identify stale sections. Restrict to `$ARGUMENTS` if specified.
4. **Apply the evidence contract before drafting any prose.** Look up
   `claims[*].status == proved` in `project.yaml`. Any sentence using
   `we prove` / `we establish` / `we show that` / `is proved` MUST
   reference one of those proved claim IDs in the same paragraph.
   Also load `target_venue` and `publication_bar` from `project.yaml`; if
   the artifact bundle cannot honestly support that venue, stop and surface
   the mismatch.
5. Draft the updates in place. Archive the prior version of each
   touched section under `paper/sections/_archive/<section>-<date>.md`.
6. Self-check by regex-searching for proof-claim language in the new
   draft and verifying coverage.
7. Append any new symbols to `paper/notation.md`.

### Validation gate

Run `manuscript_mentions_proved_claims_only(project_root,
proved_claim_ids)`. If False, **roll back** the draft, log the failure,
and surface to the user. If True, proceed.

### Verifier phase

Auto-chain `/agentic-research:review-workspace` focused on the freshly
updated sections, including venue fit. P0 findings surface to the user.

### Log

One compound entry in `logs/agent-history.md`.

## Forbidden

- Do not write `we prove` / `we establish` / `we show that` for an
  unproved claim.
- Do not delete prior draft sections; archive them.
- Do not edit `models/`, `proofs/`, `experiments/`, `literature/`, or
  `project.yaml`.
