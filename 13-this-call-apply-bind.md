# JavaScript this, call(), apply(), bind()

## Why This Topic Matters

The `this` keyword is one of the most misunderstood concepts in JavaScript.

It appears everywhere:

- Objects
- Classes
- Event Handlers
- React Components
- DOM APIs
- call()
- apply()
- bind()

Many bugs happen because developers assume `this` refers to the object where the function was defined.

That assumption is often wrong.

---

# What is this?

`this` is a special keyword that refers to an object.

Important:

```text
this is determined at CALL TIME
not DEFINITION TIME
```

Example:

```js
const user = {
  name: "John",

  greet() {
    console.log(this.name);
  }
};

user.greet();
```

Output:

```js
John
```

Here:

```js
this === user
```

---

# The Golden Rule

Do NOT ask:

```text
Where was the function created?
```

Ask:

```text
How was the function called?
```

Because JavaScript decides `this`
when the function executes.

---

# The 5 Binding Rules

JavaScript determines `this`
using these rules (highest priority first):

```text
1. new Binding
2. Explicit Binding
3. Implicit Binding
4. Default Binding
5. Arrow Functions (Lexical this)
```

Understanding these rules solves almost every `this` question. :contentReference[oaicite:1]{index=1}

---

# Rule 1: Default Binding

Simple function call.

```js
function greet() {
  console.log(this);
}

greet();
```

---

## Non-Strict Mode

Output:

```js
window
```

Browser:

```js
this === window
```

---

## Strict Mode

```js
"use strict";

function greet() {
  console.log(this);
}

greet();
```

Output:

```js
undefined
```

---

# Rule 2: Implicit Binding

Object before the dot becomes `this`.

```js
const user = {
  name: "John",

  greet() {
    console.log(this.name);
  }
};

user.greet();
```

Output:

```js
John
```

Because:

```js
this === user
```

---

Think:

```js
object.method()
```

The object before the dot becomes `this`.

---

# Losing this

Common interview question.

```js
const user = {
  name: "John",

  greet() {
    console.log(this.name);
  }
};

const fn = user.greet;

fn();
```

Output:

```js
undefined
```

Why?

Because:

```js
fn()
```

is now a normal function call.

Implicit binding is lost. :contentReference[oaicite:2]{index=2}

---

# Rule 3: Explicit Binding

JavaScript allows us to manually set `this`.

Methods:

```js
call()
apply()
bind()
```

All three explicitly control `this`. :contentReference[oaicite:3]{index=3}

---

# call()

Immediately invokes a function.

Syntax:

```js
fn.call(thisArg, arg1, arg2);
```

---

Example:

```js
function greet() {
  console.log(this.name);
}

const user = {
  name: "John"
};

greet.call(user);
```

Output:

```js
John
```

---

# call() with Arguments

```js
function greet(city) {
  console.log(
    this.name,
    city
  );
}

const user = {
  name: "John"
};

greet.call(user, "Delhi");
```

Output:

```js
John Delhi
```

---

# apply()

Works almost like call().

Difference:

```text
Arguments are passed as an array.
```

Syntax:

```js
fn.apply(thisArg, [args]);
```

---

Example:

```js
function greet(city) {
  console.log(
    this.name,
    city
  );
}

const user = {
  name: "John"
};

greet.apply(user, ["Delhi"]);
```

Output:

```js
John Delhi
```

---

# call() vs apply()

call:

```js
fn.call(obj, a, b, c);
```

apply:

```js
fn.apply(obj, [a, b, c]);
```

---

# bind()

Unlike call() and apply():

```text
bind does NOT execute immediately
```

It returns a new function. :contentReference[oaicite:4]{index=4}

---

Example:

```js
function greet() {
  console.log(this.name);
}

const user = {
  name: "John"
};

const bound =
  greet.bind(user);

bound();
```

Output:

```js
John
```

---

# Visual Difference

call:

```js
greet.call(user);
```

Runs immediately.

---

apply:

```js
greet.apply(user);
```

Runs immediately.

---

bind:

```js
const fn =
  greet.bind(user);

fn();
```

Runs later.

---

# Common React Example

Without bind:

```js
class App {
  constructor() {
    this.name = "John";
  }

  click() {
    console.log(this.name);
  }
}

button.onclick =
  app.click;
```

Problem:

```js
this lost
```

---

Solution:

```js
button.onclick =
  app.click.bind(app);
```

