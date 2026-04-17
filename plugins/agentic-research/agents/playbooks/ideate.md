# Playbook — Ideate (compound)

**Trigger phrases (Cowork):** "come up with ideas for X", "ideate on X",
"find research questions in X", "give me a research report on X".

**Supervisor action:** `ideate on <topic>`

**Agents (in order):** Literature → Ideation → Verifier → Writer

**Mode:** compound autonomous, with one optional human checkpoint at the
end.

This is the "give me a real research report" playbook. It chains a
literature pass, an ideation pass, a verification pass, and a written
report.

## Inputs

- Topic = `$TOPIC` (e.g. "influence maximization")
- Active project slug from `projects/active/`
- `notes/overview.md`

## Steps

### Step 1 — Literature pass

Execute `agents/playbooks/literature-review.md` end-to-end on `$TOPIC`.
Do not proceed to step 2 if the literature pass failed its quality
contract; instead, surface the failure to the user.

### Step 2 — Ideation pass

1. Load `agents/ideation/AGENT.md` and every
   `agents/ideation/skills/*/SKILL.md`.
2. Read the freshly written `literature/map.md` section from step 1, plus
   `notes/overview.md` and any existing `notes/idea_funnel.md`.
3. Generate 5–8 candidate research questions per the format in
   `agents/ideation/AGENT.md`. Use the `cross-pollination-ideation` skill
   to surface analogies from adjacent fields.
4. For each surviving candidate (target: 3–5), write a
   `notes/gap_briefs/<slug>.md` file.
5. For each candidate, use the `conjecture-testing` skill to sanity-check
   the most concrete claim it implies. Report the outcome inside the gap
   brief.
6. Use the `venue-targeting` skill to assign each surviving candidate a
   `Target venue`, `Backup venues`, and an explicit `Minimum publishable
   bar`.
7. Append the ranked list to `notes/idea_funnel.md`.

### Step 3 — Verifier pass

1. Load `agents/verifier/AGENT.md`.
2. Cross-check each gap brief against the literature map:
   - Is the claimed gap actually open? (P0 if dead.)
   - Is the closest prior work cited correctly? (P1 if not.)
   - Is the proposed angle realistic given the techniques in the map's
     "tools to borrow" section? (P1 if mismatched.)
   - Is the claimed target venue plausible for the contribution shape?
     (P1 if mismatched, P0 if clearly impossible under current framing.)
3. Append the findings to `reviews/suggestions.md`.
4. For every P0 finding, demote the corresponding gap brief: rename it to
   `notes/gap_briefs/_dead/<slug>.md` and add a note explaining why.

### Step 4 — Writer pass: assemble the report

1. Load `agents/writer/AGENT.md`.
2. Write `projects/active/<slug>/reports/ideate-<topic>-<date>.md` (NOT
   `paper/paper.md` — this is a discovery report, not the manuscript).
3. The report has this structure:

```
# Research ideation report — <topic> — <date>

## TL;DR
3–6 bullets summarizing the most promising candidates.

## Literature landscape
Pull the clusters and novelty assessment from the latest
`literature/map.md` section. Be precise; cite paper IDs.

## Candidate research questions
For each surviving candidate (post-Verifier):
- The question
- Why it matters
- The gap it attacks (linked to literature cluster)
- Plausible angle
- Target venue and backup venues
- Estimated tractability and novelty risk
- Verifier notes
- Linked gap brief

## Discarded candidates
What the Verifier killed and why.

## Recommended next move
One concrete next action: "Promote candidate N → Modeling pass" or
"Run a second literature pass focused on subtopic Y" or "Drop the
topic; the gap is dead."

## Provenance
- Literature run: link to the `literature/map.md` section
- Ideation run: link to the `notes/idea_funnel.md` section
- Verifier run: link to the `reviews/suggestions.md` section
- Run summaries: `logs/agent-history.md`
```

4. Do **not** touch `paper/paper.md`. The Writer's evidence contract is
   strict; this report lives under `reports/` until the user promotes a
   candidate.

### Step 5 — Log and hand off

1. Append a single compound run summary to `logs/agent-history.md`. Title
   it `## ideate: <topic> (<date>)` and inside it list each sub-agent's
   contribution.
2. Update `project.yaml`: set `active_agents: ["literature","ideation","verifier","writer"]`.
3. Surface to the user:
   - "Ideation report ready: `reports/ideate-<topic>-<date>.md`."
   - "Top candidate: <slug> | target venue: <venue> | backups: <...> —
     promote? (yes/no)"
   - "If yes, I will run the Modeling agent next."

## Failure modes to surface

- Step 1 fails → stop, ask the user to refine the topic.
- Step 2 produces zero surviving candidates → stop, write a
  "no viable angles found" report and recommend a different topic.
- Step 2 produces candidates but none with a plausible target venue →
  stop, write a "weak venue fit" report and recommend a narrower angle or
  different venue family.
- Step 3 demotes everything → stop, write a "dead-gap report" and
  recommend a pivot.

## Quality contract

The compound run is "done" when:

- A new `literature/map.md` section exists.
- ≥3 gap briefs exist (live or `_dead/`).
- A `reviews/suggestions.md` section exists.
- A `reports/ideate-<topic>-<date>.md` file exists.
- A compound entry exists in `logs/agent-history.md`.
- `paper/paper.md` is **untouched**.
