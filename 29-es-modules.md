# JavaScript ES Modules (ESM)

## Why ES Modules Matter

As applications grow, putting all JavaScript into one file becomes difficult.

Problems:

- Hard to maintain
- Naming conflicts
- Difficult to reuse code
- Large bundle sizes
- Poor organization

ES Modules solve these problems by allowing code to be split into reusable files with explicit imports and exports. They are JavaScript's official module system. :contentReference[oaicite:0]{index=0}

---

# What is an ES Module?

An ES Module is a JavaScript file that:

```text
Exports Code

↓

Imports Code

↓

Creates Reusable Modules
```

Instead of writing everything in one file, we split our application into smaller modules.

---

# Module Example

```
project/

├── math.js

├── user.js

└── app.js
```

Each file has one responsibility.

---

# Exporting Code

Example:

```js
// math.js

export const PI = 3.14159;

export function square(x) {

  return x * x;

}
```

Everything marked with:

```text
export
```

can be imported by another module.

---

# Importing Code

```js
// app.js

import {

  PI,

  square

} from "./math.js";

console.log(square(4));

console.log(PI);
```

Output:

```text
16

3.14159
```

---

# Named Exports

Named exports allow multiple values to be exported.

Example:

```js
export const name = "Akio";

export function greet() {}

export class User {}
```

Import:

```js
import {

  name,

  greet,

  User

} from "./user.js";
```

Named imports must use the exported names (unless aliased). :contentReference[oaicite:1]{index=1}

---

# Exporting Later

Instead of exporting immediately:

```js
const PI = 3.14;

function square(x) {

  return x * x;

}

export {

  PI,

  square

};
```

---

# Renaming Exports

```js
function helper() {}

export {

  helper as calculate

};
```

Import:

```js
import {

  calculate

} from "./math.js";
```

---

# Default Export

Each module may have **one** default export.

Example:

```js
export default function greet() {

  console.log("Hello");

}
```

Import:

```js
import greet

from "./greet.js";
```

Notice:

```text
No Curly Braces
```

---

# Default Export Can Have Any Name

```js
import sayHello

from "./greet.js";
```

Also valid:

```js
import hello

from "./greet.js";
```

The importing file chooses the name.

---

# Named vs Default Export

Named Export:

```js
export const PI = 3.14;
```

Import:

```js
import {

  PI

} from "./math.js";
```

---

Default Export:

```js
export default function() {}
```

Import:

```js
import greet

from "./greet.js";
```

Remember:

```text
Named Export

↓

Curly Braces

Default Export

↓

No Curly Braces
```

---

# Mixing Named and Default Exports

```js
export default function App() {}

export const version = "1.0";
```

Import:

```js
import App,

{

  version

}

from "./app.js";
```

---

# Import Everything

Sometimes you need the whole module.

```js
import * as math

from "./math.js";
```

Usage:

```js
math.square(4);

math.PI;
```

---

# Module Scope

Variables inside a module are private.

```js
const secret = "123";
```

Other files cannot access:

```js
secret
```

unless it is exported.

Modules prevent global pollution. :contentReference[oaicite:2]{index=2}

---

# Modules Execute Only Once

Even if multiple files import the same module:

```js
import "./config.js";

import "./config.js";
```

The module executes only one time.

Modules behave like:

```text
Singletons
```

---

# Live Bindings

Named exports are **live bindings**, not copied values.

```js
// counter.js

export let count = 0;

export function increment() {

  count++;

}
```

```js
// app.js

import {

  count,

  increment

}

from "./counter.js";

console.log(count);

increment();

console.log(count);
```

Output:

```text
0

1
```

Imported values stay connected to the original variable. :contentReference[oaicite:3]{index=3}

---

# Static Imports

Imports must appear at the top level.

Correct:

```js
import {

  add

}

from "./math.js";
```

Wrong:

```js
if (true) {

  import {

    add

  }

  from "./math.js";

}
```

Static imports allow build tools to analyze dependencies before execution.

---

# Dynamic Import

Sometimes modules should load only when needed.

```js
const math =

await import("./math.js");

console.log(

  math.square(5)

);
```

`import()` returns a:

```text
Promise
```

Dynamic imports are commonly used for:

- Lazy loading
- Code splitting
- Loading optional features :contentReference[oaicite:4]{index=4}

---

# Top-Level await

ES Modules support:

```js
const response =

await fetch("/config");

export const config =

await response.json();
```

`await` can be used directly at the top level of an ES module without wrapping it in an async function. :contentReference[oaicite:5]{index=5}

---

# Browser Support

In browsers:

```html
<script

type="module"

src="app.js">

</script>
```

Without:

```text
type="module"
```

the browser treats the file as a normal script.

---

# File Extensions

