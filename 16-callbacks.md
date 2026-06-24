# JavaScript Callbacks

## Why This Topic Matters

Callbacks are one of the core building blocks of JavaScript.

You'll see them in:

- Array methods
- Event listeners
- Timers
- API calls
- Promises
- Node.js

A callback is simply:

```text
A function passed to another function
to be executed later.
```

Nothing magical. It's just a function being passed around. :contentReference[oaicite:1]{index=1}

---

# What is a Callback?

A callback is:

```text
A function passed as an argument
to another function.
```

Example:

```js
function greet(name) {
  console.log(`Hello ${name}`);
}

function processUser(callback) {
  const name = "Akio";

  callback(name);
}

processUser(greet);
```

Output:

```js
Hello Akio
```

---

# Why Is It Called a Callback?

Think:

```text
"Call me back later"
```

You give a function to another function.

That function decides:

```text
When to execute it
Whether to execute it
How many times to execute it
```

---

# Callback vs Normal Function

Normal function:

```js
function greet() {
  console.log("Hello");
}

greet();
```

You call it yourself.

---

Callback:

```js
processUser(greet);
```

Another function calls it.

---

# Functions Are First-Class Citizens

JavaScript allows:

```js
Functions stored in variables
Functions passed as arguments
Functions returned from functions
```

Example:

```js
const sayHi = function() {
  console.log("Hi");
};
```

Because functions are values, callbacks are possible.

---

# Callback Execution Flow

```js
function main(callback) {
  console.log("Main");

  callback();
}

function done() {
  console.log("Done");
}

main(done);
```

Output:

```js
Main
Done
```

Flow:

```text
main()
 ↓
callback()
```

---

# Anonymous Callbacks

You don't need a named function.

---

Instead of:

```js
function greet() {
  console.log("Hello");
}

main(greet);
```

You can write:

```js
main(function() {
  console.log("Hello");
});
```

---

# Arrow Function Callbacks

Modern JavaScript:

```js
main(() => {
  console.log("Hello");
});
```

Most callbacks today use arrow functions.

---

# Synchronous Callbacks

A callback can execute immediately.

Example:

```js
function calculate(a, b, callback) {
  return callback(a, b);
}

function add(x, y) {
  return x + y;
}

console.log(
  calculate(5, 3, add)
);
```

Output:

```js
8
```

This is a:

```text
Synchronous Callback
```

because it executes immediately. :contentReference[oaicite:2]{index=2}

---

# Array Methods Use Callbacks

Example:

```js
const numbers =
  [1, 2, 3];
```

---

forEach:

```js
numbers.forEach(function(num) {
  console.log(num);
});
```

Output:

```js
1
2
3
```

---

map:

```js
const doubled =
  numbers.map(num => num * 2);
```

Output:

```js
[2, 4, 6]
```

---

filter:

```js
const even =
  numbers.filter(
    num => num % 2 === 0
  );
```

Output:

```js
[2]
```

---

# Higher-Order Functions

A function that accepts another function is called a:

```text
Higher-Order Function
```

Example:

```js
numbers.map(callback);
```

---

Relationship:

```text
Higher-Order Function
       ↑
 accepts callback
```

Callbacks and higher-order functions are two sides of the same concept. :contentReference[oaicite:3]{index=3}

---

# Asynchronous Callbacks

Callbacks become more interesting when they run later.

---

Example:

```js
console.log("Start");

setTimeout(() => {
  console.log("Timer");
}, 2000);

console.log("End");
```

Output:

```js
Start
End
Timer
```

The callback executes later. :contentReference[oaicite:4]{index=4}

---

# Why Does This Happen?

Because:

```text
setTimeout
```

uses:

```text
Web APIs
Event Loop
Callback Queue
```

The callback waits until:

```text
1. Timer finishes
2. Call Stack becomes empty
```

---

# Event Listener Callbacks

One of the most common callback examples.

```js
button.addEventListener(
  "click",
  function() {
    console.log("Clicked");
  }
);
```

The callback executes only when:

```text
User clicks button
```

:contentReference[oaicite:5]{index=5}

---

# Callback with Parameters

```js
function process(callback) {
  callback("Akio");
}

process(function(name) {
  console.log(name);
});
```

Output:

```js
Akio
```

---

