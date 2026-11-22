# Changelog

## [Unreleased]

### Added
- Organization defaults for every 519lab repository without its own copy: a Contributor Covenant 3.0 code of conduct (reports go to conduct@519lab.com), a contributing guide with the branch, commit, changelog and pull request conventions, a support page, a pull request template, and bug report, feature request and question forms. Blank issues are turned off and point to the security policy.

### Changed
- This repository is now a standalone repository for the organization's default health files, no longer a fork of `ggrace519/.github`. Its copies of the shared workflows, their tests and the workflow ADRs are removed. Callers use `ggrace519/.github`.
- `SECURITY.md` now says private vulnerability reporting works from any repository in the organization.
