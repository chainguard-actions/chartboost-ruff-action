<!-- markdownlint-disable -->

# Hardening Report: ChartBoost--ruff-action/v1.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **ChartBoost--ruff-action/v1.0.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions using mutable tags or branch names instead of full 40-character commit SHAs. This exposes the workflow to supply-chain attacks if the referenced tag is moved or the action is compromised. Failing references:
- actionlint.yml: `actions/checkout@v3`, `reviewdog/action-actionlint@v1`
- python.yml: `actions/checkout@v3`, `psf/black@stable`, `chartboost/ruff-action@v0`, `jakebailey/pyright-action@v1`
- stale.yml: `actions/stale@v7`

Locations:

- `.github/workflows/actionlint.yml:13`
- `.github/workflows/actionlint.yml:14`
- `.github/workflows/python.yml:7`
- `.github/workflows/python.yml:8`
- `.github/workflows/python.yml:9`
- `.github/workflows/python.yml:10`
- `.github/workflows/stale.yml:14`

### missing-permissions (severity: medium)

Workflow files `actionlint.yml` and `python.yml` have no top-level `permissions:` key and no job-level `permissions:` key on any of their jobs. Without explicit permissions, the default token permissions (which may be broad) are granted to the workflow. Each workflow should declare minimal required permissions.

Locations:

- `.github/workflows/actionlint.yml:1`
- `.github/workflows/python.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 7 unpinned action references by resolving each tag to its full 40-character commit SHA using lookup_action_sha, preserving the original tag as an inline comment. Added top-level `permissions: {}` to actionlint.yml and python.yml which had no permissions declarations. stale.yml already had appropriate job-level permissions (contents: write, issues: read, pull-requests: write) and only needed its actions/stale reference pinned.

