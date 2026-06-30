# JavaScript Pure Functions

## Why Pure Functions Matter

Imagine calling a function:

```js
calculatePrice(100)
```

Wouldn't it be nice if:

- It always returned the same result
- It never changed anything outside itself
- It never depended on hidden variables

That's exactly what a **Pure Function** does.

Pure functions make code:

- Predictable
- Easy to test
- Easy to debug
- Easy to reuse
- Safe for parallel execution

They are one of the foundations of Functional Programming.

---

# What is a Pure Function?

A function is **Pure** if it follows **both** of these rules:

```text
1. Same Input → Same Output

2. No Side Effects
```

If either rule is broken:

```text
The function is Impure
```

---

# Rule 1: Same Input → Same Output

A pure function always produces the same result for the same arguments.

Example:

```js
function add(a, b) {
  return a + b;
}
```

Output:

```js
add(2, 3); // 5
add(2, 3); // 5
add(2, 3); // 5
```

Always:

```text
5
```

No surprises.

---

# Rule 2: No Side Effects

A pure function must not change anything outside itself.

Bad:

```js
let total = 0;

function add(value) {
  total += value;
}
```

Problem:

```text
External variable changed
```

This is a:

```text
Side Effect
```

---

# What is a Side Effect?

A side effect is any observable change outside the function.

Examples:

- Modifying global variables
- Changing object properties
- Updating arrays
- Writing to files
- Making HTTP requests
- Reading user input
- Manipulating the DOM
- Logging to the console (often considered a side effect)

A pure function should only compute and return a value. :contentReference[oaicite:1]{index=1}

---

# Pure Function Example

```js
function square(number) {
  return number * number;
}
```

Properties:

```text
✓ Same input
✓ Same output
✓ No side effects
```

Pure.

---

# Impure Function Example

```js
let tax = 0.18;

function calculate(price) {
  return price + price * tax;
}
```

Looks harmless.

But:

```js
tax = 0.25;
```

Now:

```js
calculate(100);
```

returns a different result.

The function depends on external state.

Impure.

---

# Math.random()

```js
function random() {
  return Math.random();
}
```

Output:

```text
0.45
0.92
0.17
```

Same input.

Different output.

Impure.

---

# Date.now()

```js
function getTime() {
  return Date.now();
}
```

Different value every call.

Impure.

---

# Console.log()

```js
function greet(name) {
  console.log(name);

  return name;
}
```

Even though the return value is predictable:

```text
Printing to console
```

changes the outside world.

Technically:

```text
Impure
```

---

# Modifying Objects

Bad:

```js
function updateUser(user) {

  user.age++;

  return user;

}
```

Problem:

```text
Original object changed.
```

Side effect.

---

Better:

```js
function updateUser(user) {

  return {
    ...user,
    age: user.age + 1
  };

}
```

Original object remains unchanged.

Pure.

---

# Array Mutation

Bad:

```js
function addItem(arr) {

  arr.push(5);

  return arr;

}
```

Problem:

```text
Original array modified.
```

---

Good:

```js
function addItem(arr) {

  return [...arr, 5];

}
```

Original array stays unchanged.

---

# Immutability

Pure functions usually rely on:

```text
Immutable Data
```

Instead of changing existing data:

```text
Create New Data
```

---

Bad:

```js
user.name = "John";
```

Good:

```js
const updatedUser = {

  ...user,

  name: "John"

};
```

---

# Referential Transparency

A pure function can always be replaced with its result.

Example:

```js
add(2, 3)
```

can be replaced with:

```js
5
```

without changing program behavior.

This property is called:

```text
Referential Transparency
```

---

# Memoization

Pure functions are ideal for caching.

Example:

```js
function square(x) {
  return x * x;
}
```

If:

```js
square(10)
```

has already been computed:

```text
100
```

can be reused.

This optimization is called:

```text
Memoization
```

Pure functions make memoization safe because identical inputs always produce identical outputs. :contentReference[oaicite:2]{index=2}

---

# Testing Pure Functions

Pure functions are extremely easy to test.

Example:

```js
function multiply(a, b) {
  return a * b;
}
```

Test:

```js
expect(
  multiply(2, 3)
).toBe(6);
```

No setup.

No mocking.

No external state.

---

# Testing Impure Functions

```js
function getWeather() {

  return fetch("/weather");

}
```

Testing requires:

- Mock API
- Internet
- Error handling
- Timing

Much harder.

---

# Functional Programming

Functional Programming encourages:

- Pure Functions
- Immutability
- Composition
- Small reusable functions

Pure functions are the foundation.

---

# React Connection

React encourages pure rendering.

Example:

```js
function Greeting({ name }) {

  return <h1>Hello {name}</h1>;

}
```

Same props:

```js
{
  name: "Akio"
}
```

Always produce:

```text
Same UI
```

This makes React predictable.

---

# Redux Connection

Redux reducers must be pure.

Bad:

```js
state.count++;
```

Good:

```js
return {

  ...state,

  count: state.count + 1

};
```

Reducers should:

- Never mutate state
- Always return new state

---

# Pure vs Impure

| Pure | Impure |
|-------|---------|
| Same input → same output | Different outputs possible |
| No side effects | Has side effects |
| Doesn't mutate data | Mutates data |
| Easy to test | Harder to test |
| Predictable | Less predictable |
| Safe for memoization | Unsafe for memoization |

---

# Common Side Effects

```text
Changing Global Variables

Changing Objects

Changing Arrays

HTTP Requests

DOM Manipulation

Console Logging

Database Writes

File System Access

Timers

Reading Current Time

Random Numbers
```

---

# Common Mistakes

## Mistake 1

Using external variables.

Wrong:

```js
let discount = 10;

function price(amount) {

  return amount - discount;

}
```

Better:

```js
function price(amount, discount) {

  return amount - discount;

}
```

Everything needed is passed as arguments.

---

## Mistake 2

Mutating parameters.

Wrong:

```js
function increment(user) {

  user.age++;

}
```

Better:

```js
function increment(user) {

  return {

    ...user,

    age: user.age + 1

  };

}
```

---

## Mistake 3

Using Math.random()

```js
function rollDice() {

  return Math.random();

}
```

Randomness breaks purity.

---

# Real React Example

Bad:

```js
users.push(newUser);
```

Good:

```js
setUsers([
  ...users,
  newUser
]);
```

React prefers immutable updates.

---

# Common Interview Questions

## What is a Pure Function?

A function that:

- Always returns the same output for the same inputs
- Has no side effects

---

## What is a Side Effect?

Any observable change outside the function.

Examples:

- Mutating data
- DOM updates
- API calls
- Logging
- Reading current time

---

## Is Math.random() Pure?

No.

It returns different values for the same input.

---

## Is Date.now() Pure?

No.

The output changes over time.

---

## Why Are Pure Functions Easier to Test?

Because they don't depend on external state.

---

## Why Are Pure Functions Good for Memoization?

Because identical inputs always produce identical outputs.

---

## Are React Components Pure?

Ideally yes.

Given the same props and state, they should render the same UI.

---

# Quick Revision

✅ Pure function follows two rules

✅ Same input → same output

✅ No side effects

✅ Side effects include mutation, DOM updates, API calls, timers, logging, random values

✅ Pure functions don't mutate objects or arrays

✅ Prefer immutable updates using spread syntax

✅ Pure functions are predictable

✅ Pure functions are easy to test

✅ Pure functions support memoization

✅ Redux reducers should be pure

✅ React encourages pure rendering

✅ Referential transparency allows replacing a function call with its result

✅ Functional Programming heavily relies on pure functions