# JavaScript Engines

## Why JavaScript Engines Matter

When you write:

```js
console.log("Hello World");
```

Your computer does **NOT** understand JavaScript.

The CPU only understands:

```text
Machine Code

(Binary)

0s and 1s
```

A JavaScript Engine converts JavaScript into machine code so your CPU can execute it.

Without a JavaScript Engine:

```text
JavaScript cannot run.
```

---

# What is a JavaScript Engine?

A JavaScript Engine is a program that:

```text
Reads JavaScript

↓

Parses It

↓

Compiles It

↓

Optimizes It

↓

Executes It
```

Examples:

- Google Chrome
- Node.js
- Firefox
- Safari
- Edge

All contain JavaScript engines.

---

# Popular JavaScript Engines

| Engine | Used By |
|---------|----------|
| V8 | Chrome, Node.js, Deno |
| SpiderMonkey | Firefox |
| JavaScriptCore | Safari, Bun |
| Chakra (Legacy) | Old Microsoft Edge |

Modern engines all implement the ECMAScript specification but have different internal optimizations. :contentReference[oaicite:1]{index=1}

---

# JavaScript Engine vs JavaScript Runtime

Many developers confuse these.

JavaScript Engine:

```text
Executes JavaScript
```

Examples:

```text
V8

SpiderMonkey

JavaScriptCore
```

---

JavaScript Runtime:

Provides extra features like:

```text
Web APIs

DOM

Timers

Fetch

File System

Networking
```

Example:

```text
Chrome Runtime

↓

V8 Engine

+

Browser APIs
```

Node.js Runtime:

```text
V8 Engine

+

Node APIs
```

The engine executes JavaScript, while the runtime provides the environment around it. :contentReference[oaicite:2]{index=2}

---

# High-Level Flow

```text
JavaScript Source Code

↓

Parser

↓

Abstract Syntax Tree (AST)

↓

Interpreter

↓

Bytecode

↓

Profiler

↓

Optimizing Compiler

↓

Optimized Machine Code

↓

CPU
```

This pipeline varies by engine, but modern engines generally follow this architecture. :contentReference[oaicite:3]{index=3}

---

# Step 1: Parsing

Engine reads your code.

Example:

```js
const age = 25;
```

The parser checks:

- Syntax
- Grammar
- Missing brackets
- Missing semicolons (where relevant)

If syntax is invalid:

```text
SyntaxError
```

Program stops.

---

# Step 2: Abstract Syntax Tree (AST)

Parser converts code into a tree.

Example:

```js
2 + 3
```

AST:

```text
      +

     / \

    2   3
```

JavaScript engines don't execute raw text.

They execute structured representations.

---

# Step 3: Interpreter

Modern engines first generate:

```text
Bytecode
```

Bytecode is:

```text
Faster than source code

Slower than machine code
```

The interpreter starts executing bytecode immediately.

This helps applications start quickly.

---

# Step 4: Profiling

While code executes, the engine observes:

```text
Which functions run often?

↓

Which loops are hot?

↓

Which objects have stable shapes?
```

Frequently executed code is called:

```text
Hot Code
```

---

# Step 5: Optimizing Compiler

Hot code is compiled into highly optimized machine code.

Example:

```text
Bytecode

↓

Optimized Machine Code
```

Now execution becomes much faster.

This optimization process is one reason JavaScript performance has improved dramatically. :contentReference[oaicite:4]{index=4}

---

# Just-In-Time (JIT) Compilation

Modern JavaScript uses:

```text
JIT Compilation
```

Instead of:

```text
Pure Interpretation
```

or

```text
Ahead-of-Time Compilation
```

---

# Compilation vs Interpretation

## Compiler

```text
Entire Program

↓

Machine Code

↓

Execute
```

Examples:

```text
C

C++
```

---

## Interpreter

```text
Read One Line

↓

Execute

↓

Repeat
```

Historically:

```text
JavaScript
```

---

## JIT Compiler

Modern JavaScript:

```text
Parse

↓

Interpret

↓

Observe

↓

Optimize

↓

Machine Code
```

Best of both worlds.

---

# Why Not Compile Everything?

Imagine compiling:

```js
function unused() {}
```

that never executes.

Waste of time.

Instead:

```text
Compile only hot code.
```

This improves startup performance.

---

# V8 Architecture (Simplified)

V8 (Chrome & Node.js) uses multiple stages.

```text
JavaScript

↓

Parser

↓

AST

↓

Ignition

(Bytecode Interpreter)

↓

Profiler

↓

Sparkplug

↓

Maglev

↓

TurboFan

↓

Machine Code
```

V8 now uses several optimization tiers rather than a single compiler. :contentReference[oaicite:5]{index=5}

---

# Hidden Classes

JavaScript objects are dynamic.

Example:

```js
const user = {
  name: "Akio",
  age: 25
};
```

