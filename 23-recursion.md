# JavaScript Recursion

## Why Recursion Matters

Many real-world problems have a structure where:

```text
A big problem

↓

Can be divided into

↓

Smaller versions
of the same problem
```

Instead of using loops, recursion solves these problems naturally.

Common examples:

- Folder traversal
- DOM traversal
- Binary Trees
- Graphs
- JSON Parsing
- Fibonacci
- Merge Sort
- Quick Sort

---

# What is Recursion?

Recursion is a technique where:

```text
A function calls itself
```

to solve a smaller version of the same problem.

A recursive function always needs:

```text
1. Base Case

2. Recursive Case
```

Without both, recursion doesn't work.

---

# Thinking Recursively

Instead of asking:

```text
How do I solve
the entire problem?
```

Ask:

```text
How do I solve

ONE small part

and let recursion
solve the rest?
```

This mindset is the key to recursion.

---

# Basic Example

```js
function countDown(n) {

  if (n === 0) {
    console.log("Done");
    return;
  }

  console.log(n);

  countDown(n - 1);

}
```

Usage:

```js
countDown(3);
```

Output:

```text
3
2
1
Done
```

---

# Understanding the Base Case

This line:

```js
if (n === 0)
```

is called the:

```text
Base Case
```

It tells recursion:

```text
Stop here.
```

Without it:

```js
countDown(-1)

↓

countDown(-2)

↓

countDown(-3)
```

Forever.

---

# Recursive Case

This line:

```js
countDown(n - 1);
```

is called the:

```text
Recursive Case
```

It makes the function call itself with a smaller problem.

---

# Visualizing the Call Stack

```js
countDown(3);
```

Call Stack:

```text
countDown(3)

↓

countDown(2)

↓

countDown(1)

↓

countDown(0)

↓

Return

↓

Return

↓

Return

↓

Done
```

Every recursive call creates a new stack frame.

---

# Factorial

One of the classic recursion problems.

Formula:

```text
5!

=

5 × 4 × 3 × 2 × 1
```

Recursive relationship:

```text
n!

=

n × (n-1)!
```

---

Example:

```js
function factorial(n) {

  if (n === 0) {
    return 1;
  }

  return n *
    factorial(n - 1);

}
```

Usage:

```js
factorial(5);
```

Output:

```js
120
```

---

# How Factorial Works

```text
factorial(5)

↓

5 × factorial(4)

↓

5 × 4 × factorial(3)

↓

5 × 4 × 3 × factorial(2)

↓

5 × 4 × 3 × 2 × factorial(1)

↓

5 × 4 × 3 × 2 × 1
```

---

# Fibonacci

Recursive definition:

```text
F(n)

=

F(n-1)

+

F(n-2)
```

---

Example:

```js
function fibonacci(n) {

  if (n <= 1) {
    return n;
  }

  return fibonacci(n - 1)
       + fibonacci(n - 2);

}
```

Output:

```js
fibonacci(6);
```

```js
8
```

---

# Recursive Tree

```text
fib(5)

      5

     / \

    4   3

   / \ / \

  3  2 2  1

...
```

Notice:

```text
Many repeated calculations.
```

This is why recursive Fibonacci is inefficient.

---

# Recursion vs Iteration

Factorial using loop:

```js
function factorial(n) {

  let result = 1;

  for (
    let i = 1;
    i <= n;
    i++
  ) {

    result *= i;

  }

  return result;

}
```

Same answer.

Different approach.

---

# Recursive Sum

```js
function sum(n) {

  if (n === 1) {
    return 1;
  }

  return n +
    sum(n - 1);

}
```

Usage:

```js
sum(5);
```

Output:

```js
15
```

---

# Traversing Arrays

```js
function print(arr, index = 0) {

  if (index === arr.length)
    return;

  console.log(arr[index]);

  print(arr, index + 1);

}
```

Output:

```text
Each element printed.
```

---

# Recursive Object Traversal

```js
const person = {

  name: "Akio",

  address: {

    city: "Delhi",

    zip: 110001

  }

};
```

```js
function traverse(obj) {

  for (const key in obj) {

    if (
      typeof obj[key] === "object"
    ) {

      traverse(obj[key]);

    } else {

      console.log(obj[key]);

    }

  }

}
```

