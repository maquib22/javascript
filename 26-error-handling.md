# JavaScript Error Handling

## Why Error Handling Matters

No application is perfect.

Things can fail because of:

- Invalid user input
- Network failures
- Missing files
- Server errors
- Programming mistakes
- Unexpected data

Without proper error handling:

```text
Application Crashes

↓

Poor User Experience

↓

Hard-to-Debug Bugs
```

Good error handling makes applications:

- Reliable
- Predictable
- Easier to debug
- Easier to maintain

---

# What is an Error?

An error is an unexpected condition that prevents your program from continuing normally.

Example:

```js
const user = null;

console.log(user.name);
```

Output:

```text
TypeError:
Cannot read properties of null
```

---

# JavaScript Error Types

Common built-in errors:

```text
Error
SyntaxError
ReferenceError
TypeError
RangeError
URIError
EvalError
AggregateError
```

---

# Error Object

Every JavaScript error is an object.

Example:

```js
try {
  throw new Error("Something went wrong");
} catch (error) {
  console.log(error);
}
```

Output:

```text
Error: Something went wrong
```

---

# Error Properties

```js
try {
  throw new Error("Invalid data");
} catch (error) {
  console.log(error.name);
  console.log(error.message);
  console.log(error.stack);
}
```

Example output:

```text
Error

Invalid data

(stack trace)
```

Properties:

| Property | Purpose |
|----------|----------|
| name | Error type |
| message | Description |
| stack | Call stack at error |

---

# try...catch

Basic syntax:

```js
try {

  // Risky code

} catch (error) {

  // Handle error

}
```

---

Example

```js
try {

  JSON.parse("{");

} catch (error) {

  console.log("Invalid JSON");

}
```

Output:

```text
Invalid JSON
```

Instead of crashing, execution continues.

---

# Execution Flow

```text
try

↓

No Error

↓

Skip catch

↓

Continue
```

OR

```text
try

↓

Error

↓

catch

↓

Continue
```

---

# What Does catch Receive?

```js
try {

  throw new Error("Oops");

} catch (error) {

  console.log(error);

}
```

The variable:

```js
error
```

contains the thrown Error object.

---

# finally

`finally` always executes.

Whether:

- Success
- Failure
- Return statement
- Throw statement

Syntax:

```js
try {

} catch {

} finally {

}
```

---

Example

```js
try {

  console.log("Working");

} finally {

  console.log("Cleanup");

}
```

Output:

```text
Working

Cleanup
```

Useful for:

- Closing files
- Stopping loaders
- Cleaning resources
- Releasing locks

---

# throw

You can create your own errors.

Example:

```js
throw new Error("Invalid password");
```

Execution stops immediately.

---

# Creating Custom Errors

Example:

```js
function divide(a, b) {

  if (b === 0) {

    throw new Error(
      "Division by zero"
    );

  }

  return a / b;

}
```

Usage:

```js
try {

  divide(10, 0);

} catch (error) {

  console.log(error.message);

}
```

Output:

```text
Division by zero
```

---

# throw Any Value

Technically valid:

```js
throw "Error";
```

or

```js
throw 404;
```

or

```js
throw {
  message: "Failed"
};
```

But best practice is:

```js
throw new Error(...)
```

It provides stack traces and standard properties. :contentReference[oaicite:1]{index=1}

---

# Built-in Error Types

## ReferenceError

Using an undeclared variable.

```js
console.log(user);
```

Output:

```text
ReferenceError
```

---

## TypeError

Wrong type or invalid operation.

```js
null.name
```

Output:

```text
TypeError
```

---

## SyntaxError

Invalid JavaScript syntax.

```js
if (
```

Output:

```text
SyntaxError
```

---

## RangeError

Value outside allowed range.

```js
new Array(-1);
```

Output:

```text
RangeError
```

---

## AggregateError

Used when multiple errors are grouped together.

Example:

```js
Promise.any([
  Promise.reject("A"),
  Promise.reject("B")
]);
```

If every Promise rejects:

```text
AggregateError
```

---

# Custom Error Classes

Instead of:

```js
throw new Error("Invalid age");
```

Create:

```js
class ValidationError
  extends Error {

  constructor(message) {

    super(message);

    this.name =
      "ValidationError";

  }

}
```

Usage:

```js
throw new ValidationError(
  "Age is required"
);
```

Output:

```text
ValidationError
```

Useful when handling different categories of errors. :contentReference[oaicite:2]{index=2}

---

# Rethrowing Errors

Sometimes you cannot fully handle an error.

Example:

```js
try {

  risky();

} catch (error) {

  console.log(error);

  throw error;

}
```

The error continues upward.

---

# Nested try...catch

```js
try {

  try {

    throw new Error("Oops");

  } catch {

    console.log("Inner");

  }

} catch {

  console.log("Outer");

}
```

