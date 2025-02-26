# D2.js

[![npm version](https://badge.fury.io/js/%40terrastruct%2Fd2.svg)](https://www.npmjs.com/package/@terrastruct/d2)
[![License: MPL-2.0](https://img.shields.io/badge/License-MPL_2.0-brightgreen.svg)](https://mozilla.org/MPL/2.0/)

D2.js is a JavaScript wrapper around D2, the modern diagram scripting language. It enables running D2 directly in browsers and Node environments through WebAssembly.

## Features

- 🌐 **Universal** - Works in both browser and Node environments
- 🚀 **Modern** - Built with ESM modules, with CJS fallback
- 🔄 **Isomorphic** - Same API everywhere
- ⚡ **Fast** - Powered by WebAssembly for near-native performance
- 📦 **Lightweight** - Minimal wrapper around the core D2 engine

## Installation

```bash
# npm
npm install @terrastruct/d2

# yarn
yarn add @terrastruct/d2

# pnpm
pnpm add @terrastruct/d2

# bun
bun add @terrastruct/d2
```

## Usage

D2.js uses webworkers to call a WASM file.

```javascript
// Same for Node or browser
import { D2 } from '@terrastruct/d2';
// Or using a CDN
// import { D2 } from 'https://esm.sh/@terrastruct/d2';

const d2 = new D2();

const result = await d2.compile('x -> y');
const svg = await d2.render(result.diagram, result.options);
```

Configuring render options (see [CompileOptions](#compileoptions) for all available options):

```javascript
import { D2 } from '@terrastruct/d2';

const d2 = new D2();

const result = await d2.compile('x -> y', {
    sketch: true,
});
const svg = await d2.render(result.diagram, result.renderOptions);
```

## API Reference

### `new D2()`

Creates a new D2 instance.

### `compile(input: string, options?: CompileOptions): Promise<CompileResult>`

Compiles D2 markup into an intermediate representation.

### `render(diagram: Diagram, options?: RenderOptions): Promise<string>`

Renders a compiled diagram to SVG.

### `CompileOptions`

All [RenderOptions](#renderoptions) properties in addition to:

- `layout`: Layout engine to use ('dagre' | 'elk') [default: 'dagre']

### `RenderOptions`

- `sketch`: Enable sketch mode [default: false]
- `themeID`: Theme ID to use [default: 0]
- `darkThemeID`: Theme ID to use when client is in dark mode
- `center`: Center the SVG in the containing viewbox [default: false]
- `pad`: Pixels padded around the rendered diagram [default: 100]
- `scale`: Scale the output. E.g., 0.5 to halve the default size. The default will render SVG's that will fit to screen. Setting to 1 turns off SVG fitting to screen.
- `forceAppendix`: Adds an appendix for tooltips and links [default: false]
- `target`: Target board/s to render. If target ends with '*', it will be rendered with all of its scenarios, steps, and layers. Otherwise, only the target board will be rendered. E.g. `target: 'layers.x.*'` to render layer 'x' with all of its children. Pass '*' to render all scenarios, steps, and layers. By default, only the root board is rendered. Multi-board outputs are currently only supported for animated SVGs and so `animateInterval` must be set to a value greater than 0 when targeting multiple boards.
- `animateInterval`: If given, multiple boards are packaged as 1 SVG which transitions through each board at the interval (in milliseconds).
- `salt`: Add a salt value to ensure the output uses unique IDs. This is useful when generating multiple identical diagrams to be included in the same HTML doc, so that duplicate IDs do not cause invalid HTML. The salt value is a string that will be appended to IDs in the output.
- `noXMLTag`: Omit XML tag `(<?xml ...?>)` from output SVG files. Useful when generating SVGs for direct HTML embedding.

### `CompileResult`

- `diagram`: `Diagram`: Compiled D2 diagram
- `options`: `RenderOptions`: Render options merged with configuration set in diagram
- `fs`
- `graph`

## Development

D2.js uses Bun, so install this first.

### Building from source

```bash
git clone https://github.com/terrastruct/d2.git
cd d2/d2js/js
./make.sh all
```

If you change the main D2 source code, you should regenerate the WASM file:
```bash
./make.sh build
```

### Running the dev server

You can browse the examples by running the dev server:

```bash
./make.sh dev
```

Visit `http://localhost:3000` to see the example page.

## Contributing

Contributions are welcome!

## License

This project is licensed under the Mozilla Public License Version 2.0.
