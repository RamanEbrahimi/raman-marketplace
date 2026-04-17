# Playbook — Proof Pass

**Trigger phrases:** "run a proof pass", "attack the conjectures", "prove
C1 and C3".

**Supervisor action:** `run proof pass`

**Agent:** Proof

**Mode:** autonomous (refused in `discovery` mode).

## Inputs

- `models/model_v<current>.md`
- `conjectures/conjectures.md` (claims with status `hypothesis`,
  `proof-sketch`, or `partial-proof`)
- Existing `proofs/<claim-id>.md` files
- Optional: a list of claim IDs to attack (`$CLAIMS`); default is "all
  open claims"
- `project.yaml` venue fields (`target_venue`, `backup_venues`,
  `publication_bar`)

## Steps

1. **Load context.** `agents/proof/AGENT.md` + skills.
2. **Pick targets.** If `$CLAIMS` is empty, attack every open claim with
   status `hypothesis` or `proof-sketch`. If a claim has been attacked
   ≥3 times unsuccessfully, deprioritize it and surface to the user.
3. **For each target claim:**
   a. Open or create `proofs/<claim-id>.md`.
   b. Append a new `## Proof attempt — <date>` block.
   c. Use `proof-writer` to draft the argument step by step.
   d. Use `formula-derivation` for any non-trivial algebra.
   e. Use `conjecture-testing` to verify on at least one small case.
   f. Use `adversarial-proof-review` to stress-test the argument.
   g. Update the `## Status` line at the top of the proof file. Apply
      the promotion rules from `agents/proof/AGENT.md`. **Never call a
      sketch "proved".**
4. **Update `conjectures/conjectures.md`.** Patch only the status of the
   targeted claims. Do not rewrite the statements. 
5. **Report status changes.** In the run summary, list every claim whose
   status changed, in the form `C<n>: hypothesis -> partial-proof`.
6. **Assess venue readiness.** Using the active target venue, make one
   publication-bar judgment: `meets bar`, `close-needs-proof`,
   `close-needs-model`, `close-needs-literature`,
   `strong-but-wrong-venue`, or `not-publishable-yet`. In the run summary,
   name the main reason and the recommended route.
7. **Log** to `logs/agent-history.md`. Mark `Approval needed: yes` if any
   claim was promoted to `proved` (the Supervisor will then ask the user
   to confirm a `project.yaml` patch to `claims[*]`) or if the recommended
   route is `retarget venue`.

## Quality contract

- Each attacked claim has a new dated attempt block.
- Every gap is explicit.
- Every status promotion is justified.
- No `proved` without a complete, gap-free proof.
