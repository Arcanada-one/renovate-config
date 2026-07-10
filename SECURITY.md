# Security Policy

## Reporting a vulnerability

Report security issues privately to **security@arcanada.one**. Do not open a
public issue for a suspected vulnerability. You will receive an acknowledgement
within 72 hours.

## Scope

This repository ships JSON configuration only (Renovate preset files) — there
is no runtime code, no dependencies, and no secrets. The primary risk surface
is a malicious or overly-permissive rule (e.g. an `automerge` rule that is too
broad) landing in `default.json`/`nestjs.json`/`react.json` and propagating to
every consumer repo that extends this preset.

## Supply-chain guarantees

- **No long-lived credentials.** This repository holds no npm tokens, API
  keys, or deploy credentials.
- **Review gate.** Changes to any preset file are protected by `CODEOWNERS`
  and require a reviewed pull request before merge — a change here fans out
  to every consumer repo, so review is mandatory.
- **Secret hygiene.** `gitleaks` runs as a CI merge gate even though the repo
  is config-only, to catch accidental secret paste.
- **Blast-radius awareness.** `packageRules` that enable `automerge` are
  scoped narrowly (currently: devDependency patch/minor only) — broadening an
  automerge rule is a security-relevant change subject to the review gate
  above.

## Supported versions

There is a single unversioned `main` branch; consumer repos always resolve
`github>Arcanada-one/renovate-config` (or the `:nestjs` / `:react` named
preset) against the current `main`.
