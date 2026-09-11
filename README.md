# packages-ui

Nyuchi Design System — shared, publishable UI packages. Nyuchi's
implementation of the [Mzizi](https://github.com/bundu-labs) architecture,
consumed by the marketing sites and the documentation sites.

This is a [pnpm workspace](https://pnpm.io/workspaces) monorepo so it can hold
additional UI packages over time.

## Packages

| Package                            | Description                                                                          |
| ---------------------------------- | ------------------------------------------------------------------------------------ |
| [`@nyuchi/ui`](./packages/ui)      | Svelte 5 / SvelteKit component library on the Mzizi tokens                           |
| [`@bundu/ui`](./packages/bundu-ui) | Marketing UI kit — Astro marketing components + shadcn primitives on the same tokens |

Both packages ship **byte-identical** `styles/tokens.css`, `styles/theme.css`,
`tokens.json`, `tailwind-palette.mjs` and `styles/brand-*.css`. They are two outputs of
one generator, not two hand-maintained files.

## Tokens

All 21 Mzizi colour families (7 minerals, 7 heritage, 7 experimental) under one
`--color-*` namespace, plus the nine-step surface ladder and the connectivity status
trio. Everything is **generated** from `tokens/canon.snapshot.json`:

```sh
pnpm canon:fetch     # refresh the snapshot from canon (network, on demand)
pnpm tokens:build    # regenerate every artifact from the snapshot (offline)
pnpm tokens:check    # CI gate: fail if any generated file was hand-edited (offline)
pnpm canon:parity    # CI gate: fail if the snapshot has drifted from canon (network)
```

The snapshot is machine-written from two sources that are cross-checked against each
other — `https://api.mzizi.dev/api/v1/brand` and `mzizi-dev/mzizi-registry`'s
`lib/tokens/palette.source.ts`. Nobody types a hex. The values do **not** live in a
database; Mzizi holds no brand or primitive token data in one.

`canon:parity` reaches the network and therefore runs in **CI only** — it refuses to run
without `CI` set unless given `--force`. It is never in a build, a `prepack`, a
`postinstall` or a runtime path.

## Development

```sh
pnpm install
```

## Publishing

Packages are published to [npm](https://www.npmjs.com) automatically by the
[`publish` workflow](./.github/workflows/publish.yml) when a GitHub Release is
published — `@nyuchi/*` under the [`@nyuchi`](https://www.npmjs.com/org/nyuchi)
org and `@bundu/*` under the [`@bundu`](https://www.npmjs.com/org/bundu) org.
The workflow requires an `NPM_TOKEN` with publish access to both orgs.
`NPM_TOKEN` is an **organisation** secret on `mukoko-dev` and is visible to this
repository (verified 2026-09-12, after the transfer from `nyuchi`), so no repository-level
secret is needed.

Publishing is the owner's call. CI does not publish on a branch push — only on a
published GitHub Release or a `v*` tag.

## License

[MIT](./LICENSE) © Nyuchi Africa (Pvt) Ltd
