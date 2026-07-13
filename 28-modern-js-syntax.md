# JavaScript Modern Syntax (ES6+)

## Why Modern JavaScript Syntax Matters

Modern JavaScript introduced cleaner and more powerful syntax.

It is heavily used in:

- React
- Node.js
- Next.js
- Modern Frontend Applications
- JavaScript Libraries

Modern JavaScript makes code:

- Shorter
- Cleaner
- Easier to read
- Easier to maintain

---

# What is ES6+?

ES6 is also known as:

```text
ECMAScript 2015
```

It introduced major JavaScript features like:

```text
let
const
Arrow Functions
Destructuring
Spread Operator
Rest Parameters
Template Literals
Classes
Promises
Modules
Map
Set
```

`ES6+` means:

```text
ES6

+

Features introduced
in later ECMAScript versions
```

---

# let, const and var

Before ES6:

```js
var name = "Akio";
```

Modern JavaScript introduced:

```js
let name = "Akio";

const age = 25;
```

---

# var

`var` is function scoped.

Example:

```js
function test() {

  if (true) {

    var name = "Akio";

  }

  console.log(name);

}

test();
```

Output:

```text
Akio
```

Why?

Because:

```text
var ignores block scope
```

---

# let

`let` is block scoped.

```js
if (true) {

  let name = "Akio";

}

console.log(name);
```

Output:

```text
ReferenceError
```

The variable only exists inside the block.

---

# const

`const` is also block scoped.

```js
const age = 25;
```

But it cannot be reassigned.

```js
age = 30;
```

Output:

```text
TypeError
```

---

# Important const Concept

This is valid:

```js
const user = {

  name: "Akio"

};

user.name = "John";
```

Why?

Because:

```text
const prevents reassignment

NOT mutation
```

Invalid:

```js
user = {};
```

Valid:

```js
user.name = "John";
```

---

# Modern Rule

Use:

```text
const
```

by default.

Use:

```text
let
```

when reassignment is required.

Avoid:

```text
var
```

in modern JavaScript.

---

# Arrow Functions

Traditional function:

```js
function add(a, b) {

  return a + b;

}
```

Arrow function:

```js
const add = (a, b) => {

  return a + b;

};
```

Short version:

```js
const add =
  (a, b) => a + b;
```

---

# Implicit Return

Arrow functions can automatically return a value.

```js
const square =
  num => num * num;
```

Equivalent:

```js
const square = num => {

  return num * num;

};
```

---

# Single Parameter

Parentheses are optional.

```js
const double =
  num => num * 2;
```

Also valid:

```js
const double =
  (num) => num * 2;
```

---

# No Parameters

Parentheses are required.

```js
const greet =
  () => "Hello";
```

---

# Returning Objects

Wrong:

```js
const createUser =
  name => {

    name: name

  };
```

This does NOT return an object.

Correct:

```js
const createUser =
  name => ({

    name: name

  });
```

Shorter:

```js
const createUser =
  name => ({ name });
```

Objects must be wrapped in:

```text
()
```

when using implicit return.

---

# Arrow Functions and this

Arrow functions do NOT have their own:

```text
this
```

They inherit `this` from their lexical scope.

Example:

```js
const counter = {

  count: 0,

  start() {

    setTimeout(() => {

      this.count++;

      console.log(this.count);

    }, 1000);

  }

};
```

The arrow function remembers `this` from:

```text
start()
```

---

# When NOT to Use Arrow Functions

Avoid arrow functions as object methods when you need object `this`.

Wrong:

```js
const user = {

  name: "Akio",

  greet: () => {

    console.log(this.name);

  }

};
```

Output:

```text
undefined
```

Correct:

```js
const user = {

  name: "Akio",

  greet() {

    console.log(this.name);

  }

};
```

---

# Destructuring

Destructuring extracts values from:

```text
Arrays

Objects
```

into variables.

---

# Array Destructuring

Without destructuring:

```js
const colors = [

  "red",

  "green",

  "blue"

];

const first =
  colors[0];

const second =
  colors[1];
```

With destructuring:

```js
const [
  first,
  second
] = colors;
```

Output:

```text
first  → red

second → green
```