In browsers:

```js
import {

  add

}

from "./math.js";
```

The file extension is required.

Wrong:

```js
import {

  add

}

from "./math";
```

Modern Node.js ESM also requires file extensions. :contentReference[oaicite:6]{index=6}

---

# ES Modules Always Use Strict Mode

No need to write:

```js
"use strict";
```

Every ES Module automatically runs in:

```text
Strict Mode
```

---

# Top-Level this

Normal script:

```js
console.log(this);
```

Output (Browser):

```text
window
```

Module:

```js
console.log(this);
```

Output:

```text
undefined
```

---

# ES Modules vs CommonJS

| ES Modules | CommonJS |
|------------|----------|
| import / export | require() / module.exports |
| Static | Dynamic |
| Supports Tree Shaking | Doesn't support Tree Shaking |
| Asynchronous loading | Synchronous loading |
| Official Standard | Node.js legacy module system |

---

# Tree Shaking

Tree Shaking removes unused exports.

Example:

```js
// math.js

export function add() {}

export function subtract() {}

export function multiply() {}
```

If only:

```js
import {

  add

}

from "./math.js";
```

is used,

unused exports can be removed from the final bundle by bundlers because ES Modules are statically analyzable. :contentReference[oaicite:7]{index=7}

---

# Why Static Imports Matter

Because imports are known before execution:

```text
Bundler

↓

Analyzes Dependencies

↓

Removes Unused Code

↓

Smaller Bundle
```

This optimization is not generally possible with dynamic `require()`.

---

# Circular Dependencies

Bad:

```
A imports B

↓

B imports A
```

This can lead to:

```text
ReferenceError

or

Uninitialized Values
```

Avoid circular dependencies by restructuring modules or moving shared code into another module. :contentReference[oaicite:8]{index=8}

---

# Real React Example

```js
import React

from "react";

import UserCard

from "./UserCard";

import {

  fetchUsers

}

from "./api";
```

Modern React projects use ES Modules everywhere.

---

# Common Mistakes

## Mistake 1

Using braces with default imports.

Wrong:

```js
import {

  App

}

from "./App";
```

Correct:

```js
import App

from "./App";
```

---

## Mistake 2

Forgetting braces for named imports.

Wrong:

```js
import PI

from "./math.js";
```

Correct:

```js
import {

  PI

}

from "./math.js";
```

---

## Mistake 3

Trying to export multiple default values.

Wrong:

```js
export default A;

export default B;
```

Only one default export is allowed per module.

---

## Mistake 4

Trying to use static import inside conditions.

Wrong:

```js
if (condition) {

  import {

    helper

  }

  from "./helper.js";

}
```

Use:

```js
await import("./helper.js");
```

instead.

---

## Mistake 5

Forgetting `.js` in browser imports.

Wrong:

```js
import {

  add

}

from "./math";
```

Correct:

```js
import {

  add

}

from "./math.js";
```

---

# Common Interview Questions

## What is an ES Module?

A JavaScript file that exports and imports functionality using `export` and `import`.

---

## Difference Between Named and Default Export?

Named Export:

```text
Uses Curly Braces

Multiple Allowed
```

Default Export:

```text
No Curly Braces

Only One Allowed
```

---

## Can a Module Have Multiple Default Exports?

No.

Only one.

---

## Can a Module Have Multiple Named Exports?

Yes.

Unlimited.

---

## What is Tree Shaking?

Removing unused exports during bundling to reduce bundle size.

---

## Why Does Tree Shaking Work with ES Modules?

Because imports and exports are static and can be analyzed before execution.

---

## What are Live Bindings?

Imported values remain connected to the original exported variable instead of receiving a copy.

---

## Difference Between import and import()?

`import`

```text
Static

Top Level Only
```

`import()`

```text
Dynamic

Returns Promise

Can Be Used Anywhere
```

---

## Why Are ES Modules Better Than CommonJS?

They provide:

- Standard syntax
- Static analysis
- Tree shaking
- Native browser support
- Better tooling

---

# Quick Revision

✅ ES Modules are JavaScript's official module system

✅ Use `export` to expose functionality

✅ Use `import` to consume functionality

✅ Named exports require curly braces

✅ Default exports don't use curly braces

✅ Only one default export is allowed

✅ A module can have many named exports

✅ Modules have private scope

✅ Modules execute only once

✅ Named exports use live bindings

✅ Static imports enable tree shaking

✅ `import()` performs dynamic imports

✅ Dynamic imports return a Promise

✅ ES Modules always run in strict mode

✅ Top-level `this` is `undefined`

✅ Browser modules require `<script type="module">`

✅ Browser and Node.js ESM require file extensions

✅ React, Vite, and modern frontend projects use ES Modules
