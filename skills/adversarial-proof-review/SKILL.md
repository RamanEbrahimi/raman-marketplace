---
name: adversarial-proof-review
description: Adversarial review of mathematical proofs, theorems, and formal arguments. Use when asked to review a proof, check a paper's correctness, verify a theorem, find bugs in mathematical reasoning, audit a derivation, or stress-test a formal argument. Implements iterative self-correction from Woodruff et al. (2026) and the Generator-Verifier-Reviser pattern from Aletheia (Feng et al., 2026).
---

# Adversarial Proof Review

Rigorous, multi-pass adversarial review of mathematical proofs and formal arguments. Designed to catch subtle flaws that single-pass review misses - the technique that identified a fatal bug in a SNARG construction from LWE (Woodruff et al., 2026, Section 3.2).

## When to use

- Raman asks you to "review," "check," "verify," or "audit" a proof, theorem, or derivation
- A new paper is ingested and contains novel proofs relevant to Raman's work
- Raman presents his own proof sketch and wants it stress-tested
- Any time a formal mathematical claim needs verification before being filed to the wiki

## The protocol

### Pass 1: Initial objective review

Focus exclusively on identifying potential errors. For each claim in the proof:

1. **State the claim** being made
2. **Identify the proof technique** (induction, contradiction, construction, etc.)
3. **Check each logical step**: Does the conclusion follow from the premises?
4. **Flag gaps**: Are there unstated assumptions? Missing cases? Unjustified steps?
5. **Check definitions**: Is every term used consistently with how it was defined?
6. **Verify boundary conditions**: Do edge cases (empty set, n=0, degenerate graphs) work?

Output format for Pass 1:
```
## Pass 1: Initial Review

### Claim 1: [statement]
- Technique: [proof technique]
- Status: VERIFIED / SUSPICIOUS / GAP FOUND
- Issues: [if any]

### Claim 2: ...
```

### Pass 2: Self-correction (adversarial)

Now critique your own Pass 1 findings:

1. **For each "VERIFIED" claim**: Try harder to break it. Ask: "What if the hypothesis is false? What if I construct a specific counterexample?"
2. **For each "SUSPICIOUS" claim**: Is this a real issue or a false alarm? Can you construct a concrete instance where it fails?
3. **For each "GAP FOUND"**: Is the gap genuine or did you misread the argument? Verify by re-reading the original text.
4. **Check for hallucinations in your own review**: Did you attribute claims to the proof that aren't there? Did you misstate definitions?

### Pass 3: Distinction check

This is the critical step from the SNARGs case study. For every definition and construction pair:

1. **Extract the exact definition** (what is required)
2. **Extract what the construction achieves** (what is delivered)
3. **Compare precisely**: Does "achieved" imply "required"? Watch for:
   - "For all" vs "with high probability" gaps (perfect vs statistical)
   - "For all x" vs "for almost all x" gaps
   - Deterministic vs randomized guarantees
   - Worst-case vs average-case claims

### Pass 4: Final assessment

Choose one output format:

**COMPLETE VERIFICATION** (use only if every step is 100% rigorous):
- Summary of the proof's correctness
- Any non-obvious but valid steps worth noting

**STRUCTURED PARTIAL PROGRESS** (use if ANY gaps or concerns remain):
- **Verified lemmas**: What you can confirm is correct
- **The crux**: Pinpoint exactly where the proof stalls or has issues
- **Severity**: Is this a fatal flaw, a fixable gap, or a presentation issue?
- **Suggested fixes**: Propose strategies to bridge the gap

## Epistemic discipline

- **Distinguish**: "This step is wrong" vs "I cannot verify this step" vs "This step requires expertise I may lack"
- **Never say "the proof is correct" by default**. Absence of found errors is not proof of correctness.
- **Use neutral prompting internally**: Frame as "prove or refute" rather than "verify this is true"
- **Flag confirmation bias**: If the proof supports a result you expect to be true, be extra skeptical
- **Cite specific lines/equations**: Every critique must point to the exact location in the source

## Common failure modes to watch for

From Woodruff et al. (2026), Section 9.2:
- **Confident technical hallucinations**: Subtle algebraic errors, dropped constraints, misapplied theorems
- **Confirmation bias**: Strong tendency to support the hypothesis presented in the prompt
- **Alignment friction**: Reluctance to critique a result that looks impressive

## Integration with wiki

After completing a review:
1. If a significant flaw is found, create `wiki/reviews/<paper-slug>-review.md` documenting the issue
2. Update the corresponding source page with a "Known issues" section
3. If the proof is verified, note this in the source page's "Key claims" section with epistemic status
4. Log the review to `log.md` with op type `review`
