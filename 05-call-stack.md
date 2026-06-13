# JavaScript Call Stack

## What is the Call Stack?

The Call Stack is a data structure used by the JavaScript engine to keep track of function execution.

It follows the:

```text
LIFO
(Last In, First Out)
```

rule.

The last function added to the stack is the first one removed. :contentReference[oaicite:0]{index=0}

---

# Why Do We Need a Call Stack?

JavaScript needs a way to know:

- Which function is currently running
- Which function called it
- Where execution should return after a function finishes

The Call Stack manages all of this. :contentReference[oaicite:1]{index=1}

---

# Execution Context

Every time JavaScript executes code, it creates an:

```text
Execution Context
```

Think of it as:

```text
A box containing:
- Variables
- Function parameters
- Scope information
- this value
```

Each execution context is pushed onto the Call Stack. :contentReference[oaicite:2]{index=2}

---

# Global Execution Context

Before any code runs, JavaScript creates:

```text
Global Execution Context (GEC)
```

Example:

```js
console.log("Hello");
```

Stack:

```text
┌───────────────┐
│ Global Context│
└───────────────┘
```

The Global Context stays until the program finishes.

---

# Simple Function Example

```js
function greet() {
  console.log("Hello");
}

greet();
```

### Step 1

Global Context created.

```text
┌───────────────┐
│ Global Context│
└───────────────┘
```

---

### Step 2

`greet()` is called.

JavaScript creates a new execution context.

```text
┌───────────────┐
│ greet()       │
├───────────────┤
│ Global Context│
└───────────────┘
```

---

### Step 3

Function completes.

`greet()` is removed.

```text
┌───────────────┐
│ Global Context│
└───────────────┘
```

---

# Multiple Function Calls

```js
function first() {
  second();
}

function second() {
  third();
}

function third() {
  console.log("Inside third");
}

first();
```

---

## Stack Evolution

### Start

```text
┌───────────────┐
│ Global Context│
└───────────────┘
```

---

### first()

```text
┌───────────────┐
│ first()       │
├───────────────┤
│ Global Context│
└───────────────┘
```

---

### second()

```text
┌───────────────┐
│ second()      │
├───────────────┤
│ first()       │
├───────────────┤
│ Global Context│
└───────────────┘
```

---

### third()

```text
┌───────────────┐
│ third()       │
├───────────────┤
│ second()      │
├───────────────┤
│ first()       │
├───────────────┤
│ Global Context│
└───────────────┘
```

---

### third() finishes

```text
┌───────────────┐
│ second()      │
├───────────────┤
│ first()       │
├───────────────┤
│ Global Context│
└───────────────┘
```

---

### second() finishes

```text
┌───────────────┐
│ first()       │
├───────────────┤
│ Global Context│
└───────────────┘
```

---

### first() finishes

```text
┌───────────────┐
│ Global Context│
└───────────────┘
```

---

# Call Stack Rules

## Function Call

Push onto stack.

```js
myFunction();
```

---

## Function Return

Remove from stack.

```js
return;
```

---

# Synchronous Execution

JavaScript executes synchronous code one operation at a time.

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

JavaScript cannot execute:

```js
A
C
B
```

because the Call Stack executes code sequentially. :contentReference[oaicite:3]{index=3}

---

# Stack Overflow

When too many function calls are added to the stack.

Example:

```js
function test() {
  test();
}

test();
```

Output:

```text
RangeError:
Maximum call stack size exceeded
```

---

# Why Stack Overflow Happens

Each call creates a new execution context.

```text
test()
test()
test()
test()
test()
test()
...
```

Eventually memory is exhausted.

---

# Recursive Function Example

Correct recursion:

```js
function countDown(n) {
  if (n === 0) return;

  console.log(n);

  countDown(n - 1);
}

countDown(3);
```

Output:

```js
3
2
1
```

---

# Stack During Recursion

```text
countDown(0)
countDown(1)
countDown(2)
countDown(3)
Global
```

As functions return, they are popped off the stack.

---

# Function Return Flow

```js
function add(a, b) {
  return a + b;
}

const result = add(5, 10);

console.log(result);
```

Execution:

```text
Global
 ↓
add()
 ↓
return 15
 ↓
Global
 ↓
console.log()
```

The stack tracks where control should return.

---

# Call Stack and Errors

Example:

```js
function a() {
  b();
}

function b() {
  c();
}

function c() {
  throw new Error("Oops");
}

a();
```

Stack Trace:

```text
Error
at c()
at b()
at a()
```

The error trace is literally showing the Call Stack.

---

# Reading Stack Traces

Example:

```text
TypeError
at calculatePrice
at processOrder
at submitOrder
```

Read from top to bottom:

```text
calculatePrice
called by processOrder
called by submitOrder
```

This is one of the most important debugging skills.

---

# Call Stack vs Memory Heap

JavaScript uses two major memory areas.

## Call Stack

Stores:

```text
Execution Contexts
Function Calls
Local Variables
```

---

## Memory Heap

Stores:

```text
Objects
Arrays
Functions
Complex Data
```

Example:

```js
const user = {
  name: "Akio"
};
```

Reference:

```text
Call Stack -> Reference
Heap -> Actual Object
```

---

# Call Stack and Async Code

Many beginners think:

```js
setTimeout(...)
```

goes directly onto the stack.

Wrong.

Example:

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

Why?

Because:

```text
Call Stack
Web APIs
Callback Queue
Event Loop
```

work together.

The callback does NOT enter the stack immediately.

This is the foundation for understanding the Event Loop. :contentReference[oaicite:4]{index=4}

---

# Real Browser Example

```js
button.addEventListener("click", () => {
  console.log("Clicked");
});
```

The callback waits outside the stack until:

```text
1. User clicks
2. Event enters queue
3. Event Loop checks stack
4. Callback pushed onto stack
```

---

# Common Interview Questions

## What is the Call Stack?

A LIFO data structure used to manage function execution.

---

## What gets stored in the Call Stack?

Execution Contexts.

---

## What happens when a function is called?

A new execution context is pushed onto the stack.

---

## What happens when a function returns?

Its execution context is removed from the stack.

---

## What causes Maximum Call Stack Size Exceeded?

Infinite recursion or excessive nested function calls.

---

## Is JavaScript Multi-threaded?

No.

JavaScript execution via the Call Stack is single-threaded. Async behavior comes from the browser/runtime environment. :contentReference[oaicite:5]{index=5}

---

## Why does a stack trace show function names?

Because it displays the order of execution contexts currently in the Call Stack.

---

# React Connection

When a React component renders:

```js
App()
```

gets pushed onto the Call Stack.

Child components:

```js
Header()
Sidebar()
Content()
```

create additional function calls.

Understanding the Call Stack helps explain:

- Rendering
- Re-rendering
- Error stacks
- Infinite render loops

---

# Quick Revision

✅ Call Stack = tracks function execution

✅ Uses LIFO (Last In, First Out)

✅ Every function call creates an Execution Context

✅ Function calls push onto stack

✅ Returns pop from stack

✅ Global Execution Context is created first

✅ JavaScript executes synchronous code sequentially

✅ Infinite recursion causes Stack Overflow

✅ Stack traces come from the Call Stack

✅ Async callbacks do not enter the stack immediately

✅ Call Stack + Event Loop = JavaScript concurrency