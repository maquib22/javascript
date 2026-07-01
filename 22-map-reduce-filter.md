# JavaScript map(), filter(), reduce()

## Why These Methods Matter

When working with arrays, there are three common operations:

```text
Transform Data
Filter Data
Combine Data
```

JavaScript provides three powerful methods for these tasks:

```text
map()
filter()
reduce()
```

They are:

- Higher-Order Functions
- Non-mutating
- Chainable
- Widely used in React and modern JavaScript

---

# The Factory Assembly Line

Think of these methods as different stations in a factory.

```text
Input Array
      │
      ▼
   map()
Transform every item
      │
      ▼
  filter()
Keep only matching items
      │
      ▼
  reduce()
Combine everything into one value
```

Each method has one responsibility.

---

# Overview

| Method | Purpose | Returns |
|---------|----------|----------|
| map() | Transform every element | New Array |
| filter() | Keep matching elements | New Array |
| reduce() | Combine into one value | Any Value |

---

# map()

## What is map()?

`map()` transforms every element of an array.

It:

- Visits every item
- Applies a callback
- Returns a new array

Original array is NOT modified.

---

# Syntax

```js
array.map(callback)
```

Callback receives:

```js
(element, index, array)
```

---

# Basic Example

```js
const numbers =
  [1, 2, 3];
```

```js
const doubled =
  numbers.map(num => num * 2);
```

Output:

```js
[2, 4, 6]
```

Original:

```js
[1, 2, 3]
```

unchanged.

---

# Extracting Properties

```js
const users = [

  {
    id: 1,
    name: "Akio"
  },

  {
    id: 2,
    name: "John"
  }

];
```

```js
const names =
  users.map(user => user.name);
```

Output:

```js
["Akio", "John"]
```

---

# Transforming Objects

```js
const updated =
  users.map(user => ({
    ...user,
    active: true
  }));
```

Output:

```js
[
  {
    id: 1,
    name: "Akio",
    active: true
  },
  {
    id: 2,
    name: "John",
    active: true
  }
]
```

---

# Using Index

```js
const result =
  ["A", "B", "C"]

.map((value, index) => {

  return `${index}: ${value}`;

});
```

Output:

```js
[
  "0: A",
  "1: B",
  "2: C"
]
```

---

# filter()

## What is filter()?

`filter()` keeps only elements that satisfy a condition.

The callback must return:

```text
true
or
false
```

---

# Syntax

```js
array.filter(callback)
```

---

# Basic Example

```js
const numbers =
  [1, 2, 3, 4, 5];
```

```js
const even =
  numbers.filter(
    num => num % 2 === 0
  );
```

Output:

```js
[2, 4]
```

---

# Filtering Objects

```js
const users = [

  {
    name: "Akio",
    active: true
  },

  {
    name: "John",
    active: false
  }

];
```

```js
const activeUsers =
  users.filter(
    user => user.active
  );
```

Output:

```js
[
  {
    name: "Akio",
    active: true
  }
]
```

---

# Truthy & Falsy Filtering

```js
const values = [

  0,
  "",
  false,
  null,
  "Hello"

];
```

```js
const result =
  values.filter(Boolean);
```

Output:

```js
["Hello"]
```

Useful for removing falsy values.

---

# reduce()

## What is reduce()?

`reduce()` combines all array elements into a single value.

Examples:

- Sum
- Average
- Maximum
- Object
- Array
- Count
- Grouping

Unlike `map()` and `filter()`, `reduce()` can return ANY type.

---

# Syntax

```js
array.reduce(
  callback,
  initialValue
);
```

Callback receives:

```js
(accumulator,
 currentValue,
 index,
 array)
```

---

# Basic Example

```js
const numbers =
  [1, 2, 3, 4];
```

```js
const total =
  numbers.reduce(

    (sum, num) => {

      return sum + num;

    },

    0

  );
```

Output:

```js
10
```

---

# Understanding Accumulator

Iteration:

```text
Start

Accumulator = 0

↓

0 + 1 = 1

↓

1 + 2 = 3

↓

3 + 3 = 6

↓

6 + 4 = 10
```

Final value:

```js
10
```

---

# Finding Maximum

```js
const max =
  numbers.reduce(

    (largest, num) => {

      return Math.max(
        largest,
        num
      );

    },

    numbers[0]

  );
```

Output:

```js
4
```

---

# Counting Items

```js
const fruits = [

  "apple",

  "banana",

  "apple"

];
```

```js
const count =
  fruits.reduce(

    (acc, fruit) => {

      acc[fruit] =
        (acc[fruit] || 0) + 1;

      return acc;

    },

    {}

  );
```

Output:

```js
{
  apple: 2,
  banana: 1
}
```

---

# Grouping Data

