# 🚀 CS 246 Final Exam Study Guide

## 1. 🌐 Writing Your Own API with Express

The core of a backend application. Focus on handling incoming requests (`req`) and sending outgoing responses (`res`).

| Concept | Express Mechanism / Location | Key Takeaway / Use Case |
| :--- | :--- | :--- |
| **Parsing Query Strings** | Available in the **`req.query`** object. | Used for passing non-essential parameters, filtering, or IDs in the URL (e.g., `/planets?name=Mars`). |
| **Serving Static Files** | Use the **`app.use(express.static('public'))`** middleware. | Automatically serves client-side assets (HTML, CSS, JS, images) from a specified folder (e.g., `public`). |
| **Sending Responses** | **`res.send()`** vs. **`res.json()`** | For REST APIs, **`res.json(data)`** is preferred because it explicitly sets the `Content-Type` header to `application/json`. |
| **Server Debugging** | **`console.log()`** | Output appears **only in the server's terminal** (where Node/Express is running), *never* in the user's web browser. |

## 2. ⚡ Client-Side Data Retrieval with `fetch()`

The modern standard for making HTTP requests from the browser.

### The Two-Step Promise Process

1.  `fetch(url)` returns a **Promise** that resolves to a **Response object**. (This step resolves when *headers* are received).
2.  You must call a method like **`response.json()`** or `response.text()` to read the body content, which returns a **second Promise**.

### ⚠️ Error Handling Caveat

* The `fetch` call **only rejects** on **network errors** (e.g., no internet, connection refused).
* It **does NOT reject** on HTTP error status codes (e.g., 404 Not Found, 500 Server Error).
* **The Fix:** You must manually check the **`response.ok`** property (a boolean that is `true` for 200-299 status codes) within the first `.then()` block.

---

## 3. 📦 Containers, Dev Containers, and Codespaces

* **Container (Docker):** A **general-purpose, isolated runtime environment** that bundles an application, its code, dependencies, and OS libraries. Its main purpose is to solve the "it works on my machine" problem by ensuring consistency across environments.
* **Dev Container:** A specific type of Docker container customized for development, including tools like VS Code extensions and specific language runtimes.
* **Codespace (GitHub):** A **running instance of a Dev Container** hosted by GitHub (a cloud-based development environment).


### Codespace and Git Repository Connection

* You can launch a Codespace from an **existing GitHub repository** to start developing immediately.
* A Codespace created from a template initially has **no linked repo**. You must explicitly **Publish** it to create a new, linked repository on GitHub.

---

## 4. 💻 Command Line Interfaces (CLI) & Git

### Basic Linux File Commands

| Command | Purpose |
| :--- | :--- |
| `pwd` | **P**rint **W**orking **D**irectory (show current path). |
| `ls` | **L**ist directory contents. |
| `cd <dir>` | **C**hange **D**irectory. |
| `mkdir <dir>` | **M**ake a new **dir**ectory. |
| `touch <file>` | Create a new, empty file or update the timestamp. |
| `cat <file>` | Display the **entire content** of a file. |
| `head <file>` | Display the **beginning** (default 10 lines) of a file. |
| `tail <file>` | Display the **end** (default 10 lines) of a file. |
| `grep <pattern> <file>` | **Search** for lines matching a pattern within a file. |

### Essential Git Commands

| Command | Purpose |
| :--- | :--- |
| `git init` | Initialize a **new local repository**. |
| `git clone <url>` | Create a local copy of a **remote repository**. |
| `git add <file>` | **Stage** changes for the next commit. |
| `git commit -m "msg"` | **Record** staged changes permanently in the local history. |
| `git push` | **Upload** local commits to the remote repository (e.g., GitHub). |
| `git pull` | **Fetch and merge** changes from the remote repository. |

---

## 5. ⚙️ Bash Automation (Shell Scripting)

The practice of writing a series of shell statements in a file (`.sh`) to automate repetitive tasks.

