# vite-base

A Vite+ monorepo starter: a TypeScript web app and a publishable TypeScript package, managed with Bun workspaces and the `vp` toolchain.

## Requirements

- Node.js `>=22.18.0`
- Bun `1.4.3` (pinned in `packageManager`; Vite+ installs it when missing)
- The [Vite+ CLI](https://viteplus.dev/guide/) (`vp`)

## Structure

```
apps/
  website/          Vite app (vanilla TypeScript, unbuilt `vp dev` target)
packages/
  utils/            Publishable TypeScript package, built with `vp pack`
tsconfig.base.json  Shared strict compiler options
vite.config.ts      Root fmt/lint/staged/task configuration
```

`tools/*` is a reserved workspace glob for internal build tooling; create the directory when the first tool lands.

## Commands

| Command           | Description                                      |
| ----------------- | ------------------------------------------------ |
| `vp install`      | Install dependencies and configure Git hooks     |
| `vp run dev`      | Start the website dev server on `:5173`          |
| `vp check`        | Format, lint, and type-check the whole workspace |
| `vp run -r test`  | Run tests in every package                       |
| `vp run -r build` | Build every package in dependency order          |
| `vp run ready`    | Full pre-push gate: check, test, build           |

`vp config` (run by `prepare`) points `core.hooksPath` at `.vite-hooks/_`, whose `pre-commit` runs `vp staged` and `vp check --fix` on staged files.

## Workspace conventions

- **Dependency versions** live in the root `catalog`; reference them with `"catalog:"`.
- **Internal packages** are referenced with `workspace:*` and named under the `@arquizade/` scope.
- **Dev resolution**: `@arquizade/utils` exports a `development` condition mapping to `src/index.ts`, so apps run against source without a prior build. Production builds resolve `dist/`, and the app tsconfig opts into the same condition via `customConditions`.
- **Ordering**: `vp run -r <task>` follows the workspace dependency graph, so `utils` builds before `website`.

## Releasing

`packages/utils` is publishable (`files: ["dist"]`, `publishConfig.access: "public"`). Add a versioning tool such as Changesets before the first release; no release process is configured yet.
