# JavaScript Regular Expressions (Regex)

## Why Regex Matters

Imagine you need to:

- Validate an email
- Check if a phone number is valid
- Find all hashtags
- Extract URLs
- Remove extra spaces
- Replace all digits

Instead of writing dozens of loops and conditions, a single Regex can solve the problem.

Regex is one of the most powerful tools for working with text.

---

# What is a Regular Expression?

A Regular Expression (Regex) is:

```text
A pattern used to search,
match, validate,
extract or replace text.
```

Think of it as:

```text
Text Pattern

↓

Search Engine

↓

Matching Text
```

---

# Creating a Regex

There are two ways.

## 1. Regex Literal (Preferred)

```js
const pattern = /hello/;
```

Use this when the pattern is known beforehand.

---

## 2. RegExp Constructor

```js
const pattern =
  new RegExp("hello");
```

Useful when creating patterns dynamically.

```js
const word = "hello";

const pattern =
  new RegExp(word);
```

---

# test()

Checks whether a string matches.

Returns:

```text
true

or

false
```

Example:

```js
const regex = /hello/;

console.log(
  regex.test("hello world")
);
```

Output:

```js
true
```

---

# match()

Returns the matched text.

```js
const text =
  "Hello JavaScript";

console.log(
  text.match(/JavaScript/)
);
```

Output:

```js
["JavaScript"]
```

---

# search()

Returns:

```text
Index

of first match
```

Example:

```js
"Hello World"
  .search(/World/);
```

Output:

```js
6
```

---

# replace()

Replace matching text.

```js
const text =
  "Hello World";
```

```js
text.replace(
  /World/,
  "JavaScript"
);
```

Output:

```text
Hello JavaScript
```

---

# replaceAll()

Replace every match.

```js
"apple apple"

.replaceAll(
  "apple",
  "banana"
);
```

Output:

```text
banana banana
```

---

# split()

Regex can split strings.

```js
const text =
  "apple,banana,orange";
```

```js
text.split(/,/);
```

Output:

```js
[
  "apple",
  "banana",
  "orange"
]
```

---

# Character Classes

## Any Digit

```text
\d
```

Matches:

```text
0–9
```

Example:

```js
/\d/
```

Matches:

```text
5
```

---

## Non-Digit

```text
\D
```

Matches:

```text
Anything except digits
```

---

## Word Character

```text
\w
```

Matches:

```text
Letters

Digits

Underscore
```

Equivalent:

```text
[A-Za-z0-9_]
```

---

## Non-Word Character

```text
\W
```

Everything except:

```text
Letters

Digits

_
```

---

## Whitespace

```text
\s
```

Matches:

- Space
- Tab
- Newline

---

## Non-Whitespace

```text
\S
```

Everything except whitespace.

---

## Any Character

```text
.
```

Matches:

```text
Almost any single character
```

Except newline (unless using the `s` flag).

Example:

```js
/a.c/
```

Matches:

```text
abc

a1c

a-c
```

---

# Custom Character Classes

```js
/[abc]/
```

Matches:

```text
a

or

b

or

c
```

---

Range:

```js
/[a-z]/
```

Matches:

```text
Lowercase letters
```

---

Uppercase:

```js
/[A-Z]/
```

---

Digits:

```js
/[0-9]/
```

---

Combined:

```js
/[A-Za-z]/
```

Letters only.

---

Negated Class

```js
/[^0-9]/
```

Matches:

```text
Anything except digits
```

---

# Quantifiers

## *

```text
Zero or More
```

```js
/ab*/
```

Matches:

```text
a

ab

abb

abbb
```

---

## +

```text
One or More
```

```js
/ab+/
```

Matches:

```text
ab

abb

abbb
```

Does NOT match:

```text
a
```

---

## ?

```text
Zero or One
```

```js
/colou?r/
```

Matches:

```text
color

colour
```

---

## {n}

Exactly:

```text
n times
```

```js
/\d{4}/
```

Matches:

```text
2025
```

---

## {n,m}

Between:

```text
n and m times
```

```js
/^\d{3,5}$/
```

Matches:

```text
123

12345
```

---

# Anchors

## ^

Start of string.

```js
/^Hello/
```

Matches:

```text
Hello World
```

Not:

```text
Say Hello
```

---

## $

End of string.

```js
/World$/
```

Matches:

```text
Hello World
```

---

Together:

```js
/^Hello$/
```

Matches ONLY:

```text
Hello
```

---

# Word Boundary

```text
\b
```

Example:

```js
/\bcat\b/
```

Matches:

```text
cat
```

Not:

```text
category
```

---

# Flags

## g

Global.

Find:

```text
ALL matches
```

```js
"cat cat"

.match(/cat/g);
```

Output:

```js
[
  "cat",
  "cat"
]
```

