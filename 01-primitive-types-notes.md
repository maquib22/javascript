# JavaScript Primitive Types

## Overview

JavaScript has **7 primitive types**:

1. String
2. Number
3. BigInt
4. Boolean
5. Undefined
6. Null
7. Symbol

Primitive values are:

- Immutable
- Compared by value
- Not objects

```js
const str = "hello";
const num = 42;
const big = 9007199254740993n;
const bool = true;
const undef = undefined;
const nul = null;
const sym = Symbol("id");
```

---

# 1. String

Represents textual data.

```js
const name = "Akio";
const city = 'Gurgaon';
const message = `Hello ${name}`;
```

### Common Methods

```js
name.length;
name.toUpperCase();
name.toLowerCase();
name.includes("ki");
```

### Immutability

```js
let str = "hello";

str[0] = "H";

console.log(str); // "hello"
```

Strings cannot be modified directly.

---

# 2. Number

Represents both integers and floating-point numbers.

```js
let age = 25;
let price = 99.99;
```

### Special Values

```js
Infinity
-Infinity
NaN
```

Example:

```js
console.log(10 / 0); // Infinity
console.log("abc" * 2); // NaN
```

### Number Checks

```js
Number.isNaN(NaN);
Number.isInteger(10);
```

---

# 3. BigInt

Used for very large integers.

```js
const big = 9007199254740993n;
```

Normal numbers lose precision after:

```js
Number.MAX_SAFE_INTEGER
```

Example:

```js
console.log(9007199254740991 + 1);
console.log(9007199254740991 + 2);
```

BigInt solves this issue.

```js
const huge = 9007199254740993n;
```

---

# 4. Boolean

Represents logical values.

```js
true
false
```

Example:

```js
const isLoggedIn = true;
const hasPermission = false;
```

Used in:

```js
if (isLoggedIn) {
  console.log("Welcome");
}
```

---

# 5. Undefined

A variable declared but not assigned a value.

```js
let user;

console.log(user);
```

Output:

```js
undefined
```

---

# 6. Null

Represents intentional absence of value.

```js
let selectedUser = null;
```

Example:

```js
let data = null;
```

### Null vs Undefined

| Null | Undefined |
|--------|------------|
| Intentional empty value | Value not assigned |
| Assigned by developer | Assigned by JavaScript |
| typeof returns object | typeof returns undefined |

```js
let a = null;
let b;

console.log(a); // null
console.log(b); // undefined
```

---

# 7. Symbol

Creates unique identifiers.

```js
const id1 = Symbol("id");
const id2 = Symbol("id");

console.log(id1 === id2); // false
```

Useful for object property uniqueness.

```js
const ID = Symbol("id");

const user = {
  [ID]: 123
};
```

---

# typeof Operator

Used to check data type.

```js
typeof "hello";      // string
typeof 42;           // number
typeof true;         // boolean
typeof undefined;    // undefined
typeof Symbol();     // symbol
typeof 10n;          // bigint
```

### Famous JavaScript Quirk

```js
typeof null;
```

Output:

```js
"object"
```

This is a historical bug kept for backward compatibility.

---

# Primitive Values Are Compared By Value

```js
const a = 10;
const b = 10;

console.log(a === b);
```

Output:

```js
true
```

Strings behave the same way.

```js
const x = "hello";
const y = "hello";

console.log(x === y);
```

Output:

```js
true
```

---

# Autoboxing

Primitives are not objects, but JavaScript temporarily wraps them with object wrappers when methods are used.

```js
const str = "hello";

console.log(str.toUpperCase());
```

Internally JavaScript does something similar to:

```js
new String("hello").toUpperCase();
```

Then removes the temporary object.

This process is called **Autoboxing**.

---

# Primitive Wrapper Objects

| Primitive | Wrapper Object |
|------------|----------------|
| String | String |
| Number | Number |
| Boolean | Boolean |

Examples:

```js
new String("hello");
new Number(42);
new Boolean(true);
```

Avoid using wrapper objects directly.

---

# Primitive vs Object

## Primitive

```js
let a = 10;
let b = a;

b = 20;

console.log(a); // 10
```

## Object

```js
let user1 = {
  name: "Akio"
};

let user2 = user1;

user2.name = "John";

console.log(user1.name);
```

Output:

```js
John
```

Objects are copied by reference.

---

# Interview Questions

### How many primitive types are there in JavaScript?

7

- String
- Number
- BigInt
- Boolean
- Undefined
- Null
- Symbol

---

### Why does `typeof null` return `"object"`?

Because of a historical JavaScript bug preserved for backward compatibility.

---

### Are strings mutable?

No.

Strings are immutable.

---

### What is autoboxing?

Temporary conversion of primitives into wrapper objects when accessing properties or methods.

---

### Difference between null and undefined?

- `undefined` = value not assigned
- `null` = intentionally empty value

---

# Quick Revision

✅ 7 primitive types

✅ Immutable values

✅ Compared by value

✅ typeof null === "object"

✅ Strings are immutable

✅ BigInt handles huge integers

✅ Symbol creates unique identifiers

✅ Autoboxing allows primitive methods
