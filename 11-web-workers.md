# JavaScript Web Workers

## Why Web Workers Exist

JavaScript runs on a:

```text
Single Main Thread
```

The same thread handles:

- Rendering UI
- User interactions
- Event handlers
- JavaScript execution

If a heavy computation runs:

```js
for (let i = 0; i < 10000000000; i++) {}
```

The browser becomes unresponsive.

Users experience:

```text
Lag
Freezing
Janky UI
```

Web Workers solve this problem by moving expensive work to another thread. :contentReference[oaicite:1]{index=1}

---

# What is a Web Worker?

A Web Worker is:

```text
A JavaScript file that runs
in a separate background thread.
```

It executes independently from the main thread.

Benefits:

- Non-blocking UI
- Better performance
- CPU-intensive work in background
- Multi-core utilization

Web Workers allow long-running tasks to execute without interrupting user interactions. :contentReference[oaicite:2]{index=2}

---

# Main Thread vs Worker Thread

```text
Main Thread
├─ DOM
├─ Rendering
├─ Events
└─ User Interaction

Worker Thread
├─ Calculations
├─ Data Processing
├─ Parsing
└─ Background Tasks
```

---

# Creating a Worker

## worker.js

```js
self.onmessage = function(event) {
  console.log(event.data);
};
```

---

## main.js

```js
const worker = new Worker("worker.js");
```

This creates a new worker thread. :contentReference[oaicite:3]{index=3}

---

# Sending Data to Worker

Main thread:

```js
worker.postMessage("Hello Worker");
```

Worker:

```js
self.onmessage = function(event) {
  console.log(event.data);
};
```

Output:

```js
Hello Worker
```

Communication happens through:

```js
postMessage()
```

---

# Sending Data Back

Worker:

```js
self.postMessage("Hello Main Thread");
```

Main thread:

```js
worker.onmessage = function(event) {
  console.log(event.data);
};
```

Output:

```js
Hello Main Thread
```

Workers communicate using message passing. :contentReference[oaicite:4]{index=4}

---

# Communication Flow

```text
Main Thread
     │
postMessage()
     │
     ▼
Worker
     │
postMessage()
     │
     ▼
Main Thread
```

---

# Example: Heavy Calculation

Without Worker:

```js
button.addEventListener("click", () => {
  let total = 0;

  for (let i = 0; i < 1e9; i++) {
    total += i;
  }

  console.log(total);
});
```

Problem:

```text
UI freezes
```

---

# With Worker

## worker.js

```js
self.onmessage = function() {
  let total = 0;

  for (let i = 0; i < 1e9; i++) {
    total += i;
  }

  self.postMessage(total);
};
```

---

## main.js

```js
worker.postMessage("start");

worker.onmessage = function(event) {
  console.log(event.data);
};
```

Result:

```text
Calculation runs in background
UI stays responsive
```

---

# Worker Global Scope

Workers do not use:

```js
window
```

Instead:

```js
self
```

Example:

```js
self.postMessage("Hello");
```

---

# What Workers Can Access

Workers can use:

```js
fetch()
XMLHttpRequest
setTimeout()
setInterval()
Promises
WebSockets
IndexedDB
```

---

Example:

```js
fetch("/users")
  .then(res => res.json())
  .then(console.log);
```

Works inside workers.

---

# What Workers Cannot Access

Workers cannot access:

```js
document
window
DOM
localStorage
alert()
```

Important interview question.

Workers run outside the document context and do not have DOM access. :contentReference[oaicite:5]{index=5}

---

# Invalid Example

```js
document.getElementById("title");
```

Output:

```text
ReferenceError
```

Because:

```text
DOM is unavailable
```

inside workers.

---

# Why DOM Access Is Restricted

Imagine:

```text
Main Thread
and
Worker Thread
```

both modifying the same DOM.

This would create:

```text
Race Conditions
```

Therefore only the main thread manipulates DOM.

---

# Worker Lifecycle

Create:

```js
const worker =
  new Worker("worker.js");
```

Use:

```js
worker.postMessage();
```

Terminate:

```js
worker.terminate();
```

After termination:

```text
Worker stops permanently.
```

:contentReference[oaicite:6]{index=6}

---

# Error Handling

Worker:

```js
throw new Error("Oops");
```

Main Thread:

```js
worker.onerror = function(error) {
  console.error(error);
};
```

---

# Structured Clone Algorithm

Messages are copied between threads.

Example:

