# JavaScript Factories & Classes

## Why This Topic Matters

Imagine creating 1000 users manually:

```js
const user1 = {
  name: "John",
  login() {
    console.log("Logged In");
  }
};

const user2 = {
  name: "Alex",
  login() {
    console.log("Logged In");
  }
};
```

This quickly becomes impossible to maintain.

We need:

```text
Blueprints
```

for creating objects.

JavaScript provides:

1. Factory Functions
2. Constructor Functions
3. Classes

All solve the same problem differently. :contentReference[oaicite:1]{index=1}

---

# What is a Factory Function?

A factory function is a normal function that creates and returns a new object. :contentReference[oaicite:2]{index=2}

---

# Basic Factory Function

```js
function createUser(name) {
  return {
    name,

    login() {
      console.log(`${this.name} logged in`);
    }
  };
}
```

Usage:

```js
const user1 = createUser("John");
const user2 = createUser("Alex");
```

---

Output:

```js
user1.login();
```

```text
John logged in
```

---

# Why Called Factory?

Think of a factory:

```text
Raw Materials
    ↓
Factory
    ↓
Finished Product
```

Input:

```js
"John"
```

Output:

```js
{
  name: "John"
}
```

---

# Factory Creates New Objects

```js
const user1 = createUser("John");
const user2 = createUser("John");
```

Even though data is same:

```js
console.log(user1 === user2);
```

Output:

```js
false
```

Different objects.

---

# Factory with Closures

Factories often use closures for private data. :contentReference[oaicite:3]{index=3}

```js
function createCounter() {
  let count = 0;

  return {
    increment() {
      count++;
    },

    getCount() {
      return count;
    }
  };
}
```

---

Usage:

```js
const counter = createCounter();

counter.increment();

console.log(
  counter.getCount()
);
```

Output:

```js
1
```

---

Direct access:

```js
counter.count
```

Output:

```js
undefined
```

Private variable.

---

# Benefits of Factory Functions

## Simple Syntax

```js
const user =
  createUser("John");
```

No `new` keyword.

---

## Closures

Easy private state.

```js
let secret = "123";
```

Cannot be accessed directly.

---

## Flexible Composition

Factories work well with:

```text
Closures
Mixins
Composition
```

---

# Drawback of Factory Functions

Each object gets its own copy of methods. :contentReference[oaicite:4]{index=4}

Example:

```js
function createUser(name) {
  return {
    name,

    login() {
      console.log("Login");
    }
  };
}
```

Every object creates:

```js
login()
```

again.

---

Memory usage:

```text
User1 -> login()
User2 -> login()
User3 -> login()
```

Many copies.

---

# Constructor Functions

Before ES6 classes existed:

```js
function User(name) {
  this.name = name;
}
```

Usage:

```js
const user =
  new User("John");
```

---

# What Does new Do?

The `new` keyword performs four steps. :contentReference[oaicite:5]{index=5}

```text
1. Creates empty object
2. Sets prototype
3. Binds this
4. Returns object
```

---

Equivalent Conceptually

```js
const obj = {};

obj.name = "John";

return obj;
```

---

# Constructor Example

```js
function User(name) {
  this.name = name;

  this.login = function() {
    console.log("Login");
  };
}
```

Usage:

```js
const user =
  new User("John");
```

---

# Problem with Constructors

Same problem:

```js
this.login = function() {}
```

Creates a new function for every object.

---

# Prototype Solution

```js
function User(name) {
  this.name = name;
}

User.prototype.login =
  function() {
    console.log("Login");
  };
```

Now:

```text
One shared method
Many instances
```

---

# ES6 Classes

Classes were introduced in ES6.

They provide cleaner syntax for constructor functions and prototypes. Classes are often described as "syntactic sugar" over JavaScript's prototype system. :contentReference[oaicite:6]{index=6}

---

# Basic Class

```js
class User {
  constructor(name) {
    this.name = name;
  }

  login() {
    console.log(
      `${this.name} logged in`
    );
  }
}
```

---

Usage:

```js
const user =
  new User("John");
```

---

Output:

```js
user.login();
```

```text
John logged in
```

---

# Behind The Scenes

This:

```js
class User {}
```

roughly becomes:

```js
function User() {}

User.prototype.login =
  function() {};
```

Classes are syntax over prototypes. :contentReference[oaicite:7]{index=7}

---

# Class Methods Are Shared

