---
title: Node
---

# {% $markdoc.frontmatter.title %}

The Node provider supports NPM, Yarn, Yarn 2, and PNPM.

## Environment Variables

The Node provider sets the following environment variables:

- `CI=true`
- `NODE_ENV=production`
- `NPM_CONFIG_PRODUCTION=false`: Ensure that dev deps are always installed
- `NIXPACKS_NX_APP_NAME`: Provide a name of the NX app you want to build from your NX Monorepo

## Setup

The following major versions are available

- `14`
- `16` (Default)
- `18`

The version can be overriden by

- Setting the `NIXPACKS_NODE_VERSION` environment variable
- Specifying the `engines.node` field in `package.json`

Only a major version can be specified. For example, `14.x` or `14`.

**Node Canvas**

If [node-canvas](https://www.npmjs.com/package/canvas) is found in the `package.json` file, then the `libuuid` and `libGL` libraries are made available in the environment.

## Install

All dependencies found in `packages.json` are installed with either NPM, Yarn, or PNPM.

## Build

The build script found in `package.json` if it exists or if its an NX Monorepo `(npm|pnpm|yarn|bun) run build <NxAppName> --configuration=production`.

## Start

The start command priority is

- If its an NX Monorepo
  - If the app has a `start` target `npx nx run <appName>:start:production` or just `npx nx run <appName>:start` if no production configuration is present
  - If the app is a NextJS project: `npm run start`
  - If `targets.build.options.main` exists in the apps `Project.json`: `node <outputPath>/<mainFileName>.js` (e.g `node dist/apps/my-app/main.js`)
  - Fallback: `node <outputPath>/index.js` (e.g `node dist/apps/my-app/index.js`)
- Start script in `package.json`
- Main file
- `index.js`

## Caching

These directories are cached between builds

- Install: Global NPM/Yarn/PNPM cache directories
- Install (if Cypress detected): `~/.cache/Cypress`
- Build: `node_modules/.cache`
- Build (if NextJS detected): `.next/cache`
- Build (if its an NX Monorepo): `<outputPathForApp>`
