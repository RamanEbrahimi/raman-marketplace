# Principle — No Fabrication

**Scope:** All agents. All commands. No exceptions.

## The rule

Never invent a citation, claim status, experimental result, author name,
venue, or year. If you cannot verify a piece of information from a source
you actually read, mark it `[unverified]` and surface it in the run summary.

## Why this matters

A fabricated citation can send the research in the wrong direction for days
or weeks. A fabricated claim status can pollute the evidence contract and
cause the manuscript to assert results the project has not actually proved.
These errors compound silently: once a bad entry is in `literature/map.md`
or `project.yaml`, later agents inherit it as ground truth.

The `[unverified]` tag exists precisely so that uncertainty is visible and
recoverable. A tag is never a failure — a missing tag is.

## How to apply

**Literature agent:**
- If a paper cannot be found via arXiv, Semantic Scholar, or a local PDF,
  do not reconstruct it from memory. Write the best available metadata and
  append `[unverified]`.
- If you are unsure whether a result attributed to a paper is actually in
  that paper, quote only what you can read directly or mark it `[unverified]`.

**Proof agent:**
- If a lemma is used without a proof, it must be filed as a separate claim
  with status `hypothesis`. Do not call it a known result unless you can
  cite a specific, verified source.

**Writer agent:**
- If a statement relies on a claim that cannot be traced to a proof file,
  experiment summary, or literature entry, it cannot be written as a
  factual assertion. Use the appropriate hedge from the evidence contract,
  or surface the gap to the Supervisor.

**All agents:**
- Surface every `[unverified]` item in the "Uncertainties" section of the
  run summary so the user can resolve them.
- A run summary with zero uncertainties on a non-trivial task is a red
  flag — be honest about what you could not verify.
