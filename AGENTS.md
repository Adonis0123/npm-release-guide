# AGENTS.md

A public guide and agent skill that helps people who cannot write code publish npm packages without getting stuck on two-factor prompts.

## Layout

- `skills/npm-publish-guide/` — the installable skill. `SKILL.md` is the entry; `references/` holds the English guide pages; `scripts/` holds read-only checks; `templates/` holds files the skill copies into a user's repo.
- `docs/zh-CN/` — Chinese mirror of `references/`, same file names.
- `docs/facts.md` — the only source of factual claims. Each entry has an id (`F-xx`), an official source, a short quote and a verification date.
- `docs/outline.md` — what each page covers and for whom.
- `GLOSSARY.md` — canonical terms. Use them; avoid the listed synonyms.

## Rules

- No fact without an `F-xx` entry. When npm changes something, update `docs/facts.md` first, then every page that cites the entry, then the page's "Last verified" date.
- English and Chinese pages change together. README.md and README.zh-CN.md link to each other under the title.
- The skill never runs a command that needs a Second Factor itself, never asks the Publisher for a one-time code in chat, and never opens npm confirmation links in a browser it controls. It prepares one line for the Publisher's Real Terminal instead.
- Nothing personal in the repo: no local paths, account names, tokens, or screenshots of real accounts.
- Scripts in `skills/npm-publish-guide/scripts/` never change npm, GitHub or the user's repo; `diagnose.sh` writes nothing, `handoff.sh` writes only its handoff script and log under the system temp folder. They must work on macOS and Linux with POSIX sh; clipboard use is optional (`pbcopy`, `wl-copy`, `xclip`), falling back to printing the line.
