# 519lab/.github

Organization default community health files for 519lab (see README.md). Content only: no code, no tests, no workflows.

- Branch model: `develop` is the default branch and the one GitHub reads the defaults from, so a change takes effect when it merges into `develop`. `main` is promoted from `develop` through a pull request.
- Not a fork, and never a copy of `ggrace519/.github`. Shared workflows live there only. Adding its workflows here would give callers a second set of release tags to pin by mistake.
- A file added here becomes the default for every 519lab repository, private ones included, that lacks its own copy. Check which repositories that is before adding one. Keep README.md's table in step with the files.
- Issue templates may only set labels that exist in this repository and in every target repository (`bug`, `enhancement` and `question` exist in all of them). Link to other files here with absolute `https://github.com/519lab/.github/blob/develop/...` URLs, because the defaults are shown in other repositories.
- Background: `~/.claude/research/github-org-default-health-files.md`.
- The repository must stay public, or GitHub stops applying the defaults.
