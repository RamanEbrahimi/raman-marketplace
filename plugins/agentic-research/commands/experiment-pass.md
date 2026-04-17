---
description: Run a simulation / experiment pass on the active model
argument-hint: "[claim id or sweep description]"
---

# /agentic-research:experiment-pass

Dispatch the Simulation agent to implement and run an experiment.

## What to do

Follow `agents/playbooks/experiment-pass.md`. In short:

1. Read `agents/simulation/AGENT.md`.
2. Pick a target: `$ARGUMENTS` if non-empty; otherwise the open claim
   that would benefit most from empirical evidence.
3. Allocate `exp-<NNN>` (next integer under `experiments/`).
4. Scaffold `experiments/<exp-id>/` with `code/run.py`, `config.yaml`
   (with explicit seed and parameters), and empty `results/`.
5. Implement, run, and capture output (`results/run.log`).
6. Generate figures into `figures/exp-<NNN>-<name>.{png,pdf}`.
7. Fill `experiments/<exp-id>/summary.md` per the format in
   `agents/simulation/AGENT.md`. Be loud about anomalies.
8. If an anomaly contradicts an existing claim, do **not** edit the
   claim. Add the anomaly to the run summary's "Uncertainties" and
   recommend a Modeling pass.
9. A claim may move `hypothesis` → `simulation-supported` if the
   experiment supports it. It may move to `refuted` only with a clean
   counterexample saved to `results/`.
10. Make a venue-aware publication-bar judgment against the active
    `target_venue`: `meets bar`, `close-needs-experiment`,
    `close-needs-model`, `close-needs-literature`,
    `strong-but-wrong-venue`, or `not-publishable-yet`. Recommend the next
    route explicitly.
11. Log to `logs/agent-history.md`. Mark `Approval needed: yes` if any
    claim was refuted or the recommended route is `retarget venue`.

## Reproducibility bar

- `cd experiments/<exp-id> && python code/run.py --config config.yaml`
  reproduces the same numbers.
- Seeds explicit. No `time.time()` randomness.
- Configs checked in. No hard-coded magic numbers in code.
- Figures derived from `results/`, not pasted from elsewhere.
