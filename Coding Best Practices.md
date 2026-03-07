# Coding Best Practices & Design Patterns

## Table of Contents

1. [SOLID Principles](#solid-principles)
   - [S — Single Responsibility](#s--single-responsibility-principle-srp)
   - [O — Open/Closed](#o--openclosed-principle-ocp)
   - [L — Liskov Substitution](#l--liskov-substitution-principle-lsp)
   - [I — Interface Segregation](#i--interface-segregation-principle-isp)
   - [D — Dependency Inversion](#d--dependency-inversion-principle-dip)
2. [Design Patterns](#design-patterns)
   - **Creational:** [Singleton](#singleton-pattern), [Factory](#factory-pattern), [Builder](#builder-pattern)
   - **Structural:** [Decorator](#decorator-pattern), [Proxy](#proxy-pattern), [Facade](#facade-pattern), [Repository](#repository-pattern)
   - **Behavioral:** [Observer](#observer-pattern), [Strategy](#strategy-pattern), [Template Method](#template-method-pattern), [Command](#command-pattern)
3. [Clean Code Principles](#clean-code-principles)
   - [DRY](#dry-dont-repeat-yourself), [KISS](#kiss-keep-it-simple-stupid), [YAGNI](#yagni-you-arent-gonna-need-it), [Meaningful Naming](#meaningful-naming), [Small Functions](#small-functions)
4. [Error Handling Best Practices](#error-handling-best-practices)
   - Golden rules, Java exception hierarchy, Global Exception Handler, TypeScript/Node.js patterns
5. [Concurrency & Thread Safety](#concurrency--thread-safety)
   - Race conditions, `AtomicInteger`, `synchronized`, `ReentrantLock`, Immutability, Java Records
6. [API Design Best Practices](#api-design-best-practices)
   - RESTful URL design, HTTP status codes, API versioning, DTOs vs Entities
7. [Testing Best Practices](#testing-best-practices)
   - Testing pyramid, F.I.R.S.T., AAA pattern, good vs bad tests, mocking best practices
8. [Code Review Best Practices](#code-review-best-practices)
   - What to look for, review etiquette
9. [Secure Coding Best Practices](#secure-coding-best-practices)
   - Input validation, SQL injection, XSS, sensitive data, CORS & CSRF
10. [OOP Core Concepts](#oop-core-concepts)
    - Encapsulation, Inheritance vs Composition, Polymorphism, Abstract Class vs Interface, Key interview Q&A

---

## SOLID Principles

The SOLID principles are five design principles for writing maintainable, scalable, and testable object-oriented code.

### S — Single Responsibility Principle (SRP)

> A class should have only one reason to change.

**Bad:**

```java
public class UserService {
    public void createUser(User user) { /* save to DB */ }
    public void sendWelcomeEmail(User user) { /* send email */ }
    public String generateReport(List<User> users) { /* generate PDF */ }
}
```

**Good:**

```java
public class UserService {
    public void createUser(User user) { /* save to DB */ }
}

public class EmailService {
    public void sendWelcomeEmail(User user) { /* send email */ }
}

public class ReportService {
    public String generateReport(List<User> users) { /* generate PDF */ }
}
```

**In Angular:**

```typescript
// ❌ Bad: Component does too much
@Component({ ... })
export class UserComponent {
  users: User[] = [];
  
  fetchUsers() { /* HTTP call */ }
  sortUsers() { /* sorting logic */ }
  validateEmail(email: string) { /* validation */ }
  formatDate(date: Date) { /* formatting */ }
}

// ✅ Good: Separated concerns
@Component({ ... })
export class UserComponent {
  users = inject(UserService).getUsers();
}

@Injectable({ providedIn: 'root' })
export class UserService {
  getUsers(): Observable<User[]> { ... }
}

@Pipe({ name: 'formatDate', standalone: true })
export class FormatDatePipe implements PipeTransform { ... }
```

### O — Open/Closed Principle (OCP)

> Software entities should be open for extension but closed for modification.

**Bad:**

```java
public class DiscountService {
    public double calculate(Order order) {
        if (order.getType().equals("PREMIUM")) return order.getTotal() * 0.2;
        if (order.getType().equals("VIP")) return order.getTotal() * 0.3;
        // Must modify this class every time a new discount type is added
        return 0;
    }
}
```

**Good:**

```java
public interface DiscountStrategy {
    double calculate(Order order);
}

public class PremiumDiscount implements DiscountStrategy {
    public double calculate(Order order) { return order.getTotal() * 0.2; }
}

public class VipDiscount implements DiscountStrategy {
    public double calculate(Order order) { return order.getTotal() * 0.3; }
}

// New discounts added without modifying existing code
public class DiscountService {
    private final DiscountStrategy strategy;
    
    public DiscountService(DiscountStrategy strategy) {
        this.strategy = strategy;
    }
    
    public double calculate(Order order) {
        return strategy.calculate(order);
    }
}
```

### L — Liskov Substitution Principle (LSP)

> Subtypes must be substitutable for their base types without altering program correctness.

**Bad:**

```java
public class Rectangle {
    protected int width, height;
    
    public void setWidth(int w) { this.width = w; }
    public void setHeight(int h) { this.height = h; }
    public int getArea() { return width * height; }
}

public class Square extends Rectangle {
    @Override
    public void setWidth(int w) { this.width = w; this.height = w; } // Breaks LSP!
    @Override
    public void setHeight(int h) { this.width = h; this.height = h; }
}
```

**Good:**

```java
public interface Shape {
    int getArea();
}

public class Rectangle implements Shape {
    private final int width, height;
    public Rectangle(int w, int h) { this.width = w; this.height = h; }
    public int getArea() { return width * height; }
}

public class Square implements Shape {
    private final int side;
    public Square(int s) { this.side = s; }
    public int getArea() { return side * side; }
}
```

### I — Interface Segregation Principle (ISP)

> Clients should not be forced to depend on interfaces they don't use.

**Bad:**

```java
public interface Worker {
    void work();
    void eat();     // Not all workers eat (e.g., RobotWorker)
    void sleep();   // Not all workers sleep
}
```

**Good:**

```java
public interface Workable { void work(); }
public interface Eatable { void eat(); }
public interface Sleepable { void sleep(); }

public class Human implements Workable, Eatable, Sleepable { ... }
public class Robot implements Workable { ... }  // Only implements what it needs
```

**In Angular:**

```typescript
// ❌ Bad: One massive interface
interface CrudRepository<T> {
  getAll(): Observable<T[]>;
  getById(id: number): Observable<T>;
  create(entity: T): Observable<T>;
  update(entity: T): Observable<T>;
  delete(id: number): Observable<void>;
  generateReport(): Observable<Blob>;  // Not all repos need this
}

// ✅ Good: Segregated interfaces
interface Readable<T> {
  getAll(): Observable<T[]>;
  getById(id: number): Observable<T>;
}

interface Writable<T> {
  create(entity: T): Observable<T>;
  update(entity: T): Observable<T>;
  delete(id: number): Observable<void>;
}

interface Reportable {
  generateReport(): Observable<Blob>;
}
```

### D — Dependency Inversion Principle (DIP)

> High-level modules should not depend on low-level modules. Both should depend on abstractions.

**Bad:**

```java
public class OrderService {
    private MySQLDatabase database = new MySQLDatabase(); // Direct dependency on implementation
    
    public void saveOrder(Order order) {
        database.save(order);
    }
}
```

**Good:**

```java
public interface OrderRepository {
    void save(Order order);
}

@Repository
public class MySQLOrderRepository implements OrderRepository {
    public void save(Order order) { /* MySQL specific */ }
}

@Service
public class OrderService {
    private final OrderRepository repository; // Depends on abstraction
    
    public OrderService(OrderRepository repository) {
        this.repository = repository;
    }
    
    public void saveOrder(Order order) {
        repository.save(order);
    }
}
```

**In Angular (DI makes this natural):**

```typescript
// Interface/abstract class
export abstract class NotificationService {
  abstract notify(message: string): void;
}

// Implementation
@Injectable()
export class EmailNotificationService extends NotificationService {
  notify(message: string) { console.log('Email:', message); }
}

// Swap implementations via providers
providers: [
  { provide: NotificationService, useClass: EmailNotificationService }
]
```

---

## Design Patterns

> Design patterns are reusable solutions to commonly occurring problems in software design. They are divided into three categories: **Creational**, **Structural**, and **Behavioral**.

| Category | Patterns |
|----------|----------|
| **Creational** | Singleton, Factory, Abstract Factory, Builder, Prototype |
| **Structural** | Adapter, Decorator, Proxy, Facade, Composite |
| **Behavioral** | Strategy, Observer, Command, Template Method, Chain of Responsibility, Iterator |

### Singleton Pattern

Ensures a class has only one instance. In Spring, all beans are singletons by default. In Angular, services with `providedIn: 'root'` are singletons.

```java
// Java — Thread-safe Singleton
public class ConfigManager {
    private static volatile ConfigManager instance;
    
    private ConfigManager() {}
    
    public static ConfigManager getInstance() {
        if (instance == null) {
            synchronized (ConfigManager.class) {
                if (instance == null) {
                    instance = new ConfigManager();
                }
            }
        }
        return instance;
    }
}

// In Spring Boot — just use @Service (singleton by default)
@Service
public class ConfigManager { }
```

```typescript
// Angular — providedIn: 'root' is a singleton
@Injectable({ providedIn: 'root' })
export class ConfigService {
  private config = signal<AppConfig | null>(null);
}
```

### Factory Pattern

Creates objects without exposing creation logic. Useful when the exact type is determined at runtime.

```java
public interface Notification {
    void send(String message);
}

public class EmailNotification implements Notification { ... }
public class SmsNotification implements Notification { ... }
public class PushNotification implements Notification { ... }

public class NotificationFactory {
    public static Notification create(String type) {
        return switch (type) {
            case "EMAIL" -> new EmailNotification();
            case "SMS" -> new SmsNotification();
            case "PUSH" -> new PushNotification();
            default -> throw new IllegalArgumentException("Unknown type: " + type);
        };
    }
}
```

```typescript
// Angular — Factory provider
providers: [
  {
    provide: StorageService,
    useFactory: () => {
      return environment.production
        ? new CloudStorageService()
        : new LocalStorageService();
    }
  }
]
```

### Observer Pattern

Defines a one-to-many dependency: when one object changes state, all dependents are notified. **RxJS Observables** and **Angular Signals** are built on this pattern.

```typescript
// RxJS in Angular
@Injectable({ providedIn: 'root' })
export class EventBus {
  private subject = new Subject<AppEvent>();
  
  emit(event: AppEvent) { this.subject.next(event); }
  on(eventType: string): Observable<AppEvent> {
    return this.subject.pipe(filter(e => e.type === eventType));
  }
}

// Subscribe in component
export class NotificationComponent {
  private eventBus = inject(EventBus);
  
  notifications$ = this.eventBus.on('notification');
}
```

### Strategy Pattern

Defines a family of algorithms and makes them interchangeable. Same as the OCP example above.

```java
// Spring Boot — Multiple strategies as beans
public interface PaymentStrategy {
    void pay(BigDecimal amount);
}

@Component("creditCard")
public class CreditCardPayment implements PaymentStrategy { ... }

@Component("paypal")
public class PayPalPayment implements PaymentStrategy { ... }

@Service
public class PaymentService {
    private final Map<String, PaymentStrategy> strategies;
    
    public PaymentService(Map<String, PaymentStrategy> strategies) {
        this.strategies = strategies; // Spring auto-injects all implementations
    }
    
    public void processPayment(String method, BigDecimal amount) {
        strategies.get(method).pay(amount);
    }
}
```

### Builder Pattern

Constructs complex objects step by step. Common in Java (Lombok `@Builder`, Spring's `ResponseEntity`).

```java
// With Lombok
@Builder
@Data
public class User {
    private String name;
    private String email;
    private int age;
    private String role;
}

User user = User.builder()
    .name("John")
    .email("john@example.com")
    .age(30)
    .role("ADMIN")
    .build();

// Spring ResponseEntity uses builder pattern
return ResponseEntity.ok()
    .header("X-Custom-Header", "value")
    .body(responseDto);
```

```typescript
// TypeScript Builder
class QueryBuilder {
  private query: Partial<Query> = {};
  
  select(fields: string[]): this { this.query.fields = fields; return this; }
  where(condition: string): this { this.query.condition = condition; return this; }
  orderBy(field: string): this { this.query.orderBy = field; return this; }
  limit(n: number): this { this.query.limit = n; return this; }
  
  build(): Query { return this.query as Query; }
}

const query = new QueryBuilder()
  .select(['name', 'email'])
  .where('age > 18')
  .orderBy('name')
  .limit(10)
  .build();
```

### Decorator Pattern

Adds behavior to objects dynamically without modifying their structure. Angular decorators (`@Component`, `@Injectable`) are metadata decorators, but TypeScript also supports the pattern.

```typescript
// TypeScript class decorator
function Logger(constructor: Function) {
  console.log(`Creating instance of ${constructor.name}`);
}

@Logger
class UserService { }

// Method decorator for logging
function LogMethod(target: any, key: string, descriptor: PropertyDescriptor) {
  const original = descriptor.value;
  descriptor.value = function (...args: any[]) {
    console.log(`Calling ${key} with`, args);
    const result = original.apply(this, args);
    console.log(`${key} returned`, result);
    return result;
  };
}

class Calculator {
  @LogMethod
  add(a: number, b: number): number { return a + b; }
}
```

```java
// Java — Spring AOP is essentially the Decorator pattern
@Aspect
@Component
public class LoggingAspect {
    @Around("execution(* com.example.service.*.*(..))")
    public Object logAround(ProceedingJoinPoint joinPoint) throws Throwable {
        log.info("Before: {}", joinPoint.getSignature());
        Object result = joinPoint.proceed();
        log.info("After: {}", joinPoint.getSignature());
        return result;
    }
}
```

### Template Method Pattern

Defines the skeleton of an algorithm in a base class, letting subclasses override specific steps without changing the algorithm's structure.

```java
// Abstract class defines the template
public abstract class DataExporter {
    
    // Template method — final so subclasses can't change the algorithm
    public final void export(List<?> data) {
        List<?> filtered = filterData(data);    // Step 1: filter
        List<?> transformed = transform(filtered); // Step 2: transform
        writeOutput(transformed);                // Step 3: write
        sendNotification();                      // Step 4: notify (optional)
    }
    
    protected abstract List<?> filterData(List<?> data);
    protected abstract List<?> transform(List<?> data);
    protected abstract void writeOutput(List<?> data);
    
    // Hook — subclasses can override but don't have to
    protected void sendNotification() { /* default: do nothing */ }
}

@Component
public class CsvExporter extends DataExporter {
    @Override
    protected List<?> filterData(List<?> data) { return data.stream().filter(...).toList(); }
    @Override
    protected List<?> transform(List<?> data) { return data.stream().map(this::toCsvRow).toList(); }
    @Override
    protected void writeOutput(List<?> data) { /* write CSV file */ }
    @Override
    protected void sendNotification() { emailService.notifyExportComplete(); }
}
```

**When to use:** Report generation, data processing pipelines, test frameworks (JUnit's `@BeforeEach`/`@Test`/`@AfterEach` is template method).

**Interview tip:** Spring's `JdbcTemplate`, `RestTemplate`, `KafkaTemplate` all use this pattern — the "template" name is a direct hint.

---

### Command Pattern

Encapsulates a request as an object, allowing parameterization, queuing, logging, and undo operations.

```java
// Command interface
public interface Command {
    void execute();
    void undo();
}

// Concrete commands
public class TransferMoneyCommand implements Command {
    private final Account from;
    private final Account to;
    private final BigDecimal amount;
    
    @Override
    public void execute() {
        from.debit(amount);
        to.credit(amount);
    }
    
    @Override
    public void undo() {
        to.debit(amount);
        from.credit(amount);
    }
}

// Invoker — executes and tracks commands
public class CommandInvoker {
    private final Deque<Command> history = new ArrayDeque<>();
    
    public void execute(Command cmd) {
        cmd.execute();
        history.push(cmd);
    }
    
    public void undo() {
        if (!history.isEmpty()) {
            history.pop().undo();
        }
    }
}
```

**When to use:** Undo/redo functionality, task queues, transactional operations, audit logging.

---

### Proxy Pattern

Provides a surrogate or placeholder for another object to control access, add caching, or add cross-cutting concerns.

```java
// Subject interface
public interface UserService {
    User findById(Long id);
}

// Real implementation
@Service
public class UserServiceImpl implements UserService {
    public User findById(Long id) {
        return userRepository.findById(id).orElseThrow();
    }
}

// Caching Proxy
@Service
@Primary
public class CachingUserServiceProxy implements UserService {
    private final UserServiceImpl delegate;
    private final Map<Long, User> cache = new ConcurrentHashMap<>();
    
    public User findById(Long id) {
        return cache.computeIfAbsent(id, delegate::findById);
    }
}
```

**In Spring:** `@Transactional`, `@Cacheable`, `@Async` all work through Spring-generated proxies (AOP). The proxy intercepts method calls and adds behavior.

```java
@Service
public class OrderService {
    
    @Cacheable("orders")  // Spring creates a proxy that caches return values
    public Order getOrder(Long id) {
        return orderRepository.findById(id).orElseThrow();
    }
    
    @Transactional  // Spring creates a proxy that wraps in a transaction
    public void createOrder(Order order) {
        orderRepository.save(order);
    }
}
```

**Interview tip:** A common question is "How does `@Transactional` work?" — the answer is **proxy pattern** (Spring wraps your bean in a proxy that manages the transaction boundary).

---

### Repository Pattern

Abstracts the data layer, providing a collection-like interface for accessing domain objects. Decouples business logic from data access.

```java
// Repository interface (domain layer)
public interface OrderRepository {
    Optional<Order> findById(Long id);
    List<Order> findByCustomerId(Long customerId);
    Order save(Order order);
    void delete(Long id);
}

// JPA implementation (infrastructure layer)
@Repository
public class JpaOrderRepository implements OrderRepository {
    private final OrderJpaRepository jpa; // Spring Data JPA repo
    
    public Optional<Order> findById(Long id) {
        return jpa.findById(id).map(OrderMapper::toDomain);
    }
    
    public Order save(Order order) {
        OrderEntity entity = OrderMapper.toEntity(order);
        return OrderMapper.toDomain(jpa.save(entity));
    }
}

// Service uses the interface, not the implementation
@Service
public class OrderService {
    private final OrderRepository orderRepository; // Interface!
    
    public Order getOrder(Long id) {
        return orderRepository.findById(id)
            .orElseThrow(() -> new OrderNotFoundException(id));
    }
}
```

**Benefits:**
- Business logic doesn't know/care about SQL, JPA, MongoDB, etc.
- Easy to swap implementations (e.g., in-memory for tests)
- Centralizes query logic — no scattered SQL across the codebase

---

## Clean Code Principles

### DRY (Don't Repeat Yourself)

Every piece of knowledge should have a single, authoritative representation in a system.

```typescript
// ❌ Bad: Duplicated validation
function validateUser(user: User) { if (!user.email.includes('@')) throw new Error(); }
function validateAdmin(admin: Admin) { if (!admin.email.includes('@')) throw new Error(); }

// ✅ Good: Single validation
function validateEmail(email: string): boolean { return email.includes('@'); }
```

### KISS (Keep It Simple, Stupid)

Choose the simplest solution that works. Avoid over-engineering.

```typescript
// ❌ Over-engineered
class StringReverserFactory {
  createReverser(): StringReverser { return new StringReverser(); }
}
class StringReverser {
  reverse(s: string): string { return s.split('').reverse().join(''); }
}

// ✅ Simple
function reverseString(s: string): string {
  return s.split('').reverse().join('');
}
```

### YAGNI (You Aren't Gonna Need It)

Don't add functionality until it's actually needed.

```java
// ❌ Bad: Building for imaginary future requirements
public interface Repository<T> {
    T findById(int id);
    List<T> findAll();
    T save(T entity);
    void delete(int id);
    List<T> findByCustomQuery(String query);  // No one asked for this yet
    void exportToCsv(List<T> entities);       // No one asked for this yet
    void importFromCsv(String path);          // No one asked for this yet
}

// ✅ Good: Build what you need now
public interface Repository<T> {
    T findById(int id);
    List<T> findAll();
    T save(T entity);
    void delete(int id);
}
```

### Meaningful Naming

```java
// ❌ Bad
int d; // elapsed time in days
List<int[]> list1;
String fn(String s) { ... }

// ✅ Good
int elapsedTimeInDays;
List<Cell> flaggedCells;
String formatPhoneNumber(String rawNumber) { ... }
```

### Small Functions

Functions should do one thing, do it well, and do it only.

```java
// ❌ Bad: 50-line method doing multiple things
public void processOrder(Order order) {
    // validate... 30 lines
    // calculate totals... 20 lines
    // save... 10 lines
    // send email... 15 lines
}

// ✅ Good: Small, focused methods
public void processOrder(Order order) {
    validateOrder(order);
    calculateTotals(order);
    saveOrder(order);
    sendConfirmationEmail(order);
}
```

---

## Error Handling Best Practices

Good error handling is a key interview topic. It affects reliability, debuggability, and user experience.

### The Golden Rules

1. **Fail fast** — detect and report errors as early as possible
2. **Don't swallow exceptions** — never catch and ignore
3. **Use specific exception types** — avoid `catch (Exception e)`
4. **Include context** — exception messages should explain *what* and *why*
5. **Don't use exceptions for flow control** — they're for exceptional situations

### Java Error Handling

```java
// ❌ Bad: Swallowing exception
try {
    processOrder(order);
} catch (Exception e) {
    // Silent failure — nobody knows this broke!
}

// ❌ Bad: Catching too broadly
try {
    user = userRepository.findById(id);
} catch (Exception e) {
    return null; // NullPointerException later is worse than an exception now
}

// ✅ Good: Specific, contextual, logged
@Service
public class OrderService {
    
    public Order getOrder(Long id) {
        return orderRepository.findById(id)
            .orElseThrow(() -> new OrderNotFoundException("Order not found with id: " + id));
    }
    
    public void processOrder(Long id) {
        try {
            Order order = getOrder(id);
            paymentService.charge(order);
        } catch (OrderNotFoundException e) {
            log.warn("Order {} not found during processing", id);
            throw e; // Re-throw if caller needs to handle it
        } catch (PaymentException e) {
            log.error("Payment failed for order {}: {}", id, e.getMessage());
            throw new OrderProcessingException("Payment failed for order " + id, e);
        }
    }
}

// ✅ Good: Custom exception hierarchy
public class AppException extends RuntimeException {
    private final String errorCode;
    
    public AppException(String message, String errorCode) {
        super(message);
        this.errorCode = errorCode;
    }
}

public class OrderNotFoundException extends AppException {
    public OrderNotFoundException(Long id) {
        super("Order not found: " + id, "ORDER_NOT_FOUND");
    }
}
```

### Global Exception Handler (Spring Boot)

```java
@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(OrderNotFoundException.class)
    public ResponseEntity<ErrorResponse> handleNotFound(OrderNotFoundException ex) {
        log.warn("Not found: {}", ex.getMessage());
        return ResponseEntity.status(HttpStatus.NOT_FOUND)
            .body(new ErrorResponse(ex.getErrorCode(), ex.getMessage()));
    }

    @ExceptionHandler(ValidationException.class)
    public ResponseEntity<ErrorResponse> handleValidation(ValidationException ex) {
        return ResponseEntity.badRequest()
            .body(new ErrorResponse("VALIDATION_ERROR", ex.getMessage()));
    }

    @ExceptionHandler(Exception.class)
    public ResponseEntity<ErrorResponse> handleGeneral(Exception ex) {
        log.error("Unexpected error", ex); // Log the full stack trace
        return ResponseEntity.internalServerError()
            .body(new ErrorResponse("INTERNAL_ERROR", "An unexpected error occurred"));
    }
}

public record ErrorResponse(String code, String message) {}
```

### TypeScript / Node.js Error Handling

```typescript
// ❌ Bad
async function getUser(id: number) {
    try {
        return await userRepo.findById(id);
    } catch (e) {
        console.log(e); // Don't just log and move on
        return null;    // Caller doesn't know it failed
    }
}

// ✅ Good: Custom errors + proper propagation
class NotFoundError extends Error {
    constructor(
        public readonly resource: string,
        public readonly id: number | string
    ) {
        super(`${resource} with id ${id} not found`);
        this.name = 'NotFoundError';
    }
}

async function getUser(id: number): Promise<User> {
    const user = await userRepo.findById(id);
    if (!user) throw new NotFoundError('User', id);
    return user;
}

// Express global error handler
app.use((err: Error, req: Request, res: Response, next: NextFunction) => {
    if (err instanceof NotFoundError) {
        return res.status(404).json({ code: 'NOT_FOUND', message: err.message });
    }
    console.error('Unexpected error:', err);
    res.status(500).json({ code: 'INTERNAL_ERROR', message: 'Something went wrong' });
});
```

---

## Concurrency & Thread Safety

### Key Java Concepts

```java
// ❌ Thread-unsafe — race condition
public class Counter {
    private int count = 0;
    
    public void increment() { count++; } // Read-modify-write is NOT atomic!
    public int getCount() { return count; }
}

// ✅ Option 1: AtomicInteger (lock-free, best for simple counters)
public class Counter {
    private final AtomicInteger count = new AtomicInteger(0);
    
    public void increment() { count.incrementAndGet(); }
    public int getCount() { return count.get(); }
}

// ✅ Option 2: synchronized (for complex critical sections)
public class BankAccount {
    private double balance;
    
    public synchronized void transfer(double amount) {
        if (balance >= amount) {
            balance -= amount;
        }
    }
}

// ✅ Option 3: ReentrantLock (more flexibility than synchronized)
public class BankAccount {
    private double balance;
    private final ReentrantLock lock = new ReentrantLock();
    
    public void transfer(double amount) {
        lock.lock();
        try {
            if (balance >= amount) balance -= amount;
        } finally {
            lock.unlock(); // Always release in finally!
        }
    }
}
```

### Immutability

Immutable objects are inherently thread-safe — a key interview concept.

```java
// ✅ Immutable class
public final class Money {
    private final BigDecimal amount;
    private final String currency;
    
    public Money(BigDecimal amount, String currency) {
        this.amount = Objects.requireNonNull(amount);
        this.currency = Objects.requireNonNull(currency);
    }
    
    // Returns new object instead of mutating state
    public Money add(Money other) {
        if (!this.currency.equals(other.currency)) 
            throw new IllegalArgumentException("Cannot add different currencies");
        return new Money(this.amount.add(other.amount), this.currency);
    }
    
    // No setters — only getters
    public BigDecimal getAmount() { return amount; }
    public String getCurrency() { return currency; }
}

// In Java 16+: Records are immutable by default
public record Money(BigDecimal amount, String currency) {
    public Money {
        Objects.requireNonNull(amount);
        Objects.requireNonNull(currency);
    }
    
    public Money add(Money other) {
        return new Money(this.amount.add(other.amount), this.currency);
    }
}
```

### Common Concurrency Pitfalls

| Problem | Description | Solution |
|---------|-------------|----------|
| **Race Condition** | Two threads read/write shared state simultaneously | Synchronization, atomic variables |
| **Deadlock** | Two threads wait on each other forever | Always acquire locks in the same order |
| **Starvation** | A thread never gets CPU time | Fair locks (`new ReentrantLock(true)`) |
| **Visibility** | Thread reads stale cached value | `volatile`, `synchronized`, `Atomic*` |

---

## API Design Best Practices

### RESTful API Design

```
✅ Use nouns for resources (not verbs):
  GET    /users          → list users
  POST   /users          → create user
  GET    /users/{id}     → get user
  PUT    /users/{id}     → replace user
  PATCH  /users/{id}     → partial update
  DELETE /users/{id}     → delete user

❌ Avoid verb-based URLs:
  /getUser, /createOrder, /deleteProduct

✅ Use plural nouns:
  /users, /orders, /products

✅ Nest for relationships:
  GET /users/{id}/orders       → orders for a user
  GET /orders/{id}/items       → items in an order

✅ Use query params for filtering/sorting/pagination:
  GET /users?role=ADMIN&sort=name&page=0&size=20
```

### HTTP Status Codes — What to Return

| Scenario | Status Code |
|----------|-------------|
| Successful GET / PUT / PATCH | `200 OK` |
| Resource created (POST) | `201 Created` + `Location` header |
| No content (DELETE, background action) | `204 No Content` |
| Bad request body / validation error | `400 Bad Request` |
| Not authenticated | `401 Unauthorized` |
| Authenticated but no permission | `403 Forbidden` |
| Resource not found | `404 Not Found` |
| Server error | `500 Internal Server Error` |

```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    
    @PostMapping
    @ResponseStatus(HttpStatus.CREATED)
    public ResponseEntity<UserDto> createUser(@Valid @RequestBody CreateUserRequest req) {
        UserDto created = userService.create(req);
        URI location = ServletUriComponentsBuilder.fromCurrentRequest()
            .path("/{id}").buildAndExpand(created.id()).toUri();
        return ResponseEntity.created(location).body(created);
    }
    
    @DeleteMapping("/{id}")
    @ResponseStatus(HttpStatus.NO_CONTENT)
    public void deleteUser(@PathVariable Long id) {
        userService.delete(id);
    }
}
```

### API Versioning

```java
// Option 1: URI versioning (most common)
@RequestMapping("/api/v1/users")
@RequestMapping("/api/v2/users")

// Option 2: Header versioning
@GetMapping(headers = "X-API-Version=1")
@GetMapping(headers = "X-API-Version=2")

// Option 3: Accept header
@GetMapping(produces = "application/vnd.myapp.v1+json")
```

### DTOs vs Entities — Never Expose Entities Directly

```java
// ❌ Bad: Exposing JPA entity directly
@GetMapping("/{id}")
public User getUser(@PathVariable Long id) {
    return userRepository.findById(id).orElseThrow();
    // Exposes ALL fields including passwords, internal IDs, JPA proxies, etc.
}

// ✅ Good: Use DTOs
public record UserDto(Long id, String name, String email, String role) {}

@GetMapping("/{id}")
public UserDto getUser(@PathVariable Long id) {
    User user = userRepository.findById(id).orElseThrow();
    return new UserDto(user.getId(), user.getName(), user.getEmail(), user.getRole());
}
```

---

## Testing Best Practices

### The Testing Pyramid

```
        /\
       /E2E\          Few, slow, expensive — test critical user flows
      /------\
     /Integr. \       Medium — test interactions between components
    /----------\
   / Unit Tests \     Many, fast, cheap — test individual classes/functions
  /--------------\
```

### Unit Testing Principles (F.I.R.S.T.)

| Letter | Principle | Meaning |
|--------|-----------|---------|
| **F** | Fast | Tests must run in milliseconds |
| **I** | Independent | No shared state between tests |
| **R** | Repeatable | Same result every time, no flakiness |
| **S** | Self-validating | Pass/fail automatically, no manual check |
| **T** | Timely | Written alongside/before the production code |

### AAA Pattern (Arrange, Act, Assert)

```java
@Test
void shouldCalculateDiscountForVipCustomer() {
    // ARRANGE — set up the test
    Customer customer = new Customer("Alice", CustomerTier.VIP);
    Order order = new Order(customer, BigDecimal.valueOf(100));
    
    // ACT — invoke the behavior
    BigDecimal discount = discountService.calculate(order);
    
    // ASSERT — verify the result
    assertThat(discount).isEqualByComparingTo("30.00");
}
```

### What Makes a Good Test

```java
// ✅ Good test: descriptive name, single concern, clear structure
@Test
@DisplayName("should throw OrderNotFoundException when order does not exist")
void getOrder_whenOrderNotFound_shouldThrowException() {
    // Arrange
    when(orderRepository.findById(99L)).thenReturn(Optional.empty());
    
    // Act + Assert
    assertThatThrownBy(() -> orderService.getOrder(99L))
        .isInstanceOf(OrderNotFoundException.class)
        .hasMessageContaining("99");
}

// ❌ Bad test: unclear name, tests too much, magic numbers
@Test
void test1() {
    Order o = orderService.getOrder(1L);
    assertThat(o).isNotNull();
    assertThat(o.getTotal()).isEqualTo(50);
    assertThat(o.getStatus()).isEqualTo("COMPLETED");
    assertThat(o.getItems().size()).isEqualTo(3);
    // If this fails, which assertion is wrong? Why?
}
```

### Mocking Best Practices

```java
@ExtendWith(MockitoExtension.class)
class OrderServiceTest {
    
    @Mock private OrderRepository orderRepository;
    @Mock private PaymentService paymentService;
    @InjectMocks private OrderService orderService;
    
    @Test
    void shouldProcessPaymentWhenOrderIsValid() {
        // Only mock what the test needs
        Order order = createTestOrder();
        when(orderRepository.findById(1L)).thenReturn(Optional.of(order));
        
        orderService.processOrder(1L);
        
        // Verify important interactions
        verify(paymentService).charge(order.getPaymentInfo());
        verify(orderRepository).save(argThat(o -> o.getStatus() == COMPLETED));
    }
    
    // ✅ Use test factories/builders to avoid setup repetition
    private Order createTestOrder() {
        return Order.builder()
            .id(1L).status(PENDING)
            .total(BigDecimal.TEN)
            .build();
    }
}
```

### Test Coverage Rules of Thumb

- **Line coverage > 80%** is a common target, but it's not the goal — **meaningful tests** are
- Cover all **happy paths**, **edge cases**, and **error paths**
- Focus on **behavior**, not implementation details
- Tests should **not** break when you refactor internals

---

## Code Review Best Practices

### What to Look for in a Code Review

**Correctness:**
- Does the code do what it's supposed to?
- Are edge cases handled? (null, empty, boundary values)
- Are there potential race conditions or concurrency issues?

**Design:**
- Does it follow SOLID principles?
- Is there unnecessary duplication (DRY)?
- Is the abstraction level appropriate?
- Could it be simpler (KISS, YAGNI)?

**Readability:**
- Are names meaningful?
- Is the logic easy to follow?
- Is there adequate documentation for non-obvious logic?

**Security:**
- Is input validated?
- Are there SQL injection / XSS risks?
- Is sensitive data logged or exposed?

**Performance:**
- Are there N+1 query problems?
- Are there unnecessary loops or nested iterations?
- Is there unbounded memory usage?

### Code Review Etiquette

```
✅ DO:
  - Review the code, not the person
  - Ask questions: "What was the reason for...?" vs "This is wrong"
  - Suggest improvements: "Consider using X because..."
  - Praise good solutions explicitly
  - Be specific about what needs to change

❌ DON'T:
  - Use "you" statements: "You did this wrong"
  - Nitpick style issues (use linters/formatters instead)
  - Leave vague comments: "Fix this"
  - Block PRs for optional style preferences
  - Review more than 400 lines at a time (cognitive overload)
```

---

## Secure Coding Best Practices

Since the role requires secure coding, here are key practices:

### Input Validation

```java
// Always validate input on the server side
@PostMapping("/users")
public ResponseEntity<User> createUser(@Valid @RequestBody UserDto dto) {
    // @Valid triggers Bean Validation annotations
    return ResponseEntity.ok(userService.create(dto));
}

public class UserDto {
    @NotBlank @Size(min = 2, max = 50)
    private String name;
    
    @Email @NotBlank
    private String email;
    
    @Pattern(regexp = "^(?=.*[A-Z])(?=.*\\d).{8,}$")
    private String password;
}
```

### SQL Injection Prevention

```java
// ❌ NEVER concatenate SQL
String sql = "SELECT * FROM users WHERE name = '" + name + "'"; // SQL Injection!

// ✅ Use parameterized queries (JPA/Hibernate do this automatically)
@Query("SELECT u FROM User u WHERE u.name = :name")
Optional<User> findByName(@Param("name") String name);
```

### XSS Prevention in Angular

Angular **sanitizes** all values by default in templates. Never bypass it unless absolutely necessary.

```typescript
// ❌ Dangerous
this.domSanitizer.bypassSecurityTrustHtml(userInput);

// ✅ Angular auto-sanitizes in templates
<div>{{ userInput }}</div> <!-- Safe: auto-escaped -->

// ✅ Use Angular's DomSanitizer when needed
this.domSanitizer.sanitize(SecurityContext.HTML, userInput);
```

### Sensitive Data Handling

```java
// Never log sensitive data
log.info("User login: " + username); // ✅ OK
log.info("User password: " + password); // ❌ NEVER

// Use @JsonIgnore for sensitive fields in responses
public class UserResponse {
    private String name;
    @JsonIgnore
    private String password;
    @JsonIgnore
    private String ssn;
}
```

### CORS & CSRF

```java
@Configuration
public class SecurityConfig {
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .cors(cors -> cors.configurationSource(corsConfigSource()))
            .csrf(csrf -> csrf.csrfTokenRepository(
                CookieCsrfTokenRepository.withHttpOnlyFalse()
            ));
        return http.build();
    }
}
```

---

## OOP Core Concepts

Often tested in interviews — know these inside out.

### The Four Pillars

#### Encapsulation

Bundling data and methods that operate on it within a single unit, restricting direct access to internal state.

```java
// ❌ Bad: Exposed internal state
public class BankAccount {
    public double balance; // Anyone can change this!
}

// ✅ Good: Controlled access through methods
public class BankAccount {
    private double balance;
    
    public void deposit(double amount) {
        if (amount <= 0) throw new IllegalArgumentException("Amount must be positive");
        this.balance += amount;
    }
    
    public double getBalance() { return balance; }
    // No setBalance() — balance can only change via deposit/withdraw
}
```

#### Inheritance vs Composition

> **"Favour composition over inheritance"** — Gang of Four

```java
// ❌ Inheritance (tight coupling, fragile base class problem)
public class Dog extends Animal {
    @Override
    public void makeSound() { System.out.println("Woof"); }
}

// If Animal changes, Dog might break!

// ✅ Composition (flexible, can change behavior at runtime)
public class Dog {
    private final SoundBehavior soundBehavior; // Composed, not inherited
    
    public Dog(SoundBehavior soundBehavior) {
        this.soundBehavior = soundBehavior;
    }
    
    public void makeSound() { soundBehavior.makeSound(); }
}

// Behaviors as interfaces
public interface SoundBehavior { void makeSound(); }
public class BarkBehavior implements SoundBehavior { public void makeSound() { System.out.println("Woof"); } }
public class QuackBehavior implements SoundBehavior { public void makeSound() { System.out.println("Quack"); } }
```

**When to use inheritance:** when there's a genuine "is-a" relationship and the subclass truly extends (not changes) the parent's behavior.

**When to use composition:** almost always. When you need to reuse behavior, delegate to composed objects.

#### Polymorphism

The ability of different objects to respond to the same message in different ways.

```java
// Runtime polymorphism (method overriding)
public abstract class Shape {
    public abstract double area();
    
    public void printArea() {
        System.out.println("Area: " + area()); // Calls the right implementation at runtime
    }
}

public class Circle extends Shape {
    public double area() { return Math.PI * radius * radius; }
}

public class Rectangle extends Shape {
    public double area() { return width * height; }
}

// Compile-time polymorphism (method overloading)
public class Calculator {
    public int add(int a, int b) { return a + b; }
    public double add(double a, double b) { return a + b; }
    public String add(String a, String b) { return a + b; }
}
```

### Abstract Classes vs Interfaces

| Feature | Abstract Class | Interface |
|---------|---------------|-----------|
| **Instantiation** | No | No |
| **Multiple inheritance** | No (single extends) | Yes (multiple implements) |
| **State (fields)** | Yes | No (only constants) |
| **Constructor** | Yes | No |
| **Default methods** | Yes | Yes (Java 8+) |
| **When to use** | Shared base implementation | Defining a contract/capability |

```java
// Abstract class: shared state + some default behavior
public abstract class Vehicle {
    protected String brand;
    protected int year;
    
    public abstract void start(); // Must be overridden
    
    public void displayInfo() { // Shared behavior
        System.out.println(brand + " (" + year + ")");
    }
}

// Interface: pure contract, can implement many
public interface Flyable { void fly(); }
public interface Drivable { void drive(); }

public class FlyingCar extends Vehicle implements Flyable, Drivable {
    public void start() { /* ... */ }
    public void fly() { /* ... */ }
    public void drive() { /* ... */ }
}
```

### Key Interview Questions on OOP

**Q: What is the difference between `==` and `equals()` in Java?**
- `==` compares **references** (same object in memory)
- `equals()` compares **values** (logical equality)
- Always override `equals()` together with `hashCode()` — if two objects are equal, they must have the same hash code

**Q: What is method overriding vs overloading?**
- **Overriding**: subclass provides different implementation for a method with the **same signature** (runtime polymorphism)
- **Overloading**: same class has multiple methods with the **same name but different parameters** (compile-time polymorphism)

**Q: What is the difference between `final`, `finally`, and `finalize()`?**
- `final`: keyword — variable can't be reassigned, method can't be overridden, class can't be extended
- `finally`: block that always executes after try/catch (for cleanup)
- `finalize()`: deprecated method called by GC before object is collected (don't use)

**Q: What is a `static` method/field?**
- Belongs to the **class**, not to instances
- Can be called without creating an object
- All instances share the same static field
- Cannot access instance (`this`) from static context

```java
public class MathUtils {
    public static final double PI = 3.14159;  // shared constant
    
    public static int square(int n) { return n * n; } // utility method
}

int result = MathUtils.square(5); // No instance needed
```
