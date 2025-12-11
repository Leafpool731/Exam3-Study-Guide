# CS 246 Final Exam Study Guide

## 1. Express.js and Backend API Development

Express is a minimal web framework built on top of Node.js. It is designed to handle HTTP requests and send HTTP responses through route handlers.

**Key Mechanisms:**
- Request data from the URL is accessed via `req.query` (e.g., `/api/planets?name=Mars`).
- Static files are served using `app.use(express.static('public'))`.
- Use `res.json(data)` to send JSON-encoded responses with proper Content-Type headers.
- `console.log()` output appears only in the server terminal, never in the client browser.

**Use Cases:**
- Building REST APIs.
- Prototyping and learning.
- Lightweight microservices.

**When Not to Use:**
- High-performance or specialized applications.

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

## 2. Client-Side HTTP Requests with `fetch()`

`fetch()` is the modern standard for making HTTP requests from the browser. It returns a Promise that resolves to a Response object.

**Critical Point:** `fetch()` only rejects on network errors (e.g., no internet). It does NOT reject on HTTP error statuses (404, 500, etc.). You must manually check `response.ok` to detect server errors.

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

## 2.5 Promises and Asynchronous Control Flow

A **Promise** represents the eventual result of an asynchronous operation. It is a container for a value that may not yet exist.

**Promise States:**
- **Pending:** The operation is in progress.
- **Fulfilled (Resolved):** The operation succeeded and the value is available.
- **Rejected:** The operation failed.

Promises are fundamental to asynchronous JavaScript. Functions like `fetch()` return promises. You must handle the promise's result using `.then()`, `.catch()`, or `async/await`.

### Using `.then()` and `.catch()`

```javascript
fetch('/api/data')
  .then(response => {
    // Runs AFTER response arrives
    console.log('Got response:', response);
    return response.json(); // Returns a NEW promise
  })
  .then(data => {
    // Runs AFTER data is parsed
    console.log('Got data:', data);
  })
  .catch(error => {
    // Runs if ANY promise above fails
    console.error('Error:', error);
  });
```

**Key Characteristics:**
- `.then()` executes once the preceding promise resolves.
- Multiple `.then()` calls can be chained to sequence operations.
- `.catch()` handles rejections from any promise in the chain.

### Using `async/await` (Cleaner Syntax)

`async/await` is just a nicer way to work with promises. It makes code look synchronous.

```javascript
// async function always returns a promise
async function getData() {
  try {
    const response = await fetch('/api/data'); // Wait for response
    if (!response.ok) throw new Error('Server error');
    const data = await response.json(); // Wait for JSON parsing
    console.log('Got data:', data);
    return data;
  } catch (error) {
    console.error('Error:', error);
  }
}

// Call the async function (it returns a promise)
getData().then(d => console.log(d)).catch(e => console.error(e));
```

Key points:
- `async` keyword marks a function that returns a promise.
- `await` pauses execution until the promise resolves.
- Use `try/catch` for error handling instead of `.catch()`.
- `async/await` is easier to read than nested `.then()` chains.

When to use Promises:
- Any time you need to wait for async work (network, files, timers).
- `fetch()` always returns a promise.
- When writing functions that take time to complete.

## 3. Containers and Codespaces

A **container** bundles an application and its dependencies (libraries, runtime, system tools) into an isolated, self-contained unit that runs consistently across different computing environments. **GitHub Codespaces** provides cloud-hosted development environments powered by container technology, eliminating the need to install tools locally.

**Key Mechanisms:**
- Container isolation ensures dependencies do not interfere with the host system or other containers.
- Docker images define the container specification; Docker or compatible runtimes execute containers from images.
- **Dev Containers** (`devcontainer.json`) declaratively specify a development environment, including base image, extensions, and post-creation commands, enabling reproducible setup across team members and CI/CD pipelines.
- GitHub Codespaces provisions cloud-hosted containers with VS Code or JetBrains IDEs accessible via web browsers, eliminating local environment drift.

**Use Cases:**
- Development environments that must be identical across team members.
- CI/CD pipelines requiring containerized application builds and tests.
- Cloud-based IDEs for remote or collaborative development.
- Simplified onboarding without complex local setup instructions.

## 4. Command Line Interface and Git

The **command-line interface (CLI)** is a text-based interface for executing operating system commands and invoking programs. **Git** is a distributed version control system that tracks changes to files and coordinates collaboration across team members.

