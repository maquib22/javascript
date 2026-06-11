# JavaScript: Primitives vs Objects

## Why This Topic Matters

Understanding the difference between primitives and objects is one of the most important JavaScript concepts.

It affects:

- Memory management
- Variable assignment
- Function arguments
- Equality comparisons
- React state updates
- Performance optimizations

---

# Fundamental Difference

## Primitive

A primitive value directly contains its data.

```js
let age = 25;
```

The variable stores:

```text
25
```

---

## Object

An object variable stores a reference (memory address).

```js
const user = {
  name: "Akio"
};
```

The variable does NOT contain the actual object.

Instead:

```text
user --> Memory Address --> { name: "Akio" }
```

---

# Memory Model

## Primitive Storage

```js
let a = 10;
let b = a;
```

Memory:

```text
a -> 10
b -> 10
```

Each variable has its own copy.

---

## Object Storage

```js
let obj1 = { name: "John" };
let obj2 = obj1;
```

Memory:

```text
obj1 ----\
          --> { name: "John" }
obj2 ----/
```

Both variables point to the same object.

---

# Assignment Behavior

## Primitive Assignment

```js
let a = 10;
let b = a;

b = 20;

console.log(a);
```

Output:

```js
10
```

Reason:

`b` received a copy.

---

## Object Assignment

```js
let person1 = {
  name: "John"
};

let person2 = person1;

person2.name = "Mike";

console.log(person1.name);
```

Output:

```js
Mike
```

Reason:

Both variables reference the same object.

---

# Equality Comparison

## Primitive Equality

```js
10 === 10
```

Result:

```js
true
```

JavaScript compares actual values.

---

## Object Equality

```js
{} === {}
```

Result:

```js
false
```

Why?

Different objects occupy different memory locations.

---

Example:

```js
const a = {};
const b = {};

console.log(a === b);
```

Output:

```js
false
```

---

# Shared Reference Problem

```js
const user = {
  name: "Akio"
};

const copy = user;

copy.name = "Alex";

console.log(user.name);
```

Output:

```js
Alex
```

A very common beginner mistake.

---

# Creating Independent Copies

## Shallow Copy

```js
const user = {
  name: "Akio"
};

const copy = { ...user };
```

Now:

```js
copy.name = "Alex";
```

Will not affect:

```js
user.name
```

---

# Nested Object Problem

```js
const user = {
  profile: {
    city: "Delhi"
  }
};

const copy = { ...user };

copy.profile.city = "Mumbai";
```

Result:

```js
user.profile.city
```

Output:

```js
Mumbai
```

Because spread operator only copies one level deep.

This is called:

```text
Shallow Copy
```

---

# Deep Copy

Creates completely independent nested structures.

Example:

```js
const user = {
  profile: {
    city: "Delhi"
  }
};

const copy = structuredClone(user);
```

Now modifying nested properties won't affect the original object.

---

# Functions and Parameters

## Primitive Argument

```js
function updateAge(age) {
  age = 30;
}

let myAge = 20;

updateAge(myAge);

console.log(myAge);
```

Output:

```js
20
```

Reason:

Function receives a copy.

---

## Object Argument

```js
function updateUser(user) {
  user.name = "Mike";
}

const person = {
  name: "John"
};

updateUser(person);

console.log(person.name);
```

Output:

```js
Mike
```

Reason:

Function receives a reference to the same object.

---

# Mutability

## Mutable

Objects can be changed after creation.

```js
const user = {
  name: "Akio"
};

user.name = "Alex";
```

Valid.

---

## Immutable

Primitive values cannot be changed.

```js
let str = "hello";

str[0] = "H";
```

No effect.

Strings are immutable.

---

# Wrapper Objects

JavaScript provides object versions of primitives.

```js
new String("hello")
new Number(100)
new Boolean(true)
```

Avoid using them.

---

Bad:

```js
const name = new String("Akio");
```

Good:

```js
const name = "Akio";
```

---

# Arrays Are Objects

Many developers forget this.

```js
typeof []
```

Output:

```js
"object"
```

Array is a special type of object.

---

# Functions Are Objects

```js
function greet() {}
```

Functions can have properties.

```js
greet.language = "English";

console.log(greet.language);
```

Output:

```js
English
```

This is possible because functions are objects.

---

# Null Confusion

```js
typeof null
```

Output:

```js
"object"
```

This is a historical JavaScript bug.

Null is NOT actually an object.

---

# Object Types in JavaScript

Common built-in objects:

```js
Object
Array
Function
Date
Map
Set
RegExp
Promise
```

Examples:

```js
const arr = [];
const map = new Map();
const set = new Set();
const date = new Date();
```

---

# Interview Questions

## What is the biggest difference between primitives and objects?

Primitive values are stored directly.

Objects are stored by reference.

---

## Why does {} === {} return false?

Because they are different references.

---

## Why does changing one object variable affect another?

Because both variables point to the same memory location.

---

## What is a shallow copy?

A copy that duplicates only the first level.

Nested objects remain shared.

---

## What is a deep copy?

A copy where all nested objects are duplicated.

No shared references exist.

---

## Are arrays objects?

Yes.

```js
typeof []
```

Returns:

```js
"object"
```

---

## Are functions objects?

Yes.

Functions can store properties and methods.

---

# React Connection

This topic becomes critical in React.

Wrong:

```js
state.user.name = "Akio";
```

Mutating existing objects can prevent React from detecting changes.

Correct:

```js
setState({
  ...state,
  user: {
    ...state.user,
    name: "Akio"
  }
});
```

React relies heavily on object references.

---

# Quick Revision

✅ Primitives store values directly

✅ Objects store references

✅ Primitive assignment creates copies

✅ Object assignment copies references

✅ Objects are mutable

✅ Primitives are immutable

✅ Arrays are objects

✅ Functions are objects

✅ Spread operator creates shallow copies

✅ structuredClone() creates deep copies

✅ {} === {} returns false

✅ React depends on reference changes