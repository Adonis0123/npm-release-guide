# npm Publishing Auth

How a person or a CI job proves to the npm registry that it may publish a package, and what the publisher has to do at each moment.

## Language

### Who is publishing

**Publisher**:
The human who owns the npm account and approves anything that needs a second factor.
_Avoid_: user, developer, maintainer (when the human is meant)

**Agent**:
An AI coding assistant (Claude Code, Codex, Cursor, …) that runs commands on the Publisher's behalf.
_Avoid_: bot, assistant, AI (as a noun for the actor)

**Real Terminal**:
A terminal window the Publisher types into themselves, where npm can show prompts and open the Publisher's own browser.
_Avoid_: shell, console, TTY (except when explaining the cause)

### How identity is proven

**Second Factor**:
The extra proof npm asks for on top of being logged in; in this guide it is either a Passkey or an OTP.
_Avoid_: 2FA code, verification (as a noun for the proof itself)

**Passkey**:
A Second Factor confirmed with Touch ID, a phone, or a hardware key in the browser; npm's settings page calls it a "security key".
_Avoid_: WebAuthn, hardware token, security key (except when quoting npm's UI)

**OTP**:
A one-time code from an authenticator app; a Passkey-only Publisher has none.
_Avoid_: verification code, TOTP, 2FA code

**Browser Confirmation**:
The flow where npm prints a link, the Publisher opens it in their own logged-in browser and approves with a Passkey.
_Avoid_: web login, auth-type=web (except when quoting config)

**Login Session**:
The short-lived credential `npm login` stores on the machine so later commands know who the Publisher is.
_Avoid_: npm token (when the session is meant), auth token

### Ways a release reaches npm

**Local Release**:
A version published from the Publisher's machine; needs a Login Session and a Second Factor.
_Avoid_: manual publish

**First Release**:
The very first version of a new package name; this guide does it as a Local Release, because Trusted Publishing can only be attached to a package that already exists.
_Avoid_: initial publish, bootstrap

**Trusted Publishing**:
A link between a package and a CI workflow, after which that workflow publishes without any token or Second Factor — provided the link was created with direct publishing allowed; otherwise each version becomes a Staged Release.
_Avoid_: OIDC publishing, tokenless publish, auto publish

**Automated Release**:
A version published by a CI workflow through Trusted Publishing, usually started by pushing a version tag.
_Avoid_: CI publish, auto release

**Staged Release**:
A version CI uploads but that only goes live after the Publisher approves it with a Second Factor.
_Avoid_: staged publishing (except when quoting npm docs), pending release

**Access Token**:
A long-lived secret that lets a machine act as the Publisher; this guide treats it as a last resort.
_Avoid_: NPM_TOKEN, granular token, automation token (except when naming the npm type)

**Provenance**:
The public record on a package version saying which repository and workflow built it.
_Avoid_: attestation, SLSA (except when quoting npm)
