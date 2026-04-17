---
description: Run a literature review pass on a topic and write it into the active project
argument-hint: "<topic>"
---

# /agentic-research:literature-review

Dispatch the Literature agent against the active project and produce a
real, cited literature map for the topic the user provided.

## Inputs

- `$ARGUMENTS` — the topic (e.g. "influence maximization"). Required. If
  empty, ask the user for a topic before doing anything else.
- The active project under `projects/active/` (must be exactly one).

## What to do

1. Read the playbook at `agents/playbooks/literature-review.md` from the
   workspace root. Follow it step by step.
2. Read `agents/literature/AGENT.md` and every
   `agents/literature/skills/*/SKILL.md`.
3. Read the project's `notes/overview.md` and `literature/map.md`.
4. Use the `arxiv-search`, `academic-researcher`, and `deep-research`
   skills to gather candidate papers. Parse any PDFs in `inbox/` with
   the `pdf` skill.
5. Cluster the literature into 3–7 groups, each with ≥3 representative
   papers.
6. Write per-paper notes to
   `projects/active/<slug>/literature/papers/<short-id>.md`.
7. Append a new dated section to
   `projects/active/<slug>/literature/map.md` using the format in
   `agents/literature/AGENT.md`.
   If the project already has a target venue, include venue-sensitive
   framing risks.
8. Append BibTeX entries to
   `projects/active/<slug>/literature/bibliography.bib`.
9. Append a run summary to
   `projects/active/<slug>/logs/agent-history.md` using the schema in
   `agents/supervisor/AGENT.md`.
10. Update `projects/active/<slug>/project.yaml` so `active_agents`
    contains `"literature"`.
11. Tell the user: pass complete, new section in `literature/map.md`,
    next recommended action, and whether the current framing looks viable
    for the active target venue.

## Quality bar (do not skip)

- Every cited paper has title + authors + year + venue/source. No bare
  URLs.
- ≥3 papers per cluster.
- Explicit novelty assessment (settled / partial / open).
- Mark unverified citations `[unverified]` and surface in the run
  summary; never fabricate.

## Forbidden

- Do not write to `models/`, `proofs/`, `experiments/`, `paper/`.
- Do not edit `project.yaml` outside `active_agents`.
- Do not overwrite previous literature map sections.
