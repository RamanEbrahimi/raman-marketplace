# Agentic Research Plugin

`agentic-research` is filesystem-first plugin for agent-assisted research.
It packages specialist agents, command playbooks, principles, and skills for
theory-heavy workflows:

- literature review
- ideation
- formal modeling
- proof attempts
- experiments
- manuscript updates
- adversarial review

## Folder layout

```text
.codex-plugin/plugin.json   # Codex plugin manifest
.claude-plugin/plugin.json  # Claude/Cowork plugin manifest
agents/                     # role instructions and playbooks
commands/                   # slash command entrypoints
principles/                 # global behavior constraints
skills/                     # plugin-local skills
PRIVACY.md                  # public privacy note
TERMS.md                    # public terms note
```

## Current distribution model

Source of truth is this repo. Public distribution should happen through
GitHub, but host-specific install path differs:

1. push repo to GitHub
2. package `plugins/agentic-research/`
3. upload zip as GitHub Release asset
4. use GitHub repo itself for Claude/Cowork marketplace-based installs

## Install for local development

### Codex

Repo-local marketplace entry exists at:

- `.agents/plugins/marketplace.json`

It points to:

- `./plugins/agentic-research`

This is for local testing inside this repo.

### Claude / Cowork

Claude does not appear to use a generic "install from file/folder" flow.
On this machine, Claude stores plugin marketplaces under:

- `~/.claude/plugins/marketplaces/`

and installed plugin copies under:

- `~/.claude/plugins/cache/`

That means Claude/Cowork distribution should be treated as marketplace or
git-repo distribution, not zip-file import.

Practical implication:

1. publish repo on GitHub
2. make plugin discoverable to Claude as a marketplace/repo source
3. let Claude sync/copy plugin into its own plugin cache

### Release zip

GitHub release zip is still useful for:

- Codex-side distribution
- manual inspection
- backup packaging
- non-Claude hosts that support direct folder/file import

## Commands

| Command | Purpose |
| --- | --- |
| `/agentic-research:literature-review <topic>` | build literature map and novelty picture |
| `/agentic-research:ideate <topic>` | literature-to-idea pipeline |
| `/agentic-research:promote-idea` | promote candidate into formal project |
| `/agentic-research:refine-model` | revise model and conjectures |
| `/agentic-research:proof-pass [claim-ids]` | attack open claims |
| `/agentic-research:experiment-pass` | run experiment loop |
| `/agentic-research:update-draft` | sync manuscript with current evidence |
| `/agentic-research:review-workspace` | adversarial review of claims and framing |
| `/agentic-research:full-pipeline <topic>` | run compound workflow |

## Before first public release

- replace placeholder GitHub URLs in `.codex-plugin/plugin.json`
- add screenshots and icon if target host app benefits from visuals
- verify Claude marketplace/repo install flow
- create release notes

Step-by-step publish checklist:

- `PUBLISHING.md`
