# JavaScript Event Loop

## Why This Topic Matters

JavaScript is:

```text
Single-threaded
```

Yet it can:

- Fetch APIs
- Handle timers
- Respond to user clicks
- Run asynchronous code

How?

Through the:

```text
Event Loop
```

The Event Loop coordinates asynchronous operations with the Call Stack.

---

# The Big Picture

JavaScript concurrency works using:

```text
1. Call Stack
2. Web APIs
3. Callback Queue
4. Microtask Queue
5. Event Loop
```

Together they make JavaScript non-blocking.

---

# JavaScript is Single-Threaded

JavaScript can execute only:

```text
One piece of code at a time.
```

Example:

```js
console.log("A");
console.log("B");
console.log("C");
```

Output:

```js
A
B
C
```

Execution is sequential.

---

# The Problem

Consider:

```js
console.log("Start");

setTimeout(() => {
  console.log("Timer");
}, 2000);

console.log("End");
```

Question:

```text
How can JavaScript wait 2 seconds
without freezing everything?
```

Answer:

```text
Web APIs + Event Loop
```

---

# The Components

## 1. Call Stack

Executes JavaScript code.

Example:

```js
function greet() {
  console.log("Hello");
}

greet();
```

Stack:

```text
greet()
Global
```

Functions are pushed and popped.

---

## 2. Web APIs

Provided by the browser (or Node.js runtime).

Examples:

```text
setTimeout
fetch
DOM events
addEventListener
```

These APIs run outside the JavaScript engine.

---

# setTimeout Example

```js
console.log("Start");

setTimeout(() => {
  console.log("Timer");
}, 0);

console.log("End");
```

Output:

```js
Start
End
Timer
```

Many beginners expect:

```js
Start
Timer
End
```

Wrong.

---

# Step-by-Step Execution

### Step 1

```js
console.log("Start");
```

Output:

```js
Start
```

---

### Step 2

```js
setTimeout(...)
```

The callback moves to:

```text
Web APIs
```

Timer starts.

---

### Step 3

```js
console.log("End");
```

Output:

```js
End
```

---

### Step 4

Timer completes.

Callback enters:

```text
Callback Queue
```

---

### Step 5

Event Loop checks:

```text
Is Call Stack empty?
```

If yes:

```text
Move callback to Call Stack
```

---

### Step 6

Callback executes.

Output:

```js
Timer
```

---

# Visualization

```text
Call Stack
    ↓
Web APIs
    ↓
Callback Queue
    ↓
Event Loop
    ↓
Call Stack
```

---

# Callback Queue (Task Queue)

Stores:

```text
setTimeout callbacks
DOM events
setInterval callbacks
```

Example:

```js
button.addEventListener("click", handler);
```

When clicked:

```text
handler
```

goes into the Callback Queue.

---

# Event Loop Algorithm

The Event Loop repeatedly does:

```text
while (true) {
  if (callStack.isEmpty()) {
    moveTaskToStack();
  }
}
```

Its job is simple:

```text
Keep JavaScript running.
```

---

# Microtask Queue

Higher priority than Callback Queue.

Contains:

```text
Promise callbacks
queueMicrotask()
MutationObserver
```

---

# Promise Example

```js
console.log("Start");

Promise.resolve().then(() => {
  console.log("Promise");
});

console.log("End");
```

Output:

```js
Start
End
Promise
```

The Promise callback enters:

```text
Microtask Queue
```

---

# Microtasks vs Macrotasks

## Microtasks

Examples:

```text
Promise.then
catch
finally
queueMicrotask
```

---

## Macrotasks

Examples:

```text
setTimeout
setInterval
DOM events
```

---

# Priority Rule

Event Loop always processes:

```text
Microtask Queue
BEFORE
Callback Queue
```

---

# Classic Interview Question

```js
console.log("A");

setTimeout(() => {
  console.log("B");
}, 0);

Promise.resolve().then(() => {
  console.log("C");
});

console.log("D");
```

Output:

```js
A
D
C
B
```

---

# Why?

Execution:

```text
A
D
```

Promise callback:

```text
Microtask Queue
```

Timer callback:

```text
Callback Queue
```

Event Loop processes:

```text
Microtasks first
```

Therefore:

```text
C before B
```

---

# Multiple Promises

```js
Promise.resolve().then(() => {
  console.log(1);
});

Promise.resolve().then(() => {
  console.log(2);
});

console.log(3);
```

Output:

```js
3
1
2
```

Microtasks execute in FIFO order.

---

# Nested Microtasks

```js
Promise.resolve().then(() => {
  console.log(1);

  Promise.resolve().then(() => {
    console.log(2);
  });
});

console.log(3);
```

Output:

```js
3
1
2
```

The Event Loop drains all microtasks before moving to macrotasks.

---

# Starvation Problem

Infinite microtasks can block macrotasks.

Example:

```js
function loop() {
  Promise.resolve().then(loop);
}

loop();
```

The Callback Queue may never execute.

This is called:

```text
Microtask Starvation
```

---

# fetch() Example

```js
fetch("/api/data")
  .then(response => response.json())
  .then(data => {
    console.log(data);
  });
```

Flow:

```text
fetch
   ↓
Web API
   ↓
Promise resolved
   ↓
Microtask Queue
   ↓
Call Stack
```

---

# Browser Rendering

Browsers usually render:

```text
Between Event Loop iterations
```

Too many synchronous tasks can block rendering.

Example:

```js
while (true) {}
```

This freezes the UI.

Because:

```text
Call Stack never becomes empty.
```

---

# setInterval

```js
setInterval(() => {
  console.log("Tick");
}, 1000);
```

Every second:

```text
Callback Queue
```

receives a new task.

But execution only occurs when:

```text
Call Stack is empty.
```

---

# Real World React Example

```js
setState(newValue);
```

React schedules updates and may batch them using asynchronous mechanisms that rely on the Event Loop.

Understanding the Event Loop helps explain:

- State updates
- Re-renders
- Async behavior
- Effect execution

---

# Common Interview Questions

## What is the Event Loop?

A mechanism that moves tasks from queues to the Call Stack when it becomes empty.

---

## Is JavaScript asynchronous?

JavaScript itself is single-threaded and synchronous.

Asynchronous behavior comes from:

```text
Browser APIs / Runtime
```

---

## Difference Between Callback Queue and Microtask Queue?

Microtask Queue has higher priority.

---

## Which executes first?

```js
Promise.then
or
setTimeout
```

Answer:

```text
Promise.then
```

because it is a microtask.

---

## Why does setTimeout(..., 0) not execute immediately?

Because the callback must wait until:

```text
1. Timer finishes
2. Call Stack becomes empty
3. Higher-priority microtasks complete
```

---

# Interview Output Questions

## Question 1

```js
console.log(1);

setTimeout(() => {
  console.log(2);
}, 0);

Promise.resolve().then(() => {
  console.log(3);
});

console.log(4);
```

Output:

```js
1
4
3
2
```

---

## Question 2

```js
setTimeout(() => {
  console.log("Timer");
}, 0);

Promise.resolve().then(() => {
  console.log("Promise");
});
```

Output:

```js
Promise
Timer
```

---

# Quick Revision

✅ JavaScript is single-threaded

✅ Call Stack executes code

✅ Web APIs handle async work

✅ Event Loop moves tasks to Call Stack

✅ Callback Queue stores macrotasks

✅ Microtask Queue stores Promise callbacks

✅ Microtasks have higher priority

✅ `Promise.then()` executes before `setTimeout`

✅ `setTimeout(..., 0)` is not immediate

✅ Infinite microtasks can starve macrotasks

✅ A blocked Call Stack freezes the UI