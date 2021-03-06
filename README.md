

# Extended EJS

[![npm version](https://badge.fury.io/js/extended-ejs.svg)](https://www.npmjs.com/package/extended-ejs)
[![license](https://img.shields.io/github/license/SumiMakito/extended-ejs.svg)](LICENSE)
[![dependencies Status](https://david-dm.org/SumiMakito/extended-ejs/status.svg)](https://david-dm.org/SumiMakito/extended-ejs)
[![Known Vulnerabilities](https://snyk.io/test/github/SumiMakito/extended-ejs/badge.svg?targetFile=package.json)](https://snyk.io/test/github/SumiMakito/extended-ejs?targetFile=package.json)

Extended EJS works as a wrapper and pre-compiler for the well-known [EJS](https://github.com/mde/ejs) template, which aims to provide the missing support for blocks and the inheritance in EJS.

It's quite simple and **won't** place side effects on your original EJS templates.


## Install

Run `npm install extended-ejs` or `yarn add extended-ejs`.

## Examples

Here, assume we got two files. 

```
<!-- skeleton.ejs -->

<h1>[% @block heading %]</h1>

[% @block content %]
```

```
<!-- index.ejs -->

[% @extends "./skeleton.ejs" %]

[% @impl heading %]
Hello <span style="color: red">world</span>
[% @end %]

[% @impl content %]
<h3>My name is <%= name %>.</h3>
[% @end %]
```

Use `renderFile` to render a file and write the rendered result to an output file.

> Remember that Extended EJS is only works as a pre-compiler – that is, you can pass local variables to your template and Extended EJS renders your template exactly like what EJS does.

```
const { renderFile } = require('extended-ejs');

renderFile('index.ejs', { name: 'Makito' }, 'index.html');
```

The final `index.html` should be like...

```
<!-- index.html -->

<h1>Hello <span style="color: red">world</span></h1>

<h3>My name is Makito.</h3>
```

## Statements

A valid statement looks like `[% @ACTION PARAMETER %]`.

Extended EJS uses `[%` and `%]` to wrap a statement.

An action is always prefixed with an `@`.

Every statement will only have **one** or **zero** parameters.

> The parameter can be wrapped with `''` or  `""`. But you can also omit the quotation marks as long as the parameter **does not** contain any whitespaces.

## Extension

With `@extends`, you can extend a skeleton in your template.

**Usage:**

`[% @extend ./layout_to_extend.ejs %]`

> One template can only have **one** or **zero** @extends statement

## Block

With `@block`, you can define blocks in your template.

**Usage:**

- `[% @block aliasForBlock %]`

- `[% @block "wrap this if you want to play with spaces" %]`

> Every block definition **must** have a corresponding implementation, otherwise it will be considered to be an error. If a block in the extended template has no implementation and the strict mode is enabled, the pre-compiler will also throw an error.

## Implementation

With `@impl`, you can implement blocks that you've already defined in your template.

**Usage:**

Use `[% @impl aliasForBlock %]` to begin an implementation for a block.

**Don't** forget to end your implementation with a `[% @end %]`.

> Implementing the same block alias twice will be regarded as an error. An error will also be thrown if the extended template has already implemented the block alias that the one who extends attempts to implement when the strict mode is enabled.

## Compile

**Usage:**

```
compile(input, workDir?, fileRef?, strict? = false)
```

Pre-compile the string presented template and make it reusable.

**Example:**

```
const template = `<h1>[% @block content %]</h1>[% @impl content %]Hello <%= name %>[% @end %]`;
const compiled = compile(template, null, null);

console.log(compiled.render({ name: 'Makito' }));
// outputs: <h1>Hello Makito</h1>

console.log(compiled.render({ name: 'Makino' }));
// outputs: <h1>Hello Makino</h1>
```

## Render

#### Strings

**Usage:**

```
render(input, localVars, outputFile?, strict? = false)
```

#### Files

**Usage:**

```
renderFile(inputFile, localVars, outputFile?, strict? = false)
```

Both methods above return the rendered template as a string, with local variables rendered.

Rendered result will be written to  `outputFile` if it is not null.


## License

2018 &copy; Makito Yu

Licensed under the beloved MIT License.