The engine internally creates a hidden structure describing the object's layout.

Objects with the same property order can share this internal representation.

This makes property access much faster.

---

# Good Object Shape

```js
const a = {
  name: "John",
  age: 20
};

const b = {
  name: "Alex",
  age: 25
};
```

Same structure.

Good optimization.

---

# Bad Object Shape

```js
const a = {
  name: "John"
};

a.age = 20;
```

Later changing object structure may force the engine to update its internal representation.

Frequent shape changes can reduce optimization opportunities.

---

# Inline Caching

Suppose:

```js
user.name
```

runs millions of times.

Instead of repeatedly searching:

```text
Object

↓

Property

↓

Memory
```

The engine caches where the property is located.

Next access becomes much faster.

---

# Deoptimization

Sometimes optimized assumptions become invalid.

Example:

```js
function add(a, b) {
  return a + b;
}

add(1, 2);
add(3, 4);
```

Engine assumes:

```text
Numbers
```

Later:

```js
add("A", "B");
```

Different types.

Engine may discard optimized code and fall back to a slower version before optimizing again.

This process is called:

```text
Deoptimization
```

---

# Memory Heap

Objects live in:

```text
Memory Heap
```

Example:

```js
const user = {
  name: "Akio"
};
```

Stored in:

```text
Heap
```

---

# Call Stack

Function execution happens in:

```text
Call Stack
```

Example:

```js
function one() {
  two();
}

function two() {}

one();
```

Stack:

```text
one()

↓

two()
```

When finished:

```text
Pop

↓

Pop
```

---

# Garbage Collection

Unused objects consume memory.

Example:

```js
let user = {
  name: "Akio"
};

user = null;
```

Now:

```text
No references remain.
```

The engine can reclaim that memory automatically.

Modern engines use advanced garbage collection strategies, including generational garbage collection. :contentReference[oaicite:6]{index=6}

---

# Engine Optimizations

Modern engines perform many optimizations:

- Hidden Classes
- Inline Caching
- JIT Compilation
- Dead Code Elimination
- Constant Folding
- Inlining
- Escape Analysis (engine-dependent)

Developers don't need to implement these manually.

---

# Browser vs Node.js

Browser:

```text
HTML

↓

CSS

↓

JavaScript

↓

DOM
```

---

Node.js:

```text
JavaScript

↓

V8

↓

Node APIs

↓

Operating System
```

Same engine.

Different runtime.

---

# Real React Connection

Every React component:

```js
function App() {
  return <h1>Hello</h1>;
}
```

passes through:

```text
Parser

↓

AST

↓

Bytecode

↓

Optimized Machine Code
```

before execution.

---

# Common Mistakes

## Mistake 1

Thinking JavaScript is purely interpreted.

Modern engines use:

```text
JIT Compilation
```

---

## Mistake 2

Thinking V8 is the browser.

Wrong.

Chrome consists of:

```text
Browser

↓

V8

+

Blink

+

Browser APIs
```

V8 is only the JavaScript engine.

---

## Mistake 3

Confusing Engine with Runtime.

Engine:

```text
Executes JavaScript
```

Runtime:

```text
Provides APIs
```

---

# Common Interview Questions

## What is a JavaScript Engine?

Software that parses, compiles, optimizes, and executes JavaScript code.

---

## What does V8 do?

Executes JavaScript using a multi-stage JIT pipeline.

It powers:

- Chrome
- Node.js
- Deno

---

## What is JIT Compilation?

A technique that starts execution quickly and optimizes frequently executed code into machine code during runtime.

---

## What is an AST?

An Abstract Syntax Tree representing the program's structure.

---

## What is Bytecode?

An intermediate representation between source code and machine code.

---

## What is Hot Code?

Frequently executed code that the engine chooses to optimize.

---

## What is Deoptimization?

Discarding optimized machine code when runtime assumptions become invalid.

---

## What is Garbage Collection?

Automatic memory cleanup for objects that are no longer reachable.

---

## Difference Between Engine and Runtime?

Engine:

```text
Runs JavaScript
```

Runtime:

```text
Provides APIs and execution environment
```

---

# Quick Revision

✅ JavaScript engines execute JavaScript

✅ CPU understands only machine code

✅ Engines parse source into an AST

✅ Modern engines generate bytecode first

✅ Frequently executed code becomes optimized machine code

✅ Modern engines use JIT compilation

✅ V8 powers Chrome, Node.js, and Deno

✅ SpiderMonkey powers Firefox

✅ JavaScriptCore powers Safari and Bun

✅ Objects live in the Memory Heap

✅ Functions execute on the Call Stack

✅ Garbage Collection frees unused memory

✅ Hidden Classes improve object property access

✅ Inline Caching speeds up repeated property lookups

✅ JavaScript Engine ≠ JavaScript Runtime