Array destructuring works using:

```text
Position
```

---

# Skipping Array Values

```js
const colors = [

  "red",

  "green",

  "blue"

];

const [
  first,
  ,
  third
] = colors;
```

Output:

```text
first → red

third → blue
```

---

# Default Values

```js
const [

  first,

  second = "black"

] = ["red"];
```

Output:

```text
first  → red

second → black
```

---

# Swap Variables

Old way:

```js
let a = 1;

let b = 2;

const temp = a;

a = b;

b = temp;
```

Modern way:

```js
let a = 1;

let b = 2;

[a, b] = [b, a];
```

Output:

```text
a → 2

b → 1
```

---

# Object Destructuring

Object:

```js
const user = {

  name: "Akio",

  age: 25

};
```

Without destructuring:

```js
const name =
  user.name;

const age =
  user.age;
```

With destructuring:

```js
const {

  name,

  age

} = user;
```

Object destructuring works using:

```text
Property Name
```

---

# Renaming Variables

```js
const {

  name: userName

} = user;
```

Now:

```js
userName
```

contains:

```text
Akio
```

---

# Nested Destructuring

```js
const user = {

  address: {

    city: "Delhi"

  }

};
```

```js
const {

  address: {

    city

  }

} = user;
```

Output:

```text
Delhi
```

---

# Function Parameter Destructuring

Without destructuring:

```js
function greet(user) {

  console.log(
    user.name
  );

}
```

With destructuring:

```js
function greet({

  name

}) {

  console.log(name);

}
```

Very common in React.

---

# Spread Operator (...)

Spread means:

```text
Expand
```

Example:

```js
const numbers = [

  1,

  2,

  3

];

console.log(
  ...numbers
);
```

Equivalent:

```js
console.log(
  1,
  2,
  3
);
```

---

# Copy Arrays

```js
const numbers = [

  1,

  2,

  3

];

const copy = [

  ...numbers

];
```

Creates a new array.

---

# Merge Arrays

```js
const a = [

  1,

  2

];

const b = [

  3,

  4

];

const result = [

  ...a,

  ...b

];
```

Output:

```js
[1, 2, 3, 4]
```

---

# Spread Objects

```js
const user = {

  name: "Akio",

  age: 25

};

const copy = {

  ...user

};
```

---

# Updating Objects

Very common in React.

```js
const updatedUser = {

  ...user,

  age: 26

};
```

Later properties overwrite earlier properties.

Example:

```js
const user = {

  name: "Akio",

  age: 25

};

const updated = {

  ...user,

  age: 30

};
```

Output:

```js
{

  name: "Akio",

  age: 30

}
```

---

# Spread Creates a Shallow Copy

Important interview concept.

```js
const user = {

  name: "Akio",

  address: {

    city: "Delhi"

  }

};

const copy = {

  ...user

};
```

Now:

```js
copy.address.city =
  "Mumbai";
```

This also changes:

```js
user.address.city
```

Why?

Because nested objects share the same reference.

Spread creates:

```text
Shallow Copy
```

---

# Rest Operator (...)

Rest means:

```text
Collect
```

Same syntax:

```text
...
```

Different purpose.

---

# Rest Parameters

```js
function sum(...numbers) {

  return numbers.reduce(

    (total, num) =>

      total + num,

    0

  );

}
```

Usage:

```js
sum(
  1,
  2,
  3,
  4
);
```

Output:

```text
10
```

`numbers` becomes:

```js
[1, 2, 3, 4]
```

---

# Rest with Destructuring

```js
const numbers = [

  1,

  2,

  3,

  4

];

const [

  first,

  ...remaining

] = numbers;
```

Output:

```text
first

↓

1


remaining

↓

[2, 3, 4]
```

---

# Spread vs Rest

Same syntax:

```text
...
```

Spread:

```text
Expands Values
```

Rest:

```text
Collects Values
```

Remember:

```text
Spread

↓

Open


Rest

↓

Collect
```

---

# Template Literals

Old way:

```js
const name = "Akio";

const message =

  "Hello " + name;
```

Modern:

```js
const message =

  `Hello ${name}`;
```

Template literals use:

```text
Backticks
```

