# ggrace519/.github

Shared GitHub Actions workflows for repositories that use the `develop` → `main` branch model.

## promote

`.github/workflows/promote.yml` is a reusable workflow. It opens a single `develop` → `main` promotion pull request and rewrites that pull request's summary whenever `develop` moves again before the merge. It closes the pull request once `develop` has nothing that `main` lacks.

Add this caller to a repository as `.github/workflows/promote.yml`. Set `workflows:` to the `name:` of that repository's CI workflow:

```yaml
name: promote
on:
  workflow_run:
    workflows: [CI]
    types: [completed]
    branches: [develop]
  workflow_dispatch:
permissions:
  contents: read
  pull-requests: write
concurrency:
  group: promote-develop-to-main
  cancel-in-progress: true
jobs:
  promote:
    if: >-
      github.event_name == 'workflow_dispatch' ||
      (github.event.workflow_run.conclusion == 'success' &&
       github.event.workflow_run.event == 'push')
    uses: ggrace519/.github/.github/workflows/promote.yml@v1
    with:
      ref: ${{ github.event.workflow_run.head_sha }}
      # merge-note: "What merging to main deploys, if anything."
```

Requirements:
- The default branch must be `develop`. `workflow_run` only fires for workflows on the default branch.
- Turn on Settings → Actions → "Allow GitHub Actions to create and approve pull requests":
  `gh api -X PUT repos/OWNER/REPO/actions/permissions/workflow -f default_workflow_permissions=read -F can_approve_pull_request_reviews=true`
- A pull request opened with `GITHUB_TOKEN` does not trigger `pull_request` workflows. A required status check on `main` that only runs on `pull_request` will therefore never report for this pull request.

Merge promotion pull requests with a merge commit, never a squash or rebase.

## Versioning

Callers pin `@v1`, which is a moving major tag. Breaking changes ship as `v2`.
