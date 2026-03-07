# Spring & Hibernate Q&A

## Table of Contents

1. [Hibernate](#hibernate)
   - [What is Hibernate and why is it used?](#what-is-hibernate-and-why-is-it-used)
   - [Lazy Loading](#explain-the-concept-of-lazy-loading-in-hibernate)
   - [Caching](#caching-in-hibernate)
   - [N+1 Problem](#n1-select-problem)
2. [Spring](#spring)
   - [What is Spring Framework?](#what-is-spring-framework)
   - [Dependency Injection in Spring](#what-is-dependency-injection-in-spring)
   - [Inversion of Control (IoC)](#inversion-of-control-ioc)
   - [Creating a Spring Boot Project from Scratch](#creating-a-springspring-boot-project-from-scratch)
   - [Integrating Hibernate/JPA with Spring Boot](#integrating-hibernatejpa-with-spring-boot)
   - [Configuration Files Discovery](#configuration-files-discovery)
   - [Most Common Spring Annotations](#most-common-spring-annotations)
   - [IoC Deep Dive](#inversion-of-control-ioc-deep-dive)
   - [Dependency Injection Deep Dive](#dependency-injection-di-deep-dive)
   - [Spring Boot Auto-Configuration](#spring-boot-auto-configuration)
   - [Spring Boot](#spring-boot)
   - [Spring Core Modules](#spring-core-modules)
   - [Spring Security](#spring-security)
   - [Spring Data](#spring-data)
   - [Spring WebFlux (Reactive)](#spring-webflux-reactive)

---

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

