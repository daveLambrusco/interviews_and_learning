# Full Stack Interview Questions and Answers

- [Full Stack Interview Questions and Answers](#full-stack-interview-questions-and-answers)
  - [Java](#java)
    - [Differences Between Java 11 and Java 21](#differences-between-java-11-and-java-21)
      - [Java 11 Features](#java-11-features)
      - [Java 21 Features](#java-21-features)
  - [Angular](#angular)
  - [Hibernate](#hibernate)
  - [Spring](#spring)
    - [1. What is Spring Framework?](#1-what-is-spring-framework)
    - [2. What are the benefits of using Spring Framework?](#2-what-are-the-benefits-of-using-spring-framework)
    - [3. What is Dependency Injection in Spring?](#3-what-is-dependency-injection-in-spring)
    - [Inversion of Control (IoC)](#inversion-of-control-ioc)
      - [Key Concepts of IoC](#key-concepts-of-ioc)
      - [Benefits of IoC](#benefits-of-ioc)
    - [Dependency Injection (DI)](#dependency-injection-di)
      - [Types of Dependency Injection](#types-of-dependency-injection)
      - [Benefits of DI](#benefits-of-di)
    - [Spring Boot](#spring-boot)
  - [SQL](#sql)
    - [1. What is SQL?](#1-what-is-sql)
    - [2. What are the different types of SQL commands?](#2-what-are-the-different-types-of-sql-commands)
    - [3. What is a JOIN in SQL?](#3-what-is-a-join-in-sql)
  - [NoSQL](#nosql)
    - [1. What is NoSQL?](#1-what-is-nosql)
    - [2. What are the types of NoSQL databases?](#2-what-are-the-types-of-nosql-databases)
    - [3. What is the difference between SQL and NoSQL databases?](#3-what-is-the-difference-between-sql-and-nosql-databases)
  - [General Full Stack](#general-full-stack)
  - [CORS](#cors)
    - [What is CORS and how is it used](#what-is-cors-and-how-is-it-used)
    - [🔐 Why CORS Exists](#-why-cors-exists)
    - [⚙️ How CORS Works](#️-how-cors-works)
      - [1. **Simple Requests**](#1-simple-requests)
      - [2. **Preflight Requests**](#2-preflight-requests)
      - [🧠 Summary](#-summary)
    - [Is it possible to falsify the origin?](#is-it-possible-to-falsify-the-origin)
      - [🔒 1. **From the Browser (Client-Side JS)**](#-1-from-the-browser-client-side-js)
      - [🛠️ 2. **From Tools Like cURL or Postman**](#️-2-from-tools-like-curl-or-postman)
      - [🧠 Key Insight](#-key-insight)
      - [🔐 Secure Practice for APIs](#-secure-practice-for-apis)
    - [What if i spoof my origin and my server checks my bearer token to give me private data?](#what-if-i-spoof-my-origin-and-my-server-checks-my-bearer-token-to-give-me-private-data)
      - [🔐 Scenario 1: Protecting API from Unauthorized Access](#-scenario-1-protecting-api-from-unauthorized-access)
      - [🧨 Scenario 2: You rely on Origin *and* Token](#-scenario-2-you-rely-on-origin-and-token)
      - [🛡️ So How Do You Protect Your API?](#️-so-how-do-you-protect-your-api)
    - [TL;DR](#tldr)


## Java

1. **What are the main features of Java 8?**
   - **Lambda Expressions**: Introduce functional programming by allowing you to pass functionality as an argument to a method.
   - **Stream API**: Provides a new abstraction to process sequences of elements, making it easier to perform operations like filtering, mapping, and reducing.
   - **Default Methods**: Allow you to add new methods to interfaces without breaking existing implementations.
   - **Optional Class**: Helps in handling null values more gracefully.
   - **New Date and Time API**: Provides a comprehensive and flexible date-time handling mechanism.

2. **Explain the concept of Java Memory Management.**
   - Java memory management involves the allocation and deallocation of memory to objects. The Java Virtual Machine (JVM) manages memory through a process called garbage collection, which automatically removes objects that are no longer in use to free up memory.
,
3. **What is the difference between `==` and `equals()` in Java?**
   - `==` checks for reference equality, meaning it checks if two references point to the same object in memory.
   - `equals()` checks for value equality, meaning it checks if two objects are logically equivalent based on their state.

4. **How does the Java garbage collector work?**
   - The garbage collector in Java automatically identifies and disposes of objects that are no longer reachable in the program. It uses algorithms like Mark-and-Sweep, Generational Garbage Collection, and others to manage memory efficiently.

5. **What are the different types of exceptions in Java?**
   - **Checked Exceptions**: Exceptions that are checked at compile-time (e.g., IOException, SQLException).
   - **Unchecked Exceptions**: Exceptions that are checked at runtime (e.g., NullPointerException, ArrayIndexOutOfBoundsException).
   - **Errors**: Serious issues that a reasonable application should not try to catch (e.g., OutOfMemoryError, StackOverflowError).

### Differences Between Java 11 and Java 21

#### Java 11 Features

Released in September 2018, Java 11 introduced several important updates and enhancements:

1. **HTTP Client API**: Provides a modern, feature-rich HTTP client for synchronous and asynchronous communication.
2. **Local-Variable Syntax for Lambda Parameters**: Allows the use of `var` in lambda expressions.
3. **New String Methods**: Adds methods like `isBlank()`, `lines()`, `strip()`, `stripLeading()`, `stripTrailing()`, and `repeat()`.
4. **New File Methods**: Introduces `readString()` and `writeString()` methods in the `Files` class.
5. **Collection to an Array**: Adds a new default `toArray` method in the `java.util.Collection` interface.
6. **Not Predicate Method**: Adds a static `not` method to the `Predicate` interface.

#### Java 21 Features

Released in September 2023, Java 21 brought several new features and improvements:

1. **Record Patterns (JEP 440)**: Enhances pattern matching by allowing the deconstruction of record class instances.
2. **Pattern Matching for switch (JEP 441)**: Improves the expressiveness of switch statements by allowing patterns in case labels and handling `NullPointerException`.
3. **String Templates (JEP 430)**: Introduces string templates for easier and safer string manipulation.
4. **Sequenced Collections (JEP 431)**: Adds new collection types that maintain the order of elements.
5. **Virtual Threads (JEP 425)**: Introduces virtual threads to simplify concurrency and improve performance.
6. **Library Improvements**: Various enhancements to existing libraries and APIs.

## Angular

1. **What is Angular and how does it differ from AngularJS?**
   - Angular is a platform and framework for building single-page client applications using HTML and TypeScript. AngularJS is the original version of Angular, which is based on JavaScript. Angular (versions 2 and above) is a complete rewrite of AngularJS and offers improved performance, a more modular architecture, and better tooling.

2. **Explain the concept of Angular modules.**
   - Angular modules (NgModules) are containers for a cohesive block of code dedicated to an application domain, a workflow, or a closely related set of capabilities. They help organize an application into cohesive blocks of functionality.

3. **What are Angular services and how are they used?**
   - Angular services are singleton objects that encapsulate reusable logic and data that can be shared across components. They are typically used for tasks like fetching data from a server, logging, and other business logic.

4. **How does Angular handle data binding?**
   - Angular supports two-way data binding, which means that changes in the model automatically update the view and vice versa. This is achieved using directives like `ngModel` and property/event binding syntax.

5. **What is the purpose of Angular CLI?**
   - Angular CLI (Command Line Interface) is a powerful tool that helps automate the development workflow. It provides commands to create, build, serve, and test Angular applications, making it easier to manage projects.

## Hibernate

1. **What is Hibernate and why is it used?**
   - Hibernate is an Object-Relational Mapping (ORM) framework for Java. It simplifies database interactions by mapping Java objects to database tables, allowing developers to work with data in an object-oriented way.

2. **Explain the concept of lazy loading in Hibernate.**
   - Lazy loading is a design pattern used in Hibernate to defer the initialization of an object until it is needed. This helps improve performance by avoiding unnecessary database queries.

3. **What are the different types of Hibernate mappings?**
   - **One-to-One Mapping**: Maps a single entity to another single entity.
   - **One-to-Many Mapping**: Maps a single entity to a collection of entities.
   - **Many-to-One Mapping**: Maps multiple entities to a single entity.
   - **Many-to-Many Mapping**: Maps a collection of entities to another collection of entities.

4. **How does Hibernate caching work?**
   - Hibernate provides two levels of caching:
     - **First-Level Cache**: Associated with the session object and is enabled by default.
     - **Second-Level Cache**: Associated with the session factory and can be configured to cache data across sessions.

5. **What is the difference between `save()` and `persist()` methods in Hibernate?**
   - `save()`: Generates an identifier and inserts the record immediately.
   - `persist()`: Does not generate an identifier immediately and the record is inserted when the transaction is committed.

## Spring

### 1. What is Spring Framework?

Spring is an open-source framework for building enterprise Java applications. It provides comprehensive infrastructure support for developing Java applications. Spring promotes good programming practices by enabling a POJO-based (Plain Old Java Object) programming model.

### 2. What are the benefits of using Spring Framework?

The benefits of using Spring Framework include:

- **Lightweight:** Spring is lightweight in terms of size and transparency.
- **Inversion of Control (IoC):** Promotes loose coupling through dependency injection.
- **Aspect-Oriented Programming (AOP):** Separates business logic from system services.
- **Transaction Management:** Provides a consistent transaction management interface.
- **MVC Framework:** Offers a well-designed web MVC framework.

### 3. What is Dependency Injection in Spring?

Dependency Injection (DI) is a design pattern used to implement IoC, allowing the creation of dependent objects outside of a class and providing those objects to a class in various ways. It helps in making the code loosely coupled.

### Inversion of Control (IoC)

**Inversion of Control (IoC)** is a design principle in which the control of object creation and management is transferred from the application code to a container or framework. In the context of Spring, IoC refers to the Spring container managing the lifecycle and dependencies of the beans (objects).

#### Key Concepts of IoC

- **Bean:** An object managed by the Spring container.
- **Container:** The core of the Spring Framework that creates, configures, and manages beans.
- **Configuration:** Metadata provided to the container to define how beans should be created and managed (usually through XML, annotations, or Java configuration).

#### Benefits of IoC

- **Loose Coupling:** Promotes loose coupling between components, making the code more modular and easier to maintain.
- **Testability:** Enhances testability by allowing dependencies to be easily injected and mocked.
- **Flexibility:** Provides flexibility in configuring and managing beans.

### Dependency Injection (DI)

**Dependency Injection (DI)** is a design pattern used to implement IoC. It allows the creation of dependent objects outside of a class and provides those objects to a class in various ways. DI helps in making the code loosely coupled and easier to manage.

#### Types of Dependency Injection

1. **Constructor Injection:** Dependencies are provided through a class constructor.
2. **Setter Injection:** Dependencies are provided through setter methods.
3. **Field Injection:** Dependencies are injected directly into fields (using annotations).

Example of Constructor Injection

```java
public class MyService {
    private final MyRepository myRepository;

    @Autowired
    public MyService(MyRepository myRepository) {
        this.myRepository = myRepository;
    }

    // Other methods
}
```

#### Benefits of DI

- **Decoupling**: Reduces the dependency between classes, making the code more modular.
- **Ease of Testing**: Simplifies unit testing by allowing dependencies to be easily injected and mocked.
- **Configuration Management**: Centralizes configuration management, making it easier to change dependencies without modifying the code

### Spring Boot

1. **What is Spring Boot and how does it simplify Spring development?**
   - Spring Boot is an extension of the Spring framework that simplifies the setup and development of new Spring applications. It provides default configurations and a set of starter projects to get up and running quickly.

2. **What are the advantages of using Spring Boot?**
   - **Standalone Applications:** Easily create standalone applications with embedded servers.
   - **Auto-Configuration:** Automatically configures Spring applications based on the dependencies present.
   - **Production-Ready:** Provides production-ready features like metrics, health checks, and externalized configuration.
   - **Minimal Configuration:** Reduces the need for extensive XML configuration.

3. **Explain the concept of Spring Boot starters.**
   - Spring Boot starters are a set of convenient dependency descriptors that you can include in your application. They provide a curated set of dependencies for specific functionalities, such as web development, data access, and security.

4. **How does Spring Boot handle dependency management?**
   - Spring Boot uses Maven or Gradle for dependency management. It provides a parent POM that includes managed versions of common dependencies, ensuring compatibility and reducing the need for manual version management.

5. **What is the purpose of Spring Boot Actuator?**
   - Spring Boot Actuator provides production-ready features to help monitor and manage applications. It includes endpoints for health checks, metrics, environment information, and more.

6. **How do you configure a Spring Boot application?**
   - Spring Boot applications can be configured using properties files (`application.properties` or `application.yml`), environment variables, and command-line arguments. Spring Boot also supports externalized configuration to allow different configurations for different environments.

## SQL

### 1. What is SQL?

SQL (Structured Query Language) is a standard language for managing and manipulating relational databases. It is used to perform tasks such as querying data, updating records, and managing database structures.

### 2. What are the different types of SQL commands?

The different types of SQL commands include:

- **DDL (Data Definition Language):** Commands like `CREATE`, `ALTER`, `DROP`.
- **DML (Data Manipulation Language):** Commands like `SELECT`, `INSERT`, `UPDATE`, `DELETE`.
- **DCL (Data Control Language):** Commands like `GRANT`, `REVOKE`.
- **TCL (Transaction Control Language):** Commands like `COMMIT`, `ROLLBACK`.

### 3. What is a JOIN in SQL?

A JOIN is a SQL operation used to combine rows from two or more tables based on a related column between them. Types of JOINs include INNER JOIN, LEFT JOIN, RIGHT JOIN, and FULL JOIN.

## NoSQL

### 1. What is NoSQL?

NoSQL (Not Only SQL) databases provide a mechanism for storage and retrieval of data that is modeled in means other than the tabular relations used in relational databases. They are designed to handle large volumes of unstructured or semi-structured data.

### 2. What are the types of NoSQL databases?

The types of NoSQL databases include:
- **Document-Oriented:** Stores data as documents (e.g., MongoDB).
- **Key-Value:** Stores data as key-value pairs (e.g., Redis).
- **Column-Family:** Stores data in columns rather than rows (e.g., Cassandra).
- **Graph:** Stores data in graph structures with nodes and edges (e.g., Neo4j).

### 3. What is the difference between SQL and NoSQL databases?

The main differences between SQL and NoSQL databases are:

- **Data Model:** SQL databases use a structured, tabular data model, while NoSQL databases use various data models (document, key-value, column-family, graph).
- **Schema:** SQL databases have a fixed schema, while NoSQL databases have a flexible schema.
- **Scalability:** SQL databases are vertically scalable, while NoSQL databases are horizontally scalable.
- **Use Cases:** SQL databases are suited for complex queries and transactions, while NoSQL databases are suited for large-scale, distributed data storage.

## General Full Stack

1. **Explain the MVC architecture and its components.**
   - MVC (Model-View-Controller) is a design pattern that separates an application into three main components:
     - **Model**: Represents the application's data and business logic.
     - **View**: Represents the presentation layer and displays data to the user.
     - **Controller**: Handles user input and updates the model and view accordingly.

2. **What are RESTful web services and how do they work?**
   - RESTful web services are web services that adhere to the principles of Representational State Transfer (REST). They use standard HTTP methods (GET, POST, PUT, DELETE) to perform CRUD operations on resources, which are identified by URIs.

3. **How do you ensure security in a full stack application?**
   - Security can be ensured through various measures, such as:
     - **Authentication and Authorization**: Ensuring that users are who they claim to be and have the necessary permissions.
     - **Data Encryption**: Encrypting sensitive data both in transit and at rest.
     - **Input Validation**: Validating user inputs to prevent attacks like SQL injection and cross-site scripting (XSS).
     - **Secure Coding Practices**: Following best practices to write secure code.

4. **What is the role of a build tool in a full stack project?**
   - Build tools like Maven, Gradle, and npm automate the process of compiling code, managing dependencies, running tests, and packaging the application for deployment. They help streamline the development workflow and ensure consistency.

5. **How do you optimize the performance of a full stack application?**
   - Performance optimization can be achieved through various techniques, such as:
     - **Caching**: Storing frequently accessed data in memory to reduce database load.
     - **Asynchronous Processing**: Performing time-consuming tasks asynchronously to improve responsiveness.
     - **Database Optimization**: Using indexing, query optimization, and efficient data modeling.
     - **Code Optimization**: Writing efficient code and minimizing resource usage.
     - **Load Balancing**: Distributing traffic across multiple servers to ensure scalability and reliability.

## CORS

### What is CORS and how is it used

CORS stands for **Cross-Origin Resource Sharing**. It's a **security feature implemented in web browsers** that controls how web pages can make requests to a different domain than the one that served the web page.


### 🔐 Why CORS Exists
By default, browsers follow the **Same-Origin Policy** — which means a website can only make requests to the same origin (same protocol + domain + port). This helps prevent malicious scripts from accessing sensitive data on other websites.

CORS is a **relaxation** of that policy, allowing trusted cross-origin requests — but only under specific conditions.

### ⚙️ How CORS Works

When a browser tries to make a cross-origin request (e.g., from `https://example.com` to `https://api.anotherdomain.com`), it does the following:

#### 1. **Simple Requests**

If the request is simple (like a GET or POST with basic headers), the browser includes an `Origin` header in the request:

```http
Origin: https://example.com
```

Then, the server can respond with:

```http
Access-Control-Allow-Origin: https://example.com
```

If this header is present and matches the origin, the browser allows the response to be read.

#### 2. **Preflight Requests**

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

Let me know if you wanna go deeper into token security or see a live example!