---

# Expressions in Template Literals

```js
const a = 5;

const b = 10;

console.log(

  `Total: ${a + b}`

);
```

Output:

```text
Total: 15
```

---

# Multi-Line Strings

```js
const text = `

Hello

World

`;
```

Template literals support multi-line strings.

---

# Optional Chaining (?.)

Problem:

```js
const user = {};

console.log(

  user.address.city

);
```

Output:

```text
TypeError
```

Modern solution:

```js
const city =

  user?.address?.city;
```

Output:

```text
undefined
```

No error.

---

# Optional Function Call

```js
user.getName?.();
```

The function executes only if it exists.

---

# Optional Chaining Checks Only

```text
null

undefined
```

It does NOT stop for:

```text
0

false

""
```

---

# Nullish Coalescing (??)

Returns the right value only when the left value is:

```text
null

or

undefined
```

Example:

```js
const name =

  null ?? "Guest";
```

Output:

```text
Guest
```

---

# ?? vs ||

Very important interview topic.

```js
const value =

  0 || 10;
```

Output:

```text
10
```

Because:

```text
0 is falsy
```

---

Using `??`:

```js
const value =

  0 ?? 10;
```

Output:

```text
0
```

Because `0` is NOT:

```text
null

or

undefined
```

---

# Falsy Values

`||` checks:

```text
false

0

""

null

undefined

NaN
```

`??` checks only:

```text
null

undefined
```

---

# Default Parameters

Old way:

```js
function greet(name) {

  name =
    name || "Guest";

  return `Hello ${name}`;

}
```

Modern:

```js
function greet(

  name = "Guest"

) {

  return `Hello ${name}`;

}
```

---

# Important Default Parameter Rule

Default values trigger only when the argument is:

```text
undefined
```

Example:

```js
function test(

  value = "Default"

) {

  return value;

}
```

```js
test(undefined);
```

Output:

```text
Default
```

But:

```js
test(null);
```

Output:

```text
null
```

---

# Enhanced Object Literals

Modern JavaScript provides shorter object syntax.

---

# Property Shorthand

Old:

```js
const name = "Akio";

const age = 25;

const user = {

  name: name,

  age: age

};
```

Modern:

```js
const user = {

  name,

  age

};
```

---

# Method Shorthand

Old:

```js
const calculator = {

  add: function(a, b) {

    return a + b;

  }

};
```

Modern:

```js
const calculator = {

  add(a, b) {

    return a + b;

  }

};
```

---

# Computed Property Names

```js
const key = "name";

const user = {

  [key]: "Akio"

};
```

Output:

```js
{

  name: "Akio"

}
```

---

# Map

A `Map` stores:

```text
Key

↓

Value
```

Example:

```js
const map =
  new Map();
```

Add value:

```js
map.set(

  "name",

  "Akio"

);
```

Get value:

```js
map.get("name");
```

Output:

```text
Akio
```

---

# Map Keys Can Be Any Type

```js
const user = {

  id: 1

};

const map =
  new Map();

map.set(

  user,

  "User Data"

);
```

Get:

```js
map.get(user);
```

Output:

```text
User Data
```

---

# Common Map Methods

```js
map.set(
  key,
  value
);

map.get(key);

map.has(key);

map.delete(key);

map.clear();

map.size;
```

---

# Set

A `Set` stores:

```text
Unique Values
```

Example:

```js
const numbers =

  new Set([

    1,

    2,

    2,

    3,

    3

  ]);
```

Result:

```text
1

2

3
```

Duplicates are removed.

---

# Remove Array Duplicates

Very common interview question.

```js
const numbers = [

  1,

  2,

  2,

  3,

  3

];

const unique = [

  ...new Set(numbers)

];
```

Output:

```js
[1, 2, 3]
```

---

# Common Set Methods

```js
set.add(value);

set.has(value);

set.delete(value);

set.clear();

set.size;
```

---

# Symbol

`Symbol` creates unique primitive values.

Example:

```js
const id1 =

  Symbol("id");

const id2 =

  Symbol("id");
```

Comparison:

```js
id1 === id2;
```

Output:

```text
false
```

Every Symbol is unique.

