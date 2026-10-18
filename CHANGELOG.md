# Changelog

## [Unreleased]

### Added
- Reusable `promote` workflow. It opens and maintains one `develop` → `main` promotion pull request after CI passes on `develop`. It is extracted from homelab-swarm, and the deploy note is now an optional `merge-note` input.
- actionlint CI for the workflows in this repository.