**Essential CLI Commands:**
- `ls` lists directory contents; `cd` changes the working directory; `pwd` prints the current working directory path.
- `cat` outputs file contents; `head` and `tail` display the beginning and end of files, respectively.
- `grep` searches for text patterns within files; supports regular expressions for advanced pattern matching.
- `echo` prints text to standard output.

**Git Workflow:**
- `git clone <repository>` creates a local copy of a remote repository.
- `git add <file>` stages changes for commit; `git commit -m "<message>"` records staged changes with a descriptive message.
- `git push` uploads commits to the remote repository; `git pull` fetches and integrates remote changes.
- `git checkout -b <branch-name>` creates and switches to a new branch, enabling isolated feature development.

**Use Cases:**
- Rapid file navigation and inspection.
- Efficient version control and change tracking.
- Collaborative development with branching and merging.
- Integration with CI/CD pipelines for automated testing and deployment.

## 5. Bash Scripting

A **Bash script** is an executable text file containing a sequence of shell commands. Bash scripting enables automation of repetitive system administration tasks, data processing, and build workflows without requiring compilation or external tools.

**Key Mechanisms:**
- Variables store values accessible throughout the script using the `$VARIABLE_NAME` syntax.
- Control flow constructs (`if`, `for`, `while`) enable conditional execution and iteration.
- Command substitution (`$(command)`) allows the output of one command to be passed as input or assigned to variables.
- Exit codes (typically 0 for success, non-zero for failure) communicate script outcome to parent processes and enable conditional chaining with `&&` and `||`.

**Use Cases:**
- System administration: automated backups, log rotation, user provisioning.
- Build automation: compilation, testing, artifact generation.
- Data processing: batch file transformations, log analysis.
- Deployment: automated environment configuration and service restart.

**Example: Archive old logs**
```bash
#!/bin/bash
# Find all .log files modified more than 7 days ago and move to archive/
mkdir -p archive
find . -name "*.log" -mtime +7 -exec mv {} archive/ \;
echo "Old logs archived."
```

**When Not to Use:**
- Complex business logic or mathematical computation — use Python or JavaScript instead.
- Cross-platform applications requiring portability — use a higher-level language.

## 6. Testing

**Testing** is a systematic process of validating application behavior against expected outcomes. Automated tests provide regression prevention, documentation of behavior, and confidence in refactoring.

**Test Categories:**
- **Unit Tests:** Test individual functions or methods in isolation, verifying correct behavior for various inputs.
- **Integration Tests:** Test multiple components or modules working together, validating end-to-end workflows and API interactions.

**Coverage Metrics:**
- **Statement Coverage:** Percentage of code statements executed during test runs.
- **Path Coverage:** Percentage of all possible execution paths (branches) covered by tests. Path coverage is stricter than statement coverage.

**Testing Framework:** Vitest is a modern testing framework for JavaScript/TypeScript with fast execution, ES modules support, and a familiar Jest-compatible API.

**Example (Vitest):**
```javascript
import { describe, it, expect } from 'vitest';

function calculatePlanetDistance(name) {
  const planets = { Mars: 225, Venus: 108, Jupiter: 778 };
  return planets[name] || null;
}

describe('calculatePlanetDistance', () => {
  it('returns correct distance for Mars', () => expect(calculatePlanetDistance('Mars')).toBe(225));
  it('returns null for unknown planet', () => expect(calculatePlanetDistance('Pluto')).toBeNull());
});
```

**Best Practices:**
- Write tests for functions with business logic or complex behavior.
- Aim for high path coverage on critical code paths.
- Use descriptive test names that document expected behavior.

## 7. Security

Web application security addresses threats arising from untrusted user input and malicious actors. The **CIA triad** (Confidentiality, Integrity, Availability) defines core security objectives.

**Common Vulnerabilities:**

**SQL Injection:** Attacker crafts malicious SQL by concatenating user input directly into query strings, allowing unauthorized database access or modification.

**Example (vulnerable):**
```javascript
// DO NOT USE
const query = `SELECT * FROM users WHERE email = '${userInput}'`;
client.query(query);
```

**Mitigation:** Use parameterized queries (prepared statements) that separate query logic from data. The database driver enforces type safety and escaping.

```javascript
// CORRECT
const query = 'SELECT * FROM users WHERE email = $1';
client.query(query, [userInput]);
```

**Cross-Site Scripting (XSS):** Attacker injects malicious JavaScript into user-visible content, executing arbitrary code in victim browsers (stealing cookies, session tokens, etc.).