---

# for...of

`for...of` iterates over:

```text
Values
```

Example:

```js
const numbers = [

  10,

  20,

  30

];

for (

  const number of numbers

) {

  console.log(number);

}
```

Output:

```text
10

20

30
```

---

# for...of vs for...in

Very important interview concept.

`for...of`:

```text
Values
```

`for...in`:

```text
Keys
```

Example:

```js
const arr = [

  "A",

  "B",

  "C"

];
```

for...of:

```js
for (

  const value of arr

) {

  console.log(value);

}
```

Output:

```text
A

B

C
```

---

for...in:

```js
for (

  const index in arr

) {

  console.log(index);

}
```

Output:

```text
0

1

2
```

Remember:

```text
for...of

↓

Values


for...in

↓

Keys
```

---

# Real React Example

```js
function UserCard({

  user,

  onDelete

}) {

  const {

    id,

    name,

    address

  } = user;

  const city =

    address?.city ?? "Unknown";

  const handleDelete =

    () => onDelete?.(id);

  return (

    <div>

      <h2>

        {`${name} - ${city}`}

      </h2>

      <button

        onClick={handleDelete}

      >

        Delete

      </button>

    </div>

  );

}
```

This example uses:

```text
Destructuring

Arrow Functions

Optional Chaining

Nullish Coalescing

Template Literals
```

---

# Common Mistakes

## Mistake 1

Thinking `const` makes objects immutable.

Wrong.

```js
const user = {};

user.name = "Akio";
```

Valid.

`const` prevents reassignment.

Not mutation.

---

## Mistake 2

Confusing Spread and Rest.

Spread:

```js
const copy = [

  ...numbers

];
```

Rest:

```js
function sum(

  ...numbers

) {}
```

---

## Mistake 3

Thinking Spread Creates a Deep Copy

Wrong.

Spread creates:

```text
Shallow Copy
```

---

## Mistake 4

Using || When 0 is Valid

Bad:

```js
const count =

  value || 10;
```

Better:

```js
const count =

  value ?? 10;
```

---

## Mistake 5

Using Arrow Functions as Object Methods

Bad:

```js
const user = {

  name: "Akio",

  greet: () => {

    console.log(
      this.name
    );

  }

};
```

Use normal methods when you need object `this`.

---

# Common Interview Questions

## What is ES6?

ECMAScript 2015.

A major JavaScript language update.

---

## Difference Between let, const and var?

```text
var

↓

Function Scoped


let

↓

Block Scoped
Reassignable


const

↓

Block Scoped
Cannot Reassign
```

---

## Do Arrow Functions Have Their Own this?

No.

They inherit `this` from lexical scope.

---

## What is Destructuring?

Extracting values from arrays or objects into variables.

---

## Difference Between Spread and Rest?

Spread:

```text
Expands Values
```

Rest:

```text
Collects Values
```

---

## Does Spread Create a Deep Copy?

No.

It creates a shallow copy.

---

## Difference Between ?? and ||?

`||`

Checks all falsy values.

`??`

Checks only:

```text
null

undefined
```

---

## When Do Default Parameters Trigger?

Only when the argument is:

```text
undefined
```

---

## What is Set?

A collection of unique values.

---

## What is Symbol?

A primitive type used to create unique values.

---

## Difference Between for...of and for...in?

```text
for...of

↓

Values


for...in

↓

Keys
```

---

# Quick Revision

✅ ES6 is ECMAScript 2015

✅ Use `const` by default

✅ Use `let` when reassignment is needed

✅ Avoid `var`

✅ Arrow functions don't have their own `this`

✅ Array destructuring works by position

✅ Object destructuring works by property name

✅ Spread expands values

✅ Rest collects values

✅ Spread creates a shallow copy

✅ Template literals use backticks

✅ `${}` allows expressions

✅ `?.` safely accesses nullable properties

✅ `??` checks only null and undefined

✅ `||` checks all falsy values

✅ Default parameters trigger only for undefined

✅ Map stores key-value pairs

✅ Map keys can be any type

✅ Set stores unique values

✅ Symbol creates unique values

✅ `for...of` iterates values

✅ `for...in` iterates keys