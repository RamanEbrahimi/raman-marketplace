---
name: evidence-contract
description: Classify each prose claim before writing to paper/paper.md. Enforce the controlled vocabulary and reject illegal proof language.
---

# Skill — Evidence Contract

The Writer agent and the Supervisor both invoke this skill before committing
any update to `paper/paper.md`. The contract prevents the manuscript from
outrunning the evidence.

## Step 1 — Extract candidate claims

Scan the draft section you are about to write. For every sentence that
asserts a mathematical result, highlight it and tag it as a candidate claim.
Examples of sentences to tag:

- "We prove that the greedy algorithm achieves a (1−1/e) approximation."
- "Empirically, we observe that the threshold is near 0.5."
- "We conjecture that no polynomial-time algorithm beats ratio r."
- "Intuitively, this arises because spreading saturates early."

## Step 2 — Classify each candidate claim

Use this table to assign a **claim type** to each tagged sentence:

| Claim type | Prose you are allowed to write | Evidence required |
|------------|-------------------------------|-------------------|
| `proved` | "We prove / establish / show that …" | A file at `proofs/<claim-id>.md` exists **and** `project.yaml` has `claims[<claim-id>].status: proved` |
| `partial-proof` | "We give a partial proof of …" | `proofs/<claim-id>.md` exists with `Status: partial-proof` |
| `proof-sketch` | "We sketch a proof that …" | `proofs/<claim-id>.md` exists with `Status: proof-sketch` |
| `simulation-supported` | "Empirically, we observe …" | `experiments/<exp-id>/summary.md` exists and is linked |
| `hypothesis` | "We conjecture …" | Entry in `conjectures/conjectures.md` |
| `writing-only-interpretation` | "Intuitively …" / "We interpret this as …" | None, but the sentence **must** carry the inline tag `<!-- interpretation -->` |

## Step 3 — Resolve each claim against the artifacts

For every `proved`-type claim:

1. Open `project.yaml`. Find `claims[<claim-id>].status`.
2. If status ≠ `proved` → **reject**. The sentence must be rewritten to
   match the actual status (e.g. downgrade to "we sketch a proof").
3. Open `proofs/<claim-id>.md`. Confirm it exists and has a non-empty
   `## Argument` section with zero open `**G1.**` gaps.
4. If the proof file has open gaps → **reject**. Status cap is
   `partial-proof` until the gaps are closed.

For every `simulation-supported` claim:

1. Confirm `experiments/<exp-id>/summary.md` exists.
2. Confirm the linked figure file exists under `figures/`.
3. If either is missing → rewrite as `hypothesis`.

## Step 4 — Inject evidence anchors

Every proved or simulation-supported claim in `paper.md` must carry an
inline anchor immediately after the sentence:

```
… achieves a (1−1/e) approximation. <!-- claim:C1 proved -->
… we observe a threshold near 0.5.  <!-- exp:exp-003 simulation-supported -->
```

## Step 5 — Supervisor pre-commit check

Before finalizing the draft update, output a short **pre-commit manifest**
for the Supervisor:

```
Evidence pre-commit check
─────────────────────────
proved      C1 ✓  C3 ✓
proof-sketch C2 ✓
simulation   exp-003 ✓
hypothesis   C4 ✓
interpretations: 2 (tagged)

Rejected claims: none
```

If any claim was rejected, list it and explain why. The Supervisor must see
this manifest before the draft is appended to `paper/paper.md`.

## Hard rules (cannot be overridden by any playbook)

- "We prove / establish / show that" is **illegal** unless the matching
  claim ID has `status: proved` in `project.yaml` and a gap-free proof
  file exists.
- A simulation result can **never** promote a claim to `proved`.
- `writing-only-interpretation` sentences must be tagged inline; do not
  present them as results.
- If you cannot find the claim ID for a sentence, surface it to the
  Supervisor rather than guessing.