---

## i

Ignore case.

```js
/hello/i
```

Matches:

```text
Hello

HELLO

hello
```

---

## m

Multiline.

Makes:

```text
^

$
```

work on every line.

---

## s

DotAll.

Allows:

```text
.
```

to match newlines too.

---

# Capturing Groups

Parentheses create groups.

```js
const regex =
  /(\d{2})-(\d{2})-(\d{4})/;
```

Text:

```text
25-12-2025
```

Output:

```text
25

12

2025
```

Each group is stored separately.

---

# Named Capture Groups

```js
const regex =
/(?<year>\d{4})/;
```

Access:

```js
match.groups.year
```

Much more readable.

---

# Backreferences

Reference earlier groups.

Example:

```js
/(\w+)\s\1/
```

Matches:

```text
hello hello
```

Does NOT match:

```text
hello world
```

---

# Greedy Matching

Default behavior.

Example:

```text
<b>Hello</b><b>World</b>
```

Regex:

```js
/<b>.*<\/b>/
```

Matches:

```text
Everything
```

---

# Lazy Matching

Use:

```text
*?
```

Example:

```js
/<b>.*?<\/b>/
```

Matches:

```text
First <b>...</b>
```

only.

---

# Common Regex Patterns

## Numbers Only

```js
/^\d+$/
```

---

## Letters Only

```js
/^[A-Za-z]+$/
```

---

## Username

Letters, digits, underscore.

```js
/^\w+$/
```

---

## Email (Simple)

```js
/^[^\s@]+@[^\s@]+\.[^\s@]+$/
```

Good for basic validation.

---

## Phone Number (Simple)

```js
/^\d{10}$/
```

Matches:

```text
9876543210
```

---

## Password

At least:

- 8 characters
- One uppercase
- One lowercase
- One digit

```js
/^(?=.*\d)(?=.*[a-z])(?=.*[A-Z]).{8,}$/
```

---

# String Methods Using Regex

```js
test()

match()

matchAll()

replace()

replaceAll()

search()

split()
```

These methods make Regex useful in JavaScript.

---

# Regex vs String.includes()

```js
text.includes("cat");
```

Checks literal text.

---

Regex:

```js
/ca./
```

Matches:

```text
cat

car

cap
```

Regex is much more flexible.

---

# Common Mistakes

## Mistake 1

Forgetting `g`.

Wrong:

```js
"cat cat"

.match(/cat/);
```

Output:

```text
First match only
```

Correct:

```js
/ cat /g
```

---

## Mistake 2

Confusing `*` and `+`.

```text
*

Zero or more

+

One or more
```

---

## Mistake 3

Using greedy matching accidentally.

Bad:

```js
/<div>.*<\/div>/
```

Better:

```js
/<div>.*?<\/div>/
```

---

## Mistake 4

Using Regex for simple tasks.

Bad:

```js
/^hello$/
```

Better:

```js
text === "hello"
```

Regex is powerful, but don't use it when a simple string method is clearer.

---

# Real React Example

Email validation:

```js
const emailRegex =
/^[^\s@]+@[^\s@]+\.[^\s@]+$/;

const valid =
emailRegex.test(email);
```

Commonly used in forms.

---

# Common Interview Questions

## What is Regex?

A pattern used to match text.

---

## Difference Between test() and match()?

`test()`

Returns:

```text
true

or

false
```

`match()`

Returns matched text.

---

## Difference Between * and +?

```text
*

0 or more

+

1 or more
```

---

## What does ^ mean?

Beginning of string.

---

## What does $ mean?

End of string.

---

## Difference Between \d and \D?

```text
\d

Digits

\D

Non-digits
```

---

## Difference Between \w and \W?

```text
\w

Word character

\W

Non-word character
```

---

## What is the g flag?

Global search.

Find all matches.

---

## What is the i flag?

Ignore case.

---

## What is a Capturing Group?

Parentheses that capture part of a match for later use.

---

## What is Greedy Matching?

Matches as much text as possible.

---

## What is Lazy Matching?

Matches as little text as possible.

---

# Quick Revision

✅ Regex is a pattern-matching language

✅ Create Regex using `/pattern/` or `new RegExp()`

✅ `test()` returns true/false

✅ `match()` returns matched text

✅ `replace()` replaces matches

✅ `split()` can split using Regex

✅ `\d` matches digits

✅ `\w` matches word characters

✅ `\s` matches whitespace

✅ `.` matches almost any character

✅ `*` means zero or more

✅ `+` means one or more

✅ `?` means zero or one

✅ `^` matches the start of a string

✅ `$` matches the end of a string

✅ `g` finds all matches

✅ `i` ignores case

✅ `m` enables multiline mode

✅ `()` creates capturing groups

✅ `.*` is greedy

✅ `.*?` is lazy