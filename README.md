# renovate-config

Shared [Renovate](https://docs.renovatebot.com/) config presets for
`Arcanada-one` repositories — "update a dependency rule once, roll it out
everywhere."

> **Boundary.** Public repository, no secrets, no project-specific settings —
> presets only. Renovate resolves `github>` preset references anonymously for
> public repos, which is why this repo (like `arcanada-shared`) is public.

## Presets

| File            | Extends            | Use for                                                     |
| ---------------- | ------------------- | ------------------------------------------------------------ |
| [`default.json`](default.json) | `config:recommended` | Base preset — every consumer repo extends this at minimum. |
| [`nestjs.json`](nestjs.json)   | `default.json`        | Backend repos (NestJS, Prisma, Zod, BullMQ, Fastify, pino). |
| [`react.json`](react.json)     | `default.json`        | Frontend repos (React, Vite, Tailwind, React Query).        |

`default.json` sets the shared baseline: `config:recommended`, semantic
commits, dependency dashboard, weekly schedule, major-update approval gate,
devDependency automerge, and GitHub Actions digest pinning (per the ecosystem
S4 supply-chain rule). The named presets add stack-specific package groupings
so a related family of packages (e.g. `@nestjs/*`, or `react` +
`react-dom`) bumps in a single PR instead of one PR per package.

## Usage

A consumer repo's `renovate.json` extends this repo directly:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["github>Arcanada-one/renovate-config"]
}
```

Or a named, stack-specific preset:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["github>Arcanada-one/renovate-config:nestjs"]
}
```

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["github>Arcanada-one/renovate-config:react"]
}
```

No local override is required to pick up a rule change here — updating
`default.json` (or a named preset) in this repo rolls the new rule out to
every consumer repo on its next Renovate run. That is the "update a
dependency rule once, roll it out everywhere" behaviour this repo exists for.

## Internal version hub (pnpm Catalog)

For monorepos in the ecosystem that use pnpm workspaces (e.g.
[`arcanada-shared`](https://github.com/Arcanada-one/arcanada-shared)), the
[pnpm Catalog](https://pnpm.io/catalogs) feature (`catalog:` field in
`pnpm-workspace.yaml`) is the internal, same-repo equivalent of this preset
repo: a single place to pin a shared dependency version across every package
in the workspace. Renovate has first-class Catalog support (`matchManagers:
["npm"]` covers `catalog:` entries in `pnpm-workspace.yaml` out of the box) —
no extra configuration needed here.

## Validating changes locally

```bash
npx --yes renovate-config-validator default.json nestjs.json react.json
```

## License

[MIT](LICENSE)
