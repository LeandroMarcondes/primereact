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
