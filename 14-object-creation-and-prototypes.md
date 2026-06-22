# JavaScript Object Creation & Prototypes

## Why This Topic Matters

Everything in JavaScript's object system is built on:

```text
Objects
Prototypes
Prototype Chain
```

Understanding this topic explains:

- Classes
- Inheritance
- new keyword
- instanceof
- Method sharing
- Object.create()

Without prototypes, classes don't make sense.

---

# JavaScript is Prototype-Based

Languages like:

```text
Java
C#
C++
```

are class-based.

JavaScript is:

```text
Prototype-Based
```

Objects inherit directly from other objects.

---

# The Mystery

Consider:

```js
const user = {
  name: "Akio"
};

console.log(user.toString());
console.log(user.hasOwnProperty("name"));
```

Output:

```js
"[object Object]"
true
```

Question:

```text
Where did these methods come from?
```

We never defined them.

Answer:

```text
Prototype Chain
```

---

# What is a Prototype?

Every object contains a hidden internal reference:

```text
[[Prototype]]
```

that points to another object.

Example:

```js
const user = {
  name: "Akio"
};
```

Internally:

```text
user
  ↓
Object.prototype
  ↓
null
```

---

# Property Lookup

When JavaScript tries:

```js
user.toString()
```

it performs:

```text
1. Look inside user
2. Not found
3. Look inside prototype
4. Found
5. Execute method
```

This process is called:

```text
Prototype Chain Lookup
```

:contentReference[oaicite:1]{index=1}

---

# Prototype Chain

Example:

```js
const animal = {
  eat() {
    console.log("Eating");
  }
};

const dog =
  Object.create(animal);

dog.bark = function() {
  console.log("Woof");
};
```

---

Relationship:

```text
dog
 ↓
animal
 ↓
Object.prototype
 ↓
null
```

---

Access:

```js
dog.eat();
```

Output:

```js
Eating
```

Even though:

```js
eat()
```

doesn't exist directly on dog.

---

# Visualizing Property Lookup

```js
dog.eat();
```

Search order:

```text
dog
 ↓
animal
 ↓
Object.prototype
 ↓
null
```

Stops when found.

---

# End of Prototype Chain

Eventually every chain reaches:

```js
null
```

Example:

```text
Object.prototype
      ↓
     null
```

No more lookup possible.

---

# Accessing Prototypes

Modern way:

```js
Object.getPrototypeOf(obj);
```

Example:

```js
const user = {};

console.log(
  Object.getPrototypeOf(user)
);
```

Output:

```js
Object.prototype
```

---

# Avoid __proto__

Old way:

```js
user.__proto__
```

Works but is considered legacy.

Prefer:

```js
Object.getPrototypeOf()
```

:contentReference[oaicite:2]{index=2}

---

# Object.create()

Creates an object with a specific prototype.

---

Example:

```js
const animal = {
  eat() {
    console.log("Eating");
  }
};

const dog =
  Object.create(animal);
```

---

Chain:

```text
dog
 ↓
animal
 ↓
Object.prototype
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

---

# Object.create(null)

Creates an object with NO prototype.

```js
const dict =
  Object.create(null);
```

Chain:

```text
dict
 ↓
null
```

---

No inherited methods:

```js
dict.toString();
```

Output:

```text
TypeError
```

---

# The .prototype Property

Very common interview topic.

---

Many developers confuse:

```text
[[Prototype]]
```

with:

```text
.prototype
```

They are NOT the same.

:contentReference[oaicite:3]{index=3}

---

# [[Prototype]]

Exists on:

```text
Objects
```

Example:

```js
const user = {};
```

Has:

```text
[[Prototype]]
```

internally.

---

# .prototype

Exists on:

```text
Functions
```

Example:

```js
function User() {}
```

Has:

```js
User.prototype
```

---

Interview Shortcut:

```text
Object → [[Prototype]]

Function → .prototype
```

---

# Constructor Function Example

```js
function User(name) {
  this.name = name;
}
```

---

Every function automatically gets:

```js
User.prototype
```

---

Example:

```js
console.log(
  User.prototype
);
```

Output:

```js
{
  constructor: User
}
```

---

# Why Prototype Exists

Without prototypes:

```js
function User(name) {
  this.name = name;

  this.login = function() {
    console.log("Login");
  };
}
```

Every object creates:

```js
login()
```

again.

---

Memory:

```text
User1 → login()
User2 → login()
User3 → login()
```

Many copies.

---

# Shared Methods

Better:

```js
function User(name) {
  this.name = name;
}

User.prototype.login =
  function() {
    console.log("Login");
  };
```

---

Now:

```text
User1
User2
User3
   ↓
User.prototype
   ↓
