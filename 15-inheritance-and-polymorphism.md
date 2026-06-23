# JavaScript Inheritance & Polymorphism

## Why This Topic Matters

Imagine building:

```text
Dog
Cat
Bird
Fish
```

All of them can:

```text
Eat
Sleep
Move
```

Writing the same code repeatedly is:

```text
Duplicate
Hard to maintain
Error-prone
```

Inheritance allows code reuse.

Polymorphism allows different objects to behave differently through the same interface.

These are two of the core principles of Object-Oriented Programming (OOP).

---

# What is Inheritance?

Inheritance allows one object or class to acquire properties and methods from another.

Think:

```text
Parent
   ↓
Child
```

The child gets access to the parent's functionality.

---

# Real World Example

```text
Animal
 ├── Dog
 ├── Cat
 └── Bird
```

All animals can:

```text
Eat
Sleep
```

Specific animals can also have their own behavior.

---

# Basic Class Inheritance

```js
class Animal {
  eat() {
    console.log("Eating");
  }
}
```

---

Child:

```js
class Dog extends Animal {
  bark() {
    console.log("Woof");
  }
}
```

---

Usage:

```js
const dog = new Dog();

dog.eat();
dog.bark();
```

Output:

```js
Eating
Woof
```

---

# What Does extends Do?

```js
class Dog extends Animal {}
```

Creates a prototype relationship.

Internally:

```text
Dog.prototype
        ↓
Animal.prototype
```

---

Prototype Chain

```text
dog
 ↓
Dog.prototype
 ↓
Animal.prototype
 ↓
Object.prototype
 ↓
null
```

This is how inheritance actually works in JavaScript.

---

# Parent Constructor

Parent class:

```js
class Animal {
  constructor(name) {
    this.name = name;
  }
}
```

---

Child class:

```js
class Dog extends Animal {
  constructor(name) {
    super(name);
  }
}
```

---

Usage:

```js
const dog =
  new Dog("Rocky");

console.log(dog.name);
```

Output:

```js
Rocky
```

---

# What is super()?

Calls the parent constructor.

Example:

```js
super(name);
```

Equivalent to:

```js
Animal.call(this, name);
```

Conceptually.

---

# Why super() Is Required

Bad:

```js
class Dog extends Animal {
  constructor(name) {
    this.name = name;
  }
}
```

Output:

```text
ReferenceError
```

---

Correct:

```js
class Dog extends Animal {
  constructor(name) {
    super(name);

    this.name = name;
  }
}
```

You must call:

```js
super()
```

before accessing:

```js
this
```

---

# Inheriting Methods

Parent:

```js
class Animal {
  eat() {
    console.log("Eating");
  }
}
```

Child:

```js
class Dog extends Animal {}
```

---

Usage:

```js
const dog = new Dog();

dog.eat();
```

Output:

```js
Eating
```

Method comes from parent.

---

# Multi-Level Inheritance

```js
class Animal {}

class Mammal
  extends Animal {}

class Dog
  extends Mammal {}
```

Chain:

```text
Dog
 ↓
Mammal
 ↓
Animal
 ↓
Object
```

---

# Method Overriding

A child can replace a parent method.

Parent:

```js
class Animal {
  speak() {
    console.log("Animal sound");
  }
}
```

---

Child:

```js
class Dog extends Animal {
  speak() {
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
```

Output:

```js
Woof
```

The child version overrides the parent version.

---

# Calling Parent Method

Sometimes we want both.

```js
class Dog extends Animal {
  speak() {
    super.speak();

    console.log("Woof");
  }
}
```

Output:

```js
Animal sound
Woof
```

---

# What is Polymorphism?

Polymorphism means:

```text
Many Forms
```

Different objects respond differently to the same method call.

---

Example:

```js
dog.speak();
cat.speak();
bird.speak();
```

Same method:

```js
speak()
```

Different behavior.

---

# Without Polymorphism

Bad:

```js
if (animal.type === "dog") {
  console.log("Woof");
}

if (animal.type === "cat") {
  console.log("Meow");
}
```

Many conditions.

Hard to maintain.

---

# With Polymorphism

Parent:

```js
class Animal {
  speak() {}
}
```

---

Dog:

```js
class Dog extends Animal {
  speak() {
    console.log("Woof");
  }
}
```

---

Cat:

