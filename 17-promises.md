# JavaScript Promises

## Why Promises Exist

Before ES6, asynchronous JavaScript relied heavily on callbacks.

Example:

```js
getUser(user => {
  getOrders(user, orders => {
    getPayment(orders, payment => {
      processPayment(payment, result => {
        console.log(result);
      });
    });
  });
});
```

Problems:

- Deep nesting
- Hard to read
- Difficult error handling
- Callback Hell (Pyramid of Doom)

Promises were introduced in ES6 (2015) to make asynchronous code cleaner and easier to manage. :contentReference[oaicite:0]{index=0}

---

# What is a Promise?

A Promise is:

```text
An object representing the eventual
completion or failure of an
asynchronous operation.
```

Think of it as an **IOU** (I Owe You).

Example:

```js
const promise = fetch("/users");
```

`fetch()` doesn't return the data immediately.

It returns:

```js
Promise<Response>
```

The data will arrive later.

---

# Restaurant Analogy

Imagine ordering food.

Step 1:

```text
Place Order
```

↓

You receive:

```text
Order Token
```

↓

Kitchen prepares food.

↓

One of two things happens:

```text
Food Ready ✅
```

or

```text
Order Failed ❌
```

The order token is your Promise.

---

# Promise States

Every Promise has exactly one of three states:

```text
Pending
Fulfilled
Rejected
```

---

## Pending

Initial state.

```text
Operation still running.
```

Example:

```js
fetch("/users");
```

Network request is still in progress.

---

## Fulfilled

Operation completed successfully.

```text
Value available.
```

Example:

```js
resolve(data);
```

---

## Rejected

Operation failed.

```text
Error available.
```

Example:

```js
reject(error);
```

---

# Promise Lifecycle

```text
          Pending
         /       \
        /         \
 Fulfilled     Rejected
```

Once settled:

```text
Promise cannot change again.
```

A Promise can only settle once. :contentReference[oaicite:1]{index=1}

---

# Creating a Promise

Syntax:

```js
const promise =
  new Promise((resolve, reject) => {

  });
```

The function passed to `new Promise` is called the:

```text
Executor
```

---

# Executor Function

```js
const promise =
  new Promise((resolve, reject) => {

    console.log("Runs immediately");

  });
```

Important:

```text
Executor runs immediately.
```

Only `.then()` callbacks run asynchronously. :contentReference[oaicite:2]{index=2}

---

# resolve()

Marks the Promise as successful.

```js
const promise =
  new Promise(resolve => {
    resolve("Done");
  });
```

---

Consume it:

```js
promise.then(value => {
  console.log(value);
});
```

Output:

```js
Done
```

---

# reject()

Marks the Promise as failed.

```js
const promise =
  new Promise((resolve, reject) => {
    reject("Error");
  });
```

Consume:

```js
promise.catch(error => {
  console.log(error);
});
```

Output:

```js
Error
```

---

# .then()

Runs when Promise is fulfilled.

```js
promise.then(value => {
  console.log(value);
});
```

`value` is whatever was passed to:

```js
resolve(value)
```

---

# .catch()

Runs only when Promise is rejected.

```js
promise.catch(error => {
  console.log(error);
});
```

Receives the value passed to:

```js
reject(error)
```

---

# .finally()

Runs whether Promise succeeds or fails.

```js
promise.finally(() => {
  console.log("Finished");
});
```

Useful for:

- Loading indicators
- Cleanup
- Closing connections

---

# Example Promise

```js
const promise =
  new Promise((resolve) => {

    setTimeout(() => {
      resolve("Success");
    }, 2000);

  });
```

Usage:

```js
promise.then(result => {
  console.log(result);
});
```

Output:

```js
Success
```

(after 2 seconds)

---

# Promise Chaining

Instead of nesting:

```js
step1(() => {
  step2(() => {
    step3();
  });
});
```

Use:

```js
step1()
  .then(step2)
  .then(step3);
```

Much cleaner.

---

# Returning Values

```js
Promise.resolve(5)

.then(value => {
  return value * 2;
})

.then(value => {
  console.log(value);
});
```

Output:

```js
10
```

The returned value becomes the input of the next `.then()`.

---

# Returning Another Promise

```js
fetch("/users")

.then(response => {
  return response.json();
})

.then(data => {
  console.log(data);
});
```

`response.json()` returns another Promise.

The next `.then()` waits automatically.

This is called:

```text
Promise Chaining
```

---

# Error Propagation

```js
Promise.resolve()

.then(() => {
  throw new Error("Oops");
})

.catch(error => {
  console.log(error.message);
});
```

Output:

```js
Oops
```

Errors automatically travel down the chain.

---

# One catch() Is Enough

```js
step1()

.then(step2)

.then(step3)

.catch(error => {
  console.log(error);
});
```

Any error above reaches the single `.catch()`.

---

# Promise.resolve()

Creates an already fulfilled Promise.

