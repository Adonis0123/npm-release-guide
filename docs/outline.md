# Content outline

Working outline for writers. Every factual claim in the guide must cite an `F-xx` entry in `docs/facts.md`; anything not there is not written. Terms follow `GLOSSARY.md`.

## Audience and tone

- Reader cannot write code. They know how to open a terminal and paste one line; nothing more is assumed.
- Default action when stuck: hand the problem to an Agent with the `npm-publish-guide` skill, paste the error text. Pages explain *why*, the Agent does the *how*.
- One idea per sentence, short sentences, no jargon without a one-line plain explanation on first use (link to `concepts`).
- Every page starts with a "Last verified: YYYY-MM-DD" line and ends with its sources (F-xx links).

## Files

English pages live in `skills/npm-publish-guide/references/` (installed with the skill, read by Agents and by English readers). Chinese pages mirror them in `docs/zh-CN/` with the same file names.

### README.md / README.zh-CN.md

1. Language switch link under the title.
2. One-sentence promise: publish npm packages without getting stuck on two-factor prompts.
3. **Start here (first screen)**:
   - Install: `npx skills add Adonis0123/npm-publish-guide`
   - Then tell your Agent: "Publish this project to npm using npm-publish-guide." When stuck, paste the error text to the Agent.
4. **The one rule to remember**: when npm needs your Second Factor, your Agent hands you one line to paste into your Real Terminal; you approve in your own browser. Never type codes into the Agent chat.
5. **Want to understand it yourself?** situation table:

| Your situation | Read |
|---|---|
| Publishing a brand-new package | first-release |
| Publishing a new version from your computer | local-release |
| Want GitHub to publish for you, no more prompts | trusted-publishing |
| Publishing stopped with an error | troubleshooting |
| Confused by the npm two-factor settings page | settings-page |
| Words you don't know | concepts |

6. What this is not (only npm registry; JSR/PyPI out of scope).
7. License.

### concepts.md — words you will meet

Plain explanations, each 2–4 sentences, with "you'll see this when…": Second Factor (Passkey vs OTP; why a Passkey-only Publisher has no code), Browser Confirmation, Login Session, Access Token (and why it is a last resort; classic tokens gone), Trusted Publishing, Automated Release, Staged Release, Provenance, Real Terminal vs Agent (why an Agent's command cannot show the browser link). Short note on pnpm / yarn / bun: same registry, same rules, small command differences (table).

### first-release.md — publishing a brand-new package

Why the first version must come from your computer. Steps as the Publisher experiences them: Agent checks the package (name free, files included), Agent prepares a script and copies one line, you paste it in your Real Terminal, log in if asked, approve in your browser. What you may see right after (the `0.0.0-stage` observation, only if F-xx supports it). Then: "Now set up Trusted Publishing so you never do this again."

### local-release.md — new version from your computer

When this is still needed (no CI yet; `npm deprecate` / `dist-tag` style changes). Same paste-one-line pattern. Why it fails when an Agent runs it in the background (EOTP) and why a pipe like `| tee` breaks it too.

### trusted-publishing.md — let GitHub publish for you

What it is in one paragraph. One-time setup checklist, what the Agent does vs what you do:
- Agent: add `release.yml` from the template, create the GitHub `npm` environment limited to `v*` tags (note the `type=tag` pitfall), on release, bump with `npm version` and push only `refs/tags/v<version>`.
- You: paste one line that runs `npm trust github …` and `npm access set mfa=publish …`, approve in browser.
- Everyday release afterwards: ask the Agent "release a patch"; it pushes a tag; GitHub publishes.
- How to check it worked (Provenance shown on the npm page).
Status banner: UNVERIFIED until a real end-to-end run succeeds.

### troubleshooting.md — by error text

One section per message the reader actually sees, each: what it means, why, what to tell your Agent.
- `E401 Unauthorized` / `npm whoami` fails → not logged in.
- `EOTP` / "This operation requires a one-time password" while an Agent ran it → no Real Terminal.
- "Enter OTP:" in your own terminal but you only have a Passkey → status per facts (UNCONFIRMED cause); what to try.
- Browser link opened but nothing happened / `E404 … /-/v1/done` → link expired or opened in a browser not logged in to npm.
- `E403` / `E404` on GitHub publish → Trusted Publishing mismatch (workflow file name, environment name, repository field).
- Environment rejected the tag → environment rule created as branch instead of tag.
- Package page shows the wrong README language → note and fix pointer.

### settings-page.md — the npm two-factor settings page, item by item

Walk through: Security Keys (your Passkeys; add one more as backup), Recovery Codes (download, store in a password manager; using one freezes the account for 72 h if F-xx confirms), "Require two-factor authentication for write actions" (keep on). Package-level setting: "Require 2FA and disallow tokens" (recommended after Trusted Publishing). Recommended setup in three lines: Passkey only, write-actions on, packages disallow tokens. Describe the page in words; do not embed screenshots of real accounts.