```js
worker.postMessage({
  name: "Akio"
});
```

Worker receives:

```js
{
  name: "Akio"
}
```

The object is cloned.

Not shared.

---

# Copy vs Shared Memory

Default:

```text
Data is copied
```

between threads.

---

Example:

```js
worker.postMessage(user);
```

Worker receives a clone.

Changes inside worker won't affect original object.

---

# Transferable Objects

Large data copying is expensive.

Example:

```js
ArrayBuffer
```

can be transferred instead of copied.

---

```js
worker.postMessage(
  buffer,
  [buffer]
);
```

Ownership moves to worker.

Much faster for large datasets.

---

# SharedArrayBuffer

Special object allowing:

```text
Shared Memory
```

between threads.

---

```js
const shared =
  new SharedArrayBuffer(1024);
```

Used for:

- High-performance applications
- Games
- Scientific computing

Advanced topic.

---

# Types of Workers

## Dedicated Worker

Used by one script.

```js
new Worker("worker.js");
```

Most common.

---

## Shared Worker

Can be shared by multiple tabs.

```js
new SharedWorker("worker.js");
```

---

## Service Worker

Different from Web Workers.

Used for:

```text
Offline Support
Caching
Push Notifications
```

Example:

```js
navigator.serviceWorker
```

---

# Dedicated Worker vs Service Worker

| Feature | Web Worker | Service Worker |
|----------|----------|----------|
| Background Thread | ✅ | ✅ |
| DOM Access | ❌ | ❌ |
| Network Interception | ❌ | ✅ |
| Offline Support | ❌ | ✅ |
| Push Notifications | ❌ | ✅ |

---

# Web Worker and Event Loop

Each worker has:

```text
Own Call Stack
Own Event Loop
Own Memory
```

This is why workers can execute independently.

---

# Real World Use Cases

## Image Processing

```text
Resize images
Apply filters
Compression
```

---

## Data Analysis

```text
Sorting huge datasets
Aggregation
Statistics
```

---

## File Parsing

```text
CSV
Excel
PDF
JSON
```

---

## Crypto Operations

```text
Encryption
Hashing
Authentication
```

---

## Games

```text
Physics calculations
AI logic
Pathfinding
```

---

# React Example

Bad:

```js
const result =
  heavyCalculation();
```

UI freezes.

---

Better:

```js
Worker
```

performs calculations.

React UI remains responsive.

Popular libraries:

```text
workerize
comlink
react-use-worker
```

---

# Common Interview Questions

## What is a Web Worker?

A JavaScript thread running in the background separate from the main thread.

---

## Why Use Web Workers?

To perform expensive computations without blocking the UI.

---

## Can Web Workers Access DOM?

No.

Workers have no access to:

```js
document
window
```

:contentReference[oaicite:7]{index=7}

---

## How Do Workers Communicate?

Using:

```js
postMessage()
```

and

```js
onmessage
```

---

## Can Workers Make API Calls?

Yes.

Workers support:

```js
fetch()
```

and network requests.

---

## Can Workers Share Variables?

Not directly.

Data is cloned and sent through messages.

---

## What Is SharedArrayBuffer?

A mechanism for sharing memory between threads.

---

## Difference Between Web Worker and Service Worker?

Web Worker:

```text
Background computation
```

Service Worker:

```text
Network interception
Caching
Offline support
```

---

# Common Mistakes

## Mistake 1

Trying to access DOM.

```js
document.querySelector(...)
```

Invalid inside workers.

---

## Mistake 2

Creating too many workers.

Workers consume:

```text
Memory
CPU
Startup Cost
```

Workers are relatively heavyweight and should not be created excessively. :contentReference[oaicite:8]{index=8}

---

## Mistake 3

Using workers for tiny tasks.

Overhead may outweigh benefits.

Use workers only for expensive operations.

---

# Quick Revision

✅ JavaScript is single-threaded

✅ Web Workers create background threads

✅ Workers prevent UI blocking

✅ Workers communicate via `postMessage()`

✅ Workers use `onmessage`

✅ Workers cannot access DOM

✅ Workers use `self` instead of `window`

✅ Workers can use `fetch()`

✅ Workers have their own Event Loop

✅ Data is cloned between threads

✅ `ArrayBuffer` can be transferred

✅ `SharedArrayBuffer` enables shared memory

✅ `worker.terminate()` stops a worker

✅ Best for CPU-intensive tasks

✅ Service Workers are different from Web Workers