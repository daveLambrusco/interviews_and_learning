# Complete Guide to Unit Testing in Spring Boot

## Table of Contents

- [Complete Guide to Unit Testing in Spring Boot](#complete-guide-to-unit-testing-in-spring-boot)
  - [Table of Contents](#table-of-contents)
  - [Maven Dependencies](#maven-dependencies)
    - [Testing Dependencies in `pom.xml`](#testing-dependencies-in-pomxml)
    - [What Each Dependency Includes](#what-each-dependency-includes)
      - [1. **JUnit 5 (Jupiter)**](#1-junit-5-jupiter)
      - [2. **Mockito**](#2-mockito)
      - [3. **AssertJ**](#3-assertj)
      - [4. **Spring Test**](#4-spring-test)
  - [Testing Framework Overview](#testing-framework-overview)
    - [What is Unit Testing?](#what-is-unit-testing)
    - [Why Mock Dependencies?](#why-mock-dependencies)
  - [Annotations Explained](#annotations-explained)
    - [Class-Level Annotations](#class-level-annotations)
      - [`@ExtendWith(MockitoExtension.class)`](#extendwithmockitoextensionclass)
    - [Field-Level Annotations](#field-level-annotations)
      - [`@Mock`](#mock)
      - [`@InjectMocks`](#injectmocks)
    - [Method-Level Annotations](#method-level-annotations)
      - [`@BeforeEach`](#beforeeach)
      - [`@Test`](#test)
  - [Test Structure](#test-structure)
    - [The AAA Pattern (Arrange, Act, Assert)](#the-aaa-pattern-arrange-act-assert)
      - [1️⃣ **ARRANGE** (Given)](#1️⃣-arrange-given)
      - [2️⃣ **ACT** (When)](#2️⃣-act-when)
      - [3️⃣ **ASSERT** (Then)](#3️⃣-assert-then)
  - [Mocking Explained](#mocking-explained)
    - [`when()` - Programming Mock Behavior](#when---programming-mock-behavior)
    - [Common Mocking Patterns](#common-mocking-patterns)
      - [Return a Value](#return-a-value)
      - [Return Empty](#return-empty)
      - [Do Nothing (for void methods)](#do-nothing-for-void-methods)
      - [Return Argument (for save operations)](#return-argument-for-save-operations)
    - [Argument Matchers](#argument-matchers)
      - [`any()`](#any)
      - [`anyList()`](#anylist)
      - [`eq()` - for mixing exact and matchers](#eq---for-mixing-exact-and-matchers)
    - [`verify()` - Checking Interactions](#verify---checking-interactions)
      - [Verification with Times](#verification-with-times)
  - [Assertions Explained](#assertions-explained)
    - [AssertJ Syntax](#assertj-syntax)
      - [`assertThat()`](#assertthat)
    - [Common Assertions](#common-assertions)
      - [Collection Assertions](#collection-assertions)
      - [String Assertions](#string-assertions)
      - [Boolean Assertions](#boolean-assertions)
      - [Object Assertions](#object-assertions)
    - [Exception Assertions](#exception-assertions)
  - [Line-by-Line Breakdown](#line-by-line-breakdown)
    - [Test Setup](#test-setup)
    - [Test Data Setup](#test-data-setup)
    - [Example Test Breakdown](#example-test-breakdown)
      - [Arrange Phase](#arrange-phase)
      - [Act Phase](#act-phase)
      - [Assert Phase](#assert-phase)
    - [Exception Testing Example](#exception-testing-example)
    - [Complex Mocking Example](#complex-mocking-example)
  - [Best Practices](#best-practices)
    - [1. **Test Naming**](#1-test-naming)
    - [2. **One Assertion Per Concept**](#2-one-assertion-per-concept)
    - [3. **Test Independence**](#3-test-independence)
    - [4. **Mock Only External Dependencies**](#4-mock-only-external-dependencies)
    - [5. **Verify Important Interactions**](#5-verify-important-interactions)
  - [Running Tests](#running-tests)
    - [From Command Line](#from-command-line)
    - [From IDE (IntelliJ IDEA)](#from-ide-intellij-idea)
    - [Test Output](#test-output)
  - [Common Testing Scenarios](#common-testing-scenarios)
    - [1. **Testing a Successful Operation**](#1-testing-a-successful-operation)
    - [2. **Testing an Exception**](#2-testing-an-exception)
    - [3. **Testing with Filters**](#3-testing-with-filters)
    - [4. **Testing State Changes**](#4-testing-state-changes)
  - [Debugging Tests](#debugging-tests)
    - [When a Test Fails](#when-a-test-fails)
  - [Summary](#summary)
    - [Key Concepts](#key-concepts)
    - [Your Test Class](#your-test-class)
    - [Why This Matters](#why-this-matters)
  - [Quick Reference](#quick-reference)
  - [Next Steps](#next-steps)

## Maven Dependencies

### Testing Dependencies in `pom.xml`

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa-test</artifactId>
    <scope>test</scope>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-validation-test</artifactId>
    <scope>test</scope>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-webmvc-test</artifactId>
    <scope>test</scope>
</dependency>
```

### What Each Dependency Includes

#### 1. **JUnit 5 (Jupiter)**

- **Purpose**: Main testing framework
- **Package**: `org.junit.jupiter.*`
- **What it does**: Provides the foundation for writing and running tests
- **Key classes**:
  - `@Test` annotation
  - `Assertions` class
  - Test lifecycle methods (`@BeforeEach`, `@AfterEach`)

#### 2. **Mockito**

- **Purpose**: Mocking framework
- **Package**: `org.mockito.*`
- **What it does**: Creates fake (mock) objects to simulate dependencies
- **Why we need it**: Tests should be isolated - we don't want to test the database or other services
- **Key classes**:
  - `@Mock` annotation
  - `@InjectMocks` annotation
  - `when()`, `verify()` methods

#### 3. **AssertJ**

- **Purpose**: Fluent assertion library
- **Package**: `org.assertj.core.api.*`
- **What it does**: Provides readable assertions
- **Example**: `assertThat(result).hasSize(1)` is more readable than `assertEquals(1, result.size())`

#### 4. **Spring Test**

- **Purpose**: Spring-specific testing utilities
- **Package**: `org.springframework.test.*`
- **What it does**: Helps test Spring components with dependency injection

## Testing Framework Overview

### What is Unit Testing?

**Unit Testing** means testing individual components (units) of your application in isolation.

**Example**: Testing `ProductService` without actually calling:

- The database (`ProductRepository`)
- The mapper (`ProductMapper`)
- The criteria repository (`ProductCriteriaRepository`)

### Why Mock Dependencies?

```
Real World:
ProductService → ProductRepository → Database
                ↓
            ProductMapper

Test World:
ProductService → Mock ProductRepository (fake)
                ↓
            Mock ProductMapper (fake)
```

**Benefits**:

- ✅ Tests run **fast** (no database)
- ✅ Tests are **isolated** (one failure doesn't affect others)
- ✅ Tests are **predictable** (no external dependencies)
- ✅ You control **exactly what happens** in the test

## Annotations Explained

### Class-Level Annotations

#### `@ExtendWith(MockitoExtension.class)`

```java
@ExtendWith(MockitoExtension.class)
class ProductServiceTest {
```

**What it does**: Tells JUnit 5 to use Mockito for this test class

**Why we need it**: Enables `@Mock` and `@InjectMocks` annotations to work

**Alternative**: Without this, you'd have to manually create mocks:

```java
ProductRepository mockRepo = Mockito.mock(ProductRepository.class);
```

### Field-Level Annotations

#### `@Mock`

```java
@Mock
private ProductRepository productRepository;
```

**What it does**: Creates a **fake (mock)** version of `ProductRepository`

**Key points**:

- ❌ Does NOT create a real repository
- ❌ Does NOT connect to a database
- ✅ Creates an empty shell that you can program
- ✅ You control what it returns using `when()`

**Think of it as**: A robot that follows your instructions exactly

#### `@InjectMocks`

```java
@InjectMocks
private ProductServiceImpl productService;
```

**What it does**: Creates a **real** instance of `ProductServiceImpl` and automatically injects all the `@Mock` objects into it

**How it works**:

1. Creates `ProductServiceImpl` instance
2. Finds all fields marked with `@Mock`
3. Injects those mocks into the service (via constructor, setter, or field injection)

**Example**:

```java
// Without @InjectMocks (manual):
ProductServiceImpl service = new ProductServiceImpl(
    mockRepository, 
    mockCriteriaRepository, 
    mockMapper
);

// With @InjectMocks (automatic):
@InjectMocks
private ProductServiceImpl productService;  // Mockito does it for you!
```

### Method-Level Annotations

#### `@BeforeEach`

```java
@BeforeEach
void setUp() {
    // This runs before EVERY test method
}
```

**What it does**: Runs before each test method

**Purpose**: Set up common test data (test fixtures)

**When it runs**:

```java
setUp() → test1()
setUp() → test2()
setUp() → test3()
```

Each test gets a fresh setup!

#### `@Test`

```java
@Test
void getAllProducts_ShouldReturnAllProducts() {
    // Test code here
}
```

**What it does**: Marks a method as a test

**Naming convention**: `methodName_condition_expectedResult`

- `getAllProducts` - method being tested
- `Should` - what should happen
- `ReturnAllProducts` - expected result

## Test Structure

### The AAA Pattern (Arrange, Act, Assert)

Every test follows this pattern:

```java
@Test
void getAllProducts_ShouldReturnAllProducts() {
    // 1️⃣ ARRANGE - Set up test data and mock behavior
    when(productRepository.findAll()).thenReturn(Collections.singletonList(testProduct));
    when(productMapper.toProductResponseList(anyList())).thenReturn(Collections.singletonList(testResponse));

    // 2️⃣ ACT - Call the method you're testing
    List<ProductResponse> result = productService.getAllProducts();

    // 3️⃣ ASSERT - Verify the result
    assertThat(result).hasSize(1);
    assertThat(result.getFirst().name()).isEqualTo("Test Product");
    verify(productRepository).findAll();
    verify(productMapper).toProductResponseList(anyList());
}
```

#### 1️⃣ **ARRANGE** (Given)

Set up the test scenario:

- Create test data
- Define what mocks should return
- Set up preconditions

#### 2️⃣ **ACT** (When)

Execute the code you're testing:

- Call the method
- Perform the action

#### 3️⃣ **ASSERT** (Then)

Verify the results:

- Check return values
- Verify mock interactions
- Assert expected behavior

## Mocking Explained

### `when()` - Programming Mock Behavior

```java
when(productRepository.findAll()).thenReturn(Collections.singletonList(testProduct));
```

**Translation**: "When `findAll()` is called on the mock repository, return this list"

**Breakdown**:

- `when()` - Start programming the mock
- `productRepository.findAll()` - The method call to intercept
- `thenReturn(...)` - What to return when that method is called

**Real example**:

```java
// This is the mock - it doesn't do anything yet
@Mock
private ProductRepository productRepository;

// Now we program it: "When someone calls findById(1L), return this product"
when(productRepository.findById(1L)).thenReturn(Optional.of(testProduct));

// Later in the test, when service calls repository:
productService.getProductById(1L);  // ← This calls productRepository.findById(1L)
                                     // ← Mock intercepts and returns testProduct
```

### Common Mocking Patterns

#### Return a Value

```java
when(productRepository.findById(1L)).thenReturn(Optional.of(testProduct));
```

#### Return Empty

```java
when(productRepository.findById(999L)).thenReturn(Optional.empty());
```

#### Do Nothing (for void methods)

```java
doNothing().when(productRepository).deleteById(1L);
```

#### Return Argument (for save operations)

```java
when(productRepository.save(any(Product.class)))
    .thenAnswer(invocation -> invocation.getArgument(0));
```

**Translation**: "When save() is called, return the same object that was passed in"

### Argument Matchers

Instead of exact values, you can use matchers:

#### `any()`

```java
when(productRepository.save(any(Product.class))).thenReturn(testProduct);
```

**Meaning**: "For ANY Product object"

#### `anyList()`

```java
when(productMapper.toProductResponseList(anyList())).thenReturn(...);
```

**Meaning**: "For ANY List"

#### `eq()` - for mixing exact and matchers

```java
when(service.method(eq(1L), any(String.class))).thenReturn(...);
```

### `verify()` - Checking Interactions

```java
verify(productRepository).findAll();
```

**What it does**: Verifies that `findAll()` was called on the mock

**Why it matters**: Ensures your code actually uses the dependencies correctly

**Example**:

```java
// After calling the service method
productService.getAllProducts();

// Verify it called the repository
verify(productRepository).findAll();  // ✅ Pass if called
                                      // ❌ Fail if NOT called

// Verify it called the mapper
verify(productMapper).toProductResponseList(anyList());
```

#### Verification with Times

```java
verify(productRepository, times(1)).findAll();  // Called exactly once
verify(productRepository, never()).deleteById(any());  // Never called
verify(productRepository, atLeast(1)).findAll();  // Called at least once
```

## Assertions Explained

### AssertJ Syntax

#### `assertThat()`

```java
assertThat(result).hasSize(1);
```

**Breakdown**:

- `assertThat(result)` - Start an assertion about `result`
- `.hasSize(1)` - The condition to check

**Think of it as**: "I assert that result has size 1"

### Common Assertions

#### Collection Assertions

```java
assertThat(result).hasSize(1);           // Size is 1
assertThat(result).isEmpty();            // Collection is empty
assertThat(result).isNotEmpty();         // Collection has elements
assertThat(result).contains(item);       // Contains specific item
```

#### String Assertions

```java
assertThat(result.name()).isEqualTo("Test Product");
assertThat(result.name()).contains("Test");
assertThat(result.name()).startsWith("Test");
assertThat(result.name()).isNotNull();
```

#### Boolean Assertions

```java
assertThat(result.favorite()).isTrue();
assertThat(result.favorite()).isFalse();
```

#### Object Assertions

```java
assertThat(result).isNotNull();
assertThat(result.id()).isEqualTo(1L);
```

### Exception Assertions

```java
assertThatThrownBy(() -> productService.getProductById(999L))
    .isInstanceOf(ProductNotFoundException.class)
    .hasMessageContaining("999");
```

**What it does**:

1. Execute the code in the lambda
2. Expect it to throw an exception
3. Verify the exception type
4. Verify the message contains "999"

**Breakdown**:

- `assertThatThrownBy(...)` - Expect an exception
- Lambda `() -> ...` - Code that should throw
- `.isInstanceOf(...)` - Type of exception
- `.hasMessageContaining(...)` - Message validation

## Line-by-Line Breakdown

### Test Setup

```java
@ExtendWith(MockitoExtension.class)
class ProductServiceTest {
```

→ "Use Mockito for this test class"

```java
    @Mock
    private ProductRepository productRepository;
```

→ "Create a fake ProductRepository"

```java
    @Mock
    private ProductCriteriaRepository productCriteriaRepository;
```

→ "Create a fake ProductCriteriaRepository"

```java
    @Mock
    private ProductMapper productMapper;
```

→ "Create a fake ProductMapper"

```java
    @InjectMocks
    private ProductServiceImpl productService;
```

→ "Create a REAL ProductServiceImpl and inject all the fake dependencies into it"

### Test Data Setup

```java
    private Product testProduct;
    private ProductRequest testRequest;
    private ProductResponse testResponse;
```

→ "Declare variables to hold test data"

```java
    @BeforeEach
    void setUp() {
```

→ "This runs before each test method"

```java
        testProduct = Product.builder()
                .id(1L)
                .name("Test Product")
                .category("Electronics")
                .price(new BigDecimal("99.99"))
                .inStock(true)
                .favorite(false)
                .build();
```

→ "Create a test Product entity with sample data"

```java
        testRequest = new ProductRequest("Test Product",
                "Electronics",
                new BigDecimal("99.99"),
                true,
                false);
```

→ "Create a test request DTO with sample data"

```java
        testResponse = new ProductResponse(1L,
                "Test Product",
                "Electronics",
                new BigDecimal("99.99"),
                true,
                false);
```

→ "Create a test response DTO with sample data"

### Example Test Breakdown

```java
@Test
void getAllProducts_ShouldReturnAllProducts() {
```

→ "Test method: tests the getAllProducts functionality"

#### Arrange Phase

```java
    when(productRepository.findAll()).thenReturn(Collections.singletonList(testProduct));
```

→ "When the mock repository's findAll() is called, return a list with one product"

```java
    when(productMapper.toProductResponseList(anyList())).thenReturn(Collections.singletonList(testResponse));
```

→ "When the mock mapper converts any list, return a list with one response"

#### Act Phase

```java
    List<ProductResponse> result = productService.getAllProducts();
```

→ "Call the REAL service method (this is what we're testing)"

**What happens inside**:

1. Service calls `productRepository.findAll()`
2. Mock intercepts and returns `[testProduct]`
3. Service calls `productMapper.toProductResponseList([testProduct])`
4. Mock intercepts and returns `[testResponse]`
5. Service returns the list to us

#### Assert Phase

```java
    assertThat(result).hasSize(1);
```

→ "Verify the result list has exactly 1 item"

```java
    assertThat(result.getFirst().name()).isEqualTo("Test Product");
```

→ "Verify the first item's name is 'Test Product'"

```java
    verify(productRepository).findAll();
```

→ "Verify that findAll() was actually called on the repository mock"

```java
    verify(productMapper).toProductResponseList(anyList());
```

→ "Verify that toProductResponseList() was called on the mapper mock"

### Exception Testing Example

```java
@Test
void getProductById_WhenNotExists_ShouldThrowException() {
```

→ "Test method: tests what happens when product doesn't exist"

```java
    when(productRepository.findById(999L)).thenReturn(Optional.empty());
```

→ "When looking for product with ID 999, return empty (not found)"

```java
    assertThatThrownBy(() -> productService.getProductById(999L))
```

→ "Execute this code and expect it to throw an exception"

```java
            .isInstanceOf(ProductNotFoundException.class)
```

→ "The exception should be ProductNotFoundException"

```java
            .hasMessageContaining("999");
```

→ "The exception message should contain '999'"

### Complex Mocking Example

```java
@Test
void searchProducts_ShouldReturnFilteredProducts() {
```

→ "Test the search with filters functionality"

```java
    ProductFilterRequest filter = ProductFilterRequest.builder()
            .name("Test")
            .category("Electronics")
            .build();
```

→ "Create a filter object to search by name and category"

```java
    when(productCriteriaRepository.findByFilters(filter))
        .thenReturn(Collections.singletonList(testProduct));
```

→ "When the criteria repository is called with THIS EXACT filter object, return the test product"

```java
    when(productMapper.toProductResponseList(anyList()))
        .thenReturn(Collections.singletonList(testResponse));
```

→ "When mapper converts any list, return the test response"

```java
    List<ProductResponse> result = productService.searchProducts(filter);
```

→ "Call the service method with the filter"

```java
    assertThat(result).hasSize(1);
    assertThat(result.getFirst().name()).isEqualTo("Test Product");
```

→ "Verify we got 1 result with the correct name"

```java
    verify(productCriteriaRepository).findByFilters(filter);
    verify(productMapper).toProductResponseList(anyList());
```

→ "Verify both the repository and mapper were called"

## Best Practices

### 1. **Test Naming**

```java
// ✅ Good
void getAllProducts_ShouldReturnAllProducts()
void getProductById_WhenNotExists_ShouldThrowException()

// ❌ Bad
void test1()
void testGetProducts()
```

### 2. **One Assertion Per Concept**

```java
// ✅ Good - Multiple assertions about the same concept
assertThat(result).hasSize(1);
assertThat(result.getFirst().name()).isEqualTo("Test Product");

// ❌ Bad - Testing multiple unrelated things
assertThat(result).hasSize(1);
assertThat(otherResult).isNotNull();
```

### 3. **Test Independence**

- Each test should work independently
- Don't rely on test execution order
- Use `@BeforeEach` to reset state

### 4. **Mock Only External Dependencies**

```java
// ✅ Mock: Repositories, External APIs, File Systems
@Mock
private ProductRepository productRepository;

// ❌ Don't Mock: The class you're testing
@InjectMocks  // ← This is REAL
private ProductServiceImpl productService;
```

### 5. **Verify Important Interactions**

```java
// ✅ Verify the method was called
verify(productRepository).save(any());

// Also verify return values
assertThat(result).isNotNull();
```

## Running Tests

### From Command Line

```bash
# Run all tests
./mvnw test

# Run a specific test class
./mvnw test -Dtest=ProductServiceTest

# Run a specific test method
./mvnw test -Dtest=ProductServiceTest#getAllProducts_ShouldReturnAllProducts
```

### From IDE (IntelliJ IDEA)

1. **Right-click on test class** → "Run 'ProductServiceTest'"
2. **Right-click on test method** → "Run 'getAllProducts_Should...'"
3. **Green play button** next to class/method

### Test Output

```sh
✅ getAllProducts_ShouldReturnAllProducts - PASSED
✅ getProductById_WhenExists_ShouldReturnProduct - PASSED
❌ getProductById_WhenNotExists_ShouldThrowException - FAILED
```

## Common Testing Scenarios

### 1. **Testing a Successful Operation**

```java
@Test
void createProduct_ShouldReturnCreatedProduct() {
    // Arrange: Set up what mocks should return
    when(productMapper.toNewProduct(testRequest)).thenReturn(testProduct);
    when(productRepository.save(any(Product.class))).thenReturn(testProduct);
    when(productMapper.toProductResponse(testProduct)).thenReturn(testResponse);

    // Act: Call the method
    ProductResponse result = productService.createProduct(testRequest);

    // Assert: Check the result and interactions
    assertThat(result.name()).isEqualTo("Test Product");
    verify(productRepository).save(any(Product.class));
}
```

### 2. **Testing an Exception**

```java
@Test
void deleteProduct_WhenNotExists_ShouldThrowException() {
    // Arrange: Product doesn't exist
    when(productRepository.existsById(999L)).thenReturn(false);

    // Act & Assert: Expect an exception
    assertThatThrownBy(() -> productService.deleteProduct(999L))
            .isInstanceOf(ProductNotFoundException.class);
}
```

### 3. **Testing with Filters**

```java
@Test
void searchProducts_ShouldReturnFilteredProducts() {
    // Arrange: Create filter
    ProductFilterRequest filter = ProductFilterRequest.builder()
            .favorite(true)
            .build();
    
    when(productCriteriaRepository.findByFilters(filter))
        .thenReturn(Collections.singletonList(testProduct));

    // Act
    List<ProductResponse> result = productService.searchProducts(filter);

    // Assert
    assertThat(result).hasSize(1);
}
```

### 4. **Testing State Changes**

```java
@Test
void toggleFavorite_ShouldToggleFavoriteStatus() {
    // Arrange
    when(productRepository.findById(1L)).thenReturn(Optional.of(testProduct));
    when(productRepository.save(any(Product.class)))
        .thenAnswer(invocation -> invocation.getArgument(0));  // Return what was passed
    when(productMapper.toProductResponse(any(Product.class)))
        .thenReturn(toggledResponse);

    // Act
    ProductResponse result = productService.toggleFavorite(1L);

    // Assert: Verify the state changed
    assertThat(result.favorite()).isTrue();
}
```

## Debugging Tests

### When a Test Fails

1. **Read the error message**:

```java
Expected size: <1> but was: <0>
```

→ Expected 1 item, got 0

2. **Check your mocking**:

```java
// Did you program the mock?
when(productRepository.findAll()).thenReturn(...);
```

3. **Add debug output**:

```java
System.out.println("Result: " + result);
```

4. **Use breakpoints** in your IDE

5. **Check verify() statements**:

```
Wanted but not invoked:
productRepository.findAll();
```

→ The method was never called

## Summary

### Key Concepts

1. **Unit Tests** test ONE component in isolation
2. **Mocks** (`@Mock`) are fake dependencies
3. **`when()`** programs what mocks return
4. **`verify()`** checks if methods were called
5. **`assertThat()`** checks expected results
6. **AAA Pattern**: Arrange, Act, Assert

### Your Test Class

- ✅ Tests `ProductServiceImpl` in isolation
- ✅ Mocks all dependencies (repository, mapper)
- ✅ Tests all service methods
- ✅ Tests both success and failure scenarios
- ✅ Verifies interactions with dependencies

### Why This Matters

- 🚀 **Fast**: No database, no network
- 🎯 **Focused**: Tests one thing at a time
- 🔒 **Reliable**: Same result every time
- 📝 **Documentation**: Shows how code should work
- 🐛 **Bug Detection**: Catches issues early

## Quick Reference

| Concept | Syntax | Purpose |
|---------|--------|---------|
| Create mock | `@Mock` | Fake dependency |
| Inject mocks | `@InjectMocks` | Real object with fake deps |
| Program mock | `when(...).thenReturn(...)` | Define behavior |
| Verify call | `verify(mock).method()` | Check interaction |
| Assert value | `assertThat(...).isEqualTo(...)` | Check result |
| Assert exception | `assertThatThrownBy(...)` | Check exception |
| Before each test | `@BeforeEach` | Setup |
| Test method | `@Test` | Mark as test |

## Next Steps

1. ✅ Read through this guide
2. ✅ Run the existing tests
3. ✅ Modify a test and see what happens
4. ✅ Write a new test for a new method
5. ✅ Practice TDD (Test-Driven Development)

**Remember**: Testing is a skill that improves with practice! 🎓
