# JavaScript HTTP & Fetch API

## Why This Topic Matters

Modern web applications constantly communicate with servers.

Examples:

- Login
- Register
- Load products
- Fetch user profiles
- Submit forms
- Upload files

The primary way to do this in modern JavaScript is:

```js
fetch()
```

The Fetch API is a promise-based interface for making HTTP requests. :contentReference[oaicite:0]{index=0}

---

# What is HTTP?

HTTP stands for:

```text
HyperText Transfer Protocol
```

It is the communication protocol used between:

```text
Browser ↔ Server
```

Every API call is an HTTP request.

Example:

```text
GET /users/1
```

Server responds with:

```json
{
  "id": 1,
  "name": "John"
}
```

---

# HTTP Request Structure

A request consists of:

```text
Method
URL
Headers
Body (optional)
```

Example:

```http
POST /users

Content-Type: application/json

{
  "name": "Akio"
}
```

---

# HTTP Response Structure

A response contains:

```text
Status Code
Headers
Body
```

Example:

```http
200 OK

{
  "id": 1,
  "name": "Akio"
}
```

---

# Common HTTP Methods

## GET

Retrieve data.

```http
GET /users
```

Example:

```js
Fetch all users
```

---

## POST

Create data.

```http
POST /users
```

Example:

```js
Create a user
```

---

## PUT

Replace existing resource.

```http
PUT /users/1
```

---

## PATCH

Partially update resource.

```http
PATCH /users/1
```

---

## DELETE

Remove resource.

```http
DELETE /users/1
```

---

# HTTP Status Codes

## Success

```text
200 OK
201 Created
204 No Content
```

---

## Client Errors

```text
400 Bad Request
401 Unauthorized
403 Forbidden
404 Not Found
```

---

## Server Errors

```text
500 Internal Server Error
502 Bad Gateway
503 Service Unavailable
```

---

# What is Fetch API?

Fetch is the modern browser API for making HTTP requests. It returns a Promise that resolves to a Response object. :contentReference[oaicite:1]{index=1}

Basic syntax:

```js
fetch(url, options)
```

---

# First GET Request

```js
fetch("https://api.example.com/users")
  .then(response => response.json())
  .then(data => {
    console.log(data);
  });
```

---

# Understanding fetch()

```js
const promise = fetch(url);
```

Returns:

```js
Promise<Response>
```

Not actual data.

---

# Response Object

```js
fetch(url)
  .then(response => {
    console.log(response);
  });
```

Contains:

```js
status
ok
headers
body
url
```

---

# Parsing JSON

Most APIs return JSON.

```js
fetch(url)
  .then(response => response.json())
  .then(data => {
    console.log(data);
  });
```

`response.json()` also returns a Promise. :contentReference[oaicite:2]{index=2}

---

# Why Two .then() Calls?

```js
fetch(url)
  .then(response => response.json())
  .then(data => console.log(data));
```

Step 1:

```js
fetch()
```

returns:

```js
Promise<Response>
```

Step 2:

```js
response.json()
```

returns:

```js
Promise<Data>
```

---

# Using Async/Await

Modern approach:

```js
async function getUsers() {
  const response = await fetch(url);

  const data = await response.json();

  console.log(data);
}
```

Cleaner and easier to read. :contentReference[oaicite:3]{index=3}

---

# Making a POST Request

```js
fetch("/users", {
  method: "POST",
  headers: {
    "Content-Type": "application/json"
  },
  body: JSON.stringify({
    name: "Akio"
  })
});
```

---

# Why JSON.stringify()?

Wrong:

```js
body: {
  name: "Akio"
}
```

Correct:

```js
body: JSON.stringify({
  name: "Akio"
})
```

HTTP request bodies are text.

---

# Custom Headers

```js
fetch(url, {
  headers: {
    Authorization: "Bearer token",
    "Content-Type": "application/json"
  }
});
```

Commonly used for:

- Authentication
- API keys
- Content types

---

# Checking Response Status

```js
const response = await fetch(url);

console.log(response.status);
```

Example:

```js
200
404
500
```

---

# response.ok

Convenient success check.

```js
if (response.ok) {
  console.log("Success");
}
```

Equivalent:

```js
status >= 200 &&
status < 300
```

---

# Common Mistake

Many developers assume:

```js
fetch()
```

rejects on:

```text
404
500
```

Wrong.

Fetch resolves normally for HTTP errors and only rejects for network failures. :contentReference[oaicite:4]{index=4}

---

Example:

```js
const response = await fetch("/wrong-url");

console.log(response.status);
```

