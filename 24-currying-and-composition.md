# JavaScript Currying & Composition

## Why This Topic Matters

As applications grow, we want functions that are:

- Small
- Reusable
- Predictable
- Easy to combine

Functional Programming achieves this using:

```text
Currying

↓

Composition
```

Currying creates small reusable functions.

Composition combines small functions into larger ones.

Together they help build clean, modular code. :contentReference[oaicite:1]{index=1}

---

# What is Currying?

Currying is a transformation that converts:

```text
One function

with multiple arguments

↓

into

↓

A sequence of functions

each taking ONE argument.
```

---

# Normal Function

```js
function add(a, b, c) {
  return a + b + c;
}

add(1, 2, 3);
```

Output:

```js
6
```

---

# Curried Version

```js
function add(a) {

  return function(b) {

    return function(c) {

      return a + b + c;

    };

  };

}
```

Usage:

```js
add(1)(2)(3);
```

Output:

```js
6
```

---

# Arrow Function Version

Much cleaner.

```js
const add =
  a =>
  b =>
  c =>
    a + b + c;
```

Usage:

```js
add(1)(2)(3);
```

Output:

```js
6
```

---

# How Currying Works

Step 1

```js
add(1)
```

Returns:

```js
function(b) {}
```

---

Step 2

```js
add(1)(2)
```

Returns:

```js
function(c) {}
```

---

Step 3

```js
add(1)(2)(3)
```

Returns:

```js
6
```

Each function remembers previous arguments using:

```text
Closures
```

Currying depends entirely on closures. :contentReference[oaicite:2]{index=2}

---

# Visualizing Currying

```text
add

↓

add(1)

↓

function waiting for b

↓

add(1)(2)

↓

function waiting for c

↓

add(1)(2)(3)

↓

6
```

---

# Why Use Currying?

Imagine:

```js
function multiply(a, b) {

  return a * b;

}
```

Without currying:

```js
multiply(2, 10);
multiply(2, 20);
multiply(2, 30);
```

Repeated argument.

---

With currying:

```js
const multiply =
  a =>
  b =>
    a * b;
```

Create:

```js
const double =
  multiply(2);
```

Now:

```js
double(10);
double(20);
double(30);
```

Output:

```js
20
40
60
```

Very reusable.

---

# Currying Uses Closures

Example:

```js
const multiply =
  a =>
  b =>
    a * b;
```

When:

```js
const double =
  multiply(2);
```

The returned function remembers:

```js
a = 2
```

through closure.

---

# Partial Application

Often confused with currying.

They are NOT the same. :contentReference[oaicite:3]{index=3}

---

# What is Partial Application?

Partial application fixes some arguments of a function and returns a new function for the remaining arguments.

Example:

```js
function add(a, b, c) {

  return a + b + c;

}
```

Partial:

```js
const add5 =
  add.bind(null, 5);
```

Later:

```js
add5(2, 3);
```

Output:

```js
10
```

---

# Currying vs Partial Application

Currying:

```text
Always

↓

One argument

↓

One function
```

Example:

```js
add(1)(2)(3)
```

---

Partial Application:

```text
Fixes

Some arguments

Remaining arguments later
```

Example:

```js
add5(2, 3)
```

---

Comparison:

| Currying | Partial Application |
|----------|---------------------|
| One argument per function | Can fix multiple arguments |
| Produces unary functions | Produces smaller-arity functions |
| Transformation | Argument binding |

---

# Building a curry()

Simple implementation:

```js
function curry(fn) {

  return function curried(...args) {

    if (
      args.length >= fn.length
    ) {

      return fn(...args);

    }

    return (...nextArgs) =>

      curried(
        ...args,
        ...nextArgs
      );

  };

}
```

Usage:

```js
function add(a, b, c) {

  return a + b + c;

}

const curriedAdd =
  curry(add);

curriedAdd(1)(2)(3);
```

Output:

```js
6
```

---

# What is Function Composition?

Composition combines multiple functions into one.

Think:

```text
Output of one

↓

Input of next
```

Instead of writing:

```js
h(g(f(x)))
```

we build one composed function.

---

# Example Functions

```js
const add2 =
  x => x + 2;

const multiply3 =
  x => x * 3;
```

---

Without Composition

```js
multiply3(
  add2(5)
);
```

Output:

```js
21
```

---

# compose()

Mathematical order:

