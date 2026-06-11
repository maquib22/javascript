# JavaScript Equality Operators

## Why This Topic Matters

Many JavaScript bugs happen because developers assume all equality operators behave the same.

JavaScript actually provides **three different ways** to compare values:

1. `==` (Loose Equality)
2. `===` (Strict Equality)
3. `Object.is()` (Same-Value Equality)

Understanding when they behave differently is a favorite interview topic. :contentReference[oaicite:0]{index=0}

---

# The Three Equality Operators

| Operator | Name | Type Conversion | Recommended Usage |
|-----------|---------|---------|---------|
| `==` | Loose Equality | Yes | Rarely |
| `===` | Strict Equality | No | Default choice |
| `Object.is()` | Same-Value Equality | No | Special edge cases |

---

# Strict Equality (===)

Strict equality compares:

- Value
- Type

No type conversion occurs.

```js
5 === 5
```

Output:

```js
true
```

---

```js
5 === "5"
```

Output:

```js
false
```

Different types.

---

```js
true === 1
```

Output:

```js
false
```

Different types.

---

# Why === Is Preferred

Predictable behavior.

```js
const age = "18";

if (age === 18) {
  console.log("Adult");
}
```

Output:

```js
Nothing
```

No hidden conversion.

---

# Loose Equality (==)

Loose equality allows JavaScript to convert values before comparing them. :contentReference[oaicite:1]{index=1}

---

```js
5 == "5"
```

Output:

```js
true
```

---

```js
1 == true
```

Output:

```js
true
```

---

```js
0 == false
```

Output:

```js
true
```

---

```js
"" == false
```

Output:

```js
true
```

---

# The Only Practical Use of ==

Many senior developers use:

```js
value == null
```

Because it checks:

```js
null
undefined
```

at the same time.

Example:

```js
if (userInput == null) {
  console.log("Missing value");
}
```

Matches:

```js
null
undefined
```

Only.

---

# Object.is()

Introduced to solve a few equality edge cases. :contentReference[oaicite:2]{index=2}

---

Most comparisons behave like `===`.

```js
Object.is(5, 5)
```

Output:

```js
true
```

---

```js
Object.is("hello", "hello")
```

Output:

```js
true
```

---

# First Special Case: NaN

---

```js
NaN === NaN
```

Output:

```js
false
```

---

```js
NaN == NaN
```

Output:

```js
false
```

---

```js
Object.is(NaN, NaN)
```

Output:

```js
true
```

This is one of the biggest reasons `Object.is()` exists. :contentReference[oaicite:3]{index=3}

---

# Second Special Case: +0 and -0

---

```js
0 === -0
```

Output:

```js
true
```

---

```js
Object.is(0, -0)
```

Output:

```js
false
```

`Object.is()` treats positive and negative zero differently. :contentReference[oaicite:4]{index=4}

---

# Comparison Summary

| Comparison | Result |
|------------|---------|
| `NaN === NaN` | false |
| `Object.is(NaN, NaN)` | true |
| `0 === -0` | true |
| `Object.is(0, -0)` | false |

---

# Primitive Comparison

Primitives compare by value.

```js
const a = 10;
const b = 10;

console.log(a === b);
```

Output:

```js
true
```

---

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

# Object Comparison

Objects compare by reference.

---

```js
const obj1 = {
  name: "Akio"
};

const obj2 = {
  name: "Akio"
};

console.log(obj1 === obj2);
```

Output:

```js
false
```

Different memory references. :contentReference[oaicite:5]{index=5}

---

# Same Reference

```js
const user = {
  name: "Akio"
};

const copy = user;

console.log(user === copy);
```

Output:

```js
true
```

Both variables point to the same object.

---

# Arrays and Equality

Arrays are objects.

---

```js
[] === []
```

Output:

```js
false
```

Different array instances.

---

```js
const arr = [];

const ref = arr;

console.log(arr === ref);
```

Output:

```js
true
```

Same reference.

---

# Function Equality

Functions are objects too.

---

```js
function a() {}
function b() {}

console.log(a === b);
```

Output:

```js
false
```

Different function objects.

---

```js
const fn = a;

console.log(fn === a);
```

Output:

```js
true
```

---

# String Objects vs String Primitives

Primitive string:

```js
"hello" === "hello"
```

Output:

```js
true
```

---

String objects:

```js
new String("hello") === new String("hello")
```

Output:

```js
false
```

Different objects. :contentReference[oaicite:6]{index=6}

---

# Common Interview Traps

## Trap 1

```js
[] == false
```

Output:

```js
true
```

Because coercion occurs.

---

## Trap 2

```js
[] === false
```

Output:

```js
false
```

Different types.

---

## Trap 3

```js
null == undefined
```

Output:

```js
true
```

Special JavaScript rule. :contentReference[oaicite:7]{index=7}

---

## Trap 4

```js
null === undefined
```

Output:

```js
false
```

Different types.

---

## Trap 5

```js
{} === {}
```

Output:

```js
false
```

Different references.

---

## Trap 6

```js
[] === []
```

Output:

```js
false
```

Different references.

---

# React Connection

React internally uses `Object.is()` for many state comparison optimizations.

Example:

```js
setCount(5);
setCount(5);
```

React may skip re-rendering because values are considered equal.

Understanding equality helps explain:

- Re-renders
- Memoization
- Dependency arrays
- State updates

---

# Best Practices

## Use === By Default

Good:

```js
a === b
```

---

Avoid:

```js
a == b
```

unless you intentionally want coercion.

---

## Use Object.is() For Edge Cases

Good:

```js
Object.is(value1, value2)
```

Useful when dealing with:

```js
NaN
-0
+0
```

---

## Never Compare Objects By Structure

Bad:

```js
user1 === user2
```

when checking content.

Use:

```js
JSON.stringify(user1) === JSON.stringify(user2)
```

or a deep comparison utility.

---

# Interview Questions

## Difference Between == and ===?

`==`

- Performs type conversion

`===`

- No type conversion

---

## Difference Between === and Object.is()?

Most behavior is identical.

Differences:

```js
NaN
+0
-0
```

---

## Why Does {} === {} Return False?

Different object references.

---

## Why Does NaN === NaN Return False?

JavaScript defines NaN as unequal to every value, including itself. :contentReference[oaicite:8]{index=8}

---

## Why Does Object.is(NaN, NaN) Return True?

Because it uses Same-Value Equality semantics. :contentReference[oaicite:9]{index=9}

---

# Quick Revision

✅ `===` compares value and type

✅ `==` performs coercion

✅ `Object.is()` handles edge cases

✅ `NaN === NaN` → false

✅ `Object.is(NaN, NaN)` → true

✅ `0 === -0` → true

✅ `Object.is(0, -0)` → false

✅ Objects compare by reference

✅ Arrays compare by reference

✅ Functions compare by reference

✅ Use `===` by default

✅ Use `Object.is()` for special cases