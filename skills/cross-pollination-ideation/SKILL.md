---
name: cross-pollination-ideation
description: Generate research ideas by finding cross-disciplinary connections, retrieving obscure theorems from distant fields, and identifying analogies between different mathematical domains. Use when Raman asks for new research directions, when ingesting a paper that might connect to other fields, when looking for "bridges" between topics, or when asked to "find connections," "what other fields use this technique," or "cross-pollinate." Implements the cross-pollination techniques from Woodruff et al. (2026, Section 2.2, 4.x) and AI Behavioral Science's framework for studying AI-human strategic interactions (Jackson et al., 2025).
---

# Cross-Pollination Ideation

Systematic generation of research ideas by finding connections across disciplinary boundaries. Based on the cross-pollination case studies from Woodruff et al. (2026) - where linking Steiner trees to the Kirszbraun Extension Theorem and reframing combinatorics as measure theory led to resolving open problems.

## When to use

- Raman asks "what ideas emerge from these papers?" or "give me new research directions"
- A new paper is ingested that uses techniques from a field adjacent to Raman's
- Raman is stuck on a problem and needs a fresh angle
- Exploring whether a technique from one of Raman's topics applies elsewhere
- Enhancing OP3 (Generate new research ideas) from CLAUDE.md

## The method

### Step 1: Map the technique space

For the source concept or problem, identify:

1. **The core mathematical structure**: What is the essential abstraction? (e.g., "this is really about finding a fixed point," "this is a convex optimization under constraints," "this is about information asymmetry in a network")
2. **The proof/solution techniques used**: What tools does the paper deploy? (spectral methods, measure theory, LP duality, potential functions, coupling arguments, etc.)
3. **The modeling assumptions**: What simplifications does the paper make? Which could be relaxed?

### Step 2: Search for analogies

For each core structure identified, search for parallels in Raman's research areas:

| Source domain | Target (Raman's domains) | Bridge question |
|---|---|---|
| Technique T in field A | Does T apply to network games? | "What if we use T to analyze equilibrium structure on networks?" |
| Model assumption X | Prospect theory relaxes X | "What happens to the result when agents have biased perceptions?" |
| Optimization method M | Strategic classification uses similar structure | "Can M give tighter bounds on strategic manipulation?" |
| Game-theoretic result R | Information design extends R | "What if the principal can design the information environment?" |

Key bridges for Raman's program:
- **Spectral graph theory <-> Network games**: Eigenvalues of the adjacency/Laplacian encode equilibrium structure (Ballester-Calvo-Armengol-Zenou pattern)
- **Prospect theory <-> Any game-theoretic model**: Replace expected utility with CPT - what changes? (Raman's "Architecture of Illusion" pattern)
- **Information design <-> Strategic classification**: The principal chooses what information to reveal; the agents best-respond strategically
- **Mechanism design <-> Network economics**: Design network interventions as mechanisms
- **Non-normal matrices <-> Network dynamics**: Pseudospectra capture transient behavior that normal analysis misses (Raman's non-normal network games idea)

### Step 3: The parameterization test

From Raman's own methodology (the "Architecture of Illusion" pattern):

1. **Find the extreme cases**: Is paper X the alpha=0 case? Is paper Y the alpha=1 case?
2. **Propose the interpolation**: What happens for alpha in (0,1)?
3. **Check if the interpolation is non-trivial**: Does the behavior change qualitatively at some critical alpha*? If so, that's a research contribution.

Examples from Raman's work:
- Risk-neutral (alpha=0) vs fully loss-averse (alpha=1) agents in network games
- Full information (alpha=0) vs no information (alpha=1) in Bayesian persuasion on networks
- Myopic (k=0) vs fully strategic (k=inf) in k-hop awareness models

### Step 4: The "obscure theorem" retrieval

This is where AI has a genuine comparative advantage (Woodruff et al., 2026). For the stuck problem:

1. **Restate the problem abstractly**: Strip away the domain-specific language. What is the pure mathematical structure?
2. **Search your knowledge for theorems** about that abstract structure from any field:
   - Functional analysis (Stone-Weierstrass, Riesz Representation, Hahn-Banach)
   - Topology (Brouwer, Kakutani, Borsuk-Ulam)
   - Measure theory (Radon-Nikodym, disintegration, optimal transport)
   - Combinatorics (Ramsey, Turan, Lovasz Local Lemma)
   - Information theory (data processing inequality, Fano's inequality)
3. **Check if the theorem's conditions** are satisfied in Raman's setting
4. **If they are**: Sketch how the theorem resolves the roadblock

### Step 5: The Jackson et al. lens (AI Behavioral Science)

For ideas at the intersection of AI and behavioral/social science:

1. **AI as strategic agent**: How do AI agents behave in game-theoretic settings? Can we apply behavioral game theory tools (cognitive hierarchy, level-k) to predict AI behavior?
2. **AI as tool for behavioral science**: Can LLMs simulate participants in network games? Can they predict experimental outcomes in strategic classification settings?
3. **Human-AI interaction on networks**: How does introducing AI agents into a network game change equilibrium behavior? What are the welfare implications?
4. **Information design for AI**: How should a principal design information structures when some agents are AI and some are human (heterogeneous rationality)?

### Step 6: Quality filter

Before proposing an idea, run these checks:

1. **Novelty check**: Search the wiki for existing ideas that cover this ground. Search arXiv mentally for whether this has been done.
2. **Feasibility check**: Can Raman actually prove something here, or is this just hand-waving? What would the first lemma be?
3. **Significance check**: Does this connect to an existing open question? Does it have implications beyond a technical curiosity?
4. **The "why not" test**: If this connection is so natural, why hasn't someone done it? (Possible answers: (a) they have and you don't know, (b) there's a technical barrier, (c) the communities don't talk to each other - only (c) is a good sign)

## Output format

For each proposed idea:

```markdown
### Idea: [one-sentence pitch]

**The bridge**: [Source field] -> [Raman's domain]
**The gap**: [What's missing in the literature]
**The key insight**: [Why this connection works]
**First step**: [What the first lemma or result would be]
**Papers it builds on**: [[sources/...]]
**Risk level**: Low (incremental) / Medium (novel combination) / High (speculative)
```

Propose 3-5 ideas, ranked by feasibility * significance.

## Integration with wiki

- Offer to file promising ideas as `wiki/ideas/<slug>.md` with status `seed`
- Update relevant topic MOCs with new cross-references
- If the session produces substantial output, file to `outputs/research-proposals/`
- Log to `log.md` with op type `idea`
