---
name: run-summary
description: Append a well-formed run summary to logs/agent-history.md after every agent pass. Shared by all agents.
---

# Skill — Run Summary

Every agent pass must end by appending a structured entry to
`projects/active/$SLUG/logs/agent-history.md`. Use this skill to produce
a correctly formatted entry. Do not skip this step — the Supervisor reads
the log to determine next actions.

## Schema

```markdown
## <agent>: <task>  (<ISO timestamp>)

### What changed
- <Bullet per artifact touched, e.g. "Appended 3-cluster section to `literature/map.md`">

### Evidence added
- <Bullet per citation, proof step, or data point added. For literature: paper IDs. For proofs: claim IDs and their new status. For experiments: exp-id and key metric.>

### Uncertainties
- <Anything you could not verify, marked [unverified], or left as an open question.>

### Next actions
- <Concrete recommended next step, e.g. "Run ideation pass on cluster 2" or "Attack C1 with induction">

Approval needed: yes|no
```

## Field guidance

**`<agent>`** — Use the canonical role name, lowercase:
`supervisor`, `literature`, `ideation`, `modeling`, `proof`,
`simulation`, `writer`, `verifier`.

**`<task>`** — Brief imperative phrase matching the command that triggered
the run, e.g. `literature review — influence maximization`,
`proof pass — C1, C3`, `update draft — section 4`.

**`<ISO timestamp>`** — `YYYY-MM-DDTHH:MM` (no seconds needed). Use the
current date from the environment.

**What changed** — One bullet per file written or appended. Use backtick
paths relative to the project root. Do not list files you only read.

**Evidence added** — Concrete and linkable. For literature: `[Author et al., Year]`.
For proofs: `C1: hypothesis → proof-sketch`. For experiments: `exp-003:
fig-02 generated`. Empty bullets are not acceptable.

**Uncertainties** — Items you marked `[unverified]` in the artifacts, cases
where the quality contract was only partially met, or open questions the
next agent should know about.

**Next actions** — At least one concrete suggestion. Not "continue as
needed" — name the specific command and target.

If venue targeting is active, the first bullet under `Next actions`
should name:
- the active `target_venue`
- the publication-bar judgment (`meets bar`, `close-needs-model`, etc.)
- the recommended route (`literature-review`, `refine-model`,
  `proof-pass`, `experiment-pass`, or `retarget venue`)

**Approval needed** — `yes` if the run changed a claim to `proved`,
proposed a model-version bump, touched `mode` in `project.yaml`, or
involved any human-in-the-loop action. Otherwise `no`.

## Compound runs

For compound playbooks (ideate, full-pipeline), append **one** entry that
lists all sub-agents under "What changed":

```markdown
## ideate: influence maximization  (2026-04-11T14:30)

### What changed
- [literature] Appended 4-cluster section to `literature/map.md`.
- [ideation]  Appended 5 candidates to `notes/idea_funnel.md`. Wrote 4 gap briefs.
- [verifier]  Appended verifier pass to `reviews/suggestions.md`. Demoted 1 candidate.
- [writer]    Wrote `reports/ideate-influence-maximization-2026-04-11.md`.
...
```

## Append, never overwrite

`logs/agent-history.md` is append-only. If the file does not exist yet,
create it with a single top-level heading:

```markdown
# Agent history — <$SLUG>
```

…then append the first run entry below it.
