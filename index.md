# Ellx tutorial

For a complete reference check out [Ellx Documentation](https://docs.ellx.app).
Don't hesitate to ask us a question on [Ellx discord channel](https://discord.gg/K4cQMaQ).

You can also pull inspiration from [other users' projects](/explore).

Let's get into it.

### Projects

Ellx project is a collection of files and folders you can see in the project explorer on the left. Click on `(…)` next to an item (or right-click the item) to see a list of avaliable actions. Also <kbd>Ctrl+Shift+P</kbd> opens a *command palette*.

Projects can contain files of any type, however the following filename extensions are treated specially:

- `.js :` JavaScript module
- `.ellx :` spreadsheet
- `.md :` markdown layout

To create a folder inside your project terminate a new item's name with a slash (`/`). Alternatively you can directly create a nested file by naming it, for example, `folder/nested.js`

### Scripts
Unlike classical notebooks, most of the code is gathered in plain javascript files, which are independently testable and easy to share between projects.

`.js` files are standard ES6 modules, which means you can `import` and `export` stuff.

To import a module from the same project, use relative or absolute paths (relative to the root of the project), e.g.
```
import foo from './folder/foo.js';
import { default as bar } from '../anotherFolder/bar.js';
import * as baz from '/fileAtProjectRoot.js';
```
To import a module from anothet project (including another user's project) add a tilde (`~`) in front of the file's full path, including user and project name, e.g.
```
import slider from '~ellx-hub/lib/components/Slider';
import { plot } from '~ellx-hub/plot';
```
Note that, like in NodeJS, Ellx tries to automatically append `.js` and `/index.js` to  resolve imports without extensions.

Out of the box you can also import `.svelte` (Svelte components), `.jsx` (React components), `.vue` (Vue components: *coming soon*), `.css`, `.json`, and `.glsl` (WebGL shaders) files, like with most modern web bundlers.

Keep an eye on [~ellx-hub](/ellx-hub) user: it is Ellx official open source collection of components and utilities.

Please, contribute! Open an issue if you find one. Submit a pull request if you have a fix or an improvement!

##### NPM modules
You can directly import any NPM module published in UMD or ESM format, e.g.
```
import * as tf from '@tensorflow/tfjs@2.5';
```
Ellx is currently using [JSDelivr](https://www.jsdelivr.com) CDN to serve NPM modules.

You can also `import` from arbitrary URLs, e.g.
```
import gauss_legendre from 'https://cdn.skypack.dev/gauss_legendre';
```
We actually encourage you to try using [Skypack](https://cdn.skypack.dev) for your NPM dependencies. It is currently less stable than JSDelivr, but serves the modules directly in ESM format, which allows Ellx to collect dependencies statically.

##### Exports
Files with the same name and extensions `.md`, `.ellx` and `.js` constitute a single _namespace_. Namespaces exist to share scope and separate concerns.

All symbols you export from a `.js` file are (obviously) available to be imported by other modules (including from other projects), but can also be used within the same namespace in formulas on the spreadsheet or in the markdown layout interpolations delimited with curly braces, e.g.

{ pretty(gauss_legendre(5)) }

**Hint**. You can switch between `js/ellx/md` files in the same namespace using <kbd>Alt+1/2/3</kbd> shortcuts. Also <kbd>Ctrl+P</kbd> lets you switch between recently opened files.

### Spreadsheets
Ellx spreadsheet is probably the most important part of the project. If the scipts are *"flesh and bones"* of your application, then the spreadsheets are its *"nervous system"*.

Use them to
- fetch your data
- implement reactive logic to define, visualize, and debug the data processing flow
- `require` dynamic dependencies

It is also a sort of "back-of-the-napkin" calculation space - a "checkered paper" sheet where you can explore your data in a convenient tabular form.

Press <kbd>Alt+2</kbd> now and see for yourself.

### Layouts
Layouts are the *"skin"* of your application. By convention, each Ellx project must have a `/index.md` file in its root (like this very file for instance).

`/index.md` will become the *entry point* of the published static website for your project.

Layouts are just classical markdown files enriched with *interpolations* - Ellx expressions delimited with curly braces, where you can reference any node or exported symbol in the same namespace.

You can even define new nodes by preceding the expression with the node name and an `=` sign.

For instance:

My IP address is { fetch('https://ipinfo.io/ip').text() }

Latest space launch is:
**{launches[launches.length - 1].name}** by
**{launches[launches.length - 1].lsp.name}** on
*{launches[launches.length - 1].net}*

{ x = slider({ value: 33 }) }
{ y = slider({ value: 56 }) }

> x = { x }

> y = { y }

> $\sqrt{x^2+y^2}=$ { Math.sqrt(x*x + y*y) }

### Components
You might have noticed how certain nodes are rendered as plain text while others as graphic components.

Components are a special Ellx nodes that can maintain internal state and provide custom render and clean-up hooks. You can easily wrap any existing React, Svelte, Vue or any other components, using [Ellx component API](https://docs.ellx.app/#component-api).

You can also embed any HTML markup directly into your layout, for example, styles.

<!-- Styles in MD -->
<style>
  blockquote {
    font-family: monospace;
  }
</style>

### Thank you
for your patience!

Have fun building awesome things with Ellx!

Please [let us know](mailto:support@ellxoft.com) how it goes.