```text
Right

↓

Left
```

Formula:

```text
compose(f, g)

=

f(g(x))
```

---

Implementation:

```js
function compose(...fns) {

  return value =>

    fns.reduceRight(

      (acc, fn) => fn(acc),

      value

    );

}
```

---

Usage:

```js
const transform =
  compose(
    multiply3,
    add2
  );

transform(5);
```

Output:

```js
21
```

Execution:

```text
5

↓

add2

↓

7

↓

multiply3

↓

21
```

---

# pipe()

Many developers find left-to-right easier to read. :contentReference[oaicite:4]{index=4}

Formula:

```text
pipe(f, g)

=

g(f(x))
```

---

Implementation:

```js
function pipe(...fns) {

  return value =>

    fns.reduce(

      (acc, fn) => fn(acc),

      value

    );

}
```

---

Usage:

```js
const transform =
  pipe(
    add2,
    multiply3
  );

transform(5);
```

Output:

```js
21
```

---

# compose vs pipe

compose:

```text
Right → Left
```

```js
compose(f, g)(x)

↓

f(g(x))
```

---

pipe:

```text
Left → Right
```

```js
pipe(f, g)(x)

↓

g(f(x))
```

Most developers prefer `pipe()` because it follows natural reading order. :contentReference[oaicite:5]{index=5}

---

# Real Example

Functions:

```js
const trim =
  str => str.trim();

const lower =
  str => str.toLowerCase();

const capitalize =
  str =>

    str[0].toUpperCase() +

    str.slice(1);
```

Compose:

```js
const format =
  pipe(
    trim,
    lower,
    capitalize
  );

format("  HELLO ");
```

Output:

```text
Hello
```

---

# Benefits of Composition

Instead of one huge function:

```text
Read Data

↓

Validate

↓

Transform

↓

Save

↓

Log
```

Break into:

```text
Small Functions

↓

Compose Together
```

Much easier to maintain.

---

# Point-Free Style

Sometimes composition avoids explicitly mentioning data.

Instead of:

```js
const double =
  x => x * 2;
```

You compose existing functions.

This style is called:

```text
Point-Free Programming
```

Use it carefully—too much point-free code can reduce readability. :contentReference[oaicite:6]{index=6}

---

# Real React Connection

Currying appears in:

```js
const handleClick =
  id => () => {

    console.log(id);

  };
```

Usage:

```jsx
<button
  onClick={handleClick(5)}
>
```

The first function captures:

```js
id
```

The second function executes later.

---

# Redux Example

Middleware often looks like:

```js
store =>

next =>

action => {

  next(action);

}
```

This is currying.

Each function accepts one argument.

---

# Lodash Example

```js
_.curry(fn)
```

Creates a curried version of any function.

Libraries like Lodash provide built-in currying utilities. :contentReference[oaicite:7]{index=7}

---

# Common Mistakes

## Mistake 1

Thinking currying and partial application are identical.

They are related but different.

---

## Mistake 2

Using currying for simple code.

Bad:

```js
add(1)(2)
```

when:

```js
add(1, 2)
```

is clearer.

Use currying only when it improves reuse.

---

## Mistake 3

Confusing compose order.

Remember:

```text
compose()

↓

Right

↓

Left
```

---

# Common Interview Questions

## What is Currying?

Transforming a multi-argument function into a sequence of single-argument functions.

---

## Why Does Currying Work?

Because closures remember previously supplied arguments.

---

## Difference Between Currying and Partial Application?

Currying:

```text
One argument
per function
```

Partial Application:

```text
Fixes some arguments,
returns a function for the rest
```

---

## What is Function Composition?

Combining multiple functions into one.

---

## Difference Between compose() and pipe()?

compose:

```text
Right → Left
```

pipe:

```text
Left → Right
```

---

## Why Use Composition?

It builds complex behavior from small reusable functions.

---

# Quick Revision

✅ Currying transforms multi-argument functions into unary functions

✅ Curried functions use closures

✅ `add(1)(2)(3)` is currying

✅ Currying and partial application are different

✅ Partial application fixes some arguments

✅ Composition combines functions

✅ `compose()` executes right-to-left

✅ `pipe()` executes left-to-right

✅ Small functions are easier to test and reuse

✅ React event handlers often use currying

✅ Redux middleware is a real-world example of currying

✅ Composition promotes modular, maintainable code