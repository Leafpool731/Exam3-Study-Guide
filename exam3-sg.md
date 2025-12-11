# 🚀 CS 246 Final Exam Study Guide (Simplified)

This file explains key topics in very simple language. Each section says what the tool is, when to use it, and gives a tiny example.

## 1. Writing a Small Server with Express

Express makes a simple web server in Node.js. The server gets requests (`req`) and sends responses (`res`).

Quick facts:
- `req.query` holds query data from the URL (`/planets?name=Mars`).
- `app.use(express.static('public'))` serves files like HTML and images from the `public` folder.
- Use `res.json(data)` to send JSON back to the client.
- `console.log()` shows messages only in the server terminal, not in the browser.

When to use Express:
- Small APIs and simple web apps.
- Prototypes or learning projects.
- Small microservices that do one job.

When not to use:
- For very high-speed services or very large, tangled apps.

Example server (listens on port 3000):

```javascript
// server.js
const express = require('express');
const app = express();
const PORT = 3000;

app.use(express.json());
app.use(express.static('public'));

app.get('/', (req, res) => {
  res.send('Welcome to my API!');
});

app.listen(PORT, () => {
  console.log(`Server running on http://localhost:${PORT}`);
});
```

Run it:
```bash
node server.js
```

Example: filter by query:

```javascript
app.get('/api/planets', (req, res) => {
  const name = req.query.name;
  const planets = [
    { id: 1, name: 'Mars', distance: 225 },
    { id: 2, name: 'Venus', distance: 108 }
  ];
  const filtered = name ? planets.filter(p => p.name.toLowerCase() === name.toLowerCase()) : planets;
  res.json(filtered);
});
```

## 2. Getting Data in the Browser with `fetch()`

`fetch()` asks a server for data. It returns a `Response` object. To read the body, call `response.json()`.

Important: `fetch()` only throws on network errors. It does NOT throw for HTTP errors like 404. You must check `response.ok`.

Simple example:

```javascript
fetch('/api/planets?name=Mars')
  .then(res => {
    if (!res.ok) throw new Error('Server error ' + res.status);
    return res.json();
  })
  .then(data => console.log(data))
  .catch(err => console.error('Fetch failed:', err.message));

// Async/await version
async function loadPlanet(name) {
  const res = await fetch(`/api/planets?name=${encodeURIComponent(name)}`);
  if (!res.ok) throw new Error('Server error ' + res.status);
  const data = await res.json();
  return data[0] || null;
}
```

When to use `fetch()`:
- To load lists or details for a web page.
- To send form data with `POST` or `PUT`.

When not to use:
- For live, fast streams — use WebSockets instead.

## 3. Containers and Codespaces (Very Short)

A container (Docker) bundles your app and its tools so it runs the same anywhere.

Dev Container / Codespace: a ready-to-code workspace in a container. Open it and you have the right tools.

When to use:
- Share a working environment with teammates.
- Test the app before deploying.

When not to use:
- For tiny scripts or quick notes — containers add setup work.

## 4. Command Line and Git (Very Short)

Useful commands:
- `ls` list files, `cd` change folder, `pwd` show where you are.
- `cat` shows file content, `head` and `tail` show start/end.
- `grep` finds text inside files.

Git basics:
- `git clone` copy a repo, `git add` stage changes, `git commit -m` save a change, `git push` upload to GitHub.
- Use branches (`git checkout -b`) to try changes safely.

When to use CLI/Git:
- Fast file checks, saving work, and sharing code.

## 5. Bash Scripts (Very Short)

Bash scripts run a list of shell commands. Use them to automate simple repetitive tasks.

Example script idea: move old logs to an `archive/` folder.

When to use scripts:
- Automate backups, cleanups, or repeatable steps.

When not to use scripts:
- For complex logic; use Python or Node instead.

## 6. Testing (Very Short)

Types:
- Unit tests: test one function.
- Integration tests: test parts working together.

Why test:
- Find bugs early and keep code working after changes.

Example (Vitest):

```javascript
import { describe, it, expect } from 'vitest';

function calculatePlanetDistance(name) {
  const planets = { Mars: 225, Venus: 108, Jupiter: 778 };
  return planets[name] || null;
}

