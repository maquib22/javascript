# JavaScript Scope & Closures

## Why This Topic Matters

Scope and Closures are among the most frequently asked JavaScript interview topics.

They are used in:

- React Hooks
- Event Handlers
- Callbacks
- Async Programming
- Data Privacy
- Module Pattern
- Memoization

---

# What is Scope?

Scope determines:

```text
Where a variable can be accessed from.
```

Think of scope as the visibility region of a variable.

Example:

```js
const name = "Akio";

function greet() {
  console.log(name);
}

greet();
```

Output:

```js
Akio
```

Because `name` is inside the accessible scope.

---

# Types of Scope in JavaScript

JavaScript has:

1. Global Scope
2. Function Scope
3. Block Scope
4. Lexical Scope

---

# Global Scope

Variables declared outside any function or block belong to the global scope.

```js
const appName = "My App";

function showName() {
  console.log(appName);
}
```

Both global code and functions can access it.

---

# Problems with Global Variables

Global variables can:

- Be modified accidentally
- Cause naming collisions
- Increase coupling

Bad:

```js
let counter = 0;
```

Good:

```js
function createCounter() {
  let counter = 0;
}
```

Keep variables as local as possible.

---

# Function Scope

Variables declared with:

```js
var
```

are function-scoped.

Example:

```js
function test() {
  var x = 10;

  console.log(x);
}

test();
```

Output:

```js
10
```

---

Outside:

```js
console.log(x);
```

Output:

```text
ReferenceError
```

Because `x` exists only inside the function.

---

# Block Scope

Variables declared with:

```js
let
const
```

are block-scoped.

Example:

```js
if (true) {
  let age = 25;
}

console.log(age);
```

Output:

```text
ReferenceError
```

Because `age` only exists inside the block.

---

# var vs let vs const

```js
if (true) {
  var a = 1;
  let b = 2;
  const c = 3;
}

console.log(a); // 1
console.log(b); // Error
console.log(c); // Error
```

---

# Lexical Scope

JavaScript uses:

```text
Lexical Scope
```

Meaning:

A function's scope is determined by where it is written, not where it is called.

---

Example:

```js
const message = "Hello";

function outer() {
  function inner() {
    console.log(message);
  }

  inner();
}

outer();
```

Output:

```js
Hello
```

`inner()` can access variables from its surrounding environment.

---

# Scope Chain

When JavaScript looks for a variable:

1. Current scope
2. Parent scope
3. Parent's parent
4. Global scope

If not found:

```text
ReferenceError
```

---

Example:

```js
const a = 1;

function first() {
  const b = 2;

  function second() {
    const c = 3;

    console.log(a, b, c);
  }

  second();
}

first();
```

Output:

```js
1 2 3
```

Variable lookup:

```text
c -> second
b -> first
a -> global
```

This process is called:

```text
Scope Chain
```

---

# What is a Closure?

A closure is:

```text
A function that remembers variables
from its lexical scope even after
the outer function has finished executing.
```

This is one of JavaScript's most powerful features.

---

# Basic Closure Example

```js
function outer() {
  const name = "Akio";

  function inner() {
    console.log(name);
  }

  return inner;
}

const fn = outer();

fn();
```

Output:

```js
Akio
```

---

Question:

```text
How can inner() still access name?
```

Because of:

```text
Closure
```

The function remembers its lexical environment.

---

# Visualization

```text
outer()
 ├── name = "Akio"
 └── returns inner()

inner()
 └── still has access to name
```

Even after:

```js
outer()
```

has completed.

---

# Closure in Action: Counter

```js
function createCounter() {
  let count = 0;

  return function() {
    count++;
    return count;
  };
}

const counter = createCounter();

console.log(counter());
console.log(counter());
console.log(counter());
```

Output:

```js
1
2
3
```

The variable:

```js
count
```

persists because of closure.

---

# Data Privacy with Closures

Closures can create private variables.

```js
function createBankAccount() {
  let balance = 0;

  return {
    deposit(amount) {
      balance += amount;
    },

    getBalance() {
      return balance;
    }
  };
}

const account = createBankAccount();

account.deposit(100);

console.log(account.getBalance());
```

Output:

```js
100
```

Direct access:

```js
account.balance
```

Output:

```js
undefined
```

Private state achieved.

---

# Common Interview Question

## Loop + Closure Problem

Using `var`

```js
for (var i = 0; i < 3; i++) {
  setTimeout(() => {
    console.log(i);
  }, 1000);
}
```

Output:

```js
3
3
3
```

Why?

Because all callbacks share the same variable:

```js
i
```

After the loop ends:

```js
i = 3
```

---

# Fix Using let

```js
for (let i = 0; i < 3; i++) {
  setTimeout(() => {
    console.log(i);
  }, 1000);
}
```

Output:

```js
0
1
2
```

Because `let` creates a new binding for each iteration.

---

# Fix Using Closure

```js
for (var i = 0; i < 3; i++) {
  (function(i) {
    setTimeout(() => {
      console.log(i);
    }, 1000);
  })(i);
}
```

Output:

```js
0
1
2
```

The IIFE creates a closure around each value of `i`.

---

# Real World Example: React

```js
function App() {
  const count = 5;

  function handleClick() {
    console.log(count);
  }

  return (
    <button onClick={handleClick}>
      Click
    </button>
  );
}
```

`handleClick()` remembers:

```js
count
```

through closure.

React relies heavily on closures.

---

# Closures in Hooks

Example:

```js
useEffect(() => {
  console.log(count);
}, []);
```

A closure captures:

```js
count
```

This is why stale closures can happen in React.

---

# Stale Closure Example

```js
function createLogger() {
  let message = "Hello";

  return function() {
    console.log(message);
  };
}

const log = createLogger();

message = "Hi";
log();
```

The closure keeps the value from its lexical environment.

Understanding stale closures is important for React interviews.

---

# Memory Considerations

Closures keep references alive.

If large objects are captured:

```js
function hugeData() {
  const bigArray = new Array(1000000);

  return function() {
    console.log(bigArray.length);
  };
}
```

The array remains in memory while the closure exists.

Improper use can cause memory leaks.

---

# Common Uses of Closures

- Memoization
- Currying
- Module Pattern
- Event Handlers
- Debouncing
- Throttling
- React Hooks

---

# Interview Questions

## What is Scope?

The accessibility region of variables.

---

## What is Lexical Scope?

Scope determined by where code is written.

---

## What is a Closure?

A function that remembers its lexical environment after the outer function finishes.

---

## Why are Closures useful?

They enable:

- Data privacy
- State persistence
- Callbacks
- React Hooks

---

## Why does the loop with var print 3 3 3?

Because all callbacks share the same variable reference.

---

## How does let fix the issue?

It creates a new binding for every iteration.

---

# Quick Revision

✅ Scope controls variable visibility

✅ JavaScript uses lexical scope

✅ Variable lookup follows the scope chain

✅ `var` is function-scoped

✅ `let` and `const` are block-scoped

✅ Closures remember outer variables

✅ Closures preserve state

✅ Closures enable private variables

✅ React heavily relies on closures

✅ `var` in loops can cause bugs

✅ `let` creates separate bindings