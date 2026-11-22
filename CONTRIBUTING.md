# Contributing to 519lab projects

This is the default guide for every 519lab repository without its own `CONTRIBUTING.md`. How to set up a project and run its checks is in that project's README.

## Before you start

- For anything bigger than a small fix, open an issue first so the approach is agreed before code is written.
- Never report a security problem in an issue. Report it privately, as [SECURITY.md](https://github.com/519lab/.github/blob/develop/SECURITY.md) describes.
- Everyone taking part follows the [Code of Conduct](https://github.com/519lab/.github/blob/develop/CODE_OF_CONDUCT.md).

## Branches

- `develop` is the default branch, and every pull request targets it. `main` receives only promotion pull requests from `develop`.
- Branch from an up-to-date `develop`. Name the branch `<type>/<short-summary>`, for example `fix/token-refresh`, `feat/csv-export` or `docs/setup`.
- `develop` must stay releasable. Nothing half-finished or known to be broken merges into it.

## Commits

- Use [Conventional Commits](https://www.conventionalcommits.org/) with a scope, for example `fix(api): handle an empty response`. Write the subject as a lowercase imperative with no trailing period. Add a body that explains what changed and why when that isn't obvious.
- Sign your commits with GPG or SSH.
- Never commit secrets. Document configuration in an `.env.example` instead.

## Changelog

Every change that someone outside the code could notice gets an entry under `## [Unreleased]` in `CHANGELOG.md`, in the right category (`Added`, `Changed`, `Fixed`, ...). Write it for that reader: what changed and why it matters, and for a fix, what was wrong. Start the entry with `BREAKING:` if the change is incompatible.

Don't add version headings yourself. Where a repository uses the release automation, a bot rolls `[Unreleased]` into the next version: a `BREAKING` entry is a major bump, `Added` a minor one, anything else a patch.

## Pull requests

- Keep each pull request to one coherent change, and add its tests with it, not afterwards.
- Fill in the template: a summary, the changes, the verification (the commands you ran and what they showed) and anything to do after the merge.
- Before the merge, every review comment, including a bot's, gets an answer or a fix.
- Pull requests merge with a merge commit, not a squash, and the branch is then deleted.
