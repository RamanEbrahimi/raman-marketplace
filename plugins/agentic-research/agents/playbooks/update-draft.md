# Playbook — Update Draft

**Trigger phrases:** "update the draft", "sync the paper", "rewrite the
results section".

**Supervisor action:** `update draft`

**Agents:** Writer, then Verifier (auto-chained)

**Mode:** autonomous, with strict evidence validation.

## Inputs

- `paper/paper.md`, `paper/notation.md`
- `models/model_v<current>.md`
- `conjectures/conjectures.md`
- `proofs/`
- `experiments/*/summary.md`
- `literature/map.md`
- `project.yaml` (to know which claims are `proved`, plus the active
  `target_venue`, `backup_venues`, and `publication_bar`)

## Steps

### Writer

1. **Load context.** `agents/writer/AGENT.md` + skills.
2. **Diff the world.** Compare the artifacts above against the current
   `paper/paper.md`. Identify which sections need updating:
   - Introduction (if novelty positioning shifted)
   - Related Work (if `literature/map.md` has new clusters)
   - Model (if `current_model_version` changed)
   - Theoretical results (if any claim status changed)
   - Empirical results (if any new `experiments/<exp-id>/summary.md`)
   - Venue framing (if `target_venue` changed or current framing does not
     match it)
   - Discussion / Conclusion
3. **Apply the evidence contract** before drafting any prose:
   - Look up `claims[*].status == proved` in `project.yaml`.
   - Any "we prove / we establish / we show that" sentence must
     reference one of those claim IDs.
   - Look up `target_venue` and `publication_bar` in `project.yaml`. If
     the artifact bundle cannot honestly support that venue, stop and
     surface the mismatch instead of forcing the prose.
4. **Draft the updates.** Write each section in place, archiving the
   prior version under `paper/sections/_archive/<section>-<date>.md`.
5. **Self-check** by running the same regex check the Supervisor will
   run: search the new draft for `\bwe prove\b|\bwe establish\b|\bwe
   show that\b|\bis proved\b`. For each hit, verify a `proved` claim
   ID is referenced in the same paragraph. If not, rewrite the
   sentence using the right verb (`we conjecture`, `we sketch a
   proof`, `we observe empirically`, `we interpret`).
6. **Notation.** Append any new symbols to `paper/notation.md`.
7. **Hand off** to the Supervisor for the validation pass.

### Supervisor validation gate

- Run `manuscript_mentions_proved_claims_only(project_root,
  proved_claim_ids)`.
- If it returns False, **roll back** the draft (restore from the
  archived version), log the failure, and surface to the user.
- If it returns True, proceed.

### Verifier

1. Auto-chain a Verifier pass (see `agents/playbooks/review-workspace.md`)
   focused on the freshly updated sections, including venue fit.
2. P0 findings from this pass surface to the user.

### Log

Append a single compound run summary to `logs/agent-history.md`.

## Quality contract

- `paper/paper.md` updated.
- Old version archived under `paper/sections/_archive/`.
- Evidence check passed.
- Verifier pass appended to `reviews/suggestions.md`.
- Run summary in history.
