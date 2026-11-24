# AGENTS.md

A public guide and agent skill that helps people who cannot write code publish npm packages without getting stuck on two-factor prompts.

## Layout

- `skills/npm-publish-guide/` — the publishing skill. `SKILL.md` is the entry; `references/` holds the English guide pages; `scripts/` holds read-only checks; `templates/` holds files the skill copies into a user's repo.
- `skills/npm-version-guide/` — the versioning skill, installable on its own. `SKILL.md` is the entry; `references/` holds its English pages (`config.md` documents `.agents/release.json`); `scripts/suggest-version.mjs` derives the Bump Suggestion; `evals/` holds its trigger cases.
- `schemas/` — JSON Schemas for the per-project settings file `.agents/release.json`. The file name carries the format version (`release.v1.schema.json`); a breaking change to the format gets a new file, never an edit in place.
- `.claude-plugin/marketplace.json` — Claude Code plugin marketplace entry that lists both skill folders explicitly. Do not add a `plugin.json`: with `strict: false` and entry-level `skills` it fails to load.
- `docs/examples/` — example settings files (single package and monorepo) that must stay valid against the schema.
- `docs/zh-CN/` — Chinese mirror of `references/`, same file names.
- `docs/facts.md` — the only source of factual claims. Each entry has an id (`F-xx`), an official source, a short quote and a verification date.
- `docs/outline.md` — what each page covers and for whom.
- `GLOSSARY.md` — canonical terms. Use them; avoid the listed synonyms.

## Rules

- No fact without an `F-xx` entry. When npm changes something, update `docs/facts.md` first, then every page that cites the entry, then the page's "Last verified" date.
- English and Chinese pages change together. README.md and README.zh-CN.md link to each other under the title.
- The skills never run a command that needs a Second Factor themselves, never ask the Publisher for a one-time code in chat, and never open npm confirmation links in a browser they control. They prepare one line for the Publisher's Real Terminal instead.
- Nothing personal in the repo: no local paths, account names, tokens, or screenshots of real accounts.
- Scripts in `skills/npm-publish-guide/scripts/` never change npm, GitHub or the user's repo; `diagnose.sh` writes nothing, `handoff.sh` writes only its handoff script and log under the system temp folder. They must work on macOS and Linux with POSIX sh; clipboard use is optional (`pbcopy`, `wl-copy`, `xclip`), falling back to printing the line.
- `skills/npm-version-guide/scripts/suggest-version.mjs` is read-only too: it writes no files, makes only read git calls, and runs no project scripts. It is a zero-dependency Node.js ES module (built-ins and the git CLI only); keep it that way so the skill works without `npm install`. Its tests (`suggest-version.test.mjs`, `node --test`) build their own temporary git repositories.