Useful for deeply nested objects.

---

# DOM Traversal

DOM is a tree.

Example:

```text
Document

↓

Body

↓

Div

↓

Button
```

Recursive traversal:

```js
function walk(node) {

  console.log(node);

  node.childNodes.forEach(
    walk
  );

}
```

Tree structures naturally fit recursion.

---

# Directory Structure

```text
Project

├── src

│   ├── components

│   └── utils

└── assets
```

Recursive algorithm:

```text
Visit folder

↓

Visit every child

↓

Repeat
```

---

# Stack Overflow

Bad:

```js
function hello() {

  hello();

}
```

Output:

```text
RangeError:

Maximum call stack size exceeded
```

Every recursive call adds a new stack frame.

Eventually memory runs out.

---

# Tail Recursion

Example:

```js
function factorial(
  n,
  result = 1
) {

  if (n === 0)
    return result;

  return factorial(
    n - 1,
    result * n
  );

}
```

Called:

```text
Tail Recursion
```

Some languages optimize this.

JavaScript engines generally do **not** reliably optimize tail recursion in practice, so don't assume it prevents stack overflows. :contentReference[oaicite:1]{index=1}

---

# Recursive Backtracking

Used in:

- Sudoku
- Maze solving
- N Queens
- Word Search

Pattern:

```text
Choose

↓

Explore

↓

Undo

↓

Try next choice
```

---

# Divide and Conquer

Many algorithms use recursion.

Examples:

```text
Merge Sort

Quick Sort

Binary Search
```

Each divides the problem into smaller pieces.

---

# When to Use Recursion

Good for:

- Trees
- Graphs
- Nested Objects
- Nested Arrays
- Divide & Conquer
- Backtracking

---

Not always best for:

- Simple counting
- Basic loops
- Very deep recursion

Loops are often simpler and avoid stack overflow.

---

# Recursion vs Loop

| Loop | Recursion |
|------|-----------|
| Uses iteration | Function calls itself |
| Constant stack space | Uses Call Stack |
| Usually faster | Often easier to express tree problems |
| Great for arrays | Great for trees and nested structures |

---

# Common Mistakes

## Mistake 1

No base case.

Wrong:

```js
function test(n) {

  test(n - 1);

}
```

Infinite recursion.

---

## Mistake 2

Base case unreachable.

Wrong:

```js
if (n < 0)
```

when recursion never reaches negative numbers.

---

## Mistake 3

Not reducing the problem.

Wrong:

```js
function test(n) {

  return test(n);

}
```

Input never changes.

Infinite recursion.

---

## Mistake 4

Ignoring stack limits.

Very deep recursion can cause:

```text
Maximum call stack size exceeded
```

---

# Real React Connection

Recursion is often used for rendering nested components.

Example:

```text
Comments

└── Replies

    └── Replies

        └── Replies
```

Recursive component:

```js
<Comment>

  <Comment>

    <Comment />

  </Comment>

</Comment>
```

File explorers, menus, and comment threads commonly use recursive rendering.

---

# Common Interview Questions

## What is recursion?

A function calling itself to solve a smaller version of the same problem.

---

## What is a base case?

The stopping condition that prevents infinite recursion.

---

## Why is recursion useful?

It naturally solves problems with recursive structures like trees and nested data.

---

## What happens without a base case?

Infinite recursion leading to:

```text
Maximum call stack size exceeded
```

---

## Difference Between Recursion and Iteration?

Recursion uses function calls.

Iteration uses loops.

---

## What is Stack Overflow?

Too many recursive calls fill the call stack.

---

## When should recursion be preferred?

Tree traversal, nested data, divide & conquer, and backtracking problems.

---

# Quick Revision

✅ Recursion = function calls itself

✅ Every recursive function needs a base case

✅ Base case stops recursion

✅ Recursive case reduces the problem

✅ Each call creates a new stack frame

✅ Missing base case causes stack overflow

✅ Trees and nested objects are ideal for recursion

✅ DOM traversal commonly uses recursion

✅ Factorial and Fibonacci are classic examples

✅ Divide & Conquer algorithms use recursion

✅ JavaScript generally does not optimize tail recursion

✅ Loops are often better for simple iteration

✅ Recursion shines for self-similar problems