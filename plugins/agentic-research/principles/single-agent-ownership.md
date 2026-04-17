# Principle — Single-Agent Artifact Ownership

**Scope:** All agents. Enforced by the Supervisor.

## The ownership table

Each agent owns a specific set of output artifacts. No agent writes to
another agent's outputs.

| Agent | Owns |
|-------|------|
| Supervisor | `project.yaml`, `logs/agent-history.md` |
| Literature | `literature/map.md`, `literature/papers/`, `literature/bibliography.bib` |
| Ideation | `notes/idea_funnel.md`, `notes/gap_briefs/` |
| Modeling | `models/model_vN.md`, `conjectures/conjectures.md` (append), `paper/notation.md` (append) |
| Proof | `proofs/`, `conjectures/conjectures.md` (status updates only) |
| Simulation | `experiments/`, `data/`, `figures/` |
| Writer | `paper/paper.md`, `paper/notation.md` (append), `paper/sections/`, `reports/` |
| Verifier | `reviews/suggestions.md` |
| Inbox Parser | `literature/papers/` (new notes), `literature/bibliography.bib` (append), `inbox/processed/` |

## Why this matters

Cross-writing creates silent contradictions. If the Proof agent edits the
model to make a proof work, the Modeling agent's version history is broken
and the user has no record that the model changed. If the Writer adds a
claim to `conjectures/conjectures.md` without going through Modeling, the
claim has no proof target and no gap brief.

Ownership boundaries make the system auditable: any given file was last
written by a known agent, following a known protocol.

## How to apply

**Before writing any file**, ask: "Does my agent role own this file?"

- If yes — proceed.
- If no — do not write it. Instead, include the needed change as a
  recommendation in the run summary under "Next actions", addressed to
  the agent that owns the file.

**Shared files** (`conjectures/conjectures.md`, `paper/notation.md`)
have restricted write modes:
- Modeling appends new claim entries and notation symbols.
- Proof updates only the `Status` and `Last updated` fields of existing
  claim entries.
- Writer appends notation symbols (never renames existing ones).

Neither Proof nor Writer may add new claim entries; that is Modeling's job.
Neither Modeling nor Proof may edit the manuscript.

**If a playbook seems to ask for cross-writing:**
Stop and escalate to the Supervisor with a note: "The playbook at step N
asks [agent] to write to [file], which is owned by [other agent]. This is
a dispatch error — please clarify."

This escalation is the right call. The playbook has a bug; the agent does
not have license to override the ownership table.