---

# Method Borrowing

A very common use of call/apply.

```js
const person1 = {
  name: "John"
};

const person2 = {
  name: "Alex"
};

function greet() {
  console.log(this.name);
}
```

---

Borrow:

```js
greet.call(person1);
```

Output:

```js
John
```

---

Borrow:

```js
greet.call(person2);
```

Output:

```js
Alex
```

---

# Rule 4: new Binding

When using:

```js
new
```

JavaScript creates a new object and binds `this` to it. :contentReference[oaicite:5]{index=5}

---

Example:

```js
function User(name) {
  this.name = name;
}

const user =
  new User("John");

console.log(user.name);
```

Output:

```js
John
```

---

Internally:

```text
1. Create object
2. Bind this
3. Execute constructor
4. Return object
```

---

# Rule 5: Arrow Functions

Arrow functions behave differently.

They do NOT have their own `this`. :contentReference[oaicite:6]{index=6}

Instead:

```text
They inherit this
from the surrounding scope.
```

This is called:

```text
Lexical this
```

---

Example:

```js
const user = {
  name: "John",

  greet() {
    const arrow = () => {
      console.log(this.name);
    };

    arrow();
  }
};

user.greet();
```

Output:

```js
John
```

Arrow uses surrounding `this`.

---

# Arrow Function Gotcha

Bad:

```js
const user = {
  name: "John",

  greet: () => {
    console.log(this.name);
  }
};

user.greet();
```

Output:

```js
undefined
```

Why?

Arrow doesn't create its own `this`.

It inherits from outer scope.

---

# Arrow vs Regular Function

Regular:

```js
function() {}
```

Has its own:

```js
this
```

---

Arrow:

```js
() => {}
```

Uses surrounding:

```js
this
```

---

# Event Handler Example

Regular function:

```js
button.addEventListener(
  "click",
  function() {
    console.log(this);
  }
);
```

Output:

```js
button element
```

---

Arrow:

```js
button.addEventListener(
  "click",
  () => {
    console.log(this);
  }
);
```

Output:

```js
outer scope this
```

Not button.

---

# call(), apply(), bind() and Arrow Functions

Arrow functions ignore:

```js
call()
apply()
bind()
```

---

Example:

```js
const greet = () => {
  console.log(this);
};

greet.call(user);
```

Output:

```js
unchanged
```

Because arrow functions do not create their own `this`. :contentReference[oaicite:7]{index=7}

---

# Binding Priority

Highest → Lowest

```text
1. new
2. call/apply/bind
3. object.method()
4. normal function
```

Arrow functions are special.

They use lexical binding instead. :contentReference[oaicite:8]{index=8}

---

# Common Interview Questions

## What is this?

A reference determined by how a function is called.

---

## Does this depend on where a function is defined?

No.

It depends on how the function is invoked.

---

## Difference Between call and apply?

call:

```js
fn.call(obj, a, b);
```

apply:

```js
fn.apply(obj, [a, b]);
```

---

## Difference Between call and bind?

call:

```js
Immediate execution
```

bind:

```js
Returns new function
```

---

## Can bind be changed later?

No.

Once bound:

```js
const fn =
  greet.bind(user);
```

`this` is permanently fixed.

---

## Why Do Arrow Functions Not Have this?

They inherit `this`
from the surrounding lexical scope. :contentReference[oaicite:9]{index=9}

---

# Interview Output Questions

## Question 1

```js
const user = {
  name: "John",

  greet() {
    console.log(this.name);
  }
};

const fn =
  user.greet;

fn();
```

Output:

```js
undefined
```

---

## Question 2

```js
function greet() {
  console.log(this.name);
}

const user = {
  name: "John"
};

greet.call(user);
```

Output:

```js
John
```

---

## Question 3

```js
const user = {
  name: "John",

  greet: () => {
    console.log(this.name);
  }
};

user.greet();
```

Output:

```js
undefined
```

---

# Quick Revision

✅ `this` is determined at call time

✅ Default binding → globalThis/undefined

✅ Implicit binding → object before dot

✅ Explicit binding → call/apply/bind

✅ new binding → newly created object

✅ Arrow functions have lexical `this`

✅ call() executes immediately

✅ apply() executes immediately with array args

✅ bind() returns a new function

✅ Arrow functions ignore call/apply/bind

✅ Losing object reference often loses `this`

✅ `new` has highest binding priority