```js
class User {
  login() {}
}
```

Memory:

```text
User.prototype
       ↑
       │
User1
User2
User3
```

Only one copy exists.

---

# Constructor Method

Runs automatically when:

```js
new User()
```

is executed.

---

Example:

```js
class User {
  constructor(name) {
    this.name = name;
  }
}
```

---

# Static Methods

Belong to the class itself.

```js
class MathUtil {
  static add(a, b) {
    return a + b;
  }
}
```

Usage:

```js
MathUtil.add(2, 3);
```

Output:

```js
5
```

---

Wrong:

```js
const math =
  new MathUtil();

math.add();
```

Error.

---

# Getters

```js
class User {
  constructor(name) {
    this._name = name;
  }

  get name() {
    return this._name;
  }
}
```

Usage:

```js
user.name
```

Not:

```js
user.name()
```

---

# Setters

```js
class User {
  set name(value) {
    this._name = value;
  }
}
```

Usage:

```js
user.name = "Alex";
```

---

# Private Fields

Modern JavaScript supports:

```js
#
```

for private properties. :contentReference[oaicite:8]{index=8}

```js
class User {
  #password;

  constructor(password) {
    this.#password =
      password;
  }
}
```

---

Access:

```js
user.#password
```

Output:

```text
SyntaxError
```

---

# Inheritance

Classes support inheritance using:

```js
extends
```

and

```js
super()
```

:contentReference[oaicite:9]{index=9}

---

Example:

```js
class Animal {
  speak() {
    console.log("Sound");
  }
}

class Dog extends Animal {
  bark() {
    console.log("Woof");
  }
}
```

---

Usage:

```js
const dog =
  new Dog();

dog.speak();
dog.bark();
```

Output:

```js
Sound
Woof
```

---

# super()

Calls parent constructor.

```js
class Animal {
  constructor(name) {
    this.name = name;
  }
}

class Dog extends Animal {
  constructor(name) {
    super(name);
  }
}
```

---

# Factory vs Class

| Feature | Factory Function | Class |
|----------|----------|----------|
| Uses new | ❌ | ✅ |
| Easy Private State | ✅ | ⚠️ (# fields) |
| Closures | ✅ | ❌ |
| Shared Methods | ❌ By Default | ✅ |
| Inheritance | Manual | Built-in |
| Memory Efficient | ⚠️ Sometimes | ✅ |
| Simplicity | ✅ | ✅ |

:contentReference[oaicite:10]{index=10}

---

# Composition vs Inheritance

Modern JavaScript often prefers:

```text
Composition
```

over:

```text
Deep Inheritance
```

---

Example:

```js
const canEat = {
  eat() {
    console.log("Eating");
  }
};

const canWalk = {
  walk() {
    console.log("Walking");
  }
};

const person = {
  ...canEat,
  ...canWalk
};
```

This is called:

```text
Composition
```

---

# Real React Connection

Most React code uses:

```js
function Component() {}
```

instead of:

```js
class Component {}
```

Modern React prefers:

```text
Functions + Hooks
```

over class components.

---

# Common Interview Questions

## What is a Factory Function?

A function that creates and returns objects.

---

## Do Factory Functions Need new?

No.

```js
createUser()
```

is enough.

---

## What Does new Do?

```text
1. Creates object
2. Sets prototype
3. Binds this
4. Returns object
```

---

## Are Classes Real Classes?

Not exactly.

They are syntax built on top of prototypes. :contentReference[oaicite:11]{index=11}

---

## Why Use Classes?

Cleaner syntax.

Shared methods.

Built-in inheritance.

---

## Why Use Factory Functions?

Closures.

Private state.

Composition.

---

## What Is super()?

Calls the parent class constructor.

---

## Difference Between Static and Instance Methods?

Static:

```js
User.create()
```

Instance:

```js
user.login()
```

---

# Quick Revision

✅ Factory Functions return objects

✅ No `new` required

✅ Factories work well with closures

✅ Constructor Functions require `new`

✅ `new` creates objects automatically

✅ Classes are syntactic sugar over prototypes

✅ Class methods live on prototype

✅ Static methods belong to class

✅ Getters and setters control access

✅ `#field` creates private properties

✅ `extends` enables inheritance

✅ `super()` calls parent constructor

✅ Factories favor composition

✅ Classes favor inheritance

✅ Modern React mostly uses functions and hooks