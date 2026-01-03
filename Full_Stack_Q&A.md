# Full Stack Interview Questions and Answers

- [Full Stack Interview Questions and Answers](#full-stack-interview-questions-and-answers)
  - [Java](#java)
    - [What are the main features of Java 8?](#what-are-the-main-features-of-java-8)
    - [Explain the concept of Java Memory Management](#explain-the-concept-of-java-memory-management)
    - [What is the difference between `==` and `equals()` in Java?](#what-is-the-difference-between--and-equals-in-java)
    - [How does the Java garbage collector work?](#how-does-the-java-garbage-collector-work)
    - [What are the different types of exceptions in Java?](#what-are-the-different-types-of-exceptions-in-java)
    - [Differences Between Java 11, 17 and Java 21](#differences-between-java-11-17-and-java-21)
      - [Java 11 Features](#java-11-features)
      - [Java 17 Features (LTS)](#java-17-features-lts)
      - [Java 21 Features](#java-21-features)
      - [Migration Guide: Java 11 → 17 → 21](#migration-guide-java-11--17--21)
  - [Angular](#angular)
    - [What is Angular and how does it differ from AngularJS?](#what-is-angular-and-how-does-it-differ-from-angularjs)
    - [Explain the concept of Angular modules](#explain-the-concept-of-angular-modules)
    - [What are Angular services and how are they used?](#what-are-angular-services-and-how-are-they-used)
    - [How does Angular handle data binding?](#how-does-angular-handle-data-binding)
    - [What is the purpose of Angular CLI?](#what-is-the-purpose-of-angular-cli)
  - [Hibernate](#hibernate)
    - [What is Hibernate and why is it used?](#what-is-hibernate-and-why-is-it-used)
    - [Explain the concept of lazy loading in Hibernate](#explain-the-concept-of-lazy-loading-in-hibernate)
    - [What are the different types of Hibernate mappings?](#what-are-the-different-types-of-hibernate-mappings)
    - [How does Hibernate caching work?](#how-does-hibernate-caching-work)
    - [What is the difference between `save()` and `persist()` methods in Hibernate?](#what-is-the-difference-between-save-and-persist-methods-in-hibernate)
  - [Spring](#spring)
    - [What is Spring Framework?](#what-is-spring-framework)
    - [What are the benefits of using Spring Framework?](#what-are-the-benefits-of-using-spring-framework)
    - [What is Dependency Injection in Spring?](#what-is-dependency-injection-in-spring)
    - [Inversion of Control (IoC)](#inversion-of-control-ioc)
      - [Key Concepts of IoC](#key-concepts-of-ioc)
      - [Benefits of IoC](#benefits-of-ioc)
    - [Dependency Injection (DI)](#dependency-injection-di)
    - [Creating a Spring/Spring Boot Project from Scratch](#creating-a-springspring-boot-project-from-scratch)
      - [Option 1: Using Spring Initializr (Recommended)](#option-1-using-spring-initializr-recommended)
      - [Option 2: Manual Maven Setup](#option-2-manual-maven-setup)
      - [Option 3: Manual Gradle Setup](#option-3-manual-gradle-setup)
      - [Project Structure](#project-structure)
      - [Main Application Class](#main-application-class)
    - [Integrating Hibernate/JPA with Spring Boot](#integrating-hibernatejpa-with-spring-boot)
      - [Step 1: Add Dependencies](#step-1-add-dependencies)
      - [Step 2: Configure Database Connection](#step-2-configure-database-connection)
      - [Step 3: Create Entity Classes](#step-3-create-entity-classes)
      - [Step 4: Create Repository Interface](#step-4-create-repository-interface)
      - [Step 5: Create Service Layer](#step-5-create-service-layer)
      - [Step 6: Create Controller](#step-6-create-controller)
    - [Configuration Files Discovery](#configuration-files-discovery)
      - [1. Application Properties/YAML Discovery Order](#1-application-propertiesyaml-discovery-order)
      - [2. Profile-Specific Configuration](#2-profile-specific-configuration)
      - [3. External Configuration Sources (Priority Order)](#3-external-configuration-sources-priority-order)
      - [4. Java-Based Configuration Discovery](#4-java-based-configuration-discovery)
      - [5. Custom Configuration Files](#5-custom-configuration-files)
    - [Most Common Spring Annotations](#most-common-spring-annotations)
      - [Core Spring Annotations](#core-spring-annotations)
      - [Dependency Injection Annotations](#dependency-injection-annotations)
      - [JPA/Hibernate Annotations](#jpahibernate-annotations)
      - [REST API Annotations](#rest-api-annotations)
      - [Validation Annotations](#validation-annotations)
      - [Transaction Annotations](#transaction-annotations)
      - [Spring Boot Specific Annotations](#spring-boot-specific-annotations)
    - [Inversion of Control (IoC) Deep Dive](#inversion-of-control-ioc-deep-dive)
      - [Understanding the Fundamental Difference](#understanding-the-fundamental-difference)
      - [Traditional Flow (Without IoC) - YOU control everything](#traditional-flow-without-ioc---you-control-everything)
      - [With IoC (Spring Container) - SPRING controls everything](#with-ioc-spring-container---spring-controls-everything)
      - [What Actually Changes? Key Differences](#what-actually-changes-key-differences)
        - [1. **Control of Object Creation**](#1-control-of-object-creation)
        - [2. **Singleton Management**](#2-singleton-management)
        - [3. **Testing**](#3-testing)
        - [4. **Switching Implementations**](#4-switching-implementations)
        - [5. **Complex Dependency Trees**](#5-complex-dependency-trees)
      - [Visual Comparison](#visual-comparison)
      - [The "Aha!" Moment](#the-aha-moment)
      - [Spring IoC Container Types](#spring-ioc-container-types)
      - [Bean Lifecycle in IoC Container](#bean-lifecycle-in-ioc-container)
    - [Dependency Injection (DI) Deep Dive](#dependency-injection-di-deep-dive)
      - [Types of Dependency Injection](#types-of-dependency-injection)
        - [1. Constructor Injection (Recommended)](#1-constructor-injection-recommended)
        - [2. Setter Injection](#2-setter-injection)
        - [3. Field Injection (Not Recommended)](#3-field-injection-not-recommended)
      - [Handling Multiple Bean Candidates](#handling-multiple-bean-candidates)
      - [Bean Scopes](#bean-scopes)
      - [Circular Dependencies](#circular-dependencies)
    - [Spring Boot Auto-Configuration](#spring-boot-auto-configuration)
    - [Spring Boot](#spring-boot)
      - [What is Spring Boot and how does it simplify Spring development?](#what-is-spring-boot-and-how-does-it-simplify-spring-development)
      - [What are the advantages of using Spring Boot?](#what-are-the-advantages-of-using-spring-boot)
      - [Explain the concept of Spring Boot starters](#explain-the-concept-of-spring-boot-starters)
      - [How does Spring Boot handle dependency management?](#how-does-spring-boot-handle-dependency-management)
      - [What is the purpose of Spring Boot Actuator?](#what-is-the-purpose-of-spring-boot-actuator)
      - [How do you configure a Spring Boot application?](#how-do-you-configure-a-spring-boot-application)
      - [Spring Boot Version Evolution (1.x → 2.x → 3.x)](#spring-boot-version-evolution-1x--2x--3x)
        - [Spring Boot 1.x (2014-2019)](#spring-boot-1x-2014-2019)
        - [Spring Boot 2.x (2018-2023)](#spring-boot-2x-2018-2023)
        - [Spring Boot 3.x (2022-present)](#spring-boot-3x-2022-present)
        - [Version Comparison Table](#version-comparison-table)
      - [Spring Boot 3.x Deep Dive](#spring-boot-3x-deep-dive)
        - [Jakarta EE 9+ Migration](#jakarta-ee-9-migration)
        - [Native Compilation with GraalVM](#native-compilation-with-graalvm)
        - [Observability with Micrometer Tracing](#observability-with-micrometer-tracing)
        - [Virtual Threads Support (Java 21)](#virtual-threads-support-java-21)
        - [Problem Details (RFC 7807)](#problem-details-rfc-7807)
    - [Spring Core Modules](#spring-core-modules)
      - [Spring Core Container](#spring-core-container)
      - [Bean Scopes](#bean-scopes-1)
      - [Bean Lifecycle](#bean-lifecycle)
      - [Conditional Beans](#conditional-beans)
      - [Profiles](#profiles)
    - [Spring Security](#spring-security)
      - [Basic Configuration (Spring Boot 3.x)](#basic-configuration-spring-boot-3x)
      - [JWT Authentication](#jwt-authentication)
      - [Method-Level Security](#method-level-security)
    - [Spring Data](#spring-data)
      - [Spring Data JPA](#spring-data-jpa)
      - [Specifications (Dynamic Queries)](#specifications-dynamic-queries)
      - [Pagination and Sorting](#pagination-and-sorting)
    - [Spring WebFlux (Reactive)](#spring-webflux-reactive)
      - [Reactive Controller](#reactive-controller)
      - [Reactive Repository (MongoDB)](#reactive-repository-mongodb)
      - [WebClient (Reactive HTTP Client)](#webclient-reactive-http-client)
  - [SQL](#sql)
    - [What is SQL?](#what-is-sql)
    - [What are the different types of SQL commands?](#what-are-the-different-types-of-sql-commands)
    - [What is a JOIN in SQL?](#what-is-a-join-in-sql)
    - [Panoramica SQL](#panoramica-sql)
      - [DDL (Data Definition Language)](#ddl-data-definition-language)
        - [CREATE TABLE](#create-table)
        - [ALTER TABLE](#alter-table)
        - [DROP TABLE](#drop-table)
        - [TRUNCATE TABLE](#truncate-table)
        - [CREATE INDEX](#create-index)
      - [DML (Data Manipulation Language)](#dml-data-manipulation-language)
        - [SELECT](#select)
        - [INSERT](#insert)
        - [UPDATE](#update)
        - [DELETE](#delete)
      - [JOIN](#join)
        - [INNER JOIN](#inner-join)
        - [LEFT JOIN (LEFT OUTER JOIN)](#left-join-left-outer-join)
        - [RIGHT JOIN](#right-join)
        - [FULL OUTER JOIN](#full-outer-join)
        - [CROSS JOIN](#cross-join)
        - [SELF JOIN](#self-join)
      - [Funzioni Aggregate](#funzioni-aggregate)
        - [COUNT(\*) vs COUNT(column)](#count-vs-countcolumn)
      - [GROUP BY e HAVING](#group-by-e-having)
        - [GROUP BY](#group-by)
        - [HAVING](#having)
      - [Subquery](#subquery)
        - [Subquery nella WHERE](#subquery-nella-where)
        - [Subquery con IN](#subquery-con-in)
        - [Subquery con EXISTS](#subquery-con-exists)
        - [Subquery nella SELECT](#subquery-nella-select)
        - [Subquery nella FROM (Derived Table)](#subquery-nella-from-derived-table)
      - [Window Functions (OVER)](#window-functions-over)
        - [ROW\_NUMBER()](#row_number)
        - [RANK() e DENSE\_RANK()](#rank-e-dense_rank)
        - [PARTITION BY](#partition-by)
        - [LAG() e LEAD()](#lag-e-lead)
        - [Funzioni aggregate con OVER](#funzioni-aggregate-con-over)
      - [CASE (Conditional Logic)](#case-conditional-logic)
      - [UNION / UNION ALL](#union--union-all)
      - [CTE (Common Table Expressions)](#cte-common-table-expressions)
  - [NoSQL](#nosql)
    - [What is NoSQL?](#what-is-nosql)
    - [What are the types of NoSQL databases?](#what-are-the-types-of-nosql-databases)
    - [What is the difference between SQL and NoSQL databases?](#what-is-the-difference-between-sql-and-nosql-databases)
  - [MongoDB](#mongodb)
    - [What is MongoDB?](#what-is-mongodb)
    - [MongoDB Architecture](#mongodb-architecture)
      - [Basic Concepts](#basic-concepts)
      - [Document Structure](#document-structure)
      - [Data Types](#data-types)
    - [CRUD Operations](#crud-operations)
      - [Create (Insert)](#create-insert)
        - [Read (Query)](#read-query)
        - [Update](#update-1)
      - [Delete](#delete-1)
    - [Aggregation Pipeline](#aggregation-pipeline)
      - [Basic Aggregation](#basic-aggregation)
      - [Common Aggregation Stages](#common-aggregation-stages)
      - [Advanced Aggregation Example](#advanced-aggregation-example)
    - [Indexing in MongoDB](#indexing-in-mongodb)
      - [Index Types](#index-types)
      - [Index Management](#index-management)
    - [MongoDB with Spring Boot](#mongodb-with-spring-boot)
      - [Dependencies (pom.xml)](#dependencies-pomxml)
      - [Configuration](#configuration)
      - [Document Entity](#document-entity)
      - [Repository](#repository)
      - [MongoTemplate for Complex Queries](#mongotemplate-for-complex-queries)
      - [Reactive MongoDB](#reactive-mongodb)
      - [MongoDB Transactions (Replica Set Required)](#mongodb-transactions-replica-set-required)
  - [General Full Stack](#general-full-stack)
    - [Explain the MVC architecture and its components](#explain-the-mvc-architecture-and-its-components)
    - [What are RESTful web services and how do they work?](#what-are-restful-web-services-and-how-do-they-work)
    - [How do you ensure security in a full stack application?](#how-do-you-ensure-security-in-a-full-stack-application)
    - [What is the role of a build tool in a full stack project?](#what-is-the-role-of-a-build-tool-in-a-full-stack-project)
    - [How do you optimize the performance of a full stack application?](#how-do-you-optimize-the-performance-of-a-full-stack-application)
  - [CORS](#cors)
    - [What is CORS and how is it used](#what-is-cors-and-how-is-it-used)
    - [🔐 Why CORS Exists](#-why-cors-exists)
    - [⚙️ How CORS Works](#️-how-cors-works)
      - [Simple Requests](#simple-requests)
      - [Preflight Requests](#preflight-requests)
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

### What are the main features of Java 8?

- **Lambda Expressions**: Introduce functional programming by allowing you to pass functionality as an argument to a method.
- **Stream API**: Provides a new abstraction to process sequences of elements, making it easier to perform operations like filtering, mapping, and reducing.
- **Default Methods**: Allow you to add new methods to interfaces without breaking existing implementations.
- **Optional Class**: Helps in handling null values more gracefully.
- **New Date and Time API**: Provides a comprehensive and flexible date-time handling mechanism.

### Explain the concept of Java Memory Management

- Java memory management involves the allocation and deallocation of memory to objects. The Java Virtual Machine (JVM) manages memory through a process called garbage collection, which automatically removes objects that are no longer in use to free up memory.

### What is the difference between `==` and `equals()` in Java?

- `==` checks for reference equality, meaning it checks if two references point to the same object in memory.
- `equals()` checks for value equality, meaning it checks if two objects are logically equivalent based on their state.

### How does the Java garbage collector work?

- The garbage collector in Java automatically identifies and disposes of objects that are no longer reachable in the program. It uses algorithms like Mark-and-Sweep, Generational Garbage Collection, and others to manage memory efficiently.

### What are the different types of exceptions in Java?

- **Checked Exceptions**: Exceptions that are checked at compile-time (e.g., IOException, SQLException).
- **Unchecked Exceptions**: Exceptions that are checked at runtime (e.g., NullPointerException, ArrayIndexOutOfBoundsException).
- **Errors**: Serious issues that a reasonable application should not try to catch (e.g., OutOfMemoryError, StackOverflowError).

### Differences Between Java 11, 17 and Java 21

#### Java 11 Features

Released in September 2018, Java 11 is an **LTS (Long-Term Support)** release with several important updates:

1. **HTTP Client API**: Provides a modern, feature-rich HTTP client for synchronous and asynchronous communication.
2. **Local-Variable Syntax for Lambda Parameters**: Allows the use of `var` in lambda expressions.
3. **New String Methods**: Adds methods like `isBlank()`, `lines()`, `strip()`, `stripLeading()`, `stripTrailing()`, and `repeat()`.
4. **New File Methods**: Introduces `readString()` and `writeString()` methods in the `Files` class.
5. **Collection to an Array**: Adds a new default `toArray` method in the `java.util.Collection` interface.
6. **Not Predicate Method**: Adds a static `not` method to the `Predicate` interface.
7. **Nest-Based Access Control**: Allows private members of nested classes to be accessed without synthetic bridge methods.
8. **Epsilon GC**: A no-op garbage collector for performance testing.

#### Java 17 Features (LTS)

Released in September 2021, Java 17 is a major **LTS release** and is required for Spring Boot 3.x:

1. **Sealed Classes (JEP 409)**: Restrict which classes can extend or implement them.

   ```java
   public sealed class Shape permits Circle, Rectangle, Triangle {
       // Base class
   }

   public final class Circle extends Shape {
       private final double radius;
       public Circle(double radius) { this.radius = radius; }
   }

   public non-sealed class Rectangle extends Shape {
       // Can be extended by any class
   }
   ```

2. **Pattern Matching for instanceof (JEP 394)**: Eliminates the need for explicit casting.

   ```java
   // Before Java 17
   if (obj instanceof String) {
      String s = (String) obj;
      System.out.println(s.length());
   }

   // Java 17+
   if (obj instanceof String s) {
      System.out.println(s.length());
   }
   ```

3. **Records (JEP 395)**: Immutable data classes with auto-generated methods.

   ```java
   public record Person(String name, int age) {
      // Compact constructor for validation
      public Person {
         if (age < 0) throw new IllegalArgumentException("Age cannot be negative");
      }
   }

   // Usage
   Person person = new Person("Mario", 30);
   String name = person.name(); // Auto-generated getter
   ```

4. **Switch Expressions (JEP 406)**: Enhanced switch with arrow syntax and yield.

   ```java
   String result = switch (day) {
      case MONDAY, FRIDAY, SUNDAY -> "Relaxed";
      case TUESDAY -> "Busy";
      case THURSDAY, SATURDAY -> "Normal";
      case WEDNESDAY -> {
         System.out.println("Midweek");
         yield "Special";
      }
   };
   ```

5. **Text Blocks (JEP 378)**: Multi-line string literals.

   ```java
   String json = """
      {
         "name": "Mario Rossi",
         "age": 30,
         "city": "Rome"
      }
      """;

   String html = """
      <html>
         <body>
               <h1>Hello, World!</h1>
         </body>
      </html>
      """;
   ```

6. **New Random Generator API (JEP 356)**: Provides new interfaces and implementations for random number generation.

   ```java
   RandomGenerator generator = RandomGenerator.of("L64X128MixRandom");
   int randomInt = generator.nextInt(100);
   ```

7. **Strong Encapsulation of JDK Internals**: By default, internal APIs are strongly encapsulated.

8. **Foreign Function & Memory API (Incubator)**: Allows Java programs to interoperate with native code and memory.

9. **Context-Specific Deserialization Filters**: Helps prevent deserialization vulnerabilities.

10. **Enhanced Pseudo-Random Number Generators**: New PRNG algorithms and interfaces.

#### Java 21 Features

Released in September 2023, Java 21 is the latest **LTS release** with major improvements:

1. **Record Patterns (JEP 440)**: Enhances pattern matching by allowing the deconstruction of record class instances.

   ```java
   record Point(int x, int y) {}

   // Deconstruct record in pattern matching
   if (obj instanceof Point(int x, int y)) {
      System.out.println("x = " + x + ", y = " + y);
   }
   ```

2. **Pattern Matching for switch (JEP 441)**: Complete pattern matching in switch statements.

   ```java
   String format(Object obj) {
      return switch (obj) {
         case Integer i -> String.format("int %d", i);
         case Long l -> String.format("long %d", l);
         case Double d -> String.format("double %f", d);
         case String s -> String.format("String %s", s);
         case null -> "null";
         default -> obj.toString();
      };
   }
   ```

3. **String Templates (JEP 430 - Preview)**: Safer string interpolation.

   ```java
   String name = "Mario";
   int age = 30;
   // Preview feature - syntax may change
   String message = STR."Hello, \{name}! You are \{age} years old.";
   ```

4. **Sequenced Collections (JEP 431)**: New interfaces for ordered collections.

   ```java
   SequencedCollection<String> list = new ArrayList<>();
   list.addFirst("first");
   list.addLast("last");
   String first = list.getFirst();
   String last = list.getLast();
   SequencedCollection<String> reversed = list.reversed();
   ```

5. **Virtual Threads (JEP 444)**: Lightweight threads for high-throughput concurrent applications.

   ```java
   // Create virtual thread
   Thread vThread = Thread.ofVirtual().start(() -> {
      System.out.println("Running in virtual thread");
   });

   // Virtual thread executor
   try (var executor = Executors.newVirtualThreadPerTaskExecutor()) {
      IntStream.range(0, 10_000).forEach(i -> {
         executor.submit(() -> {
               Thread.sleep(Duration.ofSeconds(1));
               return i;
         });
      });
   }  // Automatically waits for all tasks
   ```

6. **Scoped Values (JEP 446 - Preview)**: Immutable, inheritable thread-local-like values.

   ```java
   final static ScopedValue<User> CURRENT_USER = ScopedValue.newInstance();

   // Bind value for a scope
   ScopedValue.where(CURRENT_USER, user).run(() -> {
      // CURRENT_USER.get() returns user in this scope
      processRequest();
   });
   ```

7. **Structured Concurrency (JEP 453 - Preview)**: Simplifies multithreaded programming.

   ```java
   try (var scope = new StructuredTaskScope.ShutdownOnFailure()) {
      Future<String> user = scope.fork(() -> fetchUser());
      Future<Integer> order = scope.fork(() -> fetchOrder());
      
      scope.join();          // Wait for both
      scope.throwIfFailed(); // Propagate errors
      
      return new Response(user.resultNow(), order.resultNow());
   }
   ```

8. **Unnamed Patterns and Variables (JEP 443)**: Use underscore for unused variables.

   ```java
   // Unnamed variable
   try {
      // ...
   } catch (Exception _) {
      System.out.println("An error occurred");
   }

   // Unnamed pattern
   if (obj instanceof Point(int x, int _)) {
      System.out.println("x = " + x);
   }
   ```

#### Migration Guide: Java 11 → 17 → 21

**From Java 11 to Java 17:**

| Aspect | Java 11 | Java 17 |
|--------|---------|----------|
| Status | LTS (until 2026) | LTS (until 2029) |
| Spring Boot | 2.x | 2.x and 3.x |
| Records | Not available | Final |
| Sealed Classes | Not available | Final |
| Pattern Matching | Not available | instanceof |
| Text Blocks | Not available | Final |

**From Java 17 to Java 21:**

| Aspect | Java 17 | Java 21 |
|--------|---------|----------|
| Virtual Threads | Not available | Final |
| Pattern Matching | instanceof only | switch + records |
| Sequenced Collections | Not available | Final |
| String Templates | Not available | Preview |
| Structured Concurrency | Not available | Preview |

**Common Migration Issues:**

1. **Removed APIs**: Some deprecated APIs removed (e.g., `SecurityManager`)
2. **Strong encapsulation**: `--add-opens` may be needed for reflection
3. **Module system**: Some internal APIs require explicit module access
4. **Build tools**: Update Maven/Gradle plugins for compatibility

## Angular

### What is Angular and how does it differ from AngularJS?

- Angular is a platform and framework for building single-page client applications using HTML and TypeScript. AngularJS is the original version of Angular, which is based on JavaScript. Angular (versions 2 and above) is a complete rewrite of AngularJS and offers improved performance, a more modular architecture, and better tooling.

### Explain the concept of Angular modules

- Angular modules (NgModules) are containers for a cohesive block of code dedicated to an application domain, a workflow, or a closely related set of capabilities. They help organize an application into cohesive blocks of functionality.

### What are Angular services and how are they used?

- Angular services are singleton objects that encapsulate reusable logic and data that can be shared across components. They are typically used for tasks like fetching data from a server, logging, and other business logic.

### How does Angular handle data binding?

- Angular supports two-way data binding, which means that changes in the model automatically update the view and vice versa. This is achieved using directives like `ngModel` and property/event binding syntax.

### What is the purpose of Angular CLI?

- Angular CLI (Command Line Interface) is a powerful tool that helps automate the development workflow. It provides commands to create, build, serve, and test Angular applications, making it easier to manage projects.

## Hibernate

### What is Hibernate and why is it used?

- Hibernate is an Object-Relational Mapping (ORM) framework for Java. It simplifies database interactions by mapping Java objects to database tables, allowing developers to work with data in an object-oriented way.

### Explain the concept of lazy loading in Hibernate

- Lazy loading is a design pattern used in Hibernate to defer the initialization of an object until it is needed. This helps improve performance by avoiding unnecessary database queries.

### What are the different types of Hibernate mappings?

- **One-to-One Mapping**: Maps a single entity to another single entity.
- **One-to-Many Mapping**: Maps a single entity to a collection of entities.
- **Many-to-One Mapping**: Maps multiple entities to a single entity.
- **Many-to-Many Mapping**: Maps a collection of entities to another collection of entities.

### How does Hibernate caching work?

- Hibernate provides two levels of caching:
  - **First-Level Cache**: Associated with the session object and is enabled by default.
  - **Second-Level Cache**: Associated with the session factory and can be configured to cache data across sessions.

### What is the difference between `save()` and `persist()` methods in Hibernate?

- `save()`: Generates an identifier and inserts the record immediately.
- `persist()`: Does not generate an identifier immediately and the record is inserted when the transaction is committed.

## Spring

### What is Spring Framework?

Spring is an open-source framework for building enterprise Java applications. It provides comprehensive infrastructure support for developing Java applications. Spring promotes good programming practices by enabling a POJO-based (Plain Old Java Object) programming model.

### What are the benefits of using Spring Framework?

The benefits of using Spring Framework include:

- **Lightweight:** Spring is lightweight in terms of size and transparency.
- **Inversion of Control (IoC):** Promotes loose coupling through dependency injection.
- **Aspect-Oriented Programming (AOP):** Separates business logic from system services.
- **Transaction Management:** Provides a consistent transaction management interface.
- **MVC Framework:** Offers a well-designed web MVC framework.

### What is Dependency Injection in Spring?

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

### Creating a Spring/Spring Boot Project from Scratch

#### Option 1: Using Spring Initializr (Recommended)

**Spring Initializr** is the official way to bootstrap a Spring Boot project.

1. **Web Interface**: Visit [https://start.spring.io](https://start.spring.io)
2. **IDE Integration**: Most IDEs (IntelliJ IDEA, Eclipse, VS Code) have built-in Spring Initializr support
3. **Command Line**: Use Spring CLI

**Configuration Options:**

- **Project**: Maven or Gradle
- **Language**: Java, Kotlin, or Groovy
- **Spring Boot Version**: Choose LTS version (currently 3.x)
- **Project Metadata**: Group, Artifact, Name, Package
- **Packaging**: Jar (recommended) or War
- **Java Version**: 17 or 21 (required for Spring Boot 3.x)

**Common Dependencies to Add:**

- Spring Web (for REST APIs)
- Spring Data JPA (for database access)
- PostgreSQL/MySQL Driver (database)
- Spring Security (for authentication/authorization)
- Spring Boot DevTools (for hot reload)
- Lombok (to reduce boilerplate)
- Validation (for input validation)
- Spring Boot Actuator (for monitoring)

#### Option 2: Manual Maven Setup

Create a basic `pom.xml`:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
         http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    
    <!-- Inherit from Spring Boot parent -->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.2.0</version>
        <relativePath/>
    </parent>
    
    <groupId>com.example</groupId>
    <artifactId>myapp</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>My Spring Boot App</name>
    
    <properties>
        <java.version>17</java.version>
    </properties>
    
    <dependencies>
        <!-- Spring Boot Web Starter -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        
        <!-- Spring Boot Test -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>
    
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
</project>
```

#### Option 3: Manual Gradle Setup

Create `build.gradle`:

```gradle
plugins {
    id 'java'
    id 'org.springframework.boot' version '3.2.0'
    id 'io.spring.dependency-management' version '1.1.4'
}

group = 'com.example'
version = '0.0.1-SNAPSHOT'
sourceCompatibility = '17'

repositories {
    mavenCentral()
}

dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-web'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
}

tasks.named('test') {
    useJUnitPlatform()
}
```

#### Project Structure

```sh
myapp/
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/
│   │   │       └── example/
│   │   │           └── myapp/
│   │   │               ├── MyAppApplication.java (Main class)
│   │   │               ├── controller/
│   │   │               ├── service/
│   │   │               ├── repository/
│   │   │               ├── model/
│   │   │               └── config/
│   │   └── resources/
│   │       ├── application.properties (or application.yml)
│   │       ├── application-dev.properties
│   │       ├── application-prod.properties
│   │       └── static/ (for static files)
│   └── test/
│       └── java/
│           └── com/
│               └── example/
│                   └── myapp/
│                       └── MyAppApplicationTests.java
├── pom.xml (or build.gradle)
└── README.md
```

#### Main Application Class

```java
package com.example.myapp;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class MyAppApplication {
    
    public static void main(String[] args) {
        SpringApplication.run(MyAppApplication.class, args);
    }
}
```

The `@SpringBootApplication` annotation is a combination of:

- `@Configuration`: Tags the class as a source of bean definitions
- `@EnableAutoConfiguration`: Tells Spring Boot to auto-configure based on dependencies
- `@ComponentScan`: Scans for components in the current package and sub-packages

---

### Integrating Hibernate/JPA with Spring Boot

#### Step 1: Add Dependencies

Add to `pom.xml`:

```xml
<dependencies>
    <!-- Spring Data JPA (includes Hibernate) -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>
    
    <!-- Database Driver (choose one) -->
    <!-- PostgreSQL -->
    <dependency>
        <groupId>org.postgresql</groupId>
        <artifactId>postgresql</artifactId>
        <scope>runtime</scope>
    </dependency>
    
    <!-- MySQL -->
    <dependency>
        <groupId>com.mysql</groupId>
        <artifactId>mysql-connector-j</artifactId>
        <scope>runtime</scope>
    </dependency>
    
    <!-- H2 (in-memory, for development/testing) -->
    <dependency>
        <groupId>com.h2database</groupId>
        <artifactId>h2</artifactId>
        <scope>runtime</scope>
    </dependency>
</dependencies>
```

**What's included with `spring-boot-starter-data-jpa`:**

- Hibernate ORM (the JPA implementation)
- Spring Data JPA (repository abstraction)
- Transaction management
- JDBC support

#### Step 2: Configure Database Connection

In `src/main/resources/application.properties`:

```properties
# PostgreSQL Configuration
spring.datasource.url=jdbc:postgresql://localhost:5432/mydb
spring.datasource.username=postgres
spring.datasource.password=password
spring.datasource.driver-class-name=org.postgresql.Driver

# JPA/Hibernate Configuration
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQLDialect

# Connection Pool (HikariCP - default in Spring Boot)
spring.datasource.hikari.maximum-pool-size=10
spring.datasource.hikari.minimum-idle=5
spring.datasource.hikari.connection-timeout=20000
```

Or in `application.yml`:

```yaml
spring:
  datasource:
    url: jdbc:postgresql://localhost:5432/mydb
    username: postgres
    password: password
    driver-class-name: org.postgresql.Driver
    hikari:
      maximum-pool-size: 10
      minimum-idle: 5
      
  jpa:
    hibernate:
      ddl-auto: update
    show-sql: true
    properties:
      hibernate:
        format_sql: true
        dialect: org.hibernate.dialect.PostgreSQLDialect
```

**`spring.jpa.hibernate.ddl-auto` options:**

- `none`: No action (production)
- `validate`: Validate schema, make no changes
- `update`: Update schema (development)
- `create`: Drop and create schema on startup
- `create-drop`: Create on startup, drop on shutdown

#### Step 3: Create Entity Classes

```java
package com.example.myapp.model;

import jakarta.persistence.*;
import java.time.LocalDateTime;

@Entity
@Table(name = "users")
public class User {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(nullable = false)
    private String name;
    
    @Column(unique = true, nullable = false)
    private String email;
    
    @Column(name = "created_at")
    private LocalDateTime createdAt;
    
    @OneToMany(mappedBy = "user", cascade = CascadeType.ALL, fetch = FetchType.LAZY)
    private List<Order> orders;
    
    @PrePersist
    protected void onCreate() {
        createdAt = LocalDateTime.now();
    }
    
    // Constructors, getters, setters
}
```

#### Step 4: Create Repository Interface

```java
package com.example.myapp.repository;

import com.example.myapp.model.User;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;
import java.util.Optional;

@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    Optional<User> findByEmail(String email);
    // Spring Data JPA generates the implementation automatically!
}
```

#### Step 5: Create Service Layer

```java
package com.example.myapp.service;

import com.example.myapp.model.User;
import com.example.myapp.repository.UserRepository;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

@Service
@Transactional
public class UserService {
    
    private final UserRepository userRepository;
    
    // Constructor injection (recommended)
    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
    
    public User createUser(User user) {
        return userRepository.save(user);
    }
    
    public Optional<User> getUserById(Long id) {
        return userRepository.findById(id);
    }
    
    public List<User> getAllUsers() {
        return userRepository.findAll();
    }
}
```

#### Step 6: Create Controller

```java
package com.example.myapp.controller;

import com.example.myapp.model.User;
import com.example.myapp.service.UserService;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/api/users")
public class UserController {
    
    private final UserService userService;
    
    public UserController(UserService userService) {
        this.userService = userService;
    }
    
    @GetMapping
    public List<User> getAllUsers() {
        return userService.getAllUsers();
    }
    
    @GetMapping("/{id}")
    public ResponseEntity<User> getUser(@PathVariable Long id) {
        return userService.getUserById(id)
                .map(ResponseEntity::ok)
                .orElse(ResponseEntity.notFound().build());
    }
    
    @PostMapping
    public User createUser(@RequestBody User user) {
        return userService.createUser(user);
    }
}
```

### Configuration Files Discovery

Spring Boot uses **convention over configuration** and discovers configuration files automatically:

#### 1. Application Properties/YAML Discovery Order

Spring Boot looks for configuration files in this order (later sources override earlier ones):

1. **Classpath root**: `src/main/resources/application.properties`
2. **Classpath `/config` package**: `src/main/resources/config/application.properties`
3. **Current directory**: `./application.properties`
4. **Current directory `/config`**: `./config/application.properties`

#### 2. Profile-Specific Configuration

You can have environment-specific configurations:

- `application.properties` (default)
- `application-dev.properties` (development)
- `application-test.properties` (testing)
- `application-prod.properties` (production)

**Activate a profile:**

```properties
# In application.properties
spring.profiles.active=dev
```

Or via command line:

```bash
java -jar myapp.jar --spring.profiles.active=prod
```

Or environment variable:

```bash
export SPRING_PROFILES_ACTIVE=prod
```

#### 3. External Configuration Sources (Priority Order)

1. Command line arguments
2. SPRING_APPLICATION_JSON (environment variable or system property)
3. ServletConfig init parameters
4. ServletContext init parameters
5. JNDI attributes
6. Java System properties (`System.getProperties()`)
7. OS environment variables
8. Profile-specific application properties
9. Application properties (`application.properties`)
10. `@PropertySource` annotations
11. Default properties (`SpringApplication.setDefaultProperties`)

#### 4. Java-Based Configuration Discovery

Spring Boot automatically discovers and processes:

**Component Scanning:**

```java
@SpringBootApplication // Scans current package and sub-packages
// Equivalent to:
@Configuration
@EnableAutoConfiguration
@ComponentScan(basePackages = "com.example.myapp")
```

**Auto-Configuration:**
Spring Boot automatically configures beans based on:

- Dependencies in classpath
- Existing beans
- Properties in `application.properties`

Files: Located in `spring-boot-autoconfigure.jar`

- `META-INF/spring.factories` (Spring Boot 2.x)
- `META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports` (Spring Boot 3.x)

#### 5. Custom Configuration Files

You can specify custom configuration files:

```bash
java -jar myapp.jar --spring.config.location=file:./config/custom.properties
```

Or in code:

```java
@Configuration
@PropertySource("classpath:custom.properties")
public class CustomConfig {
    // ...
}
```

---

### Most Common Spring Annotations

#### Core Spring Annotations

| Annotation | Purpose | Usage |
|------------|---------|-------|
| `@Component` | Generic stereotype for Spring-managed component | Any Spring-managed class |
| `@Service` | Specialized `@Component` for service layer | Business logic classes |
| `@Repository` | Specialized `@Component` for data access layer | DAO/Repository classes |
| `@Controller` | Specialized `@Component` for MVC controllers | Web controllers (returns views) |
| `@RestController` | `@Controller` + `@ResponseBody` | REST API controllers |
| `@Configuration` | Indicates class contains bean definitions | Configuration classes |
| `@Bean` | Declares a method produces a bean | Methods in `@Configuration` classes |
| `@ComponentScan` | Configures component scanning | Configuration classes |
| `@Import` | Imports additional configuration classes | Configuration classes |

#### Dependency Injection Annotations

| Annotation | Purpose | Example |
|------------|---------|---------|
| `@Autowired` | Injects dependency automatically | Field, constructor, setter injection |
| `@Qualifier` | Specifies which bean to inject when multiple exist | `@Qualifier("specificBean")` |
| `@Primary` | Marks bean as primary when multiple candidates exist | On bean definition |
| `@Value` | Injects values from properties | `@Value("${app.name}")` |
| `@Lazy` | Delays bean initialization until first use | On bean or `@Autowired` |

**Example:**

```java
@Service
public class OrderService {
    
    private final UserRepository userRepository;
    private final EmailService emailService;
    
    @Value("${app.max-orders}")
    private int maxOrders;
    
    // Constructor injection (recommended - no @Autowired needed in single constructor)
    public OrderService(UserRepository userRepository, 
                       @Qualifier("gmailService") EmailService emailService) {
        this.userRepository = userRepository;
        this.emailService = emailService;
    }
}
```

#### JPA/Hibernate Annotations

| Annotation | Purpose | Usage |
|------------|---------|-------|
| `@Entity` | Marks class as JPA entity | Domain model classes |
| `@Table` | Specifies table details | `@Table(name = "users")` |
| `@Id` | Marks primary key field | ID fields |
| `@GeneratedValue` | Configures ID generation strategy | With `@Id` |
| `@Column` | Configures column mapping | Fields |
| `@OneToMany` | One-to-many relationship | Collection fields |
| `@ManyToOne` | Many-to-one relationship | Reference fields |
| `@ManyToMany` | Many-to-many relationship | Collection fields |
| `@JoinColumn` | Specifies foreign key column | Relationship fields |
| `@JoinTable` | Configures join table | Many-to-many relationships |
| `@Transient` | Excludes field from persistence | Non-persisted fields |
| `@Temporal` | Specifies temporal type | Date/Time fields (pre-Java 8) |

#### REST API Annotations

| Annotation | Purpose | HTTP Method |
|------------|---------|-------------|
| `@RequestMapping` | Maps requests to handler methods | Any (configurable) |
| `@GetMapping` | Handles GET requests | GET |
| `@PostMapping` | Handles POST requests | POST |
| `@PutMapping` | Handles PUT requests | PUT |
| `@DeleteMapping` | Handles DELETE requests | DELETE |
| `@PatchMapping` | Handles PATCH requests | PATCH |
| `@PathVariable` | Extracts value from URI path | `@GetMapping("/{id}")` |
| `@RequestParam` | Extracts query parameter | `?page=1` |
| `@RequestBody` | Binds request body to parameter | POST/PUT requests |
| `@ResponseBody` | Serializes return value to response body | Return values |
| `@ResponseStatus` | Sets HTTP response status | Methods |

**Example:**

```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    
    @GetMapping("/{id}")
    public ResponseEntity<User> getUser(@PathVariable Long id) {
        // ...
    }
    
    @GetMapping
    public Page<User> getUsers(@RequestParam(defaultValue = "0") int page,
                               @RequestParam(defaultValue = "10") int size) {
        // ...
    }
    
    @PostMapping
    @ResponseStatus(HttpStatus.CREATED)
    public User createUser(@RequestBody @Valid User user) {
        // ...
    }
}
```

#### Validation Annotations

| Annotation | Purpose |
|------------|---------|
| `@Valid` | Triggers validation |
| `@NotNull` | Field must not be null |
| `@NotEmpty` | Collection/String must not be empty |
| `@NotBlank` | String must not be blank (not null, not empty, not whitespace) |
| `@Size` | Validates size of String/Collection |
| `@Min` / `@Max` | Validates numeric range |
| `@Email` | Validates email format |
| `@Pattern` | Validates against regex |

```java
public class User {
    @NotBlank(message = "Name is required")
    @Size(min = 2, max = 100)
    private String name;
    
    @Email(message = "Invalid email format")
    private String email;
    
    @Min(18)
    @Max(120)
    private Integer age;
}
```

#### Transaction Annotations

| Annotation | Purpose |
|------------|---------|
| `@Transactional` | Manages transaction boundaries |
| `@EnableTransactionManagement` | Enables transaction management |

```java
@Service
@Transactional
public class UserService {
    
    @Transactional(readOnly = true)
    public User getUser(Long id) {
        // Read-only transaction
    }
    
    @Transactional(rollbackFor = Exception.class, 
                   propagation = Propagation.REQUIRED)
    public void createUser(User user) {
        // Write transaction
    }
}
```

#### Spring Boot Specific Annotations

| Annotation | Purpose |
|------------|---------|
| `@SpringBootApplication` | Main application annotation (combines `@Configuration`, `@EnableAutoConfiguration`, `@ComponentScan`) |
| `@EnableAutoConfiguration` | Enables Spring Boot auto-configuration |
| `@ConfigurationProperties` | Binds properties to POJO |
| `@ConditionalOnProperty` | Conditional bean registration based on property |
| `@ConditionalOnClass` | Conditional bean registration based on class presence |
| `@Profile` | Activates beans for specific profiles |

```java
@Configuration
@ConfigurationProperties(prefix = "app")
public class AppProperties {
    private String name;
    private int maxUsers;
    // Getters and setters
}
```

```yaml
# application.yml
app:
  name: MyApp
  max-users: 1000
```

---

### Inversion of Control (IoC) Deep Dive

**Inversion of Control** means the framework (Spring) controls the creation and lifecycle of objects, rather than the application code.

#### Understanding the Fundamental Difference

**The key insight**: It's not just about WHERE you create objects, it's about WHO controls the creation and WHEN it happens.

#### Traditional Flow (Without IoC) - YOU control everything

```java
// Example 1: Creating dependencies inside the class
public class OrderService {
    private UserRepository userRepository = new UserRepositoryImpl();
    private EmailService emailService = new EmailServiceImpl();
    private PaymentService paymentService = new PaymentServiceImpl();
    
    public void createOrder(Order order) {
        // Use the services
    }
}

// Example 2: Even with constructor parameters, YOU still create everything
public class OrderController {
    private OrderService orderService;
    
    public OrderController() {
        // YOU manually create the entire dependency tree
        UserRepository userRepo = new UserRepositoryImpl();
        EmailService emailService = new EmailServiceImpl();
        PaymentService paymentService = new PaymentServiceImpl();
        
        // YOU wire them together
        this.orderService = new OrderService(userRepo, emailService, paymentService);
    }
}

// Main application
public class Application {
    public static void main(String[] args) {
        // YOU create everything manually
        UserRepository userRepo = new UserRepositoryImpl();
        EmailService emailService = new EmailServiceImpl();
        PaymentService paymentService = new PaymentServiceImpl();
        OrderService orderService = new OrderService(userRepo, emailService, paymentService);
        OrderController orderController = new OrderController(orderService);
        
        // If you need the same UserRepository elsewhere, you create another instance
        AdminController adminController = new AdminController(new UserRepositoryImpl());
    }
}
```

**Problems with this approach:**

1. **Tight Coupling**: `OrderService` is hardcoded to use `UserRepositoryImpl`. Want to switch to `PostgresUserRepository`? You must modify `OrderService`.

2. **Hard to Test**: How do you test `OrderService` without hitting a real database?

   ```java
   @Test
   public void testOrderService() {
       OrderService service = new OrderService();
       // No way to inject a mock UserRepository!
       // Your test will try to connect to a real database
   }
   ```

3. **Duplicate Instances**: Every time you do `new UserRepositoryImpl()`, you create a new object. If you want the same instance shared across the app (singleton pattern), YOU must manage it manually.

4. **Dependency Chain**: If `UserRepositoryImpl` needs a `DataSource`, and `DataSource` needs a `ConnectionPool`, YOU must create all of them in the right order:

   ```java
   ConnectionPool pool = new ConnectionPool("jdbc:...");
   DataSource dataSource = new DataSource(pool);
   UserRepositoryImpl userRepo = new UserRepositoryImpl(dataSource);
   OrderService orderService = new OrderService(userRepo);
   ```

5. **Configuration Changes**: Want to use a different implementation in production vs development? You need to write custom logic with `if` statements everywhere.

#### With IoC (Spring Container) - SPRING controls everything

```java
// 1. You define your interfaces
public interface UserRepository {
    User findById(Long id);
}

// 2. You provide implementations with annotations
@Repository
public class UserRepositoryImpl implements UserRepository {
    @Override
    public User findById(Long id) {
        // Database logic
    }
}

// 3. You declare dependencies in the constructor
@Service
public class OrderService {
    private final UserRepository userRepository;
    private final EmailService emailService;
    private final PaymentService paymentService;
    
    // You DON'T create these objects - Spring will inject them
    public OrderService(UserRepository userRepository, 
                       EmailService emailService,
                       PaymentService paymentService) {
        this.userRepository = userRepository;
        this.emailService = emailService;
        this.paymentService = paymentService;
    }
    
    public void createOrder(Order order) {
        // Use the services
    }
}

@RestController
public class OrderController {
    private final OrderService orderService;
    
    // You DON'T create OrderService - Spring injects it
    public OrderController(OrderService orderService) {
        this.orderService = orderService;
    }
}

// Main application
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        // You ONLY start Spring
        SpringApplication.run(Application.class, args);
        
        // Spring does ALL of this automatically:
        // 1. Scans for @Component, @Service, @Repository, @Controller
        // 2. Creates instances of ALL beans
        // 3. Figures out the dependency tree
        // 4. Injects dependencies in the right order
        // 5. Manages lifecycle (init, destroy)
        // 6. Ensures singletons are truly single instances
    }
}
```

#### What Actually Changes? Key Differences

##### 1. **Control of Object Creation**

**Traditional:**

```java
// YOU decide when to create objects
OrderService service = new OrderService();
```

**IoC:**

```java
// SPRING decides when to create objects (usually at startup)
// You just declare: "I need an OrderService"
@Autowired
private OrderService orderService; // Spring provides it
```

##### 2. **Singleton Management**

**Traditional:**

```java
// To share the same instance, YOU must manage it manually
public class UserRepositorySingleton {
    private static UserRepository instance;
    
    public static UserRepository getInstance() {
        if (instance == null) {
            instance = new UserRepositoryImpl();
        }
        return instance;
    }
}

// Everyone must remember to use getInstance()
UserRepository repo1 = UserRepositorySingleton.getInstance();
UserRepository repo2 = UserRepositorySingleton.getInstance(); // Same instance
```

**IoC:**

```java
// Spring manages singletons automatically (default scope)
@Repository
public class UserRepositoryImpl implements UserRepository {
    // Spring ensures only ONE instance exists
}

// Everywhere you inject it, you get the SAME instance
@Service
public class OrderService {
    private final UserRepository userRepository; // Same instance
}

@Service
public class AdminService {
    private final UserRepository userRepository; // Same instance as above!
}
```

##### 3. **Testing**

**Traditional:**

```java
// Hard to test - tightly coupled to real implementation
public class OrderService {
    private UserRepository userRepository = new UserRepositoryImpl();
    
    public Order getOrder(Long id) {
        User user = userRepository.findById(id); // Always hits real DB!
        // ...
    }
}

@Test
public void testGetOrder() {
    OrderService service = new OrderService();
    // Can't inject a mock - will hit real database
    Order order = service.getOrder(1L);
}
```

**IoC:**

```java
// Easy to test - you inject mocks
@Service
public class OrderService {
    private final UserRepository userRepository;
    
    public OrderService(UserRepository userRepository) {
        this.userRepository = userRepository; // Injected dependency
    }
    
    public Order getOrder(Long id) {
        User user = userRepository.findById(id);
        // ...
    }
}

@Test
public void testGetOrder() {
    // Create a mock
    UserRepository mockRepo = mock(UserRepository.class);
    when(mockRepo.findById(1L)).thenReturn(new User("Test User"));
    
    // Inject the mock
    OrderService service = new OrderService(mockRepo);
    
    // Test without hitting real database!
    Order order = service.getOrder(1L);
    verify(mockRepo).findById(1L);
}
```

##### 4. **Switching Implementations**

**Traditional:**

```java
// To change implementation, you modify source code
public class OrderService {
    // Want to switch to PostgreSQL? Edit this line in source code!
    private UserRepository userRepository = new MySQLUserRepositoryImpl();
}
```

**IoC:**

```java
// Multiple implementations
@Repository
@Profile("dev")
public class InMemoryUserRepository implements UserRepository {
    // Fast, for development
}

@Repository
@Profile("prod")
public class PostgresUserRepository implements UserRepository {
    // Real database, for production
}

@Service
public class OrderService {
    private final UserRepository userRepository;
    
    // Spring injects the right one based on active profile
    public OrderService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
}

// Change implementation just by setting a property - NO CODE CHANGES!
// application-dev.properties: spring.profiles.active=dev
// application-prod.properties: spring.profiles.active=prod
```

##### 5. **Complex Dependency Trees**

**Traditional:**

```java
// YOU must create everything in the right order
public class Application {
    public static void main(String[] args) {
        // Layer 1
        ConnectionPool pool = new ConnectionPool("jdbc:postgresql://...");
        
        // Layer 2
        DataSource dataSource = new DataSource(pool);
        
        // Layer 3
        UserRepository userRepo = new UserRepositoryImpl(dataSource);
        EmailService emailService = new EmailServiceImpl();
        
        // Layer 4
        OrderService orderService = new OrderService(userRepo, emailService);
        
        // Layer 5
        OrderController controller = new OrderController(orderService);
        
        // If dependencies change, you manually update this entire tree!
    }
}
```

**IoC:**

```java
@Configuration
public class DatabaseConfig {
    @Bean
    public ConnectionPool connectionPool() {
        return new ConnectionPool("jdbc:postgresql://...");
    }
    
    @Bean
    public DataSource dataSource(ConnectionPool pool) {
        return new DataSource(pool); // Spring injects pool
    }
}

@Repository
public class UserRepositoryImpl implements UserRepository {
    private final DataSource dataSource;
    
    public UserRepositoryImpl(DataSource dataSource) {
        this.dataSource = dataSource; // Spring injects dataSource
    }
}

@Service
public class OrderService {
    private final UserRepository userRepository;
    private final EmailService emailService;
    
    public OrderService(UserRepository userRepository, EmailService emailService) {
        this.userRepository = userRepository; // Spring figures out the entire tree
        this.emailService = emailService;
    }
}

@RestController
public class OrderController {
    private final OrderService orderService;
    
    public OrderController(OrderService orderService) {
        this.orderService = orderService; // Spring handles everything
    }
}

// Spring automatically:
// 1. Creates ConnectionPool first
// 2. Then DataSource (injects ConnectionPool)
// 3. Then UserRepositoryImpl (injects DataSource)
// 4. Then EmailService
// 5. Then OrderService (injects UserRepository and EmailService)
// 6. Finally OrderController (injects OrderService)
```

#### Visual Comparison

**Traditional (You Control):**

```
Your Code → manually creates → Object A
Your Code → manually creates → Object B  
Your Code → manually creates → Object C
Your Code → manually wires → A needs B and C
Your Code → manages lifecycle
Your Code → ensures singleton
```

**IoC (Spring Controls):**

```
Your Code → declares dependencies with annotations
Spring Container → scans for @Component, @Service, etc.
Spring Container → creates all objects
Spring Container → figures out dependency tree
Spring Container → injects dependencies automatically
Spring Container → manages lifecycle
Spring Container → ensures singletons
```

#### The "Aha!" Moment

**The question you asked:**
> "If I need to provide a class in the constructor, isn't it equal to initializing it inside the object?"

**Answer:** The difference is WHO provides it and WHEN:

**Traditional (You provide it):**

```java
public class MyApp {
    public static void main(String[] args) {
        UserRepository repo = new UserRepositoryImpl(); // YOU create it
        OrderService service = new OrderService(repo);  // YOU pass it
    }
}
```

- YOU must create `repo` before creating `service`
- YOU must know what `OrderService` needs
- If `UserRepositoryImpl` needs something, YOU must create that too
- Every time you need `OrderService`, YOU repeat this process

**IoC (Spring provides it):**

```java
@SpringBootApplication
public class MyApp {
    public static void main(String[] args) {
        SpringApplication.run(MyApp.class, args);
        // Spring creates EVERYTHING and wires it together
    }
}

// Somewhere in your code:
@RestController
public class OrderController {
    private final OrderService orderService;
    
    // You just declare: "I need an OrderService"
    // Spring says: "I got you, here's one I already created"
    public OrderController(OrderService orderService) {
        this.orderService = orderService;
    }
}
```

- SPRING creates all objects at startup (or when first needed)
- SPRING knows what `OrderService` needs (it reads the constructor)
- SPRING creates the dependency tree automatically
- YOU never call `new` - you just declare what you need

**Benefits:**

- Loose coupling
- Easy to test (inject mocks)
- Easy to swap implementations (configuration, not code)
- Singleton management handled automatically
- Complex dependency trees resolved automatically
- Lifecycle management (initialization, cleanup)

#### Spring IoC Container Types

1. **BeanFactory**: Basic container (rarely used directly)
2. **ApplicationContext**: Advanced container (recommended)
   - `ClassPathXmlApplicationContext`
   - `AnnotationConfigApplicationContext`
   - `WebApplicationContext` (for web apps)

#### Bean Lifecycle in IoC Container

```md
1. Instantiation → Spring creates bean instance
2. Populate Properties → Dependencies injected
3. BeanNameAware.setBeanName() → If bean implements BeanNameAware
4. BeanFactoryAware.setBeanFactory() → If bean implements BeanFactoryAware
5. ApplicationContextAware.setApplicationContext() → If bean implements ApplicationContextAware
6. @PostConstruct / InitializingBean.afterPropertiesSet() → Initialization
7. Custom init-method → If specified
8. Bean Ready to Use
9. @PreDestroy / DisposableBean.destroy() → On shutdown
10. Custom destroy-method → If specified
```

**Example:**

```java
@Component
public class MyBean {
    
    @PostConstruct
    public void init() {
        System.out.println("Bean initialized");
    }
    
    @PreDestroy
    public void cleanup() {
        System.out.println("Bean destroyed");
    }
}
```

---

### Dependency Injection (DI) Deep Dive

**Dependency Injection** is the mechanism to achieve IoC. Spring injects dependencies into objects rather than objects creating their dependencies.

**Benefits** of DI:

- **Decoupling**: Reduces the dependency between classes, making the code more modular.
- **Ease of Testing**: Simplifies unit testing by allowing dependencies to be easily injected and mocked.
- **Configuration Management**: Centralizes configuration management, making it easier to change dependencies without modifying the code

#### Types of Dependency Injection

##### 1. Constructor Injection (Recommended)

```java
@Service
public class OrderService {
    
    private final UserRepository userRepository;
    private final EmailService emailService;
    
    // Spring automatically injects dependencies
    // @Autowired is optional for single constructor
    public OrderService(UserRepository userRepository, 
                       EmailService emailService) {
        this.userRepository = userRepository;
        this.emailService = emailService;
    }
}
```

**Advantages:**

- Immutability (final fields)
- Required dependencies explicit
- Easy to test
- Prevents circular dependencies

##### 2. Setter Injection

```java
@Service
public class OrderService {
    
    private UserRepository userRepository;
    
    @Autowired
    public void setUserRepository(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
}
```

**Use cases:**

- Optional dependencies
- Changing dependencies at runtime

##### 3. Field Injection (Not Recommended)

```java
@Service
public class OrderService {
    
    @Autowired
    private UserRepository userRepository;
}
```

**Problems:**

- Cannot create immutable fields
- Hard to test (requires reflection)
- Hides dependencies
- Can cause circular dependencies

#### Handling Multiple Bean Candidates

When multiple beans of the same type exist:

**Option 1: `@Primary`**

```java
@Configuration
public class DataSourceConfig {
    
    @Bean
    @Primary
    public DataSource primaryDataSource() {
        // ...
    }
    
    @Bean
    public DataSource secondaryDataSource() {
        // ...
    }
}
```

**Option 2: `@Qualifier`**

```java
@Service
public class UserService {
    
    private final DataSource dataSource;
    
    public UserService(@Qualifier("secondaryDataSource") DataSource dataSource) {
        this.dataSource = dataSource;
    }
}
```

**Option 3: Custom Qualifier Annotation**

```java
@Target({ElementType.FIELD, ElementType.PARAMETER})
@Retention(RetentionPolicy.RUNTIME)
@Qualifier
public @interface Secondary {}

@Configuration
public class DataSourceConfig {
    
    @Bean
    @Secondary
    public DataSource secondaryDataSource() {
        // ...
    }
}

@Service
public class UserService {
    
    public UserService(@Secondary DataSource dataSource) {
        this.dataSource = dataSource;
    }
}
```

#### Bean Scopes

```java
@Component
@Scope("prototype") // New instance each time
public class PrototypeBean {
    // ...
}

@Component
@Scope(value = WebApplicationContext.SCOPE_REQUEST, 
       proxyMode = ScopedProxyMode.TARGET_CLASS)
public class RequestScopedBean {
    // New instance per HTTP request
}
```

Available scopes:

- **singleton** (default): One instance per container
- **prototype**: New instance for each request
- **request**: One instance per HTTP request (web)
- **session**: One instance per HTTP session (web)
- **application**: One instance per ServletContext (web)

#### Circular Dependencies

Spring can handle circular dependencies with setter injection:

```java
@Service
public class ServiceA {
    private ServiceB serviceB;
    
    @Autowired
    public void setServiceB(ServiceB serviceB) {
        this.serviceB = serviceB;
    }
}

@Service
public class ServiceB {
    private ServiceA serviceA;
    
    @Autowired
    public void setServiceA(ServiceA serviceA) {
        this.serviceA = serviceA;
    }
}
```

**Best practice**: Avoid circular dependencies by refactoring design.

---

### Spring Boot Auto-Configuration

Spring Boot automatically configures beans based on:

1. **Classpath dependencies**
2. **Existing beans**
3. **Configuration properties**

**Example**: When you add `spring-boot-starter-data-jpa`:

- Spring Boot auto-configures: DataSource, EntityManagerFactory, TransactionManager
- Based on: Properties in `application.properties`
- Conditional on: No custom beans defined

**Disable specific auto-configurations:**

```java
@SpringBootApplication(exclude = {DataSourceAutoConfiguration.class})
public class MyApp {
    // ...
}
```

**See what's auto-configured:**

```properties
# application.properties
debug=true
```

Or use Spring Boot Actuator:

```java
GET http://localhost:8080/actuator/conditions
```

---

### Spring Boot

#### What is Spring Boot and how does it simplify Spring development?

- Spring Boot is an extension of the Spring framework that simplifies the setup and development of new Spring applications. It provides default configurations and a set of starter projects to get up and running quickly.

#### What are the advantages of using Spring Boot?

- **Standalone Applications:** Easily create standalone applications with embedded servers.
- **Auto-Configuration:** Automatically configures Spring applications based on the dependencies present.
- **Production-Ready:** Provides production-ready features like metrics, health checks, and externalized configuration.
- **Minimal Configuration:** Reduces the need for extensive XML configuration.

#### Explain the concept of Spring Boot starters

- Spring Boot starters are a set of convenient dependency descriptors that you can include in your application. They provide a curated set of dependencies for specific functionalities, such as web development, data access, and security.

#### How does Spring Boot handle dependency management?

- Spring Boot uses Maven or Gradle for dependency management. It provides a parent POM that includes managed versions of common dependencies, ensuring compatibility and reducing the need for manual version management.

#### What is the purpose of Spring Boot Actuator?

- Spring Boot Actuator provides production-ready features to help monitor and manage applications. It includes endpoints for health checks, metrics, environment information, and more.

#### How do you configure a Spring Boot application?

- Spring Boot applications can be configured using properties files (`application.properties` or `application.yml`), environment variables, and command-line arguments. Spring Boot also supports externalized configuration to allow different configurations for different environments.

#### Spring Boot Version Evolution (1.x → 2.x → 3.x)

##### Spring Boot 1.x (2014-2019)

- Based on Spring Framework 4.x
- Java 6, 7, 8 support
- XML and annotation configuration
- Servlet 3.0+ containers
- Initial auto-configuration concept

```java
// Spring Boot 1.x typical setup
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

##### Spring Boot 2.x (2018-2023)

- Based on Spring Framework 5.x
- **Java 8+ required** (17 recommended)
- Reactive support with WebFlux
- Improved actuator endpoints
- Micrometer metrics integration
- Configuration properties binding improvements

Key Changes from 1.x to 2.x:

```yaml
# application.yml - Spring Boot 2.x
spring:
  datasource:
    url: jdbc:postgresql://localhost:5432/mydb
    hikari:  # HikariCP is now default
      maximum-pool-size: 10
      
management:
  endpoints:
    web:
      exposure:
        include: health,info,metrics  # New actuator config
```

##### Spring Boot 3.x (2022-present)

- Based on Spring Framework 6.x
- **Java 17+ required** (21 recommended)
- Jakarta EE 9+ (javax → jakarta namespace)
- Native compilation with GraalVM
- Observability with Micrometer Tracing
- Problem Details (RFC 7807) support
- Virtual Threads support (Java 21)

Key Changes from 2.x to 3.x:

```java
// BEFORE (Spring Boot 2.x - javax)
import javax.persistence.Entity;
import javax.persistence.Id;
import javax.servlet.http.HttpServletRequest;

// AFTER (Spring Boot 3.x - jakarta)
import jakarta.persistence.Entity;
import jakarta.persistence.Id;
import jakarta.servlet.http.HttpServletRequest;
```

##### Version Comparison Table

| Feature | Spring Boot 1.x | Spring Boot 2.x | Spring Boot 3.x |
|---------|-----------------|-----------------|------------------|
| Spring Framework | 4.x | 5.x | 6.x |
| Minimum Java | 6 | 8 | 17 |
| Servlet API | javax.servlet | javax.servlet | jakarta.servlet |
| JPA API | javax.persistence | javax.persistence | jakarta.persistence |
| Connection Pool | Tomcat | HikariCP | HikariCP |
| Reactive | Limited | WebFlux | WebFlux |
| Native Image | No | Experimental | Production-ready |
| Observability | Basic | Micrometer | Micrometer Tracing |

#### Spring Boot 3.x Deep Dive

##### Jakarta EE 9+ Migration

All `javax.*` packages changed to `jakarta.*`:

```java
// Entity example
import jakarta.persistence.*;
import jakarta.validation.constraints.*;

@Entity
@Table(name = "users")
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @NotBlank
    @Size(min = 2, max = 100)
    private String name;
    
    @Email
    private String email;
}
```

##### Native Compilation with GraalVM

```xml
<!-- pom.xml -->
<plugin>
    <groupId>org.graalvm.buildtools</groupId>
    <artifactId>native-maven-plugin</artifactId>
</plugin>
```

```bash
# Build native image
./mvnw -Pnative native:compile

# Result: ~50ms startup time vs ~2s for JVM
```

##### Observability with Micrometer Tracing

```yaml
# application.yml
management:
  tracing:
    sampling:
      probability: 1.0
  endpoints:
    web:
      exposure:
        include: health,info,metrics,prometheus
```

```java
@RestController
public class UserController {
    
    private final ObservationRegistry observationRegistry;
    
    @GetMapping("/users/{id}")
    public User getUser(@PathVariable Long id) {
        return Observation.createNotStarted("user.get", observationRegistry)
            .observe(() -> userService.findById(id));
    }
}
```

##### Virtual Threads Support (Java 21)

```yaml
# application.yml
spring:
  threads:
    virtual:
      enabled: true  # Enable virtual threads for all request handling
```

##### Problem Details (RFC 7807)

```java
@RestControllerAdvice
public class GlobalExceptionHandler extends ResponseEntityExceptionHandler {
    
    @ExceptionHandler(UserNotFoundException.class)
    public ProblemDetail handleUserNotFound(UserNotFoundException ex) {
        ProblemDetail problemDetail = ProblemDetail.forStatusAndDetail(
            HttpStatus.NOT_FOUND, ex.getMessage());
        problemDetail.setTitle("User Not Found");
        problemDetail.setProperty("userId", ex.getUserId());
        return problemDetail;
    }
}
```

Response:

```json
{
    "type": "about:blank",
    "title": "User Not Found",
    "status": 404,
    "detail": "User with id 123 not found",
    "instance": "/users/123",
    "userId": 123
}
```

### Spring Core Modules

#### Spring Core Container

The foundation of Spring Framework:

- **spring-core**: Core utilities, including the IoC container
- **spring-beans**: Bean factory and configuration
- **spring-context**: Application context, events, i18n
- **spring-expression**: Spring Expression Language (SpEL)

```java
// SpEL Example
@Value("#{systemProperties['user.home']}")
private String userHome;

@Value("#{T(java.lang.Math).random() * 100}")
private double randomNumber;

@Value("#{userService.findById(1)?.name ?: 'Unknown'}")
private String userName;
```

#### Bean Scopes

| Scope | Description |
|-------|-------------|
| singleton | One instance per Spring container (default) |
| prototype | New instance each time bean is requested |
| request | One instance per HTTP request (web) |
| session | One instance per HTTP session (web) |
| application | One instance per ServletContext (web) |

```java
@Component
@Scope("prototype")
public class PrototypeBean {
    // New instance created for each injection
}

@Component
@Scope(value = WebApplicationContext.SCOPE_REQUEST, proxyMode = ScopedProxyMode.TARGET_CLASS)
public class RequestScopedBean {
    // New instance for each HTTP request
}
```

#### Bean Lifecycle

```java
@Component
public class MyBean implements InitializingBean, DisposableBean {
    
    @PostConstruct
    public void postConstruct() {
        // Called after dependency injection
    }
    
    @Override
    public void afterPropertiesSet() {
        // Called after properties are set
    }
    
    @PreDestroy
    public void preDestroy() {
        // Called before bean destruction
    }
    
    @Override
    public void destroy() {
        // Called when container is destroyed
    }
}
```

#### Conditional Beans

```java
@Configuration
public class DatabaseConfig {
    
    @Bean
    @ConditionalOnProperty(name = "db.type", havingValue = "postgres")
    public DataSource postgresDataSource() {
        return new PostgresDataSource();
    }
    
    @Bean
    @ConditionalOnProperty(name = "db.type", havingValue = "mysql")
    public DataSource mysqlDataSource() {
        return new MysqlDataSource();
    }
    
    @Bean
    @ConditionalOnMissingBean(DataSource.class)
    public DataSource defaultDataSource() {
        return new H2DataSource();
    }
}
```

#### Profiles

```java
@Configuration
@Profile("development")
public class DevConfig {
    @Bean
    public DataSource dataSource() {
        return new EmbeddedDatabaseBuilder()
            .setType(EmbeddedDatabaseType.H2)
            .build();
    }
}

@Configuration
@Profile("production")
public class ProdConfig {
    @Bean
    public DataSource dataSource() {
        // Production database configuration
    }
}
```

```yaml
# application.yml
spring:
  profiles:
    active: development
    
---
spring:
  config:
    activate:
      on-profile: development
  datasource:
    url: jdbc:h2:mem:testdb
    
---
spring:
  config:
    activate:
      on-profile: production
  datasource:
    url: jdbc:postgresql://prod-server:5432/mydb
```

### Spring Security

#### Basic Configuration (Spring Boot 3.x)

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {
    
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        return http
            .csrf(csrf -> csrf.disable())
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/api/public/**").permitAll()
                .requestMatchers("/api/admin/**").hasRole("ADMIN")
                .requestMatchers("/api/**").authenticated()
                .anyRequest().authenticated()
            )
            .sessionManagement(session -> session
                .sessionCreationPolicy(SessionCreationPolicy.STATELESS)
            )
            .oauth2ResourceServer(oauth2 -> oauth2.jwt(Customizer.withDefaults()))
            .build();
    }
    
    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
}
```

#### JWT Authentication

```java
@Component
public class JwtTokenProvider {
    
    @Value("${jwt.secret}")
    private String jwtSecret;
    
    @Value("${jwt.expiration}")
    private long jwtExpiration;
    
    public String generateToken(Authentication authentication) {
        UserDetails userDetails = (UserDetails) authentication.getPrincipal();
        Date now = new Date();
        Date expiryDate = new Date(now.getTime() + jwtExpiration);
        
        return Jwts.builder()
            .setSubject(userDetails.getUsername())
            .setIssuedAt(now)
            .setExpiration(expiryDate)
            .signWith(SignatureAlgorithm.HS512, jwtSecret)
            .compact();
    }
    
    public boolean validateToken(String token) {
        try {
            Jwts.parser().setSigningKey(jwtSecret).parseClaimsJws(token);
            return true;
        } catch (JwtException | IllegalArgumentException e) {
            return false;
        }
    }
}
```

#### Method-Level Security

```java
@Service
public class UserService {
    
    @PreAuthorize("hasRole('ADMIN')")
    public void deleteUser(Long id) {
        // Only admins can delete
    }
    
    @PreAuthorize("#userId == authentication.principal.id or hasRole('ADMIN')")
    public User getUser(Long userId) {
        // Users can only get their own data, or admins can get any
    }
    
    @PostAuthorize("returnObject.owner == authentication.principal.username")
    public Document getDocument(Long id) {
        // Filter returned object based on ownership
    }
}
```

### Spring Data

#### Spring Data JPA

```java
@Entity
@Table(name = "users")
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String name;
    private String email;
    
    @OneToMany(mappedBy = "user", cascade = CascadeType.ALL, fetch = FetchType.LAZY)
    private List<Order> orders;
}

public interface UserRepository extends JpaRepository<User, Long> {
    
    // Query methods
    Optional<User> findByEmail(String email);
    List<User> findByNameContainingIgnoreCase(String name);
    
    // Custom query
    @Query("SELECT u FROM User u WHERE u.email LIKE %:domain")
    List<User> findByEmailDomain(@Param("domain") String domain);
    
    // Native query
    @Query(value = "SELECT * FROM users WHERE created_at > :date", nativeQuery = true)
    List<User> findRecentUsers(@Param("date") LocalDateTime date);
    
    // Modifying query
    @Modifying
    @Query("UPDATE User u SET u.active = false WHERE u.lastLogin < :date")
    int deactivateInactiveUsers(@Param("date") LocalDateTime date);
}
```

#### Specifications (Dynamic Queries)

```java
public class UserSpecifications {
    
    public static Specification<User> hasName(String name) {
        return (root, query, cb) -> 
            name == null ? null : cb.like(cb.lower(root.get("name")), "%" + name.toLowerCase() + "%");
    }
    
    public static Specification<User> hasEmail(String email) {
        return (root, query, cb) -> 
            email == null ? null : cb.equal(root.get("email"), email);
    }
    
    public static Specification<User> isActive() {
        return (root, query, cb) -> cb.isTrue(root.get("active"));
    }
}

// Usage
List<User> users = userRepository.findAll(
    Specification.where(UserSpecifications.hasName("Mario"))
        .and(UserSpecifications.isActive())
);
```

#### Pagination and Sorting

```java
@GetMapping("/users")
public Page<User> getUsers(
    @RequestParam(defaultValue = "0") int page,
    @RequestParam(defaultValue = "10") int size,
    @RequestParam(defaultValue = "name") String sortBy,
    @RequestParam(defaultValue = "asc") String direction
) {
    Sort sort = direction.equalsIgnoreCase("desc") 
        ? Sort.by(sortBy).descending() 
        : Sort.by(sortBy).ascending();
    
    Pageable pageable = PageRequest.of(page, size, sort);
    return userRepository.findAll(pageable);
}
```

### Spring WebFlux (Reactive)

#### Reactive Controller

```java
@RestController
@RequestMapping("/api/users")
public class ReactiveUserController {
    
    private final ReactiveUserRepository userRepository;
    
    @GetMapping
    public Flux<User> getAllUsers() {
        return userRepository.findAll();
    }
    
    @GetMapping("/{id}")
    public Mono<ResponseEntity<User>> getUser(@PathVariable String id) {
        return userRepository.findById(id)
            .map(ResponseEntity::ok)
            .defaultIfEmpty(ResponseEntity.notFound().build());
    }
    
    @PostMapping
    public Mono<User> createUser(@RequestBody User user) {
        return userRepository.save(user);
    }
    
    @GetMapping(value = "/stream", produces = MediaType.TEXT_EVENT_STREAM_VALUE)
    public Flux<User> streamUsers() {
        return userRepository.findAll()
            .delayElements(Duration.ofSeconds(1));
    }
}
```

#### Reactive Repository (MongoDB)

```java
public interface ReactiveUserRepository extends ReactiveMongoRepository<User, String> {
    Flux<User> findByName(String name);
    Mono<User> findByEmail(String email);
}
```

#### WebClient (Reactive HTTP Client)

```java
@Service
public class ExternalApiService {
    
    private final WebClient webClient;
    
    public ExternalApiService(WebClient.Builder builder) {
        this.webClient = builder
            .baseUrl("https://api.external.com")
            .build();
    }
    
    public Mono<ExternalData> fetchData(String id) {
        return webClient.get()
            .uri("/data/{id}", id)
            .retrieve()
            .onStatus(HttpStatusCode::is4xxClientError, 
                response -> Mono.error(new NotFoundException()))
            .bodyToMono(ExternalData.class)
            .timeout(Duration.ofSeconds(5))
            .retry(3);
    }
    
    public Flux<ExternalData> fetchAllData() {
        return webClient.get()
            .uri("/data")
            .retrieve()
            .bodyToFlux(ExternalData.class);
    }
}
```

## SQL

### What is SQL?

SQL (Structured Query Language) is a standard language for managing and manipulating relational databases. It is used to perform tasks such as querying data, updating records, and managing database structures.

### What are the different types of SQL commands?

The different types of SQL commands include:

- **DDL (Data Definition Language):** Commands like `CREATE`, `ALTER`, `DROP`.
- **DML (Data Manipulation Language):** Commands like `SELECT`, `INSERT`, `UPDATE`, `DELETE`.
- **DCL (Data Control Language):** Commands like `GRANT`, `REVOKE`.
- **TCL (Transaction Control Language):** Commands like `COMMIT`, `ROLLBACK`.

### What is a JOIN in SQL?

A JOIN is a SQL operation used to combine rows from two or more tables based on a related column between them. Types of JOINs include INNER JOIN, LEFT JOIN, RIGHT JOIN, and FULL JOIN.

### Panoramica SQL

#### DDL (Data Definition Language)

##### CREATE TABLE

```sql
CREATE TABLE employees (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE,
    department_id INT,
    salary DECIMAL(10, 2),
    hire_date DATE,
    FOREIGN KEY (department_id) REFERENCES departments(id)
);
```

##### ALTER TABLE

```sql
-- Aggiungere una colonna
ALTER TABLE employees ADD COLUMN phone VARCHAR(20);

-- Modificare una colonna
ALTER TABLE employees MODIFY COLUMN name VARCHAR(150);

-- Eliminare una colonna
ALTER TABLE employees DROP COLUMN phone;
```

##### DROP TABLE

```sql
DROP TABLE employees;
```

##### TRUNCATE TABLE

Rimuove tutti i dati da una tabella ma ne mantiene la struttura. È più veloce di DELETE e resetta gli indici auto-incrementali.

```sql
TRUNCATE TABLE employees;
```

##### CREATE INDEX

```sql
CREATE INDEX idx_employee_name ON employees(name);
CREATE INDEX idx_dept_salary ON employees(department_id, salary);
```

---

#### DML (Data Manipulation Language)

##### SELECT

```sql
-- Base
SELECT name, salary FROM employees;

-- Con condizioni
SELECT * FROM employees WHERE salary > 50000;

-- Con ordinamento
SELECT * FROM employees ORDER BY salary DESC, name ASC;

-- Con limite
SELECT * FROM employees LIMIT 10 OFFSET 20;

-- Distinct
SELECT DISTINCT department_id FROM employees;
```

##### INSERT

```sql
-- Singolo record
INSERT INTO employees (name, email, salary) 
VALUES ('Mario Rossi', 'mario@email.com', 45000);

-- Multiple righe
INSERT INTO employees (name, email, salary) VALUES
    ('Luigi Verdi', 'luigi@email.com', 48000),
    ('Anna Bianchi', 'anna@email.com', 52000);
```

##### UPDATE

```sql
UPDATE employees 
SET salary = salary * 1.1 
WHERE department_id = 3;

-- Update multipli campi
UPDATE employees 
SET salary = 55000, department_id = 5 
WHERE id = 10;
```

##### DELETE

```sql
DELETE FROM employees WHERE id = 10;

-- Delete multipli
DELETE FROM employees WHERE hire_date < '2020-01-01';
```

---

#### JOIN

##### INNER JOIN

Restituisce solo le righe che hanno corrispondenza in entrambe le tabelle.

```sql
SELECT e.name, e.salary, d.department_name
FROM employees e
INNER JOIN departments d ON e.department_id = d.id;
```

##### LEFT JOIN (LEFT OUTER JOIN)

Restituisce tutte le righe della tabella sinistra + corrispondenze dalla destra.

```sql
SELECT e.name, d.department_name
FROM employees e
LEFT JOIN departments d ON e.department_id = d.id;
-- Include anche dipendenti senza dipartimento (NULL)
```

##### RIGHT JOIN

Restituisce tutte le righe della tabella destra + corrispondenze dalla sinistra.

```sql
SELECT e.name, d.department_name
FROM employees e
RIGHT JOIN departments d ON e.department_id = d.id;
-- Include anche dipartimenti senza dipendenti
```

##### FULL OUTER JOIN

Restituisce tutte le righe di entrambe le tabelle (non supportato in MySQL, usa UNION).

```sql
SELECT e.name, d.department_name
FROM employees e
FULL OUTER JOIN departments d ON e.department_id = d.id;
```

##### CROSS JOIN

Prodotto cartesiano (ogni riga di A con ogni riga di B).

```sql
SELECT e.name, d.department_name
FROM employees e
CROSS JOIN departments d;
```

##### SELF JOIN

Join di una tabella con se stessa.

```sql
-- Trovare dipendenti con lo stesso manager
SELECT e1.name AS employee, e2.name AS colleague
FROM employees e1
INNER JOIN employees e2 ON e1.manager_id = e2.manager_id
WHERE e1.id != e2.id;
```

---

#### Funzioni Aggregate

```sql
-- COUNT
SELECT COUNT(*) FROM employees;
SELECT COUNT(DISTINCT department_id) FROM employees;

-- SUM
SELECT SUM(salary) FROM employees;

-- AVG
SELECT AVG(salary) FROM employees;

-- MIN / MAX
SELECT MIN(salary), MAX(salary) FROM employees;
```

##### COUNT(*) vs COUNT(column)

`COUNT(*)`: Conta tutte le righe, incluse quelle con valori NULL in qualsiasi colonna.

`COUNT(column)`: Conta solo le righe dove quella colonna specifica NON è NULL.

---

#### GROUP BY e HAVING

##### GROUP BY

Raggruppa righe con valori comuni.

```sql
-- Contare dipendenti per dipartimento
SELECT department_id, COUNT(*) as num_employees
FROM employees
GROUP BY department_id;

-- Salario medio per dipartimento
SELECT department_id, AVG(salary) as avg_salary
FROM employees
GROUP BY department_id;

-- Raggruppamento multiplo
SELECT department_id, YEAR(hire_date), COUNT(*)
FROM employees
GROUP BY department_id, YEAR(hire_date);
```

##### HAVING

Filtra i risultati dopo il GROUP BY (WHERE filtra prima).

```sql
-- Dipartimenti con più di 5 dipendenti
SELECT department_id, COUNT(*) as num_employees
FROM employees
GROUP BY department_id
HAVING COUNT(*) > 5;

-- Dipartimenti con salario medio > 50000
SELECT department_id, AVG(salary) as avg_salary
FROM employees
GROUP BY department_id
HAVING AVG(salary) > 50000;
```

**Ordine di esecuzione**: WHERE → GROUP BY → HAVING → ORDER BY

---

#### Subquery

##### Subquery nella WHERE

```sql
-- Dipendenti con salario sopra la media
SELECT name, salary
FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);

-- Dipendenti nello stesso dipartimento di 'Mario'
SELECT name
FROM employees
WHERE department_id = (
    SELECT department_id FROM employees WHERE name = 'Mario'
);
```

##### Subquery con IN

```sql
-- Dipendenti in dipartimenti con budget > 100000
SELECT name
FROM employees
WHERE department_id IN (
    SELECT id FROM departments WHERE budget > 100000
);
```

##### Subquery con EXISTS

```sql
-- Dipartimenti che hanno almeno un dipendente
SELECT d.department_name
FROM departments d
WHERE EXISTS (
    SELECT 1 FROM employees e WHERE e.department_id = d.id
);
```

##### Subquery nella SELECT

```sql
SELECT 
    name,
    salary,
    (SELECT AVG(salary) FROM employees) as avg_salary
FROM employees;
```

##### Subquery nella FROM (Derived Table)

```sql
SELECT dept_avg.department_id, dept_avg.avg_salary
FROM (
    SELECT department_id, AVG(salary) as avg_salary
    FROM employees
    GROUP BY department_id
) as dept_avg
WHERE dept_avg.avg_salary > 50000;
```

---

#### Window Functions (OVER)

Le window functions eseguono calcoli su un set di righe correlate senza collassarle.

##### ROW_NUMBER()

Assegna un numero progressivo.

```sql
SELECT 
    name,
    salary,
    ROW_NUMBER() OVER (ORDER BY salary DESC) as rank
FROM employees;
```

##### RANK() e DENSE_RANK()

```sql
-- RANK: salta numeri dopo i tie (1,2,2,4)
SELECT 
    name,
    salary,
    RANK() OVER (ORDER BY salary DESC) as rank
FROM employees;

-- DENSE_RANK: non salta numeri (1,2,2,3)
SELECT 
    name,
    salary,
    DENSE_RANK() OVER (ORDER BY salary DESC) as rank
FROM employees;
```

##### PARTITION BY

Divide i dati in gruppi per calcoli separati.

```sql
-- Ranking per dipartimento
SELECT 
    name,
    department_id,
    salary,
    RANK() OVER (PARTITION BY department_id ORDER BY salary DESC) as dept_rank
FROM employees;
```

##### LAG() e LEAD()

Accedono a righe precedenti o successive.

```sql
-- Confrontare con il salario precedente
SELECT 
    name,
    salary,
    LAG(salary) OVER (ORDER BY hire_date) as previous_salary,
    LEAD(salary) OVER (ORDER BY hire_date) as next_salary
FROM employees;
```

##### Funzioni aggregate con OVER

```sql
-- Running total
SELECT 
    name,
    salary,
    SUM(salary) OVER (ORDER BY hire_date) as running_total
FROM employees;

-- Media mobile
SELECT 
    name,
    salary,
    AVG(salary) OVER (
        ORDER BY hire_date 
        ROWS BETWEEN 2 PRECEDING AND CURRENT ROW
    ) as moving_avg
FROM employees;
```

---

#### CASE (Conditional Logic)

```sql
-- Case semplice
SELECT 
    name,
    salary,
    CASE 
        WHEN salary < 40000 THEN 'Low'
        WHEN salary < 60000 THEN 'Medium'
        ELSE 'High'
    END as salary_category
FROM employees;

-- Case con aggregate
SELECT 
    department_id,
    COUNT(CASE WHEN salary > 50000 THEN 1 END) as high_earners
FROM employees
GROUP BY department_id;
```

---

#### UNION / UNION ALL

```sql
-- UNION: rimuove duplicati
SELECT name FROM employees_2023
UNION
SELECT name FROM employees_2024;

-- UNION ALL: mantiene duplicati (più veloce)
SELECT name FROM employees_2023
UNION ALL
SELECT name FROM employees_2024;
```

---

#### CTE (Common Table Expressions)

```sql
-- CTE singola
WITH dept_avg AS (
    SELECT department_id, AVG(salary) as avg_salary
    FROM employees
    GROUP BY department_id
)
SELECT e.name, e.salary, da.avg_salary
FROM employees e
JOIN dept_avg da ON e.department_id = da.department_id
WHERE e.salary > da.avg_salary;

-- CTE ricorsiva (esempio: gerarchia)
WITH RECURSIVE employee_hierarchy AS (
    -- Base case
    SELECT id, name, manager_id, 1 as level
    FROM employees
    WHERE manager_id IS NULL
    
    UNION ALL
    
    -- Recursive case
    SELECT e.id, e.name, e.manager_id, eh.level + 1
    FROM employees e
    JOIN employee_hierarchy eh ON e.manager_id = eh.id
)
SELECT * FROM employee_hierarchy;
```

## NoSQL

### What is NoSQL?

NoSQL (Not Only SQL) databases provide a mechanism for storage and retrieval of data that is modeled in means other than the tabular relations used in relational databases. They are designed to handle large volumes of unstructured or semi-structured data.

### What are the types of NoSQL databases?

The types of NoSQL databases include:

- **Document-Oriented:** Stores data as documents (e.g., MongoDB).
- **Key-Value:** Stores data as key-value pairs (e.g., Redis).
- **Column-Family:** Stores data in columns rather than rows (e.g., Cassandra).
- **Graph:** Stores data in graph structures with nodes and edges (e.g., Neo4j).

### What is the difference between SQL and NoSQL databases?

The main differences between SQL and NoSQL databases are:

- **Data Model:** SQL databases use a structured, tabular data model, while NoSQL databases use various data models (document, key-value, column-family, graph).
- **Schema:** SQL databases have a fixed schema, while NoSQL databases have a flexible schema.
- **Scalability:** SQL databases are vertically scalable, while NoSQL databases are horizontally scalable.
- **Use Cases:** SQL databases are suited for complex queries and transactions, while NoSQL databases are suited for large-scale, distributed data storage.

## MongoDB

### What is MongoDB?

MongoDB is a **document-oriented NoSQL database** that stores data in flexible, JSON-like documents called BSON (Binary JSON). It's designed for scalability, high availability, and high performance.

**Key Characteristics:**

- **Document Model**: Stores data in flexible documents instead of rows and columns
- **Schema-less**: Documents in a collection can have different structures
- **Horizontal Scaling**: Built-in sharding for distributing data across multiple servers
- **Rich Query Language**: Supports complex queries, aggregations, and indexing
- **High Availability**: Replica sets provide automatic failover

### MongoDB Architecture

#### Basic Concepts

| SQL Concept | MongoDB Equivalent |
|-------------|--------------------|
| Database | Database |
| Table | Collection |
| Row | Document |
| Column | Field |
| Primary Key | _id |
| Index | Index |
| JOIN | $lookup (Aggregation) |

#### Document Structure

```javascript
// MongoDB Document (BSON)
{
    "_id": ObjectId("507f1f77bcf86cd799439011"),
    "name": "Mario Rossi",
    "email": "mario@example.com",
    "age": 30,
    "address": {
        "street": "Via Roma 1",
        "city": "Rome",
        "country": "Italy"
    },
    "hobbies": ["reading", "gaming", "cooking"],
    "orders": [
        { "orderId": 1001, "total": 150.00, "date": ISODate("2024-01-15") },
        { "orderId": 1002, "total": 75.50, "date": ISODate("2024-02-20") }
    ],
    "createdAt": ISODate("2024-01-01T10:00:00Z"),
    "active": true
}
```

#### Data Types

| Type | Description | Example |
|------|-------------|----------|
| String | UTF-8 string | `"Hello"` |
| Number | Integer or Double | `42`, `3.14` |
| Boolean | true/false | `true` |
| Date | Date/time | `ISODate("2024-01-01")` |
| ObjectId | 12-byte unique ID | `ObjectId("507f1f77bcf86cd799439011")` |
| Array | List of values | `[1, 2, 3]` |
| Object | Embedded document | `{"key": "value"}` |
| Null | Null value | `null` |
| Binary | Binary data | Binary files |

### CRUD Operations

#### Create (Insert)

```javascript
// Insert one document
db.users.insertOne({
    name: "Mario Rossi",
    email: "mario@example.com",
    age: 30
});

// Insert multiple documents
db.users.insertMany([
    { name: "Luigi Verdi", email: "luigi@example.com", age: 25 },
    { name: "Anna Bianchi", email: "anna@example.com", age: 35 }
]);
```

##### Read (Query)

```javascript
// Find all documents
db.users.find();

// Find with filter
db.users.find({ age: { $gte: 25 } });

// Find one document
db.users.findOne({ email: "mario@example.com" });

// Projection (select specific fields)
db.users.find(
    { age: { $gte: 25 } },
    { name: 1, email: 1, _id: 0 }  // 1 = include, 0 = exclude
);

// Query operators
db.users.find({
    $and: [
        { age: { $gte: 18 } },
        { age: { $lte: 65 } }
    ]
});

db.users.find({
    $or: [
        { city: "Rome" },
        { city: "Milan" }
    ]
});

// Array queries
db.users.find({ hobbies: "gaming" });  // Contains "gaming"
db.users.find({ hobbies: { $all: ["reading", "gaming"] } });  // Contains all
db.users.find({ hobbies: { $size: 3 } });  // Array has exactly 3 elements

// Nested document queries
db.users.find({ "address.city": "Rome" });

// Sorting and limiting
db.users.find().sort({ age: -1 }).limit(10).skip(20);
```

##### Update

```javascript
// Update one document
db.users.updateOne(
    { email: "mario@example.com" },
    { $set: { age: 31, updatedAt: new Date() } }
);

// Update multiple documents
db.users.updateMany(
    { active: false },
    { $set: { status: "inactive" } }
);

// Update operators
db.users.updateOne(
    { _id: ObjectId("...") },
    {
        $set: { name: "Mario" },           // Set field value
        $unset: { tempField: "" },         // Remove field
        $inc: { loginCount: 1 },           // Increment
        $push: { hobbies: "swimming" },    // Add to array
        $pull: { hobbies: "gaming" },      // Remove from array
        $addToSet: { tags: "premium" }     // Add to array if not exists
    }
);

// Replace entire document
db.users.replaceOne(
    { email: "mario@example.com" },
    { name: "Mario Rossi", email: "mario@example.com", age: 31 }
);

// Upsert (insert if not exists)
db.users.updateOne(
    { email: "new@example.com" },
    { $set: { name: "New User", age: 25 } },
    { upsert: true }
);
```

#### Delete

```javascript
// Delete one document
db.users.deleteOne({ email: "mario@example.com" });

// Delete multiple documents
db.users.deleteMany({ active: false });

// Delete all documents in collection
db.users.deleteMany({});
```

### Aggregation Pipeline

The aggregation pipeline processes documents through stages, each transforming the data.

#### Basic Aggregation

```javascript
db.orders.aggregate([
    // Stage 1: Filter
    { $match: { status: "completed" } },
    
    // Stage 2: Group and calculate
    { $group: {
        _id: "$customerId",
        totalSpent: { $sum: "$amount" },
        orderCount: { $sum: 1 },
        avgOrderValue: { $avg: "$amount" }
    }},
    
    // Stage 3: Sort
    { $sort: { totalSpent: -1 } },
    
    // Stage 4: Limit
    { $limit: 10 }
]);
```

#### Common Aggregation Stages

```javascript
// $project - Select/reshape fields
{ $project: {
    fullName: { $concat: ["$firstName", " ", "$lastName"] },
    year: { $year: "$createdAt" },
    totalWithTax: { $multiply: ["$total", 1.22] }
}}

// $lookup - Join with another collection
{ $lookup: {
    from: "orders",
    localField: "_id",
    foreignField: "userId",
    as: "userOrders"
}}

// $unwind - Deconstruct array field
{ $unwind: "$items" }

// $addFields - Add new fields
{ $addFields: {
    totalPrice: { $multiply: ["$quantity", "$price"] }
}}

// $bucket - Group into ranges
{ $bucket: {
    groupBy: "$age",
    boundaries: [0, 18, 30, 50, 100],
    default: "Other",
    output: { count: { $sum: 1 } }
}}

// $facet - Multiple pipelines in parallel
{ $facet: {
    "byStatus": [{ $group: { _id: "$status", count: { $sum: 1 } } }],
    "byCategory": [{ $group: { _id: "$category", total: { $sum: "$amount" } } }],
    "topProducts": [{ $sort: { sales: -1 } }, { $limit: 5 }]
}}
```

#### Advanced Aggregation Example

```javascript
// Sales report by month with comparison to previous month
db.sales.aggregate([
    { $match: { date: { $gte: ISODate("2024-01-01") } } },
    
    { $group: {
        _id: {
            year: { $year: "$date" },
            month: { $month: "$date" }
        },
        totalSales: { $sum: "$amount" },
        avgSale: { $avg: "$amount" },
        count: { $sum: 1 }
    }},
    
    { $sort: { "_id.year": 1, "_id.month": 1 } },
    
    { $setWindowFields: {
        sortBy: { "_id.year": 1, "_id.month": 1 },
        output: {
            prevMonthSales: {
                $shift: { output: "$totalSales", by: -1 }
            }
        }
    }},
    
    { $addFields: {
        growthRate: {
            $cond: {
                if: { $eq: ["$prevMonthSales", null] },
                then: null,
                else: {
                    $multiply: [
                        { $divide: [
                            { $subtract: ["$totalSales", "$prevMonthSales"] },
                            "$prevMonthSales"
                        ]},
                        100
                    ]
                }
            }
        }
    }}
]);
```

### Indexing in MongoDB

#### Index Types

```javascript
// Single field index
db.users.createIndex({ email: 1 });  // 1 = ascending, -1 = descending

// Compound index
db.users.createIndex({ lastName: 1, firstName: 1 });

// Unique index
db.users.createIndex({ email: 1 }, { unique: true });

// Text index (for full-text search)
db.articles.createIndex({ title: "text", content: "text" });
db.articles.find({ $text: { $search: "mongodb tutorial" } });

// TTL index (auto-delete after time)
db.sessions.createIndex({ createdAt: 1 }, { expireAfterSeconds: 3600 });

// Partial index
db.users.createIndex(
    { email: 1 },
    { partialFilterExpression: { active: true } }
);

// Sparse index (only documents with field)
db.users.createIndex({ phone: 1 }, { sparse: true });
```

#### Index Management

```javascript
// List indexes
db.users.getIndexes();

// Drop index
db.users.dropIndex("email_1");

// Drop all indexes (except _id)
db.users.dropIndexes();

// Explain query (check index usage)
db.users.find({ email: "test@example.com" }).explain("executionStats");
```

### MongoDB with Spring Boot

#### Dependencies (pom.xml)

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-mongodb</artifactId>
</dependency>

<!-- For reactive -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-mongodb-reactive</artifactId>
</dependency>
```

#### Configuration

```yaml
# application.yml
spring:
  data:
    mongodb:
      uri: mongodb://localhost:27017/mydb
      # Or individual properties:
      # host: localhost
      # port: 27017
      # database: mydb
      # username: user
      # password: pass
      # authentication-database: admin
```

#### Document Entity

```java
import org.springframework.data.annotation.Id;
import org.springframework.data.mongodb.core.mapping.Document;
import org.springframework.data.mongodb.core.mapping.Field;
import org.springframework.data.mongodb.core.index.Indexed;
import org.springframework.data.mongodb.core.index.CompoundIndex;

@Document(collection = "users")
@CompoundIndex(name = "name_email_idx", def = "{'name': 1, 'email': 1}")
public class User {
    
    @Id
    private String id;
    
    private String name;
    
    @Indexed(unique = true)
    private String email;
    
    private Integer age;
    
    @Field("addr")  // Custom field name in MongoDB
    private Address address;
    
    private List<String> hobbies;
    
    private LocalDateTime createdAt;
    
    // Getters and setters
}

public class Address {
    private String street;
    private String city;
    private String country;
}
```

#### Repository

```java
import org.springframework.data.mongodb.repository.MongoRepository;
import org.springframework.data.mongodb.repository.Query;
import org.springframework.data.mongodb.repository.Aggregation;

public interface UserRepository extends MongoRepository<User, String> {
    
    // Query methods
    Optional<User> findByEmail(String email);
    List<User> findByAgeGreaterThan(int age);
    List<User> findByAddressCity(String city);
    List<User> findByHobbiesContaining(String hobby);
    
    // Custom JSON query
    @Query("{ 'age': { $gte: ?0, $lte: ?1 } }")
    List<User> findByAgeBetween(int minAge, int maxAge);
    
    // Query with projection
    @Query(value = "{ 'active': true }", fields = "{ 'name': 1, 'email': 1 }")
    List<User> findActiveUsersProjected();
    
    // Aggregation
    @Aggregation(pipeline = {
        "{ $match: { 'active': true } }",
        "{ $group: { _id: '$address.city', count: { $sum: 1 } } }",
        "{ $sort: { count: -1 } }"
    })
    List<CityCount> countUsersByCity();
}

// For aggregation result
public record CityCount(String id, long count) {}
```

#### MongoTemplate for Complex Queries

```java
import org.springframework.data.mongodb.core.MongoTemplate;
import org.springframework.data.mongodb.core.query.Query;
import org.springframework.data.mongodb.core.query.Criteria;
import org.springframework.data.mongodb.core.query.Update;
import org.springframework.data.mongodb.core.aggregation.Aggregation;

@Service
public class UserService {
    
    private final MongoTemplate mongoTemplate;
    
    public List<User> findUsersByComplexCriteria(String city, int minAge, List<String> hobbies) {
        Query query = new Query();
        
        query.addCriteria(Criteria.where("address.city").is(city));
        query.addCriteria(Criteria.where("age").gte(minAge));
        query.addCriteria(Criteria.where("hobbies").all(hobbies));
        query.with(Sort.by(Sort.Direction.DESC, "createdAt"));
        query.limit(20);
        
        return mongoTemplate.find(query, User.class);
    }
    
    public void updateUserAge(String id, int newAge) {
        Query query = Query.query(Criteria.where("_id").is(id));
        Update update = new Update()
            .set("age", newAge)
            .set("updatedAt", LocalDateTime.now());
        
        mongoTemplate.updateFirst(query, update, User.class);
    }
    
    public List<AggregationResult> aggregateUserStats() {
        Aggregation aggregation = Aggregation.newAggregation(
            Aggregation.match(Criteria.where("active").is(true)),
            Aggregation.group("address.city")
                .count().as("userCount")
                .avg("age").as("avgAge"),
            Aggregation.sort(Sort.Direction.DESC, "userCount"),
            Aggregation.limit(10)
        );
        
        return mongoTemplate.aggregate(
            aggregation, "users", AggregationResult.class
        ).getMappedResults();
    }
}
```

#### Reactive MongoDB

```java
import org.springframework.data.mongodb.repository.ReactiveMongoRepository;
import reactor.core.publisher.Flux;
import reactor.core.publisher.Mono;

public interface ReactiveUserRepository extends ReactiveMongoRepository<User, String> {
    Flux<User> findByAgeGreaterThan(int age);
    Mono<User> findByEmail(String email);
}

@RestController
@RequestMapping("/api/users")
public class ReactiveUserController {
    
    private final ReactiveUserRepository repository;
    
    @GetMapping(produces = MediaType.TEXT_EVENT_STREAM_VALUE)
    public Flux<User> streamAllUsers() {
        return repository.findAll()
            .delayElements(Duration.ofMillis(100));
    }
    
    @GetMapping("/{id}")
    public Mono<ResponseEntity<User>> getUser(@PathVariable String id) {
        return repository.findById(id)
            .map(ResponseEntity::ok)
            .defaultIfEmpty(ResponseEntity.notFound().build());
    }
}
```

#### MongoDB Transactions (Replica Set Required)

```java
@Service
public class OrderService {
    
    private final MongoTemplate mongoTemplate;
    
    @Transactional
    public void createOrder(Order order) {
        // All operations in a single transaction
        mongoTemplate.insert(order);
        mongoTemplate.updateFirst(
            Query.query(Criteria.where("_id").is(order.getUserId())),
            new Update().inc("orderCount", 1),
            User.class
        );
    }
}
```

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

Let me know if you wanna go deeper into token security or see a live example!