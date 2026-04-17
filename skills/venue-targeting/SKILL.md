---
name: venue-targeting
description: Pick a target publication venue, define the venue-specific publication bar, and route loop-backs after proof, experiment, or verifier passes.
---

# Skill — Venue Targeting

Use this skill whenever the workflow needs to:

- choose a target venue for a candidate idea
- decide whether current results are publishable
- determine whether to loop back to Literature, Modeling, Proof, or
  Experiment
- retarget to a different venue family without losing the core idea

The key rule: **never judge "publishable enough" in the abstract.**
Always judge against a named target venue or venue family.

## Required project state

When a candidate is still in ideation, store venue choice in
`notes/gap_briefs/<slug>.md`.

When a candidate is promoted, copy the active venue state into
`project.yaml`:

```yaml
target_venue: ICLR
backup_venues:
  - NeurIPS
  - AAAI
venue_family: ml-top
publication_bar:
  headline_result: "clear ML-relevant conceptual or algorithmic result"
  novelty: high
  theory_depth: medium
  empirical_depth: high
  writing_style: "ML-first framing with strong related-work positioning"
loop_history: []
```

These fields are optional until a candidate is promoted, but once the
project is in `mode: project`, they should be treated as part of the
canonical state.

## What to write in each gap brief

Every surviving gap brief should include:

- `Target venue`
- `Backup venues`
- `Venue-fit rationale`
- `Minimum publishable bar`
- `Loop triggers`

Do not leave these vague. Name the missing ingredient that would cause a
loop.

## Venue families

Use the most specific named venue that fits, but reason through the
family-level bar.

### `ml-top`

Named venues:
- ICLR
- NeurIPS
- ICML
- AAAI
- COLM

Typical bar:
- Clear ML problem relevance.
- Strong novelty relative to recent ML literature.
- Either strong empirical evidence, or a concept/theorem with obvious ML
  significance.
- Framing must explain why the result matters for learning, inference,
  optimization, generalization, agents, or deployment.

Fatal weaknesses:
- Nice theorem with weak ML relevance.
- Pure toy model with no credible learning implication.
- Empirical section too thin for claims made.

### `algorithms-theory`

Named venues:
- FOCS
- STOC
- SODA
- ITCS

Typical bar:
- Theorem-first contribution.
- Real novelty in technique, model, or bound.
- Tight proofs and clean statements.
- Simulations are optional and usually secondary.

Fatal weaknesses:
- Main theorem is incremental.
- Proof is mostly adaptation without conceptual payoff.
- Framing relies on experiments instead of theorem depth.

### `econ-theory`

Named venues:
- Econometrica
- QJE
- AER
- Theoretical Economics

Typical bar:
- Economic significance and model insight must both be strong.
- Comparative statics, equilibrium characterization, mechanism insight,
  or welfare implications should matter economically.
- For top general-interest venues like AER/QJE, pure theory usually needs
  unusually broad importance.

Fatal weaknesses:
- Elegant math with weak economic interpretation.
- No clear welfare / equilibrium / identification / policy stake.
- Contribution fits specialist theory outlet better than general-interest
  economics.

### `market-design-game-theory`

Named venues:
- ACM EC
- WINE
- GEB
- SAGT
- Games and Economic Behavior

Typical bar:
- Incentives, equilibria, welfare, strategic interaction, or algorithmic
  market design must be central.
- Theory can dominate, but mechanism intuition must be crisp.
- Empirical support can help, but does not replace theorem value.

Fatal weaknesses:
- Nice combinatorics with no strategic content.
- Mechanism claims without equilibrium clarity.
- Results better framed as pure CS theory or pure economics.

### `networks-social-computation`

Named venues:
- WWW
- KDD
- AAMAS
- Network Science
- Management Science

Typical bar:
- Clear network or multi-agent problem.
- Good fit between theory, data, and mechanism/behavioral story.
- Strong baselines or realistic structural assumptions often matter.

Fatal weaknesses:
- Network framing is superficial.
- Literature positioning ignores adjacent social/network science work.
- No credible evaluation plan where venue expects one.

### `ai-agents-multiagent`

Named venues:
- AAMAS
- ICAPS
- UAI
- IJCAI

Typical bar:
- Multi-agent reasoning, planning, uncertainty, or coordination should be
  central.
- Theoretical contributions should connect to agent behavior or decision
  making, not float as detached math.

Fatal weaknesses:
- Results read like generic optimization with agents renamed in.
- No connection to agentic benchmarks, planning settings, or strategic
  environments where relevant.

## Venue selection procedure

When ranking a candidate, score:

1. `problem_fit` — does the venue care about this question?
2. `result_fit` — theorem / mechanism / empirical / lower-bound shape.
3. `novelty_fit` — how high the incremental-novelty tolerance is.
4. `evidence_fit` — how much proof, experiment, or literature coverage is
   required.
5. `writing_fit` — whether the story can be told in that venue's language.

Choose:
- one `Target venue`
- one or two `Backup venues`

Do not choose a prestige target that the contribution shape obviously does
not support.

## Publication-bar outcomes

Every proof pass, experiment pass, or verifier pass should end with one
of these judgments:

- `meets bar`
- `close-needs-proof`
- `close-needs-experiment`
- `close-needs-model`
- `close-needs-literature`
- `strong-but-wrong-venue`
- `not-publishable-yet`

These are routing judgments, not just prose labels.

## Routing rubric

### Route to `proof-pass` again when

- the model is good
- literature framing is stable
- main gap is proof completeness
- the venue bar is blocked mainly by missing lemmas, gaps, or tightness

### Route to `experiment-pass` again when

- the venue expects stronger empirical validation
- theorem side is adequate but evidence breadth is weak
- baselines, ablations, stress tests, or figures are missing

### Route to `refine-model` when

- proof attempts expose the wrong primitive, assumption, payoff, or
  statement
- current theorem set lacks a headline result for the target venue
- the venue demands a cleaner or more general formulation than current
  model supports

### Route to `literature-review` when

- novelty risk rises
- a nearby paper weakens the gap
- framing needs a different subcommunity or result lineage
- stronger baselines, comparisons, or borrowable techniques are needed

### Route to `retarget venue` when

- the results are real and coherent
- the target venue bar is too high or mismatched
- another named venue fits the exact contribution better

Retargeting is not failure. It is often correct research strategy.

## Loop triggers by failure type

Map failure type to route:

| Failure type | Route |
| --- | --- |
| Theorem technically incomplete but on-track | `proof-pass` |
| Theorem true only under changed assumptions | `refine-model` |
| Main statement not novel enough | `literature-review` |
| Results solid but too narrow for target venue | `retarget venue` |
| ML venue but no convincing ML relevance | `literature-review` or `retarget venue` |
| Theory venue but contribution depends on weak experiments | `refine-model` |
| Econ venue but no economic stakes | `refine-model` or `retarget venue` |
| Market-design venue but weak incentive content | `refine-model` |

## Output expectations for agents

When this skill is used, the run summary should make four things explicit:

1. active target venue
2. publication-bar judgment
3. main reason for the judgment
4. recommended next route

Example:

```markdown
### Next actions
- Target venue: `ACM EC` — publication-bar judgment: `close-needs-model`.
- Reason: proofs suggest the current result is technically sound but not
  yet mechanism-first enough for EC.
- Recommended route: `refine-model` to strengthen incentive / welfare
  content before another proof pass.
```
