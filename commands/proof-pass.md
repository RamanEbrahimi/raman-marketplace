---
description: Run a proof pass on open conjectures
argument-hint: "[claim ids, comma-separated; default: all open]"
---

# /agentic-research:proof-pass

Dispatch the Proof agent to attack open conjectures.

## What to do

Follow `agents/playbooks/proof-pass.md`. In short:

1. Read `agents/proof/AGENT.md` and skills.
2. Targets: `$ARGUMENTS` if non-empty (comma-separated claim IDs);
   otherwise every claim with status `hypothesis`, `proof-sketch`, or
   `partial-proof`.
3. For each target claim:
   - Open or create `proofs/<claim-id>.md`.
   - Append a new dated `## Proof attempt` block.
   - Use `proof-writer` to draft the argument.
   - Use `formula-derivation` for non-trivial algebra.
   - Use `conjecture-testing` for at least one small-case verification.
   - Use `adversarial-proof-review` to stress-test the argument.
   - Update the file's `## Status` per the promotion rules in
     `agents/proof/AGENT.md`. **Never call a sketch "proved".**
4. Patch only the status of the targeted claims in
   `conjectures/conjectures.md`. Do not rewrite their statements.
5. In the run summary, list every status change as
   `C<n>: <old> -> <new>`.
6. Make a venue-aware publication-bar judgment against the active
   `target_venue`: `meets bar`, `close-needs-proof`,
   `close-needs-model`, `close-needs-literature`,
   `strong-but-wrong-venue`, or `not-publishable-yet`. Recommend the next
   route explicitly.
7. Log to `logs/agent-history.md`. Mark `Approval needed: yes` if any
   claim was promoted to `proved` or the recommended route is `retarget
   venue`.

## Forbidden

- Do not edit `models/`, `paper/`, `experiments/`, `literature/`.
- Do not promote a claim to `proved` without zero gaps + adversarial
  review pass + small-case check.
- Do not edit `project.yaml`'s `claims` directly — surface the change in
  the run summary so the Supervisor can patch it after user approval.
