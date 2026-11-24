# npm-publish-guide

[简体中文](README.zh-CN.md)

Last verified: 2026-11-24

Publish npm packages without getting stuck on two-factor prompts. No coding needed: when you run into a problem, hand it to your Agent.

## Start here

Install the skill (requires Node.js 22.20 or later [F-61]):

```sh
npx skills add Adonis0123/npm-publish-guide
```

This installs the `npm-publish-guide` skill from `skills/npm-publish-guide/SKILL.md` [F-60].

Then tell your AI coding agent (Claude Code, Codex, Cursor, …):

> Publish this project to npm using npm-publish-guide.

If you're stuck: paste the error into your Agent and say "use npm-publish-guide".

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

[F-43]: docs/facts.md
[F-44]: docs/facts.md
[F-60]: docs/facts.md
[F-61]: docs/facts.md
