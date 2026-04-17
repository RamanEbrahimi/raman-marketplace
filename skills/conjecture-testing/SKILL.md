---
name: conjecture-testing
description: Systematically test mathematical conjectures by searching for counterexamples, verifying small cases, and constructing proofs or refutations. Use when Raman proposes a conjecture, when a paper claims an unproven bound, when testing whether a theorem generalizes, or when asked to "find a counterexample," "test this conjecture," "verify this bound," or "check if this holds." Implements the simulation and counterexample search techniques from Woodruff et al. (2026) and the neuro-symbolic verification loop from Aletheia (Feng et al., 2026).
---

# Conjecture Testing

Systematic framework for testing mathematical conjectures - searching for counterexamples, verifying small cases computationally, and attempting proofs or refutations. Based on techniques from Woodruff et al. (2026, Sections 2.3, 3.1) and Aletheia's Generator-Verifier-Reviser loop (Feng et al., 2026).

## When to use

- Raman proposes a conjecture or asks "does X hold?"
- A paper claims an unproven inequality or bound
- Testing whether a known result extends to a new setting (e.g., from complete graphs to general networks, from risk-neutral to prospect-theoretic agents)
- Any "I wonder if..." moment in research ideation

## The protocol

### Phase 1: Formalize the conjecture

Before testing anything, make the conjecture precise:

1. **State the conjecture formally** with all quantifiers explicit
2. **Identify the domain**: What objects is it about? (graphs, games, functions, distributions?)
3. **List all assumptions**: What constraints must the objects satisfy?
4. **State the conclusion**: What property is claimed to hold?
5. **Identify the type**: Universal ("for all X, P(X)") vs existential ("there exists X such that P(X)")

### Phase 2: Minimal counterexample search

Start with the smallest possible instances (this is how the Submodular Welfare conjecture was refuted - n=3 items, m=2 agents):

1. **Identify the minimal non-trivial dimensions**: What is the smallest instance that isn't degenerate?
2. **Enumerate or construct candidates**:
   - For graph conjectures: Start with K_2, K_3, paths, cycles, stars
   - For game-theoretic conjectures: Start with 2-player, 2-strategy games
   - For network conjectures: Start with line graphs, star graphs, complete graphs
   - For optimization: Test against known tight examples
3. **Check each candidate** against the conjecture
4. **If a counterexample is found**: Verify it rigorously (check all conditions are satisfied, compute the exact values)

### Phase 3: Computational verification (neuro-symbolic loop)

When manual enumeration is insufficient, write code to test:

```python
# Template for conjecture testing
def test_conjecture(parameters):
    """
    Returns True if conjecture holds for these parameters,
    False with a counterexample otherwise.
    """
    # 1. Generate/enumerate instances
    # 2. For each instance, check if conjecture holds
    # 3. If violation found, return (False, counterexample)
    # 4. If all pass, return (True, None)
    pass
```

Guidelines for the neuro-symbolic loop:
- **Write the verification code first**, before proposing any proof
- **Start with small parameters** and increase
- **If the code finds a counterexample**: Verify it by hand before reporting
- **If the code confirms for small cases**: This is evidence, not proof. Report the range tested.
- **If the code hits errors**: Read the traceback, fix, and re-run (this is the self-correction loop)

### Phase 4: Boundary analysis

If the conjecture holds for small cases, probe its boundaries:

1. **Parameterize**: Can you find a family of instances where the gap between LHS and RHS shrinks? This suggests the bound may be tight.
2. **Relax assumptions**: What happens if you drop one condition? Does it break?
3. **Interpolate**: If the conjecture is known for extreme cases (p=0 and p=1), what happens at p=0.5? (This is the "Architecture of Illusion" pattern Raman uses.)

### Phase 5: Proof attempt or refutation

Based on the evidence gathered:

**If counterexample found:**
1. State the counterexample with full verification
2. Identify which specific step or intuition fails
3. Ask: Is there a weaker version that might still hold?
4. Propose the corrected conjecture

**If conjecture appears to hold:**
1. Attempt a proof, decomposing into lemmas
2. For each lemma, flag whether it's proven or assumed
3. Identify the "crux" - the hardest step
4. If stuck, report as STRUCTURED PARTIAL PROGRESS (from the adversarial-proof-review skill)

## Output format

```markdown
## Conjecture: [formal statement]

### Evidence
- **Small cases tested**: [range and results]
- **Computational verification**: [code results if applicable]
- **Boundary behavior**: [what happens at extremes]

### Verdict: REFUTED / SUPPORTED / INCONCLUSIVE

### [If refuted] Counterexample
- **Instance**: [precise specification]
- **Verification**: [showing all conditions hold and conclusion fails]
- **Why it fails**: [intuition for what goes wrong]
- **Weaker version**: [proposed fix if any]

### [If supported] Proof sketch
- **Strategy**: [high-level approach]
- **Proven lemmas**: [what's established]
- **Open gaps**: [what remains]
```

## Integration with wiki

- File results to `wiki/questions/<conjecture-slug>.md` with status `open` / `partially-answered` / `closed`
- If the conjecture relates to an existing idea, update the idea page
- If a counterexample is found, create a concept page for the technique used
- Log to `log.md` with op type `conjecture`