```js
Promise.resolve("Hello")
```

Equivalent:

```js
new Promise(resolve => {
  resolve("Hello");
});
```

---

# Promise.reject()

Creates an already rejected Promise.

```js
Promise.reject("Error");
```

Equivalent:

```js
new Promise((_, reject) => {
  reject("Error");
});
```

---

# Promise.all()

Waits until all Promises succeed.

```js
Promise.all([
  fetch("/users"),
  fetch("/posts")
]);
```

Result:

```text
Resolves when ALL succeed.
```

If one rejects:

```text
Entire Promise rejects.
```

---

# Promise.all() Example

```js
const p1 =
  Promise.resolve(1);

const p2 =
  Promise.resolve(2);

Promise.all([p1, p2])

.then(values => {
  console.log(values);
});
```

Output:

```js
[1, 2]
```

---

# Promise.allSettled()

Waits for every Promise.

Even failed ones.

```js
Promise.allSettled([
  p1,
  p2
]);
```

Returns:

```js
[
  {
    status: "fulfilled"
  },
  {
    status: "rejected"
  }
]
```

Useful when you need all results. :contentReference[oaicite:3]{index=3}

---

# Promise.race()

Returns the first settled Promise.

```js
Promise.race([
  p1,
  p2
]);
```

Winner:

```text
First fulfilled
OR
First rejected
```

---

# Promise.any()

Returns the first fulfilled Promise.

Ignores rejected ones.

If every Promise rejects:

```text
AggregateError
```

---

# Promise and Event Loop

```js
console.log("A");

Promise.resolve()

.then(() => {
  console.log("B");
});

console.log("C");
```

Output:

```js
A
C
B
```

Why?

`.then()` callbacks enter the:

```text
Microtask Queue
```

Microtasks run before macrotasks like `setTimeout()`. :contentReference[oaicite:4]{index=4}

---

# Promise vs Callback

| Feature | Callback | Promise |
|----------|----------|----------|
| Readability | Poor for nested async | Better |
| Chaining | Difficult | Easy |
| Error Handling | Manual | Centralized |
| Callback Hell | Common | Avoided |
| Async/Await Support | No | Yes |

---

# Promise Constructor Anti-Pattern

Bad:

```js
function getData() {
  return new Promise((resolve) => {
    fetch("/users")
      .then(resolve);
  });
}
```

Better:

```js
function getData() {
  return fetch("/users");
}
```

Don't wrap an existing Promise inside another Promise unless you're converting a callback-based API. :contentReference[oaicite:5]{index=5}

---

# Real React Example

```js
useEffect(() => {

  fetch("/users")

    .then(response =>
      response.json()
    )

    .then(data => {
      setUsers(data);
    });

}, []);
```

Most data fetching in React starts with Promises.

---

# Common Interview Questions

## What is a Promise?

An object representing the eventual completion or failure of an asynchronous operation.

---

## What are the three Promise states?

```text
Pending
Fulfilled
Rejected
```

---

## Can a Promise change after it is fulfilled?

No.

A settled Promise is immutable.

---

## Difference Between then() and catch()?

`then()`

Handles success.

`catch()`

Handles errors.

---

## Difference Between Promise.all() and Promise.allSettled()?

`Promise.all()`

Fails immediately if any Promise rejects.

`Promise.allSettled()`

Waits for every Promise regardless of success or failure.

---

## Why does Promise.then() execute before setTimeout()?

Because Promise callbacks are placed in the:

```text
Microtask Queue
```

which has higher priority than the Callback (Macrotask) Queue.

---

## What is Promise Chaining?

Returning values or Promises from `.then()` so the next `.then()` receives the result.

---

# Common Mistakes

## Mistake 1

Forgetting to return a Promise.

Bad:

```js
fetch("/users")

.then(response => {
  response.json();
})

.then(data => {
  console.log(data);
});
```

`data` becomes:

```js
undefined
```

Correct:

```js
fetch("/users")

.then(response => {
  return response.json();
})

.then(data => {
  console.log(data);
});
```

---

## Mistake 2

Wrapping an existing Promise with `new Promise()` unnecessarily.

---

## Mistake 3

Creating multiple `catch()` blocks when one at the end is enough.

---

# Quick Revision

✅ Promise represents a future value

✅ Solves Callback Hell

✅ States: Pending → Fulfilled / Rejected

✅ Executor runs immediately

✅ `.then()` handles success

✅ `.catch()` handles errors

✅ `.finally()` runs always

✅ Promise settles only once

✅ Returning from `.then()` passes value to next `.then()`

✅ Returning a Promise enables chaining

✅ `Promise.all()` waits for all

✅ `Promise.allSettled()` waits for every Promise

✅ `Promise.race()` returns the first settled Promise

✅ `Promise.any()` returns the first fulfilled Promise

✅ Promise callbacks are microtasks

✅ Don't wrap an existing Promise inside `new Promise()`