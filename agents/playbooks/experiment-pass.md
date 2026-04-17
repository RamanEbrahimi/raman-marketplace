# Playbook — Experiment Pass

**Trigger phrases:** "run an experiment pass", "simulate the model", "run
a sweep on parameter X".

**Supervisor action:** `run experiment pass`

**Agent:** Simulation

**Mode:** autonomous (refused in `discovery` mode).

## Inputs

- `models/model_v<current>.md`
- `conjectures/conjectures.md`
- `experiments/` (existing experiments and their summaries)
- Optional: target claim ID or parameter sweep description (`$TARGET`)
- `project.yaml` venue fields (`target_venue`, `backup_venues`,
  `publication_bar`)

## Steps

1. **Load context.** `agents/simulation/AGENT.md` + skills.
2. **Pick a target.** If `$TARGET` is empty, choose the open claim with
   the highest `simulation-supported`/`hypothesis` ratio (i.e., the
   claim that would benefit most from empirical evidence). Otherwise use
   `$TARGET`.
3. **Allocate an experiment ID.** `exp-<NNN>` where `NNN` is the next
   integer under `experiments/`.
4. **Scaffold the experiment.**
   - `experiments/<exp-id>/code/run.py`
   - `experiments/<exp-id>/config.yaml` (seed, parameters, sweeps)
   - `experiments/<exp-id>/results/`
   - `experiments/<exp-id>/summary.md`
5. **Implement.** Write Python that:
   - reads `config.yaml`
   - runs the simulation deterministically
   - writes raw output to `results/`
   - generates figures into `figures/exp-<NNN>-<name>.{png,pdf}`
6. **Run.** Execute the experiment. Capture stdout/stderr to
   `results/run.log`.
7. **Summarize.** Fill in `summary.md` per the format in
   `agents/simulation/AGENT.md`. Be loud about anomalies.
8. **Surface anomalies.** If the simulation contradicts a claim, do
   *not* edit the claim. Add the anomaly to the run summary's
   "Uncertainties" section and recommend a Modeling pass.
9. **Update statuses (carefully).** A claim may move from `hypothesis`
   to `simulation-supported` if the experiment supports it. It may
   move to `refuted` only if the simulation produces a clean
   counterexample (which must be saved to `results/`).
10. **Assess venue readiness.** Using the active target venue, make one
    publication-bar judgment: `meets bar`, `close-needs-experiment`,
    `close-needs-model`, `close-needs-literature`,
    `strong-but-wrong-venue`, or `not-publishable-yet`. In the run
    summary, name the main reason and the recommended route.
11. **Log** to `logs/agent-history.md`. Mark `Approval needed: yes` if
    the experiment refuted a claim or if the recommended route is
    `retarget venue`.

## Quality contract

- Reproducible from `cd experiments/<exp-id> && python code/run.py
  --config config.yaml`.
- Seeds explicit; no `time.time()` randomness.
- Figures regenerated from `results/`.
- `summary.md` filled out with anomalies surfaced.
