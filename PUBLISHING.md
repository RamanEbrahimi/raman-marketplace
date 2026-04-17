# Publishing `agentic-research`

This file assumes you are publishing from current monorepo first, not
splitting plugin into standalone repo yet.

Important:

- Codex can use repo-local manifests and packaged artifacts.
- Claude/Cowork appears to use marketplace or git-based plugin syncing under
  `~/.claude/plugins/marketplaces/` and `~/.claude/plugins/cache/`.
- Do not rely on a generic "install from file/folder" UI for Claude.

## 1. Create git repo

From `/Users/macbook/Desktop/agentic-research`:

```bash
git init
git add .
git commit -m "Initial agentic-research plugin release prep"
```

## 2. Create GitHub repo

Create public repo, ideally:

- `agentic-research`

Then connect local repo:

```bash
git remote add origin git@github.com:<your-username>/agentic-research.git
git branch -M main
git push -u origin main
```

## 3. Replace placeholder URLs

Edit:

- `.codex-plugin/plugin.json`

Replace:

- `https://github.com/your-username/agentic-research`

with your real repo URL everywhere it appears.

## 4. Package plugin

From repo root:

```bash
chmod +x scripts/package-plugin.sh
./scripts/package-plugin.sh 0.1.0
```

Output:

- `releases/agentic-research-0.1.0.zip`

## 5. Create GitHub release

Suggested tag:

- `v0.1.0`

Attach:

- `releases/agentic-research-0.1.0.zip`

Release notes should include:

- what plugin does
- supported host apps
- install steps
- known limitations

## 6. Claude / Cowork distribution

From local Claude state on this machine, plugins are managed through
marketplace sources, not direct zip import. So for Claude users, GitHub repo
matters more than release zip.

Target flow for Claude users:

1. point Claude to marketplace or git source containing this plugin
2. let Claude sync plugin into `~/.claude/plugins/cache/`
3. enable/install plugin from that marketplace source

If you want smooth Claude adoption, best next improvement is turning this
repo into a proper Claude marketplace source instead of only shipping zip
artifacts.

## 7. Test download flow

Before announcing:

1. download release zip fresh from GitHub
2. unzip into separate temp folder
3. confirm manifest exists at `agentic-research/.codex-plugin/plugin.json`
4. confirm main docs render

## 8. Nice next upgrades

- add icon and screenshots under `assets/`
- add changelog
- add CI check that manifest JSON parses
- split plugin into dedicated repo if monorepo becomes noisy
