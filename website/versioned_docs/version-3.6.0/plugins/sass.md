---
id: version-3.6.0-sass-plugin
title: SassPlugin
original_id: sass-plugin
---

## Description

Allows using Sass, A professional grade CSS extension language.

## Install

```bash
yarn add node-sass --dev
// OR
npm install node-sass --save-dev
```

## Usage

check [Sass website](http://sass-lang.com/) for more information. note: The Sass
plugin generates CSS, Therefor it must be chained prior to the CSSPlugin to be
used.

### Setup

Import from FuseBox

```js
const { SassPlugin } = require("fuse-box");
```

Inject into a chain.

```js
fuse.plugin([SassPlugin(), CSSPlugin()]);
```

Or add it to the main config plugins list to make it available across bundles.

```js
FuseBox.init({
  plugins: [[SassPlugin(), CSSPlugin()]],
});
```

### Require file in your code

```js
import "./styles/main.scss";
```

## Options

`SassPlugin` accepts a `key/value` `Sass` object options as a parameter. For
example:

```js
fuse.plugin(
  SassPlugin({
    outputStyle: "compressed",
  }),
  CSSPlugin(),
);
```

In order to use Sass indented syntax set `indentedSyntax` option:

```js
fuse.plugin(
  SassPlugin({
    indentedSyntax: true,
  }),
  CSSPlugin(),
);
```

In order to use custom functions set `functions` option. See
[node-sass documentation](https://github.com/sass/node-sass) for more info.

```js
fuse.plugin(
  SassPlugin({
    functions: {
      "torem($size)": function(size) {
        size.setUnit("rem");
        return size;
      },
    },
  }),
  CSSPlugin(),
);
```

## Resource file

To avoid setting resource file (which could contain variable and other shared
resources) on each file, it's possible to define it in the config.

```js
plugins: [
  [
    SassPlugin({
      resources: [{ test: /.*/, file: "resources.scss" }],
    }),
    CSSPlugin(),
  ],
];
```

In the example above, all the files in the project, including node_modules will
share the same resource file which in our case located next to `fuse.js`. It
would also understand an absolute path.

For example, having an SCSS file with the contents:

`project/src/index.scss`

```scss
body {
  background-color: $color1;
}
```

and a resource file in `project/resources.scss`

```scss
$color1: orange;
```

Will result in the following output:

```scss
@import "../resources.scss";
body {
  background-color: $color1;
}
```

As you can see here, the resource file is resolved automatically relatively to
the file's original location.

## Macros

Macros is a unique feature available only in `FuseBox` to give you more
flexibility on how to define paths for importing files in `SASS` . To enable
macros add:

```js
SassPlugin({ importer: true });
```

By default, you have 3 macros available:

That same as [home directory](#home-directory)

```css
@import "$homeDir/test2.scss";
```

Your application root.

```css
@import "$appRoot/src/test.scss";
```

`Tilde` that points to `node_modules`

```css
@import "~bootstrap/dist/bootstrap.css";
```

You can override any of these by providing a key:

```js
plugins: [
  [
    SassPlugin({ importer: true, macros: { $homeDir: "custom/dir/" } }),
    CSSPlugin(),
  ],
];
```