```js
const users = [

  {
    name: "Akio",
    city: "Delhi"
  },

  {
    name: "John",
    city: "Mumbai"
  },

  {
    name: "Alex",
    city: "Delhi"
  }

];
```

```js
const grouped =
  users.reduce(

    (acc, user) => {

      if (!acc[user.city]) {

        acc[user.city] = [];

      }

      acc[user.city].push(user);

      return acc;

    },

    {}

  );
```

Output:

```js
{
  Delhi: [...],
  Mumbai: [...]
}
```

---

# Method Chaining

The real power comes from combining them.

Example:

```js
const users = [

  {
    name: "Akio",
    active: true,
    salary: 100
  },

  {
    name: "John",
    active: false,
    salary: 200
  },

  {
    name: "Alex",
    active: true,
    salary: 300
  }

];
```

```js
const totalSalary =
  users

    .filter(user => user.active)

    .map(user => user.salary)

    .reduce(
      (sum, salary) => sum + salary,
      0
    );
```

Output:

```js
400
```

Pipeline:

```text
Users

↓

filter()

↓

map()

↓

reduce()

↓

Result
```

This is one of the most common real-world patterns. :contentReference[oaicite:1]{index=1}

---

# map() vs forEach()

## map()

```js
const result =
  numbers.map(
    num => num * 2
  );
```

Returns:

```text
New Array
```

---

## forEach()

```js
numbers.forEach(
  num => console.log(num)
);
```

Returns:

```js
undefined
```

Use:

```text
map()

when you need a new array.

forEach()

for side effects.
```

---

# filter() vs find()

```js
const users = [

  {
    id: 1
  },

  {
    id: 2
  }

];
```

---

filter:

```js
users.filter(
  user => user.id === 2
);
```

Output:

```js
[
  {
    id: 2
  }
]
```

---

find:

```js
users.find(
  user => user.id === 2
);
```

Output:

```js
{
  id: 2
}
```

---

# some() vs every()

```js
numbers.some(
  num => num > 3
);
```

Returns:

```js
true
```

if ANY element matches.

---

```js
numbers.every(
  num => num > 0
);
```

Returns:

```js
true
```

only if ALL elements match.

---

# Common Mistakes

## Mistake 1

Using map() without returning.

Wrong:

```js
numbers.map(num => {

  num * 2;

});
```

Output:

```js
[
  undefined,
  undefined,
  undefined
]
```

Correct:

```js
numbers.map(num => {

  return num * 2;

});
```

---

## Mistake 2

Forgetting reduce() initial value.

Wrong:

```js
[].reduce(
  (a, b) => a + b
);
```

Output:

```text
TypeError
```

Always provide an initial value unless you intentionally rely on the first array element. :contentReference[oaicite:2]{index=2}

---

## Mistake 3

Using filter() when map() is needed.

Bad:

```js
numbers.filter(
  num => num * 2
);
```

`filter()` expects:

```text
true / false
```

Not transformed values.

---

## Mistake 4

Mutating objects inside map().

Bad:

```js
users.map(user => {

  user.active = true;

  return user;

});
```

Better:

```js
users.map(user => ({

  ...user,

  active: true

}));
```

---

# Performance

Each method creates a new array.

Example:

```js
users

.filter(...)

.map(...)

.filter(...);
```

Creates intermediate arrays.

For most applications:

```text
Readability
>
Micro-optimization
```

Optimize only after measuring performance.

---

# Real React Example

```js
users

.filter(user => user.active)

.map(user => (

  <UserCard

    key={user.id}

    user={user}

  />

));
```

This is one of the most common React patterns.

---

# Common Interview Questions

## What does map() return?

A new array with transformed elements.

---

## Does map() modify the original array?

No.

---

## What does filter() return?

A new array containing only matching elements.

---

## What does reduce() return?

Any value.

Examples:

- Number
- Object
- Array
- String
- Boolean

---

## Difference Between map() and forEach()?

`map()`

Returns a new array.

`forEach()`

Returns `undefined`.

---

## Difference Between filter() and find()?

`filter()`

Returns an array.

`find()`

Returns the first matching element (or `undefined`).

---

## Why Should reduce() Have an Initial Value?

Without one:

- Empty arrays throw a `TypeError`
- The first array element becomes the initial accumulator, which can lead to unexpected behavior. :contentReference[oaicite:3]{index=3}

---

# Quick Revision

✅ `map()` transforms every element

✅ `filter()` keeps matching elements

✅ `reduce()` combines into one value

✅ All three are Higher-Order Functions

✅ All three accept callbacks

✅ Original array is not modified

✅ `map()` returns a new array

✅ `filter()` returns a new array

✅ `reduce()` can return any type

✅ `map()` is for transformation

✅ `filter()` is for selection

✅ `reduce()` is for aggregation

✅ Method chaining creates powerful data pipelines

✅ Always provide an initial value to `reduce()`

✅ React uses `map()` extensively for rendering lists