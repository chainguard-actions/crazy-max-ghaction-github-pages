<!-- markdownlint-disable -->

# Hardening Report: crazy-max--ghaction-github-pages/v5.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **crazy-max--ghaction-github-pages/v5.0.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Rule (a): A ${{ }} expression is directly interpolated inside a run: shell block. In the 'Gen dummy page' step, `${{ github.sha }}` is embedded directly in a heredoc shell command string. Any ${{ ... }} expression inside a run: block is a script-injection risk because the value is substituted by the Actions template engine before the shell ever sees it, bypassing shell quoting. Fix: use the GITHUB_SHA environment variable instead (e.g., `$GITHUB_SHA`), which is already available as a safe env var.

Locations:

- `.github/workflows/ci.yml:83`

### unpinned-uses (severity: high)

Multiple workflow files reference external actions using mutable tag refs instead of pinned 40-character commit SHAs. This exposes the workflow to supply-chain attacks if the referenced tag is moved or the upstream repository is compromised. Affected references:
- .github/workflows/ci.yml: `actions/checkout@v6`, `crazy-max/ghaction-github-status@v4`
- .github/workflows/cleanup.yml: `actions/github-script@v8`
- .github/workflows/labels.yml: `actions/checkout@v6`, `crazy-max/ghaction-github-labeler@v5`
- .github/workflows/validate.yml: `actions/checkout@v6`, `docker/bake-action/subaction/list-targets@v6`, `docker/bake-action@v6`
All should be pinned to full 40-character SHA digests (e.g., `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`).

Locations:

- `.github/workflows/ci.yml:37`
- `.github/workflows/ci.yml:87`
- `.github/workflows/cleanup.yml:24`
- `.github/workflows/labels.yml:28`
- `.github/workflows/labels.yml:31`
- `.github/workflows/validate.yml:27`
- `.github/workflows/validate.yml:30`
- `.github/workflows/validate.yml:47`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, unpinned-uses

**Notes:**

Fixed script-injection in ci.yml by replacing `${{ github.sha }}` with `$GITHUB_SHA` in the heredoc shell block (GITHUB_SHA is a safe built-in env var). Pinned all 8 unpinned action references across 4 workflow files to full 40-character commit SHAs: actions/checkout@v6→d23441a, crazy-max/ghaction-github-status@v4→fa6ac37, actions/github-script@v8→ed59741, crazy-max/ghaction-github-labeler@v5→24d110a, docker/bake-action@v6→5be5f02 (used for both docker/bake-action and docker/bake-action/subaction/list-targets). Original tag names preserved as inline comments.

