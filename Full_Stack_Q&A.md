# Full Stack Interview Questions and Answers

This file serves as a **hub** linking to topic-specific Q&A files. Each technology has its own dedicated file for clarity and easy navigation.

## Table of Contents

1. [📚 Topic Index](#-topic-index) — Links to all topic-specific files
2. [General Full Stack](#general-full-stack) — MVC, REST, security, build tools, performance
3. [CORS](#cors) — Same-origin policy, preflight requests, origin spoofing, bearer tokens

---

## 📚 Topic Index

### Language & Framework Q&A

- **[Java Q&A](Java%20questions.md)** — Core Java concepts (Java 8/11/17/21 features, memory management, exceptions, algorithms & data structures)
- **[Angular Q&A](Angular%20questions.md)** — Angular framework (components, signals, directives, routing, forms, DI, RxJS, Docker)
- **[Node.js Q&A](Node%20questions.md)** — Node.js backend development (event loop, REST APIs, Express, async programming, testing)
- **[Spring & Hibernate Q&A](Spring%20questions.md)** — Spring Framework, Spring Boot, Spring Security, Spring Data, Spring WebFlux, Hibernate/JPA
- **[Java Testing Q&A](Java%20Testing.md)** — Unit testing in Spring Boot (JUnit 5, Mockito, AssertJ)

### Database Q&A

- **[SQL, NoSQL & MongoDB Q&A](SQL%20questions.md)** — SQL (DDL, DML, JOINs, window functions, CTEs), NoSQL concepts, MongoDB (CRUD, aggregation, indexing)

### Architecture & Infrastructure

- **[Microservices Architecture Q&A](Microservices%20Architecture.md)** — DDD, service boundaries, distributed systems, sagas, circuit breakers, distributed tracing
- **[Kafka Q&A](Kafka/Kafka.md)** — Apache Kafka (topics, partitions, producers, consumers, brokers, ZooKeeper)
- **[Docker Q&A](Docker/Docker.md)** — Docker containers, images, volumes, networks
- **[Kubernetes Q&A](K8S/Kubernetes.md)** — Kubernetes orchestration

### Best Practices & Exercises

- **[Coding Best Practices & Design Patterns](Coding%20Best%20Practices.md)** — SOLID principles, design patterns (Singleton, Factory, Observer, Strategy, Builder, Decorator), clean code, secure coding
- **[Coding Exercises](Coding%20Exercises.md)** — Interview coding exercises (Map/HashMap cheat sheets, Two Sum, array methods)

---

## General Full Stack

### Explain the MVC architecture and its components

- MVC (Model-View-Controller) is a design pattern that separates an application into three main components:
  - **Model**: Represents the application's data and business logic.
  - **View**: Represents the presentation layer and displays data to the user.
  - **Controller**: Handles user input and updates the model and view accordingly.

### What are RESTful web services and how do they work?

- RESTful web services are web services that adhere to the principles of Representational State Transfer (REST). They use standard HTTP methods (GET, POST, PUT, DELETE) to perform CRUD operations on resources, which are identified by URIs.

### How do you ensure security in a full stack application?

- Security can be ensured through various measures, such as:
  - **Authentication and Authorization**: Ensuring that users are who they claim to be and have the necessary permissions.
  - **Data Encryption**: Encrypting sensitive data both in transit and at rest.
  - **Input Validation**: Validating user inputs to prevent attacks like SQL injection and cross-site scripting (XSS).
  - **Secure Coding Practices**: Following best practices to write secure code.

### What is the role of a build tool in a full stack project?

- Build tools like Maven, Gradle, and npm automate the process of compiling code, managing dependencies, running tests, and packaging the application for deployment. They help streamline the development workflow and ensure consistency.

### How do you optimize the performance of a full stack application?

- Performance optimization can be achieved through various techniques, such as:
  - **Caching**: Storing frequently accessed data in memory to reduce database load.
  - **Asynchronous Processing**: Performing time-consuming tasks asynchronously to improve responsiveness.
  - **Database Optimization**: Using indexing, query optimization, and efficient data modeling.
  - **Code Optimization**: Writing efficient code and minimizing resource usage.
  - **Load Balancing**: Distributing traffic across multiple servers to ensure scalability and reliability.

---

## CORS

### What is CORS and how is it used

CORS stands for **Cross-Origin Resource Sharing**. It's a **security feature implemented in web browsers** that controls how web pages can make requests to a different domain than the one that served the web page.

### 🔐 Why CORS Exists

By default, browsers follow the **Same-Origin Policy** — which means a website can only make requests to the same origin (same protocol + domain + port). This helps prevent malicious scripts from accessing sensitive data on other websites.

CORS is a **relaxation** of that policy, allowing trusted cross-origin requests — but only under specific conditions.

### ⚙️ How CORS Works

When a browser tries to make a cross-origin request (e.g., from `https://example.com` to `https://api.anotherdomain.com`), it does the following:

#### Simple Requests

If the request is simple (like a GET or POST with basic headers), the browser includes an `Origin` header in the request:

```http
Origin: https://example.com
```

Then, the server can respond with:

```http
Access-Control-Allow-Origin: https://example.com
```

If this header is present and matches the origin, the browser allows the response to be read.

#### Preflight Requests

For non-simple requests (e.g., using custom headers or methods like PUT/DELETE), the browser sends a **preflight OPTIONS** request:

```http
OPTIONS /data HTTP/1.1
Origin: https://example.com
Access-Control-Request-Method: PUT
```

The server must respond with:

```http
Access-Control-Allow-Origin: https://example.com
Access-Control-Allow-Methods: PUT
```

Only then will the browser send the actual request.

#### 🧠 Summary

- **CORS = Controlled way to relax same-origin policy**
- Used to allow or restrict resources on a web server to be requested from other domains
- Requires cooperation from the server (client can't override it)
- Protects users from malicious cross-site attacks

---

### Is it possible to falsify the origin?

Great question — and the short answer is:

❌ No, browsers **do not allow you to falsify the `Origin` header** in normal client-side JavaScript.

But there's more nuance depending on context. Let's break it down:

#### 🔒 1. **From the Browser (Client-Side JS)**

When your code runs in the browser (e.g., using `fetch()` or `XMLHttpRequest`), the `Origin` header is:

- **automatically set by the browser**
- **not editable by JavaScript**

This is intentional — it's a security feature. Allowing developers to spoof the `Origin` would break the entire CORS model.

#### 🛠️ 2. **From Tools Like cURL or Postman**

Yes, you **can manually set** the `Origin` header using tools like:

```bash
curl -H "Origin: https://malicious-site.com" https://target-api.com/data
```

But here’s the catch:

- **CORS is a browser-enforced policy.**
- If you're using `curl`, Postman, or server-side code, **CORS doesn't apply** — the server might still log or check the header, but there’s no browser to block the response.

So spoofing the `Origin` this way doesn’t break anything unless the server **relies on that header for auth/security**, which would be a mistake.

This means:

   The browser decides: "Can my frontend JavaScript access the response from this cross-origin request?"

   If the CORS headers from the server are wrong, the browser will block access to the response — even if the server sends back the data.

If someone tries to:

   Build a malicious site like evil.com

   Put JavaScript on it that tries to call your API at https://api.mybank.com/account-info

The browser running on your computer will block that request unless CORS is allowed.

That’s how CORS protects users.

---

#### 🧠 Key Insight

- CORS protects **users**, not servers.
- It stops **malicious websites** from accessing private APIs/data through the user's browser.
- It does **not** protect against direct requests made from tools or backend services.

---

#### 🔐 Secure Practice for APIs

If you're building a secure API:

- Don't rely on `Origin` alone for access control.
- Use **authentication tokens**, **API keys**, or **session-based validation**.
- CORS is like a **firewall** for browsers, not a lock on your backend.

---

### What if i spoof my origin and my server checks my bearer token to give me private data?

Here’s the core idea:

⚠️ If you spoof the `Origin` **but you're already sending a valid Bearer token**, then **the server doesn’t care about the Origin** — it just sees a legit authenticated request.

So technically:
> ✅ If you have a valid token, you **can spoof your Origin** (e.g., using curl or Postman), and the server will still give you the private data — because **auth beats origin**.

So… is this a problem?

It depends on **what you're trying to protect** and **who you're protecting it from**.

#### 🔐 Scenario 1: Protecting API from Unauthorized Access

If you're using CORS + Bearer tokens, and your server checks the **token** for identity/authorization, you're in good shape. Spoofing Origin doesn’t matter.

> **CORS isn’t for keeping attackers out of your API.**
It’s for **preventing a malicious website** from making your browser send authenticated requests *on your behalf* (e.g., with cookies/session).

---

#### 🧨 Scenario 2: You rely on Origin *and* Token

Let’s say your server says:

> “Only allow requests from `https://myfrontend.com` *and* a valid Bearer token.”

If someone manually sends a request with:

```http
Origin: https://myfrontend.com
Authorization: Bearer <stolen-valid-token>
```

Then yeah — **they get in**.

So again:

- **Origin can be spoofed**
- **Bearer token is the real gatekeeper**
- If someone gets a valid token, they can call your API from anywhere (including Postman, Python, cURL, etc.)

---

#### 🛡️ So How Do You Protect Your API?

If you want to be extra secure:

1. **Use short-lived tokens (JWTs or access tokens)**

   - Reduce the window of abuse if a token leaks.

2. **Use HTTPS (always)**

   - Prevent token sniffing.

3. **Check additional headers or fingerprinting (optional)**

   - Some advanced APIs verify IP ranges, user-agent, or even use device/browser fingerprinting (but can get messy).

4. **Use Refresh Tokens + Rotation**

   - Make it harder for an attacker to hold onto valid tokens for long.

---

### TL;DR

- Spoofing `Origin` works — but only matters if your server actually uses it for security.
- The **Bearer token is the real security control** — so protect it.
- CORS protects **users**, not servers.
