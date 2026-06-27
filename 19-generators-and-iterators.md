# JavaScript Generators & Iterators

## Why This Topic Matters

JavaScript has a built-in iteration system used by:

- Arrays
- Strings
- Maps
- Sets
- for...of
- Spread operator (...)
- Destructuring

Behind all of these are:

```text
Iterables
↓
Iterators
↓
Generators
```

Generators make creating iterators much easier.

---

# What is an Iterator?

An iterator is an object that allows you to access values one at a time.

Instead of returning all values at once, it provides them on demand.

Every iterator must have:

```js
next()
```

The `next()` method returns an object:

```js
{
  value,
  done
}
```

---

# Manual Iterator Example

```js
function createCounter(max) {
  let count = 0;

  return {
    next() {
      if (count < max) {
        return {
          value: count++,
          done: false
        };
      }

      return {
        value: undefined,
        done: true
      };
    }
  };
}
```

Usage:

```js
const counter =
  createCounter(3);

console.log(counter.next());
console.log(counter.next());
console.log(counter.next());
console.log(counter.next());
```

Output:

```js
{ value: 0, done: false }
{ value: 1, done: false }
{ value: 2, done: false }
{ value: undefined, done: true }
```

---

# Understanding next()

Every call:

```js
iterator.next()
```

moves the iterator forward by one step.

Think:

```text
Book

Page 1
↓

Page 2
↓

Page 3
```

The iterator remembers where it stopped.

---

# What is an Iterable?

An iterable is any object that can produce an iterator.

Built-in iterables include:

```text
Array
String
Map
Set
TypedArray
```

Examples:

```js
const arr = [1, 2, 3];

for (const num of arr) {
  console.log(num);
}
```

Arrays work because they are iterable. :contentReference[oaicite:1]{index=1}

---

# Symbol.iterator

Objects become iterable by implementing:

```js
Symbol.iterator
```

Example:

```js
const arr = [1, 2, 3];

const iterator =
  arr[Symbol.iterator]();
```

Now:

```js
console.log(iterator.next());
```

Output:

```js
{
  value: 1,
  done: false
}
```

---

# Iterator Protocol

An object is an iterator if:

```text
It has a next() method
```

that returns:

```js
{
  value,
  done
}
```

---

# Iterable Protocol

An object is iterable if it implements:

```js
Symbol.iterator
```

which returns an iterator.

---

# What is a Generator?

A generator is a special function that can:

- Pause execution
- Return a value
- Resume from where it stopped

Generators use:

```js
function*
```

instead of:

```js
function
```

They use:

```js
yield
```

instead of `return`. :contentReference[oaicite:2]{index=2}

---

# Basic Generator

```js
function* numbers() {
  yield 1;
  yield 2;
  yield 3;
}
```

Usage:

```js
const gen =
  numbers();
```

Nothing runs yet.

Generators are lazy.

---

# Calling next()

```js
console.log(gen.next());
```

Output:

```js
{
  value: 1,
  done: false
}
```

Next call:

```js
gen.next();
```

Output:

```js
{
  value: 2,
  done: false
}
```

Third call:

```js
{
  value: 3,
  done: false
}
```

Fourth call:

```js
{
  value: undefined,
  done: true
}
```

---

# How yield Works

Example:

```js
function* demo() {

  console.log("A");

  yield 1;

  console.log("B");

  yield 2;

  console.log("C");
}
```

Execution:

```js
const gen = demo();

gen.next();
```

Output:

```text
A
```

Returns:

```js
1
```

Generator pauses.

Next:

```js
gen.next();
```

Output:

```text
B
```

Returns:

```js
2
```

Generator pauses again.

Next:

```js
gen.next();
```

Output:

```text
C
```

Generator finishes.

---

# yield vs return

`yield`

```text
Pause execution
```

Can happen multiple times.

---

`return`

```text
Finish execution
```

Only once.

---

Example:

```js
function* test() {

  yield 1;

  return 2;

  yield 3;
}
```

Output:

```js
1
2
```

The last `yield` never executes.

---

# for...of with Generators

Generators are iterable.

```js
function* numbers() {

  yield 1;
  yield 2;
  yield 3;

}
```

Usage:

```js
for (const num of numbers()) {

  console.log(num);

}
```

Output:

```js
1
2
3
```

---

# Spread Operator

Works because generators are iterable.

```js
function* numbers() {

  yield 1;
  yield 2;
  yield 3;

}
```

```js
console.log(
  [...numbers()]
);
```

Output:

```js
[1, 2, 3]
```

---

# Lazy Evaluation

Generators create values only when requested.

Example:

```js
function* ids() {

  let id = 1;

  while (true) {

    yield id++;

  }

}
```

Nothing is stored in memory.

Each value is produced only when:

```js
next()
```

is called. This is called **lazy evaluation**. :contentReference[oaicite:3]{index=3}

