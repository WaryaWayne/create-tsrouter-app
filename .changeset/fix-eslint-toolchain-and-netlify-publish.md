---
'@tanstack/create': patch
---

fix(create): correct netlify.toml key, eslint scripts, and missing eslint dep

- The generated `netlify.toml` for both React and Solid used `dir` under
  `[build]`, which is not a valid Netlify configuration key. Per Netlify's
  TanStack Start guide it must be `publish`. Closes #423.
- The eslint toolchain had `format` and `check` scripts swapped: `format`
  ran prettier in read-only mode while `check` mutated files. Swap them so
  `format` writes (`prettier --write . && eslint --fix`) and `check` is
  read-only (`prettier --check .`). Closes #403.
- `@tanstack/eslint-config` lists `eslint` as a peer dependency, so eslint
  was not installed by package managers that don't auto-install peers. Add
  `eslint` to `devDependencies` in the eslint toolchain. Closes #417.
