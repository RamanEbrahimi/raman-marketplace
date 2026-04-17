---
name: claim-lifecycle
description: Read, update, and promote claim statuses through the controlled vocabulary. Includes the project.yaml patch protocol and the conjectures.md update procedure.
---

# Skill — Claim Lifecycle

Claims move through a controlled status chain. This skill governs all reads
and writes to `conjectures/conjectures.md` and the `claims` block in
`project.yaml`. No agent should touch claim statuses without following this
protocol.

## The status chain

```
question  →  hypothesis  →  proof-sketch  →  partial-proof  →  proved
                                                             →  refuted
```

- **Forward promotion** requires positive evidence (see requirements below).
- **Demotion** (e.g. `proved → partial-proof`) is allowed without user
  approval when new evidence invalidates a prior promotion.
- **Lateral moves** (`hypothesis → refuted`, `proof-sketch → refuted`) are
  allowed with an explicit counterexample.
- `question` is the starting status for something not yet formalised.
- `simulation-supported` is a separate annotation that can co-exist with
  any status (it records empirical corroboration, not proof strength).

## Promotion requirements

| Target status | Required evidence |
|---------------|------------------|
| `hypothesis` | A gap brief exists at `notes/gap_briefs/<slug>.md` |
| `proof-sketch` | A dated attempt in `proofs/<claim-id>.md` with a clear strategy and at least one justified step |
| `partial-proof` | Proof attempt has a complete argument with explicitly labelled gaps (`**G1.**`, …) |
| `proved` | (1) Zero gaps in the proof file, (2) an `adversarial-proof-review` pass recorded, (3) at least one small-case verification, (4) **user approval** surfaced and confirmed in chat |
| `refuted` | An explicit, checkable counterexample written into the proof file |

`proved` always requires **user approval** — the Proof agent reports the
promotion request in the run summary and the Supervisor surfaces it. The
Supervisor patches `project.yaml` only after the user confirms in chat.

## How to update `conjectures/conjectures.md`

The file uses YAML-frontmatter blocks, one per claim:

```markdown
## C<n>: <one-line statement>

- **Status:** <status>
- **Added:** <ISO date>
- **Last updated:** <ISO date>
- **Linked proof:** `proofs/C<n>.md` (or `—` if none)
- **Linked gap brief:** `notes/gap_briefs/<slug>.md` (or `—`)
- **Notes:** <free text — why promoted, what changed>
```

**Update rules:**
1. Patch only the `Status`, `Last updated`, and `Notes` fields.
2. Do not rewrite the statement (`## C<n>: …` line).
3. Do not delete any claim entry; if refuted, set `Status: refuted`.
4. Append new claims at the bottom; do not re-number existing ones.

## How to patch `project.yaml`

The `claims` block looks like:

```yaml
claims:
  C1:
    status: proved
    proof_file: proofs/C1.md
  C2:
    status: hypothesis
    proof_file: ~
```

**Only the Supervisor patches `project.yaml`.** Other agents report
their desired status change in the run summary. The Supervisor then
applies the patch after validation (and user approval for `proved`).

When patching:
- Update `status` and `proof_file`.
- Never remove a claim entry.
- Update `current_model_version` only with explicit user approval.
- Never change `mode` (discovery ↔ project) without explicit user
  approval.

## Claim count helper

Before dispatching any proof or writer pass, the Supervisor should report:

```
Claims summary
──────────────
proved         : N
partial-proof  : N
proof-sketch   : N
hypothesis     : N
question       : N
refuted        : N
─────────────────
open (attackable): N
```

"Open" means status ∈ {`question`, `hypothesis`, `proof-sketch`,
`partial-proof`}.
