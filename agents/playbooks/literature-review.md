# Playbook — Literature Review

**Trigger phrases (Cowork):** "run literature review for X", "literature
loop for X", "map the literature on X".

**Supervisor action:** `run literature loop`

**Agent:** Literature

**Mode:** autonomous

## Inputs

- Topic = `$TOPIC` (from the user, e.g. "influence maximization")
- Active project slug from `projects/active/` (must be exactly one)
- `notes/overview.md`
- Previous `literature/map.md`
- Any PDFs in `inbox/`
- Optional: `project.yaml` venue fields if a target venue already exists

## Steps

1. **Load context.** Read `agents/literature/AGENT.md` and every
   `agents/literature/skills/*/SKILL.md`. Read the project's
   `notes/overview.md` and the existing `literature/map.md`. List the
   PDFs in `inbox/`.
2. **Plan the search.** Write a one-paragraph search plan covering:
   - the precise question being mapped
   - which sub-areas to cover
   - which sub-areas to *exclude* and why
   - the time window (default: last 10 years, plus seminal older work)
3. **Run the searches.** Use the `arxiv-search`, `academic-researcher`,
   and `deep-research` skills to gather candidate papers. Parse PDFs in
   `inbox/` with the `pdf` skill.
4. **Cluster.** Group the papers into 3–7 clusters by approach /
   assumption family / result type. Each cluster needs ≥3 representative
   papers.
5. **Per-paper notes.** For every paper that matters (≥10 papers
   recommended), create
   `projects/active/<slug>/literature/papers/<short-id>.md` using the
   format in `agents/literature/AGENT.md`.
6. **Append to the map.** Append a new section to
   `projects/active/<slug>/literature/map.md` using the section format in
   `agents/literature/AGENT.md`. Include:
   - scope
   - clusters
   - novelty assessment
   - venue-sensitive framing risks (when a target venue or candidate venue
     family is known)
   - tools the project can borrow
   - impossibility / negative results
   - recommended next actions
7. **Update the bibliography.** Append BibTeX entries to
   `projects/active/<slug>/literature/bibliography.bib`. Every cited
   paper must have a key.
8. **Validate.** Self-check before logging:
   - Every cited paper has title + authors + year + venue/source.
   - No bare URLs.
   - At least 3 papers per cluster.
   - Novelty assessment is explicit.
   - No fabricated citations. Mark uncertain entries `[unverified]`.
9. **Log.** Append a run summary to
   `projects/active/<slug>/logs/agent-history.md` using the schema in
   `agents/supervisor/AGENT.md`. Use the `agentic-research log-run` CLI
   command if running locally; otherwise write the markdown directly.
10. **Update `project.yaml`.** Set `active_agents: ["literature"]`. Do
    not touch other fields.
11. **Hand off.** Tell the user: literature pass complete, point them at
    the new section in `literature/map.md`, and recommend the next
    action ("run ideation pass" if discovery mode, "refine model" if
    project mode), including whether the current framing looks plausible
    for the active target venue.

## Failure modes to surface

- Topic too broad → ask the user to narrow.
- Too few results → say so plainly; don't pad the map with weak entries.
- All search skills unavailable → fall back to a single skill and note the
  degradation in the run summary.

## Quality contract

This playbook is "done" when:

- `literature/map.md` has a new dated section.
- `literature/papers/` has at least 10 new files.
- `bibliography.bib` has new entries.
- `logs/agent-history.md` has a run summary.
- `project.yaml` lists `literature` as the active agent.