* **Basic Structure:** Scripts are a series of standard CLI commands.
* **Input/Output:** Use `echo` for output and `read` for accepting user input.
* **Conditionals:** Use `if/then/fi` blocks.
    * Relational operators use special syntax inside conditional brackets (`[ ]`), e.g., **`-lt`** (less than), **`-gt`** (greater than), **`-eq`** (equal to).
* **Loops:** Use `for` and `while` loops for repetitive actions.

---

## 6. ✅ Software Testing Concepts

### Hierarchy of Testing

* **Unit Testing:** Tests the **smallest individual functions/modules in isolation** to guarantee they produce the intended results. (Focus of frameworks like **Vitest**).
* **Integration Testing:** Tests that different components or modules work together correctly when assembled.
* **Usability Testing:** Tests the user-friendliness and intuitiveness of the product.

### Coverage Types

* **Statement Coverage:** Ensures that every **executable statement** in the code is executed at least once by the test suite.
* **Path Coverage:** Ensures that every possible **sequence of control flow** (or path) through the code is executed. Path coverage is more rigorous and implies statement coverage.


---

## 7. 🔒 Security Best Practices

Based on the **CIA Triad** model, which defines the core goals of information security.


| Principle | Goal | Defense Strategy |
| :--- | :--- | :--- |
| **Confidentiality** | Keep data private and unauthorized access blocked. | Data encryption (at rest/in transit), strong hashing for passwords. |
| **Integrity** | Ensure data is accurate and untampered. | Input sanitization, validation. |
| **Availability** | Ensure systems stay online and functional for users. | Proper server configuration, denial-of-service (DoS) defenses. |

### Major Web Vulnerabilities

| Attack | Description | Defense / Remedy |
| :--- | :--- | :--- |
| **SQL Injection (SQLi)** | An attacker inserts malicious SQL code into an input field, allowing them to access, modify, or delete database data. | **Always use SQL parameters** (e.g., `$1, $2` in the `pg` module). The library will automatically sanitize the input. |
| **Cross-Site Scripting (XSS)** | An attacker injects malicious client-side scripts (e.g., JavaScript) into a webpage viewed by other users. | **Never trust user input.** All user input must be sanitized/escaped before being displayed in the browser. |

---

## 8. 🧠 Artificial Intelligence (AI) Overview

### Categories of AI

* **Classical / Symbolic AI:** Relies on **explicitly programmed rules and logic** (e.g., if/then statements, expert systems).
* **Machine Learning (ML):** The machine **discovers its own rules and patterns** by processing large amounts of data (e.g., Neural Networks, Decision Trees).
* **Large Language Models (LLMs):** A specific, modern ML application (using the **Transformer** architecture and **Attention** mechanism).
    * **Goal:** To predict the **next token** in a sequence.
    * **Classification:** LLMs are considered **Artificial Narrow Intelligence (ANI)**, but often exhibit unexpected **Emergent Behavior** (like complex reasoning) due to the scale of their training.

### The Softmax Function

* **Purpose:** Converts a vector of raw prediction scores (logits) into a vector of **probabilities**.
* **Properties:** The output values are between 0 and 1, and their sum equals 1.
* **Formula:** $$S(z_i) = \frac{e^{z_i}}{\sum_{j=1}^{K} e^{z_j}}$$


**Example (Logits = [2.0, 1.0, 0.5]):**
1.  **Exponentials ($e^z$):** $[7.389, 2.718, 1.648]$
2.  **Sum of Exponentials:** $11.755$
3.  **Probabilities:**
    * $P_1 = 7.389 / 11.755 \approx \mathbf{0.628}$
    * $P_2 = 2.718 / 11.755 \approx \mathbf{0.231}$
    * $P_3 = 1.648 / 11.755 \approx \mathbf{0.140}$
    * (Sum: $0.628 + 0.231 + 0.140 = 0.999 \approx 1.0$)