---

# Infinite Sequence

```js
function* infinite() {

  let i = 0;

  while (true) {

    yield i++;

  }

}
```

Usage:

```js
const gen =
  infinite();

console.log(gen.next().value);
console.log(gen.next().value);
console.log(gen.next().value);
```

Output:

```js
0
1
2
```

Unlike arrays, generators can represent infinite data safely.

---

# Fibonacci Generator

```js
function* fibonacci() {

  let a = 0;
  let b = 1;

  while (true) {

    yield a;

    [a, b] = [
      b,
      a + b
    ];

  }

}
```

Produces Fibonacci numbers one by one.

---

# Passing Values Into Generators

Generators are two-way.

```js
function* demo() {

  const name =
    yield "Enter name";

  console.log(name);

}
```

Usage:

```js
const gen =
  demo();

gen.next();

gen.next("Akio");
```

Output:

```js
Akio
```

The second `next()` sends data back into the generator.

---

# yield*

Delegates to another generator.

```js
function* first() {

  yield 1;
  yield 2;

}

function* second() {

  yield* first();

  yield 3;

}
```

Output:

```js
1
2
3
```

---

# Custom Iterable Object

```js
const range = {

  start: 1,
  end: 3,

  *[Symbol.iterator]() {

    for (
      let i = this.start;
      i <= this.end;
      i++
    ) {

      yield i;

    }

  }

};
```

Usage:

```js
for (const num of range) {

  console.log(num);

}
```

Output:

```js
1
2
3
```

Generators make implementing iterables much simpler. :contentReference[oaicite:4]{index=4}

---

# Async Generators

Use:

```js
async function*
```

instead of:

```js
function*
```

Example:

```js
async function* stream() {

  yield await fetch("/users");

}
```

Consume with:

```js
for await (
  const item of stream()
) {

  console.log(item);

}
```

Useful for streaming asynchronous data. :contentReference[oaicite:5]{index=5}

---

# Common Use Cases

Generators are useful for:

- Infinite sequences
- Pagination
- Lazy loading
- Parsing large files
- State machines
- Streaming APIs
- Tree traversal
- Unique ID generation

---

# Generator vs Normal Function

| Feature | Function | Generator |
|----------|----------|-----------|
| Executes immediately | ✅ | ❌ |
| Can pause | ❌ | ✅ |
| Multiple values | ❌ | ✅ |
| Uses return | ✅ | ❌ (yield) |
| Lazy | ❌ | ✅ |

---

# Iterator vs Generator

| Iterator | Generator |
|----------|-----------|
| Object | Special Function |
| Manual next() | Automatically creates iterator |
| More code | Less code |
| Uses next() | Uses yield |

A generator automatically returns an iterator. :contentReference[oaicite:6]{index=6}

---

# Common Mistakes

## Mistake 1

Forgetting the `*`.

Wrong:

```js
function numbers() {}
```

Correct:

```js
function* numbers() {}
```

---

## Mistake 2

Expecting generator code to run immediately.

Wrong:

```js
const gen =
  numbers();
```

No code executes yet.

Execution starts only when:

```js
gen.next();
```

---

## Mistake 3

Confusing `yield` with `return`.

`yield`

```text
Pause
```

`return`

```text
Finish
```

---

# Real React Connection

Generators are not common in React components.

However, libraries and tools may use them for:

- State machines
- Data pipelines
- Redux-Saga
- Streaming data

For example, Redux-Saga uses generator functions to describe asynchronous workflows. :contentReference[oaicite:7]{index=7}

---

# Common Interview Questions

## What is an Iterator?

An object with a `next()` method that returns:

```js
{
  value,
  done
}
```

---

## What is an Iterable?

An object implementing:

```js
Symbol.iterator
```

---

## What is a Generator?

A special function that can pause and resume using `yield`.

---

## Difference Between yield and return?

`yield`

- Pauses execution
- Can happen multiple times

`return`

- Ends execution
- Happens once

---

## Are Generators Lazy?

Yes.

Values are generated only when requested.

---

## Why Are Generators Memory Efficient?

Because they don't create all values upfront.

They produce one value at a time.

---

## What Does yield* Do?

Delegates iteration to another iterable or generator.

---

# Quick Revision

✅ Iterator has a `next()` method

✅ `next()` returns `{ value, done }`

✅ Iterable implements `Symbol.iterator`

✅ Arrays, Strings, Maps, and Sets are iterable

✅ Generator uses `function*`

✅ `yield` pauses execution

✅ `return` finishes execution

✅ Generators automatically return iterators

✅ Generators are lazy

✅ `for...of` works with generators

✅ Spread operator works with generators

✅ `yield*` delegates to another generator

✅ Async generators use `async function*`

✅ `for await...of` consumes async generators

✅ Generators are ideal for infinite sequences and lazy evaluation