# Multiple Callbacks

Example:

```js
function calculator(
  a,
  b,
  operation
) {
  return operation(a, b);
}
```

---

Addition:

```js
calculator(
  5,
  3,
  (a, b) => a + b
);
```

---

Multiplication:

```js
calculator(
  5,
  3,
  (a, b) => a * b
);
```

Same function.

Different behavior.

---

# Callback Pattern

```js
function doTask(callback) {
  console.log("Task started");

  callback();

  console.log("Task finished");
}
```

Output:

```js
Task started
Callback
Task finished
```

---

# Error-First Callback Pattern

Very common in Node.js.

Pattern:

```js
callback(error, result)
```

---

Example:

```js
function getUser(callback) {
  const error = null;

  const user = {
    name: "Akio"
  };

  callback(error, user);
}
```

Usage:

```js
getUser((err, user) => {
  if (err) {
    console.error(err);
    return;
  }

  console.log(user);
});
```

Always check error first. :contentReference[oaicite:6]{index=6}

---

# Callback Hell

The biggest problem with callbacks.

---

Example:

```js
getUser(user => {
  getOrders(user, orders => {
    getPayment(
      orders,
      payment => {
        processPayment(
          payment,
          result => {
            console.log(result);
          }
        );
      }
    );
  });
});
```

---

Shape:

```text
getUser
 └─ getOrders
      └─ getPayment
           └─ processPayment
```

Looks like a pyramid.

---

Problems:

```text
Hard to read
Hard to debug
Hard to maintain
```

This is called:

```text
Callback Hell
```

:contentReference[oaicite:7]{index=7}

---

# Pyramid of Doom

Another name for:

```text
Callback Hell
```

Visual:

```text
callback(
  callback(
    callback(
      callback()
    )
  )
)
```

---

# Solution: Promises

Instead of:

```js
getUser(user => {
  getOrders(user, orders => {});
});
```

Use:

```js
getUser()
  .then(getOrders)
  .then(processPayment);
```

Cleaner.

---

# Solution: Async/Await

Even cleaner.

```js
const user =
  await getUser();

const orders =
  await getOrders(user);
```

This is why Promises and Async/Await were introduced.

---

# Closures and Callbacks

Callbacks often create closures.

---

Example:

```js
function outer() {
  let count = 0;

  return function() {
    count++;

    console.log(count);
  };
}
```

Returned callback remembers:

```js
count
```

through closure.

---

# Real React Example

```js
<button
  onClick={() => {
    console.log("Clicked");
  }}
>
```

The function passed to:

```js
onClick
```

is a callback.

React heavily relies on callbacks.

---

# Common Callback APIs

## Timers

```js
setTimeout(callback);
setInterval(callback);
```

---

## Events

```js
addEventListener(
  "click",
  callback
);
```

---

## Arrays

```js
map(callback)
filter(callback)
forEach(callback)
reduce(callback)
```

---

## Promises

```js
promise.then(callback);
```

Even Promise handlers are callbacks.

---

# Common Interview Questions

## What is a Callback?

A function passed to another function to be executed later.

---

## Why Are Callbacks Useful?

They allow:

```text
Flexible behavior
Code reuse
Async programming
```

---

## Difference Between Synchronous and Asynchronous Callbacks?

Synchronous:

```js
forEach
map
filter
```

Run immediately.

---

Asynchronous:

```js
setTimeout
fetch
event listeners
```

Run later.

---

## What is Callback Hell?

Deeply nested callbacks that become difficult to read and maintain.

---

## How Do Promises Solve Callback Hell?

By flattening nested callback structures into chains.

---

## What is an Error-First Callback?

A callback whose first parameter is an error.

Example:

```js
callback(error, result);
```

---

# Quick Revision

✅ Callback = function passed to another function

✅ Functions are first-class citizens

✅ Higher-order functions accept callbacks

✅ Callbacks can be named or anonymous

✅ Array methods use callbacks

✅ Event listeners use callbacks

✅ setTimeout uses callbacks

✅ Callbacks can be synchronous

✅ Callbacks can be asynchronous

✅ Error-first callbacks are common in Node.js

✅ Nested callbacks create Callback Hell

✅ Promises solve Callback Hell

✅ Async/Await simplifies async callbacks

✅ React uses callbacks heavily