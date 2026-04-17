---
name: inbox-parser
description: Extract, classify, and integrate PDF papers from the project inbox/ folder into the literature pipeline. Use this skill whenever PDFs are present in inbox/, when a user says "I dropped some papers in the inbox", or before any literature or ideation pass that should incorporate locally stored papers.
---

# Skill — Inbox Parser

Papers the user drops into `projects/active/$SLUG/inbox/` are raw material
waiting to be absorbed. This skill extracts their content, classifies them
against the active literature map, and files them so every downstream agent
(Literature, Ideation, Proof) can use them without re-reading the originals.

The goal is to turn a pile of PDFs into first-class project artifacts in one
pass — so nothing the user manually gathered gets silently ignored.

## Step 1 — Discover and triage

1. List all files in `projects/active/$SLUG/inbox/`.
2. Separate PDFs from non-PDFs. Warn about non-PDFs but do not process them.
3. For each PDF, check `literature/papers/` to see if a note already exists
   for it (match by filename stem or DOI if extractable). Skip already-filed
   papers and note them as "already processed".

## Step 2 — Extract each new PDF

Use the `pdf` skill to extract text and tables from each unprocessed PDF.
Pull out, at minimum:

- **Title** (from the document header or metadata)
- **Authors** (all of them)
- **Year** and **Venue/Source** (conference, journal, arXiv ID, or "preprint")
- **Abstract** (verbatim or close paraphrase)
- **Key contribution** — one sentence
- **Techniques used** — bullet list
- **Limitations** — bullet list

If any of these fields cannot be reliably extracted, mark them `[unverified]`.
Do not guess author names or years.

## Step 3 — Write per-paper notes

For each extracted paper, write a file at
`projects/active/$SLUG/literature/papers/<short-id>.md`.

`<short-id>` format: `<firstauthor-lastname>-<year>-<first-content-word>`,
e.g. `kempe-2003-influence`.

```markdown
# <Title>

- **Authors:** ...
- **Year:** ...
- **Venue:** ...
- **Source:** inbox/<filename>.pdf
- **arXiv / DOI:** ... (or `—`)

## Key contribution
One sentence.

## Relation to project
How this paper connects to `notes/overview.md`. Be specific about which
open question it bears on.

## Techniques used
- ...

## Limitations / open questions
- ...

## BibTeX
\`\`\`bibtex
@...{...}
\`\`\`
```

If the BibTeX cannot be generated reliably from the extracted text, leave
a `[unverified]` placeholder and flag it in the run summary.

## Step 4 — Append to bibliography

Append the BibTeX entry to
`projects/active/$SLUG/literature/bibliography.bib`.

Do not create duplicate entries. If an entry for the same DOI or arXiv ID
already exists, skip it and note the skip.

## Step 5 — Cross-reference with literature map

Open the most recent section of `projects/active/$SLUG/literature/map.md`.
For each processed paper, determine:

- Which existing cluster does it belong to? (if any)
- Does it strengthen, weaken, or clarify the novelty assessment?
- Does it introduce a technique the project could borrow?

Append a short "Inbox absorption note" block at the bottom of the most
recent `literature/map.md` section:

```markdown
### Inbox absorption — <ISO date>

Papers absorbed from `inbox/`:
- `<short-id>` — [Author et al., Year] — <one sentence on relevance and cluster fit>
- ...

Novelty impact: <none / strengthens gap / weakens gap / requires reassessment>
```

## Step 6 — Move processed files

After successful processing, move each PDF from `inbox/` to
`inbox/processed/`. This prevents double-processing on future runs.
Do not delete originals.

## Step 7 — Run summary

Append a run summary to `logs/agent-history.md` using the `run-summary`
skill. Under "Evidence added", list each paper by its `<short-id>` and
whether it was newly filed or already present.

## Edge cases

| Situation | Action |
|-----------|--------|
| PDF is password-protected | Note it in the run summary, skip, leave in `inbox/` |
| PDF is a scan with no selectable text | Note it as `[ocr-needed]`, skip, leave in `inbox/` |
| PDF is clearly not a research paper | Note it, skip, move to `inbox/non-papers/` |
| `inbox/` does not exist | Create it and report "inbox created; nothing to process" |
