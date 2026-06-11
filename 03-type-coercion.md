# JavaScript Type Coercion

## What is Type Coercion?

Type Coercion is JavaScript's ability to automatically convert one data type into another when performing operations.

Example:

```js
"5" + 2
```

Output:

```js
"52"
```

JavaScript converted:

```js
2 -> "2"
```

before concatenation.

---

# Why It Matters

Many JavaScript bugs happen because developers don't understand coercion.

Understanding coercion helps with:

- Debugging
- Interview questions
- Equality comparisons
- Conditional logic
- React forms and APIs

---

# Two Types of Coercion

## 1. Implicit Coercion

JavaScript automatically converts values.

```js
"5" + 1
```

JavaScript performs conversion behind the scenes.

---

## 2. Explicit Coercion

Developer intentionally converts values.

```js
Number("5")
String(123)
Boolean(1)
```

---

# String Coercion

When one operand is a string and the operator is `+`,
JavaScript prefers string concatenation.

```js
"5" + 2
```

Output:

```js
"52"
```

---

```js
"Hello " + "World"
```

Output:

```js
"Hello World"
```

---

```js
"10" + true
```

Output:

```js
"10true"
```

---

```js
"10" + null
```

Output:

```js
"10null"
```

---

```js
"10" + undefined
```

Output:

```js
"10undefined"
```

---

# Numeric Coercion

Operators other than `+` generally try to convert operands to numbers.

---

## Subtraction

```js
"10" - 5
```

Output:

```js
5
```

Conversion:

```js
Number("10") - 5
```

---

## Multiplication

```js
"10" * 2
```

Output:

```js
20
```

---

## Division

```js
"20" / 2
```

Output:

```js
10
```

---

## Modulus

```js
"10" % 3
```

Output:

```js
1
```

---

# Boolean to Number Conversion

```js
Number(true)
```

Output:

```js
1
```

---

```js
Number(false)
```

Output:

```js
0
```

---

Examples:

```js
true + 1
```

Output:

```js
2
```

---

```js
false + 5
```

Output:

```js
5
```

---

# Null Conversion

```js
Number(null)
```

Output:

```js
0
```

---

Examples:

```js
null + 1
```

Output:

```js
1
```

---

```js
null == 0
```

Output:

```js
false
```

This surprises many developers.

---

# Undefined Conversion

```js
Number(undefined)
```

Output:

```js
NaN
```

---

```js
undefined + 1
```

Output:

```js
NaN
```

---

# Equality Coercion

## Loose Equality (==)

Performs type coercion.

```js
"5" == 5
```

Output:

```js
true
```

JavaScript converts:

```js
Number("5")
```

before comparison.

---

## Strict Equality (===)

No type coercion.

```js
"5" === 5
```

Output:

```js
false
```

Different types.

---

# Common Equality Examples

```js
1 == true
```

Output:

```js
true
```

Because:

```js
true -> 1
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

```js
null == undefined
```

Output:

```js
true
```

Special JavaScript rule.

---

```js
null === undefined
```

Output:

```js
false
```

Different types.

---

# Truthy and Falsy Values

Every value in JavaScript becomes either:

```text
Truthy
or
Falsy
```

when evaluated in a boolean context.

---

# Falsy Values

Only 8 values are falsy.

```js
false
0
-0
0n
""
null
undefined
NaN
```

Everything else is truthy.

---

# Truthy Examples

```js
"0"
[]
{}
"false"
"hello"
42
```

All evaluate to:

```js
true
```

in boolean contexts.

---

# Boolean Conversion

```js
Boolean(1)
```

Output:

```js
true
```

---

```js
Boolean(0)
```

Output:

```js
false
```

---

```js
Boolean("hello")
```

Output:

```js
true
```

---

```js
Boolean("")
```

Output:

```js
false
```

---

# Object Coercion

Objects are generally converted to primitives before operations.

Example:

```js
const obj = {
  valueOf() {
    return 10;
  }
};

console.log(obj + 5);
```

Output:

```js
15
```

JavaScript first calls:

```js
valueOf()
```

---

# Weird Interview Questions

## Example 1

```js
[] + []
```

Output:

```js
""
```

Both arrays become empty strings.

---

## Example 2

```js
[] + {}
```

Output:

```js
"[object Object]"
```

---

## Example 3

```js
{} + []
```

May behave differently depending on context.

Often interpreted as:

```js
0
```

---

## Example 4

```js
[] == false
```

Output:

```js
true
```

Because:

```js
[] -> ""
"" -> 0
false -> 0
```

Result:

```js
0 == 0
```

---

# Explicit Type Conversion

## String()

```js
String(123)
```

Output:

```js
"123"
```

---

## Number()

```js
Number("123")
```

Output:

```js
123
```

---

```js
Number("abc")
```

Output:

```js
NaN
```

---

## Boolean()

```js
Boolean(1)
```

Output:

```js
true
```

---

```js
Boolean(0)
```

Output:

```js
false
```

---

# Best Practices

## Prefer Strict Equality

Good:

```js
a === b
```

Avoid:

```js
a == b
```

unless you intentionally need coercion.

---

## Convert Explicitly

Good:

```js
const age = Number(inputValue);
```

Avoid relying on hidden conversions.

---

## Be Careful with Truthy/Falsy Checks

Bad:

```js
if (value)
```

when `0` is a valid value.

Better:

```js
if (value !== null)
```

or

```js
if (value !== undefined)
```

depending on requirements.

---

# Interview Questions

## What is Type Coercion?

Automatic conversion of one data type into another.

---

## Difference Between == and ===?

`==`

- Performs coercion

`===`

- No coercion
- Compares type and value

---

## How many falsy values exist?

8

```js
false
0
-0
0n
""
null
undefined
NaN
```

---

## Why does "5" + 2 return "52"?

Because `+` prefers string concatenation when a string is present.

---

## Why does "5" - 2 return 3?

Because `-` forces numeric conversion.

---

# Quick Revision

✅ Type coercion = automatic type conversion

✅ `+` often triggers string conversion

✅ `-`, `*`, `/`, `%` trigger numeric conversion

✅ `==` performs coercion

✅ `===` does not perform coercion

✅ `null == undefined` is true

✅ `null === undefined` is false

✅ Only 8 falsy values exist

✅ Prefer explicit conversions

✅ Prefer `===` over `==`