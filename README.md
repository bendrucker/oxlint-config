# @bendrucker/oxlint-config

Shared [Oxlint](https://oxc.rs/docs/guide/usage/linter) presets.

| Preset | For | Adds |
| --- | --- | --- |
| `base.json` | Every JS or TS project | `correctness`, `suspicious`, `pedantic`, and `perf` categories as errors, minus the pedantic rules listed as off, plus opted-in style and restriction rules. Test-file overrides for the vitest plugin, which also covers `bun:test` and `node:test`. |
| `typescript.json` | TypeScript projects | TS style and restriction rules, `strict-boolean-expressions`, and offs for core rules the compiler or a type-aware rule replaces. The type-aware rules run under `oxlint --type-aware` with `oxlint-tsgolint` installed. |
| `bun.json` | Bun projects | Steers file I/O from `node:fs` to `Bun.file` and `Bun.write`. |

```sh
npm install --save-dev --save-exact oxlint @bendrucker/oxlint-config
```

```jsonc
// .oxlintrc.json
{
  "$schema": "./node_modules/oxlint/configuration_schema.json",
  "extends": [
    "./node_modules/@bendrucker/oxlint-config/base.json",
    "./node_modules/@bendrucker/oxlint-config/typescript.json"
  ],
  "ignorePatterns": ["dist/**"]
}
```

`extends` in a JSON config takes file paths, not package names, so presets are referenced through `node_modules`. Repo-specific settings stay in the consumer: size and complexity thresholds, `ignorePatterns` (not inherited through `extends`), import restrictions, and path overrides.

## Categories

Presets turn on whole upstream categories rather than listing their rules, so a new Oxlint release can add rules to a consumer's run. Consumers pin `oxlint` and take those through dependency updates, where CI shows the new violations before they merge.

## Releases

Every push to `main` that changes a preset publishes to npm. `package.json` holds `major.minor`, and the patch is the commit count.
