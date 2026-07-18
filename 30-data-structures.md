# JavaScript Data Structures

## Why Data Structures Matter

## What is a Data Structure?

## Why Choosing the Right Data Structure is Important

## Data Structure Trade-offs

## Built-in JavaScript Data Structures

### Arrays
- What is an Array?
- Memory Representation
- Time Complexity
- Common Methods
- When to Use
- When Not to Use
- Examples
- Real-world Example

### Objects
- What is an Object?
- Internal Working
- Time Complexity
- Property Lookup
- Prototype Chain
- When to Use
- Examples

### Map
- Why Map was Introduced
- Map vs Object
- Methods
- Time Complexity
- Examples

### Set
- What is Set?
- Unique Values
- Methods
- Time Complexity
- Remove Duplicates Example

### WeakMap
- Garbage Collection
- Weak References
- Limitations
- Real-world Example

### WeakSet
- Weak References
- Use Cases
- Examples

---

# Implementing Common Data Structures

## Stack (LIFO)

### What is Stack?

### LIFO Principle

### Stack Diagram

```text
Push

┌───┐
│ 3 │
├───┤
│ 2 │
├───┤
│ 1 │
└───┘

↓

Pop

┌───┐
│ 2 │
├───┤
│ 1 │
└───┘
```

### JavaScript Implementation

### Operations

- push
- pop
- peek
- isEmpty
- size

### Time Complexity

### Real-world Uses

- Undo
- Browser History
- Function Call Stack

---

## Queue (FIFO)

### Queue Diagram

```text
Front → A → B → C ← Rear
```

### JavaScript Implementation

### Operations

- enqueue
- dequeue
- front
- rear

### Time Complexity

### Real-world Examples

- Printer Queue
- Task Queue
- Message Queue

---

## Linked List

### What is Linked List?

### Node Structure

```text
+------+------+
| Data | Next | ------>
+------+------+
```

### Types

- Singly Linked List
- Doubly Linked List

### Operations

- Insert
- Delete
- Search

### Time Complexity

### JavaScript Implementation

---

## Binary Search Tree (BST)

### What is BST?

### Rules

```text
Left < Root < Right
```

### Tree Diagram

```text
        10
       /  \
      5   20
     / \    \
    2   8   30
```

### Operations

- Insert
- Search
- Delete

### Traversals

- Inorder
- Preorder
- Postorder

### Complexity

---

# Big-O Comparison

| Structure | Access | Search | Insert | Delete |
|-----------|--------|--------|--------|--------|
| Array | O(1) | O(n) | O(n) | O(n) |
| Object | O(1) | O(1) | O(1) | O(1) |
| Map | O(1) | O(1) | O(1) | O(1) |
| Set | O(1) | O(1) | O(1) | O(1) |
| Stack | O(n) | O(n) | O(1) | O(1) |
| Queue | O(n) | O(n) | O(1) | O(1) |
| Linked List | O(n) | O(n) | O(1)* | O(1)* |
| BST (Average) | O(log n) | O(log n) | O(log n) | O(log n) |

---

# Which Data Structure Should You Use?

```text
Need ordered collection?
        │
        ├── Yes → Array
        │
Need key-value pairs?
        │
        ├── String keys → Object
        ├── Any key type → Map
        │
Need unique values?
        │
        ├── Set
        │
Need Undo?
        │
        ├── Stack
        │
Need FIFO?
        │
        ├── Queue
        │
Need frequent insertion/deletion?
        │
        ├── Linked List
        │
Need sorted searching?
        │
        ├── Binary Search Tree
```

---

# React Examples

- Managing selected IDs using Set
- Caching API responses with Map
- Using Arrays for rendering lists
- Storing component configuration in Objects

---

# Common Mistakes

- Using Array for frequent lookup instead of Map
- Using Object when keys are objects
- Using `shift()` in large queues
- Assuming Set maintains sorted order
- Using WeakMap like a normal Map

---

# Common Interview Questions

- Object vs Map
- Map vs WeakMap
- Set vs Array
- Stack vs Queue
- Why is `shift()` O(n)?
- Why is `push()` O(1)?
- When should you use a Linked List?
- Explain BST search.
- How does JavaScript implement arrays internally?
- What is the difference between WeakSet and Set?

---

# Quick Revision

✅ Arrays → Ordered collections with index access

✅ Objects → String-keyed data

✅ Map → Key-value pairs with any key type

✅ Set → Unique values

✅ WeakMap → Object keys with automatic garbage collection

✅ WeakSet → Weakly held object values

✅ Stack → LIFO

✅ Queue → FIFO

✅ Linked List → Fast insert/delete

✅ Binary Search Tree → Sorted hierarchical data

✅ Choose the data structure based on the most frequent operation

---

# One Interview Question That Appears Everywhere

```js
const users = ["Alice", "Bob", "Charlie"];

console.log(users.includes("Charlie"));
```

**Question:** What is the time complexity of `includes()`?

**Answer:** `O(n)` because it may need to scan each element until it finds a match (or reaches the end). If you need frequent membership checks, a `Set` offers average-case `O(1)` lookups. :contentReference[oaicite:1]{index=1}