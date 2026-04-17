# Principle — Evidence Language

**Scope:** Writer agent. Supervisor pre-commit check. All agents that
produce prose about results.

## The controlled vocabulary

The strength of language in `paper/paper.md` must match the strength of
the underlying evidence — no stronger, no weaker. This table governs every
result statement:

| Phrase | When allowed | Requires |
|--------|-------------|----------|
| "We prove / establish / show that …" | `status: proved` only | Gap-free proof file + `project.yaml` status confirmed |
| "We give a partial proof of …" | `status: partial-proof` | Proof file with explicit gaps labelled |
| "We sketch a proof that …" | `status: proof-sketch` | Proof file with strategy and at least one step |
| "Empirically, we observe …" | `status: simulation-supported` | Linked experiment summary and figure |
| "We conjecture …" | `status: hypothesis` or `question` | Entry in `conjectures/conjectures.md` |
| "Intuitively …" / "We interpret this as …" | Any status | Must be tagged `<!-- interpretation -->` inline |

## Why the distinction matters

Referees and readers calibrate their trust based on language. Writing "we
prove" for a result that has only been simulation-supported is not a small
overstatement — it is a claim about the nature of knowledge that is simply
false. It also creates downstream liability: the paper cannot be corrected
without a retraction-level edit.

The vocabulary also serves the project internally. When a claim is labelled
`hypothesis` in both the manuscript and `project.yaml`, the Proof agent
knows it is an open target. When it is labelled `proved`, the Writer knows
the language can be upgraded. A consistent vocabulary makes the entire
pipeline readable at a glance.

## Hard boundaries

These are the two lines that cannot be crossed for any reason:

1. **"We prove" without a gap-free proof file.** A proof sketch is not a
   proof. A simulation is not a proof. An argument that "clearly follows"
   is not a proof. If you cannot point to `proofs/<claim-id>.md` with
   `Status: proved`, the word "prove" cannot appear.

2. **Simulations promoting a claim to `proved`.** A simulation can raise
   a claim from `hypothesis` to `simulation-supported`. It cannot raise it
   to `partial-proof`, `proof-sketch`, or `proved`. These statuses require
   formal argument, not empirical evidence.

## Calibration — erring on the side of honesty

If you are uncertain which status applies, choose the weaker one and flag
it in the run summary. A `proof-sketch` that is later upgraded is a sign
of honest progress. A `proved` that is later demoted is a sign of a
mistake that may have already propagated into the manuscript.

The goal is not to sound confident. The goal is to be accurate about what
is known.
