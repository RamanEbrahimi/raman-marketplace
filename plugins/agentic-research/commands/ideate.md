---
description: Compound pipeline — literature → ideation → verifier → report — for a topic
argument-hint: "<topic>"
---

# /agentic-research:ideate

Run the full ideation compound playbook on a topic. Produces a real
research-ideation report inside `projects/active/<slug>/reports/`.

## Inputs

- `$ARGUMENTS` — the topic. Required.
- The active project under `projects/active/`.

## What to do

Follow `agents/playbooks/ideate.md` end-to-end. The playbook chains:

1. **Literature pass** — `agents/playbooks/literature-review.md` on
   `$ARGUMENTS`. If empty, old, or short, run `/literature-review` agent to get the most recent related papers.
2. **Ideation pass** — Generate 5–8 candidates per
   `agents/ideation/AGENT.md`. Use `cross-pollination-ideation` for
   analogies. Use `conjecture-testing` to sanity-check the most concrete
   claim each candidate implies. Write
   `notes/gap_briefs/<slug>.md` for the 3–5 surviving candidates. Assign
   each survivor a target venue, backup venues, and a minimum publishable
   bar.
3. **Verifier pass** — `agents/verifier/AGENT.md`. Cross-check each gap
   brief against `literature/map.md`. P0 findings demote candidates to
   `notes/gap_briefs/_dead/`. If needed, re-run `/literature-review`. 
4. **Report** — Write `projects/active/<slug>/reports/ideate-<topic>-<date>.md`
   using the structure in `agents/playbooks/ideate.md` step 4. **Do not
   touch `paper/paper.md`.**
5. **Log** — One compound entry in `logs/agent-history.md`.
6. **Surface** — Tell the user: report path, top candidate, target venue,
   backup venues, "promote? (yes/no)".

## Quality bar (compound)

- New literature map section.
- ≥3 gap briefs (live or dead).
- New `reviews/suggestions.md` section.
- New report file under `reports/`.
- `paper/paper.md` untouched.
- Compound entry in `logs/agent-history.md`.

## Failure modes

- Step 1 fails its quality contract → stop, ask user to refine topic.
- Step 2 yields zero surviving candidates → write a "no viable angles"
  report and stop.
- Step 2 yields candidates but none with a plausible target venue → write
  a "weak venue fit" report and stop.
- Step 3 demotes everything → write a "dead-gap report" and recommend a
  pivot.