Output:

```js
404
```

No exception thrown.

---

# Proper Error Handling

```js
try {
  const response = await fetch(url);

  if (!response.ok) {
    throw new Error("Request failed");
  }

  const data = await response.json();

} catch (error) {
  console.error(error);
}
```

---

# Network Error vs HTTP Error

## Network Error

```text
No internet
DNS failure
Connection lost
```

Triggers:

```js
catch()
```

---

## HTTP Error

```text
404
500
401
```

Does NOT automatically trigger:

```js
catch()
```

Must manually check:

```js
response.ok
```

---

# AbortController

Used to cancel requests.

```js
const controller =
  new AbortController();

fetch(url, {
  signal: controller.signal
});

controller.abort();
```

Useful for:

- Search boxes
- React cleanup
- Preventing duplicate requests

Fetch supports request cancellation via AbortController. :contentReference[oaicite:5]{index=5}

---

# Handling Loading State

Common UI pattern:

```js
loading = true
```

Request starts.

```js
loading = false
```

Request completes.

---

Example:

```js
try {
  setLoading(true);

  const response =
    await fetch(url);

} finally {
  setLoading(false);
}
```

---

# Fetch Multiple APIs

Sequential:

```js
const users =
  await fetch("/users");

const posts =
  await fetch("/posts");
```

Slow.

---

Parallel:

```js
const [users, posts] =
  await Promise.all([
    fetch("/users"),
    fetch("/posts")
  ]);
```

Faster.

---

# File Upload Example

```js
const formData =
  new FormData();

formData.append("file", file);

fetch("/upload", {
  method: "POST",
  body: formData
});
```

Do NOT manually set:

```js
Content-Type
```

Browser handles it.

---

# CORS

Stands for:

```text
Cross-Origin Resource Sharing
```

Example:

```js
localhost:3000
```

trying to access:

```js
api.example.com
```

Browser may block the request.

CORS is controlled by the server.

---

# Fetch Lifecycle

```text
fetch()
    ↓
HTTP Request Sent
    ↓
Server Processing
    ↓
Response Headers Received
    ↓
Promise Resolved
    ↓
response.json()
    ↓
Data Available
```

---

# Fetch and Event Loop

```js
fetch(url)
  .then(data => {
    console.log(data);
  });
```

Flow:

```text
Call Stack
    ↓
Web API
    ↓
Network Request
    ↓
Promise Resolution
    ↓
Microtask Queue
    ↓
Call Stack
```

Promise callbacks execute as microtasks.

---

# Real React Example

```js
useEffect(() => {
  async function getUsers() {
    const response =
      await fetch("/users");

    const data =
      await response.json();

    setUsers(data);
  }

  getUsers();
}, []);
```

This pattern appears in almost every React application.

---

# XMLHttpRequest vs Fetch

| Feature | XMLHttpRequest | Fetch |
|----------|----------|----------|
| Promise Based | ❌ | ✅ |
| Async/Await | ❌ | ✅ |
| Cleaner Syntax | ❌ | ✅ |
| AbortController | Limited | ✅ |
| Modern Standard | ❌ | ✅ |

Fetch is the recommended modern API. :contentReference[oaicite:6]{index=6}

---

# Common Interview Questions

## What is Fetch API?

A promise-based browser API for making HTTP requests. :contentReference[oaicite:7]{index=7}

---

## What does fetch() return?

```js
Promise<Response>
```

---

## Does fetch reject on 404?

No.

Must check:

```js
response.ok
```

or:

```js
response.status
```

:contentReference[oaicite:8]{index=8}

---

## Why call response.json()?

To parse the response body into a JavaScript object.

---

## Difference Between GET and POST?

GET:

```text
Retrieve Data
```

POST:

```text
Create Data
```

---

## What is CORS?

A browser security mechanism controlling cross-origin requests.

---

## What is AbortController?

A way to cancel in-progress fetch requests. :contentReference[oaicite:9]{index=9}

---

# Quick Revision

✅ HTTP = communication protocol between browser and server

✅ Fetch API makes HTTP requests

✅ fetch() returns Promise<Response>

✅ response.json() returns Promise<Data>

✅ GET retrieves data

✅ POST creates data

✅ PUT replaces data

✅ PATCH updates data

✅ DELETE removes data

✅ Always check response.ok

✅ Fetch does NOT reject on 404/500

✅ Use try/catch for network errors

✅ AbortController cancels requests

✅ Promise.all() runs requests in parallel

✅ Fetch works with Promises and Async/Await

✅ Fetch callbacks run through the Event Loop microtask queue