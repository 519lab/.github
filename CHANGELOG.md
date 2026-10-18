# Changelog

## [Unreleased]

### Docs
- Corrected the README: CI does run on the promotion pull request. The earlier text wrongly said required checks on `main` would never report.

## [1.0.0] - 2026-10-17

### Added
- Reusable `promote` workflow. It opens and maintains one `develop` → `main` promotion pull request after CI passes on `develop`. It is extracted from homelab-swarm, and the deploy note is now an optional `merge-note` input.
- actionlint CI for the workflows in this repository.