describe('calculatePlanetDistance', () => {
  it('returns Mars distance', () => expect(calculatePlanetDistance('Mars')).toBe(225));
  it('returns null for unknown', () => expect(calculatePlanetDistance('Pluto')).toBeNull());
});
```

When to write tests:
- For important functions and behavior you don't want to break.

## 7. Security (Very Short)

Basic rules:
- Treat all outside input as unsafe. Validate and clean it.
- Use parameterized queries for databases (do not build SQL with string + user input).
- Escape user text before showing it on a web page.

Bad example (do not do this):
```javascript
const query = `SELECT * FROM users WHERE email = '${userInput}'`;
```

Good example:
```javascript
const query = 'SELECT * FROM users WHERE email = $1';
client.query(query, [userInput]);
```

## 8. AI (Very Short)

Softmax turns raw model scores into probabilities that add up to 1.

Simple softmax in Python:

```python
import math

def softmax(logits):
    exps = [math.exp(x) for x in logits]
    s = sum(exps)
    return [e / s for e in exps]

print(softmax([2.0, 1.0, 0.5]))
```

When to use softmax:
- To show probabilities for classification tasks.

Softmax formula (plain text):

S(z_i) = e^{z_i} / (e^{z_1} + e^{z_2} + ... + e^{z_K})

---

If you want, I can now:
- Make a tiny `index.html` to test the `fetch()` examples in the browser, or
- Extract the `server.js` into a runnable file and add a minimal `package.json` so you can run the server locally.

---

## Review Questions (Practice — Multiple Choice)

Try these quick questions in the same style as the professor's quizzes. Pick the best answer, then check the Answers section below.

1) Which of these statements about Express is incorrect?
  A) Express apps are written in Java
  B) Express can be used to implement an API
  C) Express is built on top of Node.js
  D) Express can deliver web pages

2) When a user types a URL in a browser and presses Enter, what HTTP request is sent?
  A) GET
  B) DELETE
  C) POST
  D) FETCH

3) In an Express route handler, how could a programmer extract `temperature` from the URL `...?latitude=45.0&longitude=-85.0&temperature=37.0`?
  A) `req['query']['temperature']`
  B) `req.query['temperature']`
  C) `req.query.temperature`
  D) All of the above

4) Given a JavaScript object `person`, which statement sends it back to the client inside a handler (`(req, res) => { ... }`)?
  A) `req.json(person);`
  B) `res.send(object.person);`
  C) `res.send(person.toJson());`
  D) `res.json(person);`

5) What best describes the purpose of `fetch()`?
  A) Execute a POST request
  B) Send HTTP requests and receive HTTP responses
  C) Send an HTTP request (one-way only)
  D) Execute a GET request only

6) Which of the following cannot be included inside a `fetch()` call options (the second argument)?
  A) The default language
  B) A path to a resource to fetch
  C) The method name (e.g., `GET`, `POST`)
  D) Headers

7) What is the return type of the `fetch()` method?
  A) `await`
  B) A JSON object
  C) `async`
  D) A Promise

8) When using `fetch()` to connect to many public APIs, what do they often require the request to include?
  A) A promise
  B) An API key
  C) A credit card number
  D) A promissory note

9) Which Express helper is commonly used to serve static files (like `index.html` or images)?
  A) `res.static()`
  B) `app.static()`
  C) `app.use(express.static('public'))`
  D) `express.serve('public')`

10) Which response method automatically sets `Content-Type` to `application/json` when sending an object?
   A) `res.sendJSON()`
   B) `res.json()`
   C) `res.send()`
   D) `req.json()`

---

## Answers (check yourself)

1) A — Express apps are written in JavaScript (not Java). Option A is incorrect.

2) A — `GET` is sent when you enter a URL and press Enter. `FETCH` is not an HTTP method.

3) D — All three forms (`req['query']['temperature']`, `req.query['temperature']`, `req.query.temperature`) work.

4) D — Use `res.json(person)` to send a JavaScript object as JSON with the right header.

5) B — `fetch()` sends HTTP requests and receives responses (it is not limited to POST or GET only).

6) A — "The default language" is not a standard `fetch()` option. Paths, method, and headers are valid parts of a request.

7) D — `fetch()` returns a Promise. You `await` the promise or use `.then()` to get the Response.

8) B — Many APIs require an API key (or some form of authentication) included in headers or query parameters.

9) C — `app.use(express.static('public'))` is the common way to serve static files in Express.

10) B — `res.json()` sets `Content-Type: application/json` and sends the JSON-encoded object.

Use these for quick self-quizzes — I can also convert this to a printable quiz sheet or make an interactive `index.html` quiz if you want.