```js
class Cat extends Animal {
  speak() {
    console.log("Meow");
  }
}
```

---

Usage:

```js
animals.forEach(animal => {
  animal.speak();
});
```

Output:

```js
Woof
Meow
Woof
Meow
```

No conditions needed.

---

# Real Polymorphism Example

```js
class Shape {
  area() {}
}
```

---

Rectangle:

```js
class Rectangle extends Shape {
  area() {
    return this.width *
           this.height;
  }
}
```

---

Circle:

```js
class Circle extends Shape {
  area() {
    return Math.PI *
           this.radius *
           this.radius;
  }
}
```

---

Usage:

```js
shapes.forEach(shape => {
  console.log(
    shape.area()
  );
});
```

Each object computes area differently.

---

# Polymorphism Through Interfaces

JavaScript doesn't have traditional interfaces.

Instead:

```text
Duck Typing
```

---

Rule:

```text
If it behaves like a duck,
treat it like a duck.
```

---

Example:

```js
const dog = {
  speak() {
    console.log("Woof");
  }
};

const cat = {
  speak() {
    console.log("Meow");
  }
};
```

---

Usage:

```js
function makeSound(animal) {
  animal.speak();
}
```

Works for both.

---

# Prototype-Based Inheritance

Inheritance existed before classes.

---

Parent:

```js
const animal = {
  eat() {
    console.log("Eating");
  }
};
```

---

Child:

```js
const dog =
  Object.create(animal);

dog.bark = function() {
  console.log("Woof");
};
```

---

Usage:

```js
dog.eat();
```

Output:

```js
Eating
```

Because of the prototype chain.

---

# Classical vs Prototype Inheritance

Class Syntax:

```js
class Dog
  extends Animal {}
```

---

Prototype Syntax:

```js
Object.create(
  Animal.prototype
);
```

---

Both ultimately use:

```text
Prototype Chain
```

under the hood.

---

# instanceof and Inheritance

```js
class Animal {}

class Dog
  extends Animal {}

const dog =
  new Dog();
```

---

Checks:

```js
dog instanceof Dog
```

Output:

```js
true
```

---

```js
dog instanceof Animal
```

Output:

```js
true
```

Because:

```text
Animal.prototype
```

exists in the prototype chain.

---

# Inheritance Problems

Inheritance is powerful but can become problematic.

---

Deep inheritance:

```text
Animal
 ↓
Mammal
 ↓
Pet
 ↓
Dog
 ↓
GoldenRetriever
```

Hard to maintain.

---

Changes at the top affect everything below.

---

# Composition Over Inheritance

Modern JavaScript often prefers:

```text
Composition
```

instead of deep inheritance.

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
```

---

Compose:

```js
const person = {
  ...canEat,
  ...canWalk
};
```

---

Benefits:

```text
Flexible
Reusable
Less coupling
```

---

# Real React Connection

React previously relied heavily on:

```js
class Component
```

Inheritance.

---

Modern React prefers:

```js
Functions
Hooks
Composition
```

---

Instead of:

```text
Inheritance
```

React encourages:

```text
Component Composition
```

---

# Common Interview Questions

## What is Inheritance?

A mechanism that allows one class or object to acquire functionality from another.

---

## What is Polymorphism?

The ability for different objects to respond differently to the same method call.

---

## What does extends do?

Creates an inheritance relationship.

---

## What does super() do?

Calls the parent constructor or parent method.

---

## Why is super() required?

Because child constructors must initialize the parent before using `this`.

---

## What is Method Overriding?

Replacing a parent method in a child class.

---

## What is Duck Typing?

If an object has the required behavior, it can be used regardless of its type.

---

## Why is Composition often preferred?

Less coupling and more flexibility than deep inheritance trees.

---

# Quick Revision

✅ Inheritance enables code reuse

✅ Child classes inherit parent functionality

✅ `extends` creates inheritance

✅ `super()` calls parent constructor

✅ Child methods can override parent methods

✅ Polymorphism = same method, different behavior

✅ Method overriding enables polymorphism

✅ JavaScript inheritance uses prototypes underneath

✅ `instanceof` checks the prototype chain

✅ Duck typing enables flexible polymorphism

✅ Deep inheritance can become problematic

✅ Modern JavaScript often prefers composition

✅ React favors composition over inheritance