**Mitigation:** Escape user-provided strings before rendering to HTML. Replace special characters (`<`, `>`, `&`, `"`, `'`) with HTML entities.

```javascript
function escapeHTML(str) {
  return str
    .replace(/&/g, '&amp;')
    .replace(/</g, '&lt;')
    .replace(/>/g, '&gt;')
    .replace(/"/g, '&quot;')
    .replace(/'/g, '&#39;');
}
```

**General Principles:**
- Treat all external input as untrusted; validate format, length, and content against expected patterns.
- Apply principle of least privilege: grant minimum necessary permissions.
- Keep dependencies updated to patch known vulnerabilities.

## 8. Artificial Intelligence and Softmax

**Artificial Intelligence (AI)** broadly refers to computational systems designed to perform tasks typically requiring human intelligence. AI applications include natural language processing, computer vision, robotics, and autonomous decision-making.

**Machine Learning Classification:** A fundamental AI task where a trained model predicts which category an input belongs to. Given input features, the model outputs raw scores (logits) for each category. These scores must be converted to normalized probabilities for interpretation and loss computation during training.

**Softmax Function:** Converts raw model scores into a probability distribution over categories. The softmax transformation ensures output values sum to 1 and remain strictly between 0 and 1, enabling probabilistic interpretation and training with cross-entropy loss.

**Softmax Formula:**

$$S(z_i) = \frac{e^{z_i}}{\sum_{j=1}^{K} e^{z_j}}$$

Where:
- $z_i$ is the raw score (logit) for category $i$.
- $K$ is the total number of categories.
- $S(z_i)$ is the normalized probability for category $i$.

**Example (Python):**

```python
import math

def softmax(logits):
    exps = [math.exp(x) for x in logits]
    s = sum(exps)
    return [e / s for e in exps]

# Output: probabilities that sum to 1.0
print(softmax([2.0, 1.0, 0.5]))  # [0.665, 0.244, 0.090]
```

**Use Cases:**
- Multi-class classification (e.g., image categorization, text sentiment, disease diagnosis).
- Probability estimation for decision-making under uncertainty.
- Training neural networks with cross-entropy loss.

---

If you want, I can now:
- Make a tiny `index.html` to test the `fetch()` examples in the browser, or
- Extract the `server.js` into a runnable file and add a minimal `package.json` so you can run the server locally.


---

## **Review: Multiple-Choice Practice**

Below are 10 short practice questions. Pick an answer, then click **Show answers** to reveal the correct choices.

1) **Which of these statements about Express is incorrect?**

- **A.** `Express apps are written in Java`
- **B.** `Express can be used to implement an API`
- **C.** `Express is built on top of Node.js`
- **D.** `Express can deliver web pages`

2) **When a user types a URL and presses Enter, what HTTP request is sent?**

- **A.** `GET`
- **B.** `DELETE`
- **C.** `POST`
- **D.** `FETCH`

3) **How to read `temperature` from `...?latitude=45.0&longitude=-85.0&temperature=37.0` inside a route?**

- **A.** ``req['query']['temperature']``
- **B.** ``req.query['temperature']``
- **C.** ``req.query.temperature``
- **D.** All of the above

4) **Which sends a JS object back to the client inside `(req, res) => {}`?**

- **A.** `req.json(person);`
- **B.** `res.send(object.person);`
- **C.** `res.send(person.toJson());`
- **D.** `res.json(person);`

5) **What best describes `fetch()`?**

- **A.** Execute a POST request
- **B.** Send HTTP requests and receive HTTP responses
- **C.** Send an HTTP request (one-way only)
- **D.** Execute a GET request only

6) **Which is NOT a valid `fetch()` option (second argument)?**

- **A.** The default language
- **B.** A path to a resource to fetch
- **C.** The method name (`GET`, `POST`)
- **D.** Headers

7) **What does `fetch()` return?**

- **A.** `await`
- **B.** A JSON object
- **C.** `async`
- **D.** A Promise

8) **What do many public APIs require you to include in requests?**

- **A.** A promise
- **B.** An API key
- **C.** A credit card number
- **D.** A promissory note

9) **Which helper serves static files in Express?**

- **A.** `res.static()`
- **B.** `app.static()`
- **C.** `app.use(express.static('public'))`
- **D.** `express.serve('public')`

10) **Which method sets `Content-Type: application/json` for you?**

- **A.** `res.sendJSON()`
- **B.** `res.json()`
- **C.** `res.send()`
- **D.** `req.json()`
 
11) **Which Express middleware parses JSON request bodies into `req.body`?**

