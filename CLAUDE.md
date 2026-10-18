# ggrace519/.github

Shared reusable GitHub Actions workflows. This repo is public so that private repos can call them.

- Branch model: `develop` is the default and integration branch. `main` is promoted through a PR, and releases are tagged on main. Callers pin the moving major tag `v1`, so moving `v1` changes the behavior of every caller. Treat it as a release.
- Lint: `actionlint` (CI: `.github/workflows/lint.yml`).
- `promote.yml` came from homelab-swarm's inline script. Keep the logic in sync here only. homelab-swarm now calls this workflow.
