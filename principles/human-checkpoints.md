# Principle — Human Checkpoints

**Scope:** All agents. All playbooks.

## The rule

Stop and surface a checkpoint to the user — do not auto-advance — in any
of the following situations:

### 1. A claim is being promoted to `proved`

The Proof agent reports the promotion in the run summary. The Supervisor
surfaces it as:

> "Proof agent reports that **C<n>** is fully proved. Shall I update
> `project.yaml` to `status: proved` and proceed? (yes/no)"

Wait for an explicit "yes" before patching `project.yaml`.

### 2. A model version bump is proposed

The Modeling agent writes `model_v<N+1>.md` and then asks:

> "New model version written at `models/model_v<N+1>.md`. Key changes:
> [diff summary]. Shall I update `current_model_version` in
> `project.yaml` from v<N> to v<N+1>? (yes/no)"

Wait for confirmation before the Supervisor patches `project.yaml`.

### 3. A mode flip is triggered

Switching between `discovery` and `project` mode changes which commands
are available. Surface this as:

> "This action would switch the project from `discovery` mode to `project`
> mode (or vice versa). This affects which agents can run. Confirm?
> (yes/no)"

### 4. A Verifier P0 finding blocks the paper

When the Verifier files a P0:

> "Verifier has filed a P0 finding: [finding summary, pointer to file].
> This blocks the current draft update. The paper cannot proceed until
> this is resolved. Recommended action: [suggestion]. Shall I route to
> [agent] to address it?"

### 5. A compound playbook reaches a designated checkpoint

The playbooks `ideate.md` and `full-pipeline.md` contain explicit
checkpoints (e.g., "promote candidate?" at the end of ideate, and
multiple checkpoints in full-pipeline). Honor every one of them, even
if the prior steps ran cleanly.

## Why this matters

The compound playbooks are autonomous by default, which is efficient.
But efficiency breaks down when a decision has lasting consequences:
a wrong model version becomes the foundation for proofs and experiments;
a premature `proved` status poisons the manuscript. The checkpoints are
placed exactly where mistakes are expensive and human judgment adds
real value.

Skipping a checkpoint to "keep the pipeline moving" is the wrong call
even when the agent is confident. Confidence is not the same as correctness.

## What a well-formed checkpoint looks like

A checkpoint message should give the user:

1. **What happened** — which agent finished, what it produced.
2. **What the decision is** — what will change if the user says yes.
3. **A specific recommendation** — what you think the right call is and why.
4. **A clear yes/no prompt** — so the user does not have to guess the format.

Example:

> "Literature pass complete. 4 clusters found; the novelty gap for
> 'adaptive seeding under partial observability' appears open (no direct
> prior). Top-ranked ideation candidate: `notes/gap_briefs/adaptive-seeding.md`.
> Promote this candidate to a Modeling pass? (yes / no / show me the gap brief first)"