login()
```

One shared method.

---

# The new Keyword

When JavaScript executes:

```js
const user =
  new User("Akio");
```

It performs four steps.

---

## Step 1

Create empty object.

```js
{}
```

---

## Step 2

Connect prototype.

```text
object.[[Prototype]]
          ↓
User.prototype
```

---

## Step 3

Bind:

```js
this
```

to new object.

---

## Step 4

Return object.

---

Visualization:

```text
new User()

1. {}
2. Link prototype
3. this = object
4. return object
```

:contentReference[oaicite:4]{index=4}

---

# instanceof

Checks whether a constructor's prototype exists somewhere in the prototype chain.

---

Example:

```js
function User() {}

const user =
  new User();

console.log(
  user instanceof User
);
```

Output:

```js
true
```

---

Internally:

```text
Is User.prototype
inside user's prototype chain?
```

If yes:

```js
true
```

:contentReference[oaicite:5]{index=5}

---

# hasOwnProperty()

Checks only the object's own properties.

---

Example:

```js
const user = {
  name: "Akio"
};

console.log(
  user.hasOwnProperty("name")
);
```

Output:

```js
true
```

---

Inherited property:

```js
user.hasOwnProperty(
  "toString"
);
```

Output:

```js
false
```

Because:

```js
toString
```

comes from prototype.

---

# hasOwnProperty vs in

Example:

```js
"name" in user
```

Output:

```js
true
```

---

Example:

```js
"toString" in user
```

Output:

```js
true
```

Because:

```text
in checks entire prototype chain
```

---

Comparison:

```text
hasOwnProperty()
   ↓
Own properties only

in
   ↓
Own + inherited properties
```

:contentReference[oaicite:6]{index=6}

---

# Object.assign()

Used for shallow copying.

```js
const user = {
  name: "Akio"
};

const copy =
  Object.assign({}, user);
```

---

Equivalent:

```js
const copy = {
  ...user
};
```

---

Important:

```text
Shallow Copy Only
```

Nested objects remain shared.

:contentReference[oaicite:7]{index=7}

---

# Shadowing

Child properties override prototype properties.

---

Example:

```js
const animal = {
  sound: "Animal"
};

const dog =
  Object.create(animal);

dog.sound = "Woof";
```

---

Access:

```js
console.log(dog.sound);
```

Output:

```js
Woof
```

---

The child property shadows the prototype property.

:contentReference[oaicite:8]{index=8}

---

# Classes and Prototypes

Class:

```js
class User {
  login() {
    console.log("Login");
  }
}
```

---

Under the hood:

```js
function User() {}

User.prototype.login =
  function() {};
```

Classes are syntactic sugar over prototypes.

:contentReference[oaicite:9]{index=9}

---

# Common Mistakes

## Mistake 1

Confusing:

```text
.prototype
```

with

```text
[[Prototype]]
```

---

## Mistake 2

Using:

```js
Object.setPrototypeOf()
```

frequently.

Changing prototypes after creation can hurt performance.

Prefer:

```js
Object.create()
```

during creation.

:contentReference[oaicite:10]{index=10}

---

## Mistake 3

Modifying:

```js
Object.prototype
```

Example:

```js
Object.prototype.sayHi =
  function() {};
```

Avoid this.

It affects every object in your application.

:contentReference[oaicite:11]{index=11}

---

# Common Interview Questions

## What is a Prototype?

An object that another object inherits from.

---

## What is the Prototype Chain?

A chain of objects JavaScript searches during property lookup.

---

## Difference Between [[Prototype]] and .prototype?

```text
[[Prototype]]
→ exists on objects

.prototype
→ exists on functions
```

---

## What Does new Do?

```text
1. Create object
2. Link prototype
3. Bind this
4. Return object
```

---

## What is Object.create()?

Creates an object with a specified prototype.

---

## What does instanceof check?

Whether Constructor.prototype exists in the object's prototype chain.

---

## Difference Between hasOwnProperty and in?

```text
hasOwnProperty
→ own properties only

in
→ own + inherited properties
```

---

# Quick Revision

✅ Every object has a hidden `[[Prototype]]`

✅ Property lookup follows the prototype chain

✅ Chains eventually end at `null`

✅ `Object.prototype` sits near the top of most chains

✅ `Object.create()` creates objects with custom prototypes

✅ `[[Prototype]]` and `.prototype` are different

✅ Functions have `.prototype`

✅ Objects have `[[Prototype]]`

✅ `new` performs 4 steps

✅ Methods should live on prototypes

✅ `instanceof` checks the prototype chain

✅ `hasOwnProperty()` checks own properties only

✅ `in` checks own + inherited properties

✅ Classes are syntactic sugar over prototypes