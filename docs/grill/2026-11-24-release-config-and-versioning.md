# Decision log: versioning skill and per-project config (2026-11-24)

Interviewer: Claude Code (cc002). Proxy: agy (named by the owner). Red-team: Codex. 1 round + 1 re-ask; stop: frontier empty. Owner delegated all answers ("都按照推荐的来吧，我不想看").

## Decided by the owner (earlier in the session)

- One repo `npm-release-guide` with two separately installable skills: `npm-publish-guide`, `npm-version-guide` (ADR 0001).
- Script decides the bump first; the AI only handles what the script cannot classify.
- Release flow unchanged: `npm version` + push `v*` tag + Trusted Publishing.
- Install: `npx skills add Adonis0123/npm-release-guide` (project-level, commit `skills-lock.json`); second path: Claude Code plugin marketplace.
- Per-project settings in `.agents/release.json`, written by the AI on first run after inferring and asking once; without it, read-only work proceeds on inferred values, outward actions ask first.
- Defaults: 0.x breaking → minor; Pre-release tag `next`. This repo tags its own releases (git tag only).

## Decided by the Proxy

| Id | Decision | Reversibility |
|---|---|---|
| Q16 | Zero npm dependencies: Node.js built-ins + git; small built-in version parser; complex ranges go to needs_ai | easy |
| Q17 | `skills/npm-version-guide/scripts/suggest-version.mjs` (ES module); existing sh scripts unchanged | easy |
| Q18 | `key: value` output like `diagnose.sh`, `--json` for machines, exit 0 when AI review is needed | easy |
| Q19 | Optional emoji or `:shortcode:` prefix, then Conventional Commits; `!`/`BREAKING CHANGE` → major, feat → minor, fix/perf → patch, others none, release commits ignored, unparseable → needs_ai; range since latest `v*` or `<name>@*` tag; 0.x with `zeroMajor: "minor"`: breaking → minor, feat/fix → patch | easy |
| Q20 | package.json signals: removed exports/bin entry, main/module/types removed or changed, raised `engines.node`, raised or new required peer → breaking; first-time `exports` → needs_ai; new optional peer → feat | easy |
| Q21 | `publicInterface` default = paths behind exports/main/module/types/bin only; fix/refactor/perf touching them → needs_ai | easy |
| Q22 | Schema at `schemas/release.v1.schema.json`, `"version": 1`, `additionalProperties: false` (allow `$schema`); **amended after red-team**: top-level `publish` and `versioning` are defaults, each `packages[]` entry is `{ "dir", "publish"?, "versioning"? }` and overrides them, because npm links Trusted Publishing per package | costly |
| Q23 | Monorepo v1: one package per release, commits filtered to its dir, `v*` tags unchanged; coordinated multi-package releases → changesets (documented only) | easy |
| Q24 | Publish skill's everyday release runs `../npm-version-guide/scripts/suggest-version.mjs` when present, else "patch for fixes, minor for features, ask"; version skill never publishes, ends at `npm version <level>` after a yes | easy |
| Q25 | `.claude-plugin/marketplace.json`: one entry `npm-release-guide`, `source "./"`, `strict: false`, no `version`, explicit skill dirs; check with `claude plugin validate` | costly |
| Q26 | `skills/npm-version-guide/evals/evals.json`: 4 positive cases + 1 negative | easy |

## Red-team (Codex)

- Q22: overturn → resolved by the per-package override above; also pin the schema by a versioned file name instead of a moving one. Verify that `.agents/release.json` is not packed (`npm pack --dry-run --ignore-scripts`).
- Q25: hold. Explicit skill dirs still let Claude scan `skills/`; adding a `plugin.json` later together with `strict: false` and entry-level `skills` would fail to load.

## Escalations

None. The Q22 overturn proposed a fix compatible with the decision, so it was adopted instead of escalated.
