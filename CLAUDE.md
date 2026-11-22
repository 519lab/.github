# 519lab/.github

Organization default community health files for 519lab (see README.md). Content only: no code, no tests, no workflows.

- Branch model: `develop` is the default branch, and GitHub serves the defaults from it, so a change applies to every repository as soon as it merges into `develop` (verified 2026-11-22: other repositories showed the new files while `main` lacked them). `main` is promoted from `develop` through a pull request.
- Check what a repository inherits with `gh api repos/519lab/<repo>/community/profile` (code of conduct, contributing, PR template) and GraphQL `contactLinks` (the issue `config.yml`). GraphQL `issueTemplates` and `isBlankIssuesEnabled` ignore issue forms, so check forms in the web UI.
- Not a fork, and never a copy of `ggrace519/.github`. Shared workflows live there only. Adding its workflows here would give callers a second set of release tags to pin by mistake.
- A file added here becomes the default for every 519lab repository, private ones included, that lacks its own copy. Check which repositories that is before adding one. Keep README.md's table in step with the files.
- Issue templates may only set labels that exist in this repository and in every target repository (`bug`, `enhancement` and `question` exist in all of them). Link to other files here with absolute `https://github.com/519lab/.github/blob/develop/...` URLs, because the defaults are shown in other repositories.
- The repository must stay public, or GitHub stops applying the defaults.
