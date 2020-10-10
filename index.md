# Ellx tutorial

For a complete reference check out [Ellx Documentation](https://docs.ellx.app).
Don't hesitate to ask us a question on [Ellx discord channel](https://discord.gg/K4cQMaQ).

You can also pull inspiration from [other users' projects](/explore).

Let's get into it.

## Projects

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
- define reactive logic to define the data processing flow
- `require` dynamic dependencies

It is also a sort of "back-of-the-napkin" calculation space - a "checkered paper" sheet where you can explore your data in a convenient tabular form.

Press <kbd>Alt+2</kbd> now and see for yourself.

### Layouts

## Reactivity
Ellx is a reactive programming environment for building rich interactive documents and applications. It is similar to notebooks like Jupyter and Observable since they all allow for exploratory programming, i.e. executing and visualizing code as you type it. It also provides tabular spreadsheet view which helps to get insight from data, very much like Excel or Google spreadsheets.


## Variable declaration

Ellx applications are reactive calculation graphs, very much like Excel or other spreadsheets, which in turn makes variables _spreasheet nodes._ By convention each node must have a name followed by assignment expression. Hence dogmatic "hello world" program looks like this:

```
{ name = 'world' }

Hello { name }!
```

> { name = 'world' }

> Hello { name }!

You can see the output of the program inside the preview on the right. Curly braces denote Ellx expression, everything else in this file is plain markdown.



And just like cells Excel which can derive values from other cells and update reactively, Ellx nodes can do the same!

> { uppercaseName = name.toUpperCase() }

```
{ uppercaseName = name.toUpperCase() }
```

should render "WORLD" but try changing `name` on line 21 and watch magic happen:

> Hello uppercase { uppercaseName }!



## Browser runtime environment

Note that `toUpperCase` is a [method](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/toUpperCase) of JavaScript String type. Ellx nodes are JavaScript expressions executed right inside your browser so you are free to use and combine any browser APIs and see the results in an instant!

Let's fetch our current IP address using browser [fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API).

```
{ ip = fetch('https://ipinfo.io/ip').text().split('.') }
```

> $ip=$ { ip = fetch('https://ipinfo.io/ip').text().split('.') }

Let $x$ be the first byte of the address and $y$ the second byte.

> $x=$ { x = ip[0] }

> $y=$ { y = ip[1] }

Then hypotenuse of the triangle with catheti $x$ and $y$ equals

> $\sqrt{x^2+y^2}=$ { Math.sqrt(x*x + y*y) }

This example is quite useless although now we have all tools required to fetch and process the space launch API data, not visualize yet. But first, let's take a look at Ellx project structure.

## Projects


 Spreadsheet and JavaScript module files contain application logic and layout contains representation layer.

Anything exported from `index.js` is available in `index.ellx` and `index.js`. Any node declared in `index.ellx` is available in `index.md`. This is all to Ellx declaration precedence.

This variable is declared, but not exported from `index.js` (please export it so that the error goes):
```
{ justAVar }
```

> { justAVar }


This node is declared (guess what) in `index.ellx`:
```
{ nodeFromIndexDotEllx }
```

> { nodeFromIndexDotEllx }

Let's take a closer look at each file type.

### Layouts

<!-- Styles in MD -->
<style>
  blockquote {
    font-family: monospace;
  }
</style>

> To return launches between August 20th, 2015 and September 20th, 2015:
> https://launchlibrary.net/1.4/launch/2015-08-20/2015-09-20

```
data = fetch("https://launchlibrary.net/1.4/launch/2015-08-20/2015-09-20").json().launches
```

#### Note on order of precedence

```
 pretty(data)
```
{ pretty(data) }

### Sheets

Navigate to sheet

Components intro


