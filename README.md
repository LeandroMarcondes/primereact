[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Fork of PrimeReact](https://img.shields.io/badge/fork%20of-primefaces%2Fprimereact-blue)](https://github.com/primefaces/primereact)

# PrimeReact (Leandro's fork)

This is Leandro Abreu's personal fork of [PrimeReact](https://github.com/primefaces/primereact), the open source UI component library for React. It's based on the last MIT-licensed release before the upstream project archived and moved development to [PrimeUI](https://primeui.dev/nextchapter), and carries custom fixes and tweaks on top of that base for use in Leandro's own projects.

This fork is maintained independently and isn't affiliated with PrimeTek or PrimeUI. For the official library, documentation, and community support, see [primereact.org](https://primereact.org/) or the upstream repo linked above.

## Development

This repo is a Next.js site (the docs/showcase app) plus the component library under `components/lib`.

```
# install dependencies
npm install

# run the docs/showcase site locally
npm run dev

# build the component library (dist/)
npm run build:lib

# run tests
npm run test:check

# lint, format check, type check, and audit
npm run build:check
```

## Using this fork in a project

This isn't published to npm, so install it straight from git. `master` is source only - the `build` branch carries the compiled output, so point at that branch, not `master`:

```
npm install github:LeandroMarcondes/primereact#build
```

npm pins the exact commit it resolved `#build` to in `package-lock.json` and won't re-check the branch on a normal `npm install` afterwards. To pick up a newer fix, force a re-resolve:

```
npm install primereact@github:LeandroMarcondes/primereact#build
```

## Releasing a fix

1. Commit and push the fix on `master` as normal.
2. From `master`, build fresh into a throwaway folder. This runs the same two steps `npm run build:lib` runs internally, just without the `build:check` gate in front of them (that gate also runs lint/format/type checks and an `npm audit`, which isn't needed here and can fail on pre-existing issues unrelated to your change):

    ```
    NODE_ENV=production INPUT_DIR=components/lib/ OUTPUT_DIR=.release-build/ npx rollup -c
    INPUT_DIR=components/lib/ OUTPUT_DIR=.release-build/ npx gulp build-resources
    ```

    `INPUT_DIR` and `OUTPUT_DIR` are read by `rollup.config.js` and `gulpfile.js` (not real rollup/gulp options) to know where the component source lives and where to write the build. Both commands must point at the same `OUTPUT_DIR`, and it must end with a trailing slash. Run them from the repo root, in order:

    - `npx rollup -c` reads every component under `INPUT_DIR` and bundles each one into its own CJS/ESM/minified JS files under `OUTPUT_DIR` (e.g. `.release-build/datatable/datatable.esm.js`), plus a `package.json` at the root of `OUTPUT_DIR` describing the package (name, version, `main`/`module` entry points, etc.) - this is what makes the output directory installable as `primereact` on its own. `NODE_ENV=production` turns on minification for this step.
    - `npx gulp build-resources` then copies everything rollup doesn't handle into that same `OUTPUT_DIR`: theme CSS under `resources/themes/`, fonts, images, each component's `.d.ts` type declarations, and each component's own small `package.json` (the one with `"main": "./button.cjs.js"` etc. that lets `primereact/button` resolve correctly).

    If you're on Windows PowerShell instead of a bash-like shell, set the env vars first since PowerShell doesn't support the `VAR=value command` syntax:

    ```
    $env:NODE_ENV="production"; $env:INPUT_DIR="components/lib/"; $env:OUTPUT_DIR=".release-build/"; npx rollup -c
    npx gulp build-resources
    ```

3. Switch to `build`, wipe its old content, and replace it with the fresh output:

    ```
    git checkout build
    git rm -rf .
    cp -r .release-build/. .
    rm -rf .release-build
    git add -A
    git commit -m "Rebuild output"
    git push origin build
    git checkout master
    ```

4. In any project consuming the fork, run `npm install primereact@github:LeandroMarcondes/primereact#build` (or `npm run update:primereact` if that script's set up) to pick up the change.

## Import

Each component can be imported individually so only what you use gets bundled:

```javascript
import { Button } from 'primereact/button';

export default function MyComponent() {
  return <Button label="PrimeReact" />;
}
```

## Theming

PrimeReact has two theming modes: styled or unstyled.

**Styled mode** ships pre-skinned components with opinionated themes like Material, Bootstrap, or PrimeOne. Import the theme CSS you want, see the [Themes](https://primereact.org/theming) docs for the full list.

```javascript
import 'primereact/resources/themes/lara-light-cyan/theme.css';
```

**Unstyled mode** is off by default. Set `unstyled` to true on the PrimeReact context to enable it globally, see the [Unstyled mode](https://primereact.org/unstyled) docs for details.

## Credit

All credit for the original library goes to the [PrimeReact/PrimeTek team](https://github.com/primefaces/primereact/graphs/contributors) and its contributors.
