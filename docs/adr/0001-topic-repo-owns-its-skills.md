# Topic repo owns its skills

Status: accepted (2026-11-24)

## Context

The owner publishes self-authored skills through the `adonis-skills` collection, exported from a private source after an allowlist and privacy scan. This repo adds a second skill (`npm-version-guide`) next to `npm-publish-guide`, so it now looks like a small skill collection of its own.

## Decision

This repo keeps both skills. `adonis-skills` only links to it. The owner installs the skills from this repo, the same way the `sideby` skill is installed from the `sideby` repo.

## Why

Both skills cite the same fact list (`docs/facts.md`) and the same bilingual guide pages. When npm changes a rule, the fact, the pages and the skills must change in one commit. Splitting the skills into `adonis-skills` would mean two repos per change and cross-repo citation links.

## Consequences

- Two skills, two install targets: `npx skills add Adonis0123/npm-release-guide --skill <name>`.
- `npm-publish-guide` hands the version decision to `npm-version-guide` when it is installed and falls back to a minimal rule otherwise.
- `adonis-skills` must not copy these skills; a copy would drift.
