# Principle — Append, Never Overwrite

**Scope:** All agents writing to append-only artifacts.

## The rule

The following files are append-only. Once a section is written, it is
never deleted, edited, or restructured:

| File | Why it is append-only |
|------|-----------------------|
| `literature/map.md` | Each pass is a dated record of what was known at that time. Rewrites destroy provenance. |
| `notes/idea_funnel.md` | A failed or discarded idea is still evidence of the search space explored. |
| `logs/agent-history.md` | The log is the audit trail. Editing it is equivalent to falsifying a lab notebook. |
| `reviews/suggestions.md` | Verifier findings must be addressable by their original location. |
| `proofs/<claim-id>.md` | Each proof attempt is a dated record; new attempts are new sections, not replacements. |

Model files (`models/model_vN.md`) are **versioned**, not append-only:
bump to `model_v<N+1>.md` and leave `model_v<N>.md` untouched.

## Why this matters

Overwriting a prior section erases the decision trail. The user — and future
agents — need to be able to read *why* the project evolved the way it did,
not just where it ended up. An append-only history also makes mistakes
recoverable: if an agent appends something incorrect, the prior state is
still there.

## How to apply

**Before writing to any of the above files:**
1. Open the file and scroll to the end.
2. Confirm you are appending (not replacing) an existing section.
3. Add a date-stamped header for the new section (e.g.,
   `## Literature pass — influence maximization — 2026-04-11`).

**If you think an old section needs correction:**
- Do not edit it. Instead, append a new section that supersedes it and
  notes what changed and why (e.g.,
  `## Correction — literature pass 2026-04-10: the novelty gap for [X] is weaker than stated`).

**Model versions:**
- When the Modeling agent proposes a new model, write `model_v<N+1>.md`
  as a new file. Never overwrite `model_v<N>.md`.
- Update `current_model_version` in `project.yaml` only after the user
  approves the bump (see the `claim-lifecycle` skill and the
  `human-checkpoints` principle).

**If a file does not exist yet:**
- Create it with a minimal header and then append the first section.
  That is not an overwrite.