Output:

```text
Inner
```

Since the inner catch handled the error, the outer one never runs.

---

# Async Error Handling

Wrong:

```js
try {

  fetch("/users");

} catch {

  console.log("Error");

}
```

Won't catch network failures because `fetch()` is asynchronous.

---

Correct

```js
try {

  const response =
    await fetch("/users");

} catch (error) {

  console.log(error);

}
```

`await` allows `try...catch` to handle Promise rejections. :contentReference[oaicite:3]{index=3}

---

# Promise Error Handling

```js
fetch("/users")

.then(response => {

  return response.json();

})

.catch(error => {

  console.log(error);

});
```

Equivalent async version:

```js
try {

  const response =
    await fetch("/users");

} catch (error) {

}
```

---

# throw Inside async

```js
async function login() {

  throw new Error("Failed");

}
```

This doesn't throw synchronously.

It returns:

```text
Rejected Promise
```

Usage:

```js
login().catch(console.error);
```

---

# Multiple catch()

Bad:

```js
promise

.catch(...)

.catch(...)

.catch(...);
```

Better:

```js
promise

.then(...)

.catch(...);
```

One centralized handler is often enough.

---

# Optional Chaining vs try...catch

Bad:

```js
try {

  console.log(
    user.profile.name
  );

} catch {

}
```

Better:

```js
console.log(
  user?.profile?.name
);
```

Avoid exceptions when simple property checks are enough.

---

# Fail Fast

Instead of allowing invalid input:

```js
function login(user) {

}
```

Validate early:

```js
if (!user) {

  throw new Error(
    "User required"
  );

}
```

Failing early makes bugs easier to diagnose.

---

# Real React Example

```js
async function loadUsers() {

  try {

    const response =
      await fetch("/users");

    const data =
      await response.json();

    setUsers(data);

  } catch (error) {

    setError(error.message);

  }

}
```

This is a common pattern in React applications.

---

# Common Mistakes

## Mistake 1

Using try...catch for validation.

Bad:

```js
try {

  if (age < 18)
    throw Error();

} catch {}
```

Prefer normal conditionals unless an exceptional situation occurs.

---

## Mistake 2

Ignoring errors.

Bad:

```js
catch (error) {

}
```

Always:

- Log
- Handle
- Rethrow

Don't silently swallow errors.

---

## Mistake 3

Throwing strings.

Bad:

```js
throw "Oops";
```

Better:

```js
throw new Error("Oops");
```

---

## Mistake 4

Expecting try...catch to catch asynchronous callbacks.

Wrong:

```js
try {

  setTimeout(() => {

    throw new Error("Boom");

  }, 1000);

} catch {

  console.log("Caught");

}
```

Output:

```text
Uncaught Error
```

The callback runs later, after the `try` block has already finished.

---

# Error Handling Best Practices

- Throw `Error` objects, not strings.
- Handle errors at the appropriate level.
- Don't swallow exceptions silently.
- Use custom error classes for different error categories.
- Use `finally` for cleanup.
- Validate input early.
- Prefer optional chaining over exceptions for missing properties.
- Catch Promise rejections.

---

# Common Interview Questions

## What does try...catch do?

Handles runtime exceptions so the program can recover instead of crashing.

---

## What does finally do?

Runs regardless of whether an error occurred.

---

## Difference Between throw and catch?

`throw`

Creates an exception.

`catch`

Handles an exception.

---

## Can try...catch catch syntax errors?

Only if the syntax error occurs while evaluating code dynamically (for example, with `eval()` or the `Function` constructor). A syntax error that prevents the script from parsing cannot be caught because the program never starts executing. :contentReference[oaicite:4]{index=4}

---

## Can try...catch catch asynchronous errors?

Only when the asynchronous operation is awaited or its Promise rejection is handled.

A later callback (like one in `setTimeout`) is **not** caught by an earlier `try...catch`.

---

## Why use custom error classes?

To distinguish different error categories and handle them differently.

---

## Why throw Error instead of a string?

`Error` objects include:

- name
- message
- stack trace

making debugging much easier.

---

# Quick Revision

✅ Errors are objects

✅ `try` contains risky code

✅ `catch` handles exceptions

✅ `finally` always executes

✅ `throw` creates an exception

✅ Prefer `throw new Error()`

✅ Built-in errors include `TypeError`, `ReferenceError`, `SyntaxError`, `RangeError`, and `AggregateError`

✅ Create custom errors by extending `Error`

✅ Async functions return rejected Promises when they throw

✅ `try...catch` works with `await`

✅ `try...catch` does not automatically catch errors from later asynchronous callbacks

✅ Don't swallow errors silently

✅ Use `finally` for cleanup

✅ Validate input early