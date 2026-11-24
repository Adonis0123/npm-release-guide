# npm-release-guide

[简体中文](README.zh-CN.md)

Last verified: 2026-11-24

Pick the right version number and publish npm packages without getting stuck on two-factor prompts. No coding needed: hand it to your Agent.

## Start here

Install both skills into your project (recommended; requires Node.js 22.20 or later [F-61]):

```sh
npx skills add Adonis0123/npm-release-guide
```

This installs `npm-publish-guide` and `npm-version-guide` into the current project from `skills/*/SKILL.md` [F-60], [F-111]. Commit the installed folder and `skills-lock.json` so teammates get the same setup [F-111], [F-112].

- Install only one: `npx skills add Adonis0123/npm-release-guide --skill npm-version-guide` (or `--skill npm-publish-guide`) (observed to work in our test [F-113])
- Update later: `npx skills update` [F-112]
- Pin a version: `npx skills add Adonis0123/npm-release-guide@v0.1.0` (from the CLI docs; not run in our test [F-113])

**Already have a `skills/` folder in your project?** The installer treats a folder named `skills/` as the folder of an agent called OpenClaw [F-116], so it may add two links there, `skills/npm-publish-guide` and `skills/npm-version-guide`, next to your own skills [F-118]. Add these two lines to `.gitignore` so Git leaves them out (without a `/` at the end, or Git will not ignore them [F-118]):

```text
skills/npm-publish-guide
skills/npm-version-guide
```

You can also name the agents you use when installing (ids for other agents are in the [agent list](https://github.com/vercel-labs/skills/blob/main/docs/agents.md)) [F-117]. In our test a later `npx skills update` still added a link again, so keep the `.gitignore` lines too [F-118]:

```sh
npx skills add Adonis0123/npm-release-guide -a claude-code -a codex
```

Claude Code users can install as a plugin instead [F-114] (automatic updates for third-party marketplaces are off by default [F-115]):

```text
/plugin marketplace add Adonis0123/npm-release-guide
/plugin install npm-release-guide@npm-release-guide
```

Then tell your AI coding agent (Claude Code, Codex, Cursor, …):

- To publish: `Publish this project to npm using npm-publish-guide.`
- To pick a version number: `Which version should the next release be? Use npm-version-guide.`

If you're stuck: paste the error into your Agent and say "use npm-publish-guide" (or "use npm-version-guide").

## Project settings

The first time you run a skill in a project, your Agent inspects the repo, shows you the settings it inferred, and asks once (yes or no) whether to save them to `.agents/release.json`. Commit that file; after that, your Agent uses it automatically. See [config](skills/npm-version-guide/references/config.md) for the file format.

## The one rule

When npm needs your Second Factor, your Agent gives you one line to paste into your Real Terminal; you approve in your own browser [F-43], [F-44]. Never type codes into the Agent chat.

## How it works

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="docs/images/publish-flow.en.dark.svg">
  <img alt="Flow: ask your AI, it checks the package; a new package gets one first release from your own terminal, then GitHub is linked to npm once; after that every release is a tag your AI pushes and GitHub publishes" src="docs/images/publish-flow.en.light.svg">
</picture>

## Want to understand it yourself?

| Your situation | Read |
| --- | --- |
| Not sure which version number to use | [versioning](skills/npm-version-guide/references/versioning.md) |
| Publishing a brand-new package | [first-release](skills/npm-publish-guide/references/first-release.md) |
| Publishing a new version from your computer | [local-release](skills/npm-publish-guide/references/local-release.md) |
| Want GitHub to publish for you, no more prompts | [trusted-publishing](skills/npm-publish-guide/references/trusted-publishing.md) |
| Publishing stopped with an error | [troubleshooting](skills/npm-publish-guide/references/troubleshooting.md) |
| Confused by the npm two-factor settings page | [settings-page](skills/npm-publish-guide/references/settings-page.md) |
| Words you do not know | [concepts](skills/npm-publish-guide/references/concepts.md) |

## Scope

This guide covers publishing to the npm registry (`registry.npmjs.org`) only. Publishing with pnpm, yarn, or bun goes to the same registry; the differences are on the [concepts](skills/npm-publish-guide/references/concepts.md) page. JSR and PyPI are out of scope.

## License

MIT

## Sources

- [F-43](docs/facts.md)
- [F-44](docs/facts.md)
- [F-60](docs/facts.md)
- [F-61](docs/facts.md)
- [F-111](docs/facts.md)
- [F-112](docs/facts.md)
- [F-113](docs/facts.md)
- [F-114](docs/facts.md)
- [F-115](docs/facts.md)
- [F-116](docs/facts.md)
- [F-117](docs/facts.md)
- [F-118](docs/facts.md)

[F-43]: docs/facts.md
[F-44]: docs/facts.md
[F-60]: docs/facts.md
[F-61]: docs/facts.md
[F-111]: docs/facts.md
[F-112]: docs/facts.md
[F-113]: docs/facts.md
[F-114]: docs/facts.md
[F-115]: docs/facts.md
[F-116]: docs/facts.md
[F-117]: docs/facts.md
[F-118]: docs/facts.md
