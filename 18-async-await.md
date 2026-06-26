# JavaScript Async/Await

## Why Async/Await Exists

Before ES2017, asynchronous code was written mainly using Promises.

Example:

```js
fetch("/users")
  .then(response => response.json())
  .then(data => {
    console.log(data);
  })
  .catch(error => {
    console.error(error);
  });
```

This works well.

But with multiple asynchronous operations,
Promise chains become difficult to read.

Example:

```js
getUser()
  .then(getOrders)
  .then(getPayment)
  .then(processPayment)
  .catch(handleError);
```

Async/Await was introduced in ES2017 to make asynchronous code look like synchronous code while still using Promises underneath. :contentReference[oaicite:1]{index=1}

---

# What is async?

The `async` keyword transforms a normal function into an asynchronous function.

Example:

```js
async function greet() {
  return "Hello";
}
```

Even though we return a string:

```js
"Hello"
```

JavaScript actually returns:

```js
Promise.resolve("Hello")
```

---

# async Always Returns a Promise

Example:

```js
async function greet() {
  return "Hello";
}

console.log(greet());
```

Output:

```text
Promise {
  "Hello"
}
```

Equivalent to:

```js
function greet() {
  return Promise.resolve("Hello");
}
```

---

# What is await?

`await` pauses execution of the current async function until a Promise settles.

Example:

```js
const response =
  await fetch("/users");
```

Execution waits only inside that async function.

The rest of the application continues running normally. :contentReference[oaicite:2]{index=2}

---

# Basic Example

Promise version:

```js
fetch("/users")
  .then(response =>
    response.json()
  )
  .then(data => {
    console.log(data);
  });
```

---

Async/Await version:

```js
async function getUsers() {
  const response =
    await fetch("/users");

  const data =
    await response.json();

  console.log(data);
}
```

Much easier to read.

---

# await Can Only Be Used...

Inside:

```text
async functions
```

Example:

```js
async function load() {
  const data =
    await fetch("/users");
}
```

---

Wrong:

```js
const data =
  await fetch("/users");
```

Output:

```text
SyntaxError
```

(Unless using top-level await inside ES modules.)

---

# Step-by-Step Execution

Example:

```js
async function load() {

  console.log("A");

  const response =
    await fetch("/users");

  console.log("B");
}

load();

console.log("C");
```

Output:

```text
A
C
B
```

Why?

Execution pauses only inside:

```js
load()
```

The main thread continues executing.

---

# await Does NOT Block JavaScript

Many beginners think:

```text
await freezes JavaScript
```

Wrong.

It pauses only the current async function.

Everything else continues.

---

Example:

```js
async function test() {
  await Promise.resolve();

  console.log("Inside");
}

test();

console.log("Outside");
```

Output:

```text
Outside
Inside
```

---

# Multiple await Statements

```js
async function load() {

  const user =
    await getUser();

  const orders =
    await getOrders(user);

  const payment =
    await getPayment(orders);

}
```

Each line waits for the previous one.

This is called:

```text
Sequential Execution
```

---

# Sequential vs Parallel

Sequential:

```js
const users =
  await fetch("/users");

const posts =
  await fetch("/posts");
```

Total time:

```text
Request1
+
Request2
```

---

Parallel:

```js
const [users, posts] =
  await Promise.all([
    fetch("/users"),
    fetch("/posts")
  ]);
```

Both requests start together.

Much faster.

---

# Promise.all() with Async/Await

```js
async function load() {

  const [users, posts] =
    await Promise.all([
      fetch("/users"),
      fetch("/posts")
    ]);

}
```

Use this when requests are independent.

---

# Error Handling

Instead of:

```js
promise
  .then(...)
  .catch(...);
```

Use:

```js
try {

  const response =
    await fetch("/users");

} catch (error) {

  console.error(error);

}
```

This makes asynchronous error handling look like synchronous code. :contentReference[oaicite:3]{index=3}

---

# Complete Example

```js
async function getUsers() {

  try {

    const response =
      await fetch("/users");

    if (!response.ok) {
      throw new Error(
        "Request Failed"
      );
    }

    const data =
      await response.json();

    console.log(data);

  } catch (error) {

    console.error(error);

  }

}
```

---

# await Works With Any Promise

```js
const value =
  await Promise.resolve(10);

console.log(value);
```

Output:

```js
10
```

---

# await and Non-Promises

If you await a normal value:

```js
const value =
  await 100;
```

JavaScript converts it into:

