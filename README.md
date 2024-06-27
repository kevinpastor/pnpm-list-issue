# pnpm-list-issue

There seems to be an issue with `pnpm list` when a peer dependency is installed with a different version for a workspace package.

In this repository, `lib` has both a peer dependency and a dev dependency on `lodash`. `app` has a dependency on `lib` and on `lodash`. The nuance is that `lib` specified its dev dependency on `lodash` to `4.17.20` and `app` specified its dependency on `lodash` to `4.17.21`.

When running `pnpm list lodash --depth Infinity` in the `packages/app` folder, we would expect `4.17.20` and `4.17.21` to be listed. However, only `4.17.21` is listed.

```bash
$ pnpm list lodash --depth Infinity
Legend: production dependency, optional only, dev only

app@1.0.0 F:\playground\pnpm-list-issue\packages\app  

dependencies:
lodash 4.17.21
```

When running `pnpm list lodash@^4.0.0 --depth Infinity`, the same occurs.

When running `pnpm list lodash@4.17.20 --depth Infinity`, nothing is listed.

Looking inside the `packages/app/node_modules` folder, we can easilty find `lodash@4.17.20` at `packages/app/node_modules/lib/node_modules/lodash`.
