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
# Only push-triggered and manual runs share the group. A develop merge also re-runs lint on the
# open promotion PR; that run's promote job is skipped by `if`, but concurrency applies before
# `if`, so in a shared group it would cancel the push run that does the work (#17).
concurrency:
  group: >-
    ${{ (github.event_name == 'workflow_dispatch' || github.event.workflow_run.event == 'push')
        && 'promote-develop-to-main' || format('promote-skip-{0}', github.run_id) }}
  cancel-in-progress: true
jobs:
  promote:
    if: >-
      github.event_name == 'workflow_dispatch' ||
      (github.event.workflow_run.conclusion == 'success' &&
       github.event.workflow_run.event == 'push')
    uses: ggrace519/.github/.github/workflows/promote.yml@56da419a93d4eca7fe1771297dc734fbd9c39fb4 # v1.0.2
    with:
      ref: ${{ github.event.workflow_run.head_sha }}
      # merge-note: "What merging to main deploys, if anything."
      # base: master   # promotion target; defaults to main
      # runs-on: '["self-hosted","linux"]'   # label or JSON array; defaults to ubuntu-latest
```

Requirements:
- The default branch must be `develop`. `workflow_run` only fires for workflows on the default branch.
- Turn on Settings → Actions → "Allow GitHub Actions to create and approve pull requests":
  `gh api -X PUT repos/OWNER/REPO/actions/permissions/workflow -f default_workflow_permissions=read -F can_approve_pull_request_reviews=true`
- CI does run on the promotion pull request. Because this workflow starts from a `workflow_run` chain, GitHub credits the `pull_request` runs to the user whose push began the chain, not to `GITHUB_TOKEN`. tetris-c #14 showed this, with `triggering_actor` set to the pushing user. Required status checks on `main` therefore report as usual.

Merge promotion pull requests with a merge commit, never a squash or rebase.

## release

`.github/workflows/release.yml` is a reusable workflow for repositories that keep a [Keep a Changelog](https://keepachangelog.com) `CHANGELOG.md` and semantic versions. On every push to `main` it reads the top `## [X.Y.Z] - YYYY-MM-DD` section. When that version has no tag yet, it creates the tag `vX.Y.Z` on the pushed commit and a GitHub release whose notes are that section. With `dry-run` it only validates, so the same workflow gates the promotion pull request: a promotion whose changelog is not ready fails its check before the merge.

It enforces, and fails with an `::error::` that names the fix:
- `[Unreleased]` has no entries left; they were rolled into a dated version section.
- The top section is a semantic version with a date and at least one entry.
- That version is newer than the latest existing tag.
- If its tag already exists it points at this commit (a re-run, which is a no-op). Otherwise `main` moved without a new version.

Add this caller to a repository as `.github/workflows/release.yml`:

```yaml
name: release
on:
  push:
    branches: [main]
  pull_request:
    branches: [main]
permissions:
  contents: write
concurrency:
  group: release-${{ github.ref }}
  cancel-in-progress: false
jobs:
  release:
    uses: ggrace519/.github/.github/workflows/release.yml@<sha> # vX.Y.Z
    with:
      dry-run: ${{ github.event_name == 'pull_request' }}
      # major-tag: true      # also move v1 (for 1.x.y) to the release; see below
      # changelog: CHANGELOG.md
      # tag-prefix: v
      # runs-on: '["self-hosted","linux"]'   # label or JSON array; defaults to ubuntu-latest
    # secrets:
    #   token: ${{ secrets.RELEASE_TOKEN }}  # only to start tag-triggered workflows or bypass a tag ruleset
```

Inputs: `ref` (commit to release, default the triggering commit), `changelog`, `tag-prefix`, `major-tag`, `dry-run`, `runs-on`. Outputs: `version`, `tag`, `released` (`'true'` when this run created the release) and `url`.

Notes:
- A `1.2.0-rc.1` section makes a pre-release and does not move the major tag.
- Tags and releases created with `GITHUB_TOKEN` start no other workflow. A build that runs `on: push: tags` will not fire. Either chain the build in the same workflow (`needs: release`, `if: needs.release.outputs.released == 'true'`, tag in `needs.release.outputs.tag`) or pass a `token` secret (a fine-grained PAT with contents: write).
- `major-tag` moves an existing `vX` tag. A tag ruleset that blocks updates, like this repository's `protect-release-tags`, refuses that for `GITHUB_TOKEN`, and the run fails after the release was created. A re-run repairs the tag once the ruleset lets the workflow through. On a user-owned repository the `github-actions` app cannot be a bypass actor (the API answers "Actor GitHub Actions integration must be part of the ruleset source or owner organization"), so exclude the major tag from the ruleset instead. Release tags `vX.Y.Z` stay protected:
  ```sh
  id=$(gh api repos/OWNER/REPO/rulesets --jq '.[] | select(.name == "protect-release-tags") | .id')
  gh api -X PUT "repos/OWNER/REPO/rulesets/$id" --input - <<< '{"conditions": {"ref_name": {
    "include": ["refs/tags/v*"], "exclude": ["refs/tags/v[0-9]", "refs/tags/v[0-9][0-9]"]}}}'
  ```
  The alternative is a `token` secret holding a fine-grained PAT (contents: write) of a repository admin, who bypasses the ruleset.
- Adopting it in a repository whose `main` is already tagged at the changelog's top version is a no-op until the next version lands.

## Versioning

Releases are tagged `vX.Y.Z` on main by `self-release.yml`, which runs the `release` workflow above on every push to `main` and validates the changelog on the promotion pull request. `v1` is moved to the latest 1.x release by the same run; the tag ruleset excludes the major tag so that `GITHUB_TOKEN` may move it. Breaking changes ship as `v2`. A tag ruleset (`protect-release-tags`) blocks moving or deleting `v*` tags for everyone except repo admins.

Pin callers to a release's commit SHA, with the version in a comment, not to `@v1`. The workflow runs with write access to the caller's repo, often on self-hosted runners, so a moved tag would change that code without review. Bump the SHA in each caller on purpose when a new release ships.