- **A.** `express.urlencoded()`
- **B.** `express.json()`
- **C.** `express.static()`
- **D.** `body-parser()`

12) **Code task — Express route:** Write a short Express `POST /api/planets` route that reads JSON from the request body and responds with 201 and the saved planet object (assume in-memory push to an array `planets`).

13) **Code task — fetch POST:** Write a `fetch()` snippet that POSTs JSON `{ name: 'Mars' }` to `/api/planets`, checks `response.ok`, and logs the returned JSON.

14) **Code task — Softmax (JS):** Write a small JavaScript function `softmax(arr)` that returns the softmax probabilities for an array of numbers.

15) **Code task — Parameterized SQL (pg):** Show a parameterized `pg` query to select a user by email (using `$1`).

16) **Code task — Bash one-liner:** Give a bash one-liner that moves `.log` files older than 7 days from the current directory into `archive/`.

17) **Multiple choice — Where should `app.use(express.json())` be placed?**

- **A.** After routes
- **B.** Before routes
- **C.** Only inside route handlers
- **D.** It does not matter

18) **Multiple choice — Which HTTP status code commonly means "Created" after a successful POST?**

- **A.** 200
- **B.** 201
- **C.** 400
- **D.** 500

19) **Code task — Vitest:** Write a tiny Vitest test that checks a function `add(a,b)` returns the sum.

20) **Code task — XSS defense:** Give a simple JavaScript function `escapeHtml(s)` that replaces `<`, `>`, and `&` with their HTML entities.

<details>
<summary><strong>Show answers</strong></summary>

**Answers**

1) **A** — Express apps are written in JavaScript (not Java).

2) **A** — `GET` is sent when you enter a URL and press Enter.

3) **D** — All three forms work: `req['query']['temperature']`, `req.query['temperature']`, `req.query.temperature`.

4) **D** — `res.json(person)` sends the object as JSON with correct headers.

5) **B** — `fetch()` sends HTTP requests and receives responses; it's not limited to POST or GET.

6) **A** — "The default language" is not a standard `fetch()` option.

7) **D** — `fetch()` returns a Promise. Use `await` or `.then()` to get the Response object.

8) **B** — Many APIs require an API key (or other authentication) in headers or query params.

9) **C** — `app.use(express.static('public'))` is the usual helper to serve static files.

10) **B** — `res.json()` sets `Content-Type: application/json` and sends the JSON.

10) **B** — `res.json()` sets `Content-Type: application/json` and sends the JSON.

11) **B** — `express.json()` parses JSON payloads and fills `req.body`.

12) **Sample solution (Express POST route)**

```javascript
const planets = [];
app.post('/api/planets', (req, res) => {
  const planet = req.body; // assume JSON like { name: 'Mars' }
  planet.id = planets.length + 1;
  planets.push(planet);
  res.status(201).json(planet);
});
```

13) **Sample solution (fetch POST)**

```javascript
fetch('/api/planets', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ name: 'Mars' })
})
  .then(res => {
    if (!res.ok) throw new Error('Server error ' + res.status);
    return res.json();
  })
  .then(data => console.log('Created', data))
  .catch(err => console.error(err));
```

14) **Sample solution (softmax in JS)**

```javascript
function softmax(arr) {
  const exps = arr.map(x => Math.exp(x));
  const sum = exps.reduce((a, b) => a + b, 0);
  return exps.map(e => e / sum);
}
```

15) **Sample solution (parameterized SQL with `pg`)**

```javascript
const query = 'SELECT * FROM users WHERE email = $1';
const values = [userEmail];
client.query(query, values, (err, res) => { /* handle result */ });
```

16) **Sample solution (bash one-liner)**

```bash
mkdir -p archive && find . -maxdepth 1 -name '*.log' -mtime +7 -exec mv {} archive/ \;
```

17) **B** — `app.use(express.json())` should be placed before route handlers so `req.body` is available.

18) **B** — `201 Created` indicates a new resource was successfully created after a POST.

19) **Sample solution (Vitest test for `add`)**

```javascript
import { it, expect } from 'vitest';
import { add } from './math';

it('adds two numbers', () => {
  expect(add(2, 3)).toBe(5);
});
```

20) **Sample solution (escapeHtml)**

```javascript
function escapeHtml(s) {
  return s.replace(/&/g, '&amp;').replace(/</g, '&lt;').replace(/>/g, '&gt;');
}
```

</details>

Use this version for quick study — I can also make a small interactive HTML quiz that tracks your answers if you want.
