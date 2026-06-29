# JavaScript Higher-Order Functions

## Why This Topic Matters

Higher-Order Functions (HOFs) are one of the foundations of modern JavaScript.

They are heavily used in:

- React
- Node.js
- Express Middleware
- Array Methods
- Functional Programming
- Utility Libraries

If you've written:

```js
array.map(...)
```

or

```js
button.addEventListener(...)
```

you're already using Higher-Order Functions.

---

# What is a Higher-Order Function?

A Higher-Order Function is a function that does **at least one** of these:

```text
1. Accepts another function as an argument
OR
2. Returns another function
```

If it does either one, it is a Higher-Order Function.

---

# Why Are Higher-Order Functions Possible?

Because JavaScript treats functions as:

```text
First-Class Citizens
```

This means functions can be:

- Stored in variables
- Passed as arguments
- Returned from functions
- Stored in objects
- Stored in arrays

Example:

```js
const greet = function () {
  console.log("Hello");
};
```

Functions behave like any other value.

---

# Type 1: Function Accepts Another Function

Example:

```js
function execute(callback) {
  callback();
}

function greet() {
  console.log("Hello");
}

execute(greet);
```

Output:

```text
Hello
```

Here:

```text
execute()
```

is the Higher-Order Function.

```text
greet()
```

is the Callback Function.

---

# Type 2: Function Returns Another Function

Example:

```js
function multiply(multiplier) {

  return function(number) {
    return number * multiplier;
  };

}
```

Usage:

```js
const double = multiply(2);

console.log(double(5));
```

Output:

```js
10
```

The returned function remembers:

```js
multiplier
```

through closure.

---

# Visualizing Higher-Order Functions

```text
Higher-Order Function

      ↓

Accepts Function
OR
Returns Function
```

---

# Callback Functions

Most Higher-Order Functions use callbacks.

Example:

```js
function process(callback) {

  callback();

}
```

The callback decides:

```text
What work should be done
```

The Higher-Order Function decides:

```text
When to execute it
```

---

# Array Methods Are Higher-Order Functions

Example:

```js
const numbers = [1, 2, 3];
```

---

## map()

```js
const doubled =
  numbers.map(num => num * 2);
```

Output:

```js
[2, 4, 6]
```

---

## filter()

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

## reduce()

```js
const sum =
  numbers.reduce(
    (total, num) => total + num,
    0
  );
```

Output:

```js
6
```

---

## forEach()

```js
numbers.forEach(num => {
  console.log(num);
});
```

Output:

```text
1
2
3
```

All of these are Higher-Order Functions.

---

# Creating Your Own HOF

Example:

```js
function calculate(a, b, operation) {

  return operation(a, b);

}
```

Addition:

```js
calculate(
  5,
  3,
  (a, b) => a + b
);
```

Output:

```js
8
```

Multiplication:

```js
calculate(
  5,
  3,
  (a, b) => a * b
);
```

Output:

```js
15
```

Same Higher-Order Function.

Different behavior.

---

# Returning Functions

Example:

```js
function greet(language) {

  return function(name) {

    console.log(
      `${language}: ${name}`
    );

  };

}
```

Usage:

```js
const english =
  greet("Hello");

english("Akio");
```

Output:

```text
Hello: Akio
```

---

# Closures + HOF

Returned functions remember variables.

```js
function counter() {

  let count = 0;

  return function() {

    count++;

    return count;

  };

}
```

Usage:

```js
const increment =
  counter();

console.log(increment());
console.log(increment());
```

Output:

```js
1
2
```

Closure makes this possible.

---

# Event Listeners

Example:

```js
button.addEventListener(
  "click",
  () => {
    console.log("Clicked");
  }
);
```

Higher-Order Function:

```js
addEventListener()
```

Callback:

```js
() => {}
```

---

# setTimeout()

```js
setTimeout(() => {

  console.log("Done");

}, 1000);
```

Higher-Order Function:

```js
setTimeout()
```

Callback:

```js
() => {}
```

---

# Promise.then()

```js
fetch("/users")

.then(response => {

  console.log(response);

});
```

Higher-Order Function:

```js
then()
```

Callback:

```js
response => {}
```

---

# Function Factory

A function that creates other functions.

Example:

```js
function createMultiplier(multiplier) {

  return function(number) {

    return number * multiplier;

  };

}
```

Usage:

```js
const triple =
  createMultiplier(3);

console.log(triple(4));
```

Output:

```js
12
```

This pattern is very common.

---

# Function Composition

Higher-Order Functions make function composition possible.

Example:

```js
const add2 = x => x + 2;

const multiply3 =
  x => x * 3;
```

Compose:

```js
const result =
  multiply3(add2(5));
```

Output:

```js
21
```

Composition is a major Functional Programming concept.

---

# Real React Example

```js
users.map(user => (

  <UserCard
    key={user.id}
    user={user}
  />

));
```

Higher-Order Function:

```js
map()
```

Callback:

```js
user => (...)
```

React relies heavily on Higher-Order Functions.

---

# Middleware Example (Express)

```js
app.use((req, res, next) => {

  console.log(req.url);

  next();

});
```

Higher-Order Function:

```js
use()
```

Callback:

```js
(req, res, next) => {}
```

---

# Higher-Order Components (React)

Older React used:

```js
withRouter(Component)
```

A function returning another component.

This is another Higher-Order Function pattern.

---

# Advantages

Higher-Order Functions make code:

- Reusable
- Flexible
- Modular
- Easier to test
- Easier to compose
- Less repetitive

---

# Common Mistakes

## Mistake 1

Calling the callback immediately.

Wrong:

```js
button.addEventListener(
  "click",
  greet()
);
```

Correct:

```js
button.addEventListener(
  "click",
  greet
);
```

---

## Mistake 2

Confusing callback with Higher-Order Function.

Example:

```js
numbers.map(num => num * 2);
```

Higher-Order Function:

```js
map()
```

Callback:

```js
num => num * 2
```

---

## Mistake 3

Thinking every function is a Higher-Order Function.

Wrong:

```js
function add(a, b) {
  return a + b;
}
```

This is a normal function.

It neither:

- Accepts a function
- Returns a function

---

# Common Interview Questions

## What is a Higher-Order Function?

A function that:

- Accepts another function
- Returns another function
- Or both

---

## Why Can JavaScript Support Higher-Order Functions?

Because functions are first-class citizens.

---

## Is map() a Higher-Order Function?

Yes.

It accepts a callback.

---

## Is filter() a Higher-Order Function?

Yes.

---

## Is reduce() a Higher-Order Function?

Yes.

---

## Is setTimeout() a Higher-Order Function?

Yes.

It accepts a callback.

---

## Difference Between Callback and Higher-Order Function?

Higher-Order Function:

```js
map()
```

Callback:

```js
num => num * 2
```

---

## Why Are Higher-Order Functions Useful?

They separate:

```text
How work is done

from

What work is done
```

This makes code reusable.

---

# Quick Revision

✅ Higher-Order Function accepts or returns a function

✅ JavaScript supports HOFs because functions are first-class citizens

✅ Callback is passed into a Higher-Order Function

✅ `map()`, `filter()`, `reduce()`, and `forEach()` are HOFs

✅ `setTimeout()` and `addEventListener()` are HOFs

✅ `Promise.then()` is a HOF

✅ Functions can return other functions

✅ Returned functions often create closures

✅ HOFs improve code reuse and flexibility

✅ React heavily uses Higher-Order Functions

✅ HOF ≠ Callback

✅ A callback is the function passed into a HOF