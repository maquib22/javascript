# JavaScript IIFE & Modules

## Why This Topic Matters

Before ES6 introduced modules (`import` / `export`), JavaScript developers used:

- IIFEs
- Module Pattern

to achieve:

- Encapsulation
- Data Privacy
- Namespace Management
- Avoiding Global Variables

Modern JavaScript modules are built on similar ideas.

---

# What is an IIFE?

IIFE stands for:

```text
Immediately Invoked Function Expression
```

It is a function that:

1. Is created immediately
2. Executes immediately

Syntax:

```js
(function () {
  console.log("I run immediately");
})();
```

Output:

```js
I run immediately
```

---

# Why Do We Need IIFEs?

Without IIFEs:

```js
var name = "Akio";

function greet() {
  console.log(name);
}
```

Variables become global.

Too many globals create:

- Naming collisions
- Memory issues
- Hard-to-maintain code

IIFEs solve this by creating a private scope.

---

# Basic IIFE Syntax

## Traditional Syntax

```js
(function () {
  console.log("Hello");
})();
```

---

## Alternative Syntax

```js
(function () {
  console.log("Hello");
}());
```

Both are valid.

---

# Why Parentheses?

This fails:

```js
function () {
  console.log("Hello");
}();
```

Because JavaScript expects:

```text
Function Declaration
```

Wrapping with parentheses converts it into a:

```text
Function Expression
```

which can be invoked immediately.

---

# Named IIFE

```js
(function greet() {
  console.log("Hello");
})();
```

Output:

```js
Hello
```

The name is only available inside the function itself.

Useful for recursion.

---

# IIFE with Parameters

```js
(function(name) {
  console.log(`Hello ${name}`);
})("Akio");
```

Output:

```js
Hello Akio
```

---

# Arrow Function IIFE

Modern syntax:

```js
(() => {
  console.log("Hello");
})();
```

---

# Creating Private Variables

One major use of IIFEs:

```js
const counter = (function() {
  let count = 0;

  return {
    increment() {
      count++;
    },

    getCount() {
      return count;
    }
  };
})();

counter.increment();

console.log(counter.getCount());
```

Output:

```js
1
```

Direct access:

```js
counter.count
```

Output:

```js
undefined
```

---

# How Does This Work?

This combines:

```text
IIFE
+
Closure
```

The variable:

```js
count
```

remains alive because returned methods form closures over it.

---

# Module Pattern

The Module Pattern uses:

```text
IIFE + Closures
```

to create private and public members.

---

# Basic Module Pattern

```js
const UserModule = (function() {
  let users = [];

  function addUser(user) {
    users.push(user);
  }

  function getUsers() {
    return users;
  }

  return {
    addUser,
    getUsers
  };
})();
```

Usage:

```js
UserModule.addUser("Akio");

console.log(UserModule.getUsers());
```

Output:

```js
["Akio"]
```

---

# Private vs Public Members

Private:

```js
users
addUser()
```

until explicitly returned.

Public:

```js
return {
  addUser,
  getUsers
}
```

Only returned members are accessible outside.

---

# Revealing Module Pattern

A variation of the Module Pattern.

---

```js
const Calculator = (function() {
  function add(a, b) {
    return a + b;
  }

  function subtract(a, b) {
    return a - b;
  }

  return {
    add,
    subtract
  };
})();
```

Usage:

```js
Calculator.add(2, 3);
```

Output:

```js
5
```

The implementation stays hidden.

---

# Namespace Problem

Without modules:

```js
var utils = {};
var config = {};
var user = {};
```

Large applications create many globals.

Two libraries may define:

```js
var user;
```

leading to collisions.

---

# Using IIFE as Namespace

```js
const App = (function() {
  const version = "1.0";

  function start() {
    console.log("App started");
  }

  return {
    start
  };
})();
```

Usage:

```js
App.start();
```

Private:

```js
version
```

Public:

```js
start()
```

---

# Real World Example

Suppose two scripts exist:

### File A

```js
var counter = 1;
```

### File B

```js
var counter = 10;
```

Problem:

```text
Global variable collision
```

Using IIFE:

```js
(function() {
  var counter = 1;
})();
```

Variables stay isolated.

---

# ES6 Modules

Modern JavaScript introduced:

```js
export
import
```

Example:

### math.js

```js
export function add(a, b) {
  return a + b;
}
```

---

### app.js

```js
import { add } from "./math.js";

console.log(add(2, 3));
```

Output:

```js
5
```

ES6 Modules largely replaced the Module Pattern.

---

# Why ES6 Modules Are Better

Advantages:

- Static analysis
- Tree shaking
- Better tooling
- Dependency management
- Native browser support

---

# Module Scope

Every ES6 module has its own scope.

Variables are not automatically global.

Example:

```js
const secret = 42;
```

Accessible only inside the module unless exported.

---

# IIFE vs ES6 Modules

| Feature | IIFE | ES6 Modules |
|---------|------|-------------|
| Private Scope | Yes | Yes |
| Imports | Manual | Native |
| Exports | Manual | Native |
| Dependency Management | Difficult | Easy |
| Tree Shaking | No | Yes |
| Modern Standard | No | Yes |

---

# Relationship with Closures

IIFEs rely heavily on closures.

Example:

```js
const counter = (function() {
  let count = 0;

  return function() {
    return ++count;
  };
})();
```

The returned function remembers:

```js
count
```

through closure.

---

# Common Interview Questions

## What is an IIFE?

A function expression that executes immediately after creation.

---

## Why use IIFEs?

To:

- Avoid global pollution
- Create private variables
- Encapsulate code

---

## Why are parentheses needed?

To convert a function declaration into a function expression.

---

## How do IIFEs create private state?

By combining:

```text
IIFE + Closure
```

---

## What is the Module Pattern?

A design pattern that uses IIFEs and closures to create public and private members.

---

## Are IIFEs still used today?

Less frequently.

ES6 Modules have replaced most use cases.

However, understanding IIFEs is important for:

- Legacy codebases
- Interviews
- Library internals

---

## Difference Between Module Pattern and ES6 Modules?

ES6 Modules provide native support for:

```text
import/export
```

while Module Pattern uses:

```text
IIFE + Closure
```

---

# Common Mistakes

## Mistake 1

```js
function() {
  console.log("Hello");
}();
```

Invalid syntax.

---

Correct:

```js
(function() {
  console.log("Hello");
})();
```

---

## Mistake 2

Assuming private variables are accessible:

```js
module.count
```

Private members remain hidden unless returned.

---

# Quick Revision

✅ IIFE = Immediately Invoked Function Expression

✅ Executes immediately after creation

✅ Creates isolated scope

✅ Prevents global namespace pollution

✅ Uses closures for private variables

✅ Module Pattern = IIFE + Closures

✅ Public members are returned

✅ Private members remain hidden

✅ ES6 Modules replaced most IIFE use cases

✅ Every ES6 module has its own scope

✅ `import` and `export` are modern module systems