```js
Promise.resolve(100)
```

Output:

```js
100
```

---

# Async Arrow Functions

```js
const getUsers =
  async () => {

    const response =
      await fetch("/users");

  };
```

Very common in React.

---

# Async Class Methods

```js
class UserService {

  async getUsers() {

    return fetch("/users");

  }

}
```

Classes support async methods.

---

# Mixing then() and await

Bad:

```js
const response =
  await fetch("/users")
    .then(res => res.json());
```

Choose one style.

Better:

```js
const response =
  await fetch("/users");

const data =
  await response.json();
```

---

# forEach() Problem

Bad:

```js
users.forEach(async user => {

  await saveUser(user);

});
```

`forEach()` doesn't wait for async callbacks.

---

Better:

```js
for (const user of users) {

  await saveUser(user);

}
```

Or run them in parallel:

```js
await Promise.all(
  users.map(saveUser)
);
```

---

# await Inside Loops

Sequential:

```js
for (const id of ids) {

  await fetchUser(id);

}
```

Each request waits for the previous one.

---

Parallel:

```js
await Promise.all(

  ids.map(fetchUser)

);
```

Starts all requests together.

---

# Async/Await and Event Loop

Example:

```js
console.log("A");

async function test() {

  await Promise.resolve();

  console.log("B");

}

test();

console.log("C");
```

Output:

```text
A
C
B
```

Reason:

After `await`, the remaining code is scheduled as a microtask.

It follows the same Event Loop rules as Promise callbacks. :contentReference[oaicite:4]{index=4}

---

# Top-Level await

Modern ES Modules support:

```js
const response =
  await fetch("/users");
```

Without wrapping inside an async function.

Works only in ES modules.

---

# Async/Await vs Promises

| Feature | Promises | Async/Await |
|----------|----------|-------------|
| Readability | Good | Excellent |
| Chaining | Required | Not Required |
| Error Handling | `.catch()` | `try/catch` |
| Underlying Mechanism | Promise | Promise |
| Sequential Code | Harder | Easier |

Async/Await is syntax built on top of Promises—it does not replace them. :contentReference[oaicite:5]{index=5}

---

# Common Mistakes

## Mistake 1

Forgetting async.

Wrong:

```js
function load() {

  await fetch("/users");

}
```

Output:

```text
SyntaxError
```

---

Correct:

```js
async function load() {

  await fetch("/users");

}
```

---

## Mistake 2

Using await unnecessarily.

Wrong:

```js
const value =
  await 5;
```

Works, but adds no value.

---

## Mistake 3

Running independent requests sequentially.

Bad:

```js
const users =
  await fetch("/users");

const posts =
  await fetch("/posts");
```

Better:

```js
const [users, posts] =
  await Promise.all([
    fetch("/users"),
    fetch("/posts")
  ]);
```

---

## Mistake 4

Using async callbacks with `forEach()`.

Use:

```js
for...of
```

or

```js
Promise.all()
```

instead.

---

# Real React Example

```js
useEffect(() => {

  async function loadUsers() {

    try {

      const response =
        await fetch("/users");

      const users =
        await response.json();

      setUsers(users);

    } catch (error) {

      console.error(error);

    }

  }

  loadUsers();

}, []);
```

This is one of the most common patterns in React applications.

---

# Common Interview Questions

## What does async do?

Marks a function as asynchronous and makes it always return a Promise.

---

## What does await do?

Pauses execution of the current async function until a Promise settles.

---

## Can await be used outside an async function?

Normally no.

Exception:

```text
Top-Level await
```

inside ES modules.

---

## Does async/await replace Promises?

No.

It is syntax built on top of Promises.

---

## Does await block JavaScript?

No.

It pauses only the current async function.

---

## Why use Promise.all() with async/await?

To run independent asynchronous tasks in parallel.

---

## Why doesn't async work with forEach()?

Because `forEach()` does not wait for async callbacks.

Use:

```js
for...of
```

or

```js
Promise.all()
```

---

# Quick Revision

✅ `async` always returns a Promise

✅ `await` pauses only the current async function

✅ `await` works only inside async functions (or top-level ES modules)

✅ Async/Await is built on Promises

✅ `try/catch` handles async errors

✅ Use `Promise.all()` for parallel work

✅ Avoid async `forEach()`

✅ `for...of` supports sequential awaits

✅ Code after `await` runs as a microtask

✅ Async/Await improves readability but does not change how JavaScript executes asynchronous code