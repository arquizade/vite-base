# @arquizade/utils

Shared TypeScript utilities for the [`vite-base`](../../README.md) monorepo.

## Development

```bash
vp run -C packages/utils test    # unit tests
vp run -C packages/utils build   # emit dist/ via vp pack
vp run -C packages/utils dev     # rebuild on change
```

## Consumption

The package exports a `development` condition pointing at `src/index.ts`, so workspace apps resolve source in dev and `dist/index.mjs` in production builds:

```ts
import { fn } from "@arquizade/utils";
```

Type declarations are emitted to `dist/index.d.mts` and exposed via the `types` export condition.
