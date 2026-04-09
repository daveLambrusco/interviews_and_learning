# A Practical Guide to Resilience4j with Spring Boot

## Table of Contents

1. [What Was Hystrix and Why It Was Abandoned](#1-what-was-hystrix-and-why-it-was-abandoned)
2. [What Is Resilience4j and the Key Differences](#2-what-is-resilience4j-and-the-key-differences)
3. [Categorizing Calls by Type: the "Lane" Pattern](#3-categorizing-calls-by-type-the-lane-pattern)
4. [The Complete Call Flow](#4-the-complete-call-flow)
5. [The Circuit Breaker: How It Works and How It Trips](#5-the-circuit-breaker-how-it-works-and-how-it-trips)
6. [The Retry: Attempt Logic](#6-the-retry-attempt-logic)
7. [The Bulkhead Pattern and Multi-Tenant Isolation](#7-the-bulkhead-pattern-and-multi-tenant-isolation)
8. [Monitoring Circuit Breakers](#8-monitoring-circuit-breakers)
9. [Managing the HTTP Client: Single-Tenant and Multi-Tenant Approaches](#9-managing-the-http-client-single-tenant-and-multi-tenant-approaches)
10. [Summary Table: Hystrix vs Resilience4j](#10-summary-table-hystrix-vs-resilience4j)
11. [General Best Practices](#11-general-best-practices)
12. [Dynamic Circuit Breakers: How the Registry Works and When to Use Annotations](#12-dynamic-circuit-breakers-how-the-registry-works-and-when-to-use-annotations)
13. [RestClient vs RestTemplate: What Changes with Resilience4j](#13-restclient-vs-resttemplate-what-changes-with-resilience4j)

## 1. What Was Hystrix and Why It Was Abandoned

Hystrix was Netflix's resilience library, widely adopted across the Java ecosystem. It was known for two core mechanisms:

- **Circuit Breaker**: an automatic switch that halted outgoing calls to a failing downstream service.
- **Automatic Fallback**: when a call failed, Hystrix would automatically invoke an alternative method.

```java
// HYSTRIX — old style
@HystrixCommand(fallbackMethod = "getUsersFallback",
    commandProperties = {
        @HystrixProperty(name = "execution.isolation.thread.timeoutInMilliseconds", value = "5000")
    })
public List<UserDTO> getUsers(String tenantId) { ... }

public List<UserDTO> getUsersFallback(String tenantId, Throwable e) {
    return new ArrayList<>();
}
```

**Why was it abandoned?**

In **November 2018**, Netflix placed Hystrix in *maintenance-only* mode: no new features, only critical security patches.

From Spring Cloud 2022 onwards, support was removed entirely. Hystrix is incompatible with Spring Boot 3.x because it depends on the legacy `javax.*` namespace, while Spring Boot 3 migrated to `jakarta.*`.

## 2. What Is Resilience4j and the Key Differences

Resilience4j is the modern replacement for Hystrix. It is **lightweight, modular, and built on Java 8+** (lambdas and functional interfaces).

### The Fundamental Difference: Thread Pool vs Functional Decorators

|                    | Hystrix                                           | Resilience4j                                        |
|--------------------|---------------------------------------------------|-----------------------------------------------------|
| **Model**          | Each call is dispatched to a separate thread pool | Wraps the original function (no extra threads)      |
| **Overhead**       | High (thread context switching)                   | Minimal (runs on the calling thread)                |
| **Components**     | Circuit Breaker + Fallback only                   | CB + Retry + RateLimiter + Bulkhead + TimeLimiter   |
| **Configuration**  | Inline annotation parameters                      | Centralized YAML                                    |
| **Spring Boot 3**  | Incompatible                                      | Natively compatible                                 |

### How the Decorator Pattern Works

In Hystrix, your call was **executed on a separate thread** from the pool — that was the isolation mechanism, at the cost of thread-switching overhead.

In Resilience4j, your call is **wrapped** (decorated) like a set of nested functions:

```sh
[Retry [ CircuitBreaker [ Your HTTP call ] ] ]
```

No additional threads are involved. The functional pattern adds logic before and after the call **on the same thread**.

For this reason the Retry-outer / CB-inner order is mandatory, see [§4](#4-the-complete-call-flow) and [§11](#11-general-best-practices).

## 3. Categorizing Calls by Type: the "Lane" Pattern

In any application that integrates with heterogeneous services, not all HTTP calls share the same requirements: a lightweight query has very different timeout and retry needs compared to a bulk data export.

An effective architectural pattern is to define **traffic categories** (referred to here as "lanes"), each with its own configured HTTP client and resilience policy.

```sh
┌──────────────────────────────────────────────────────────────────┐
│                        HttpClientFacade                          │
│                                                                  │
│  ┌───────────────────────────────────────────────────────────┐   │
│  │  LANE EXT  (External APIs — standard calls)               │   │
│  │  HttpClient: ConnectTimeout=10s, ReadTimeout=120s         │   │
│  │  Retry: 3 attempts, exponential backoff (5s→10s→20s)      │   │
│  │  CB Config: external_config (window of 20 calls)          │   │
│  └───────────────────────────────────────────────────────────┘   │
│                                                                  │
│  ┌───────────────────────────────────────────────────────────┐   │
│  │  LANE EXT_LONG  (External APIs — heavy operations)        │   │
│  │  HttpClient: ConnectTimeout=10s, ReadTimeout=300s         │   │
│  │  Retry: 2 attempts, 10s backoff                           │   │
│  │  CB Config: external_long_config (window of 5 calls)      │   │
│  └───────────────────────────────────────────────────────────┘   │
│                                                                  │
│  ┌───────────────────────────────────────────────────────────┐   │
│  │  LANE INT  (Internal microservices — standard)            │   │
│  │  HttpClient: 120s                                         │   │
│  │  Retry: DISABLED (max-attempts=1)                         │   │
│  │  CB Config: internal_config (window of 10 calls)          │   │
│  └───────────────────────────────────────────────────────────┘   │
│                                                                  │
│  ┌───────────────────────────────────────────────────────────┐   │
│  │  LANE INT_LONG  (Internal microservices — heavy)          │   │
│  │  HttpClient: 300s                                         │   │
│  │  Retry: DISABLED (max-attempts=1)                         │   │
│  │  CB Config: internal_long_config (window of 6 calls)      │   │
│  └───────────────────────────────────────────────────────────┘   │
└──────────────────────────────────────────────────────────────────┘
```

**Why is Retry disabled for internal calls?**

This is a deliberate architectural decision against *cascading failure*. If an internal microservice is under stress and every caller retries three times, the load on that service **triples**, making the situation worse. For third-party external APIs, retrying makes sense: the server may be temporarily rate-limited (`429 Too Many Requests`) and a fresh attempt after a few seconds is likely to succeed.

→ For a detailed analysis of the cascading failure problem, see [§6](#6-the-retry-attempt-logic).

## 4. The Complete Call Flow

Consider an `OrderService.getOrders(tenantId)` that delegates to a centralized HTTP client for calls to an external API.

### Step-by-Step

```java
OrderService
    |
    v
httpClient.executeExternal(tenant="acme", serviceLabel="OrderService_getOrders", ...)
    |
    v
executeWithResilience(tenant, name="EXT_OrderService_getOrders", configName="external_config", supplier)
    |
    +-> 1. computes instanceName = "EXT_OrderService_getOrders-acme"
    |        (convention: LANE_ClassName_methodName-tenantId)
    |
    +-> 2. CircuitBreakerRegistry.circuitBreaker("EXT_OrderService_getOrders-acme", "external_config")
    |        creates the CB with the YAML config if absent; returns the existing instance otherwise.
    |        (→ CB mechanics: §5 | per-tenant dynamic CBs: §7)
    |
    +-> 3. RetryRegistry.retry("EXT_OrderService_getOrders-acme", "external_config")
    |        same logic: creates or retrieves the Retry instance for this tenant+service pair.
    |        (→ Retry mechanics: §6)
    |
    +-> 4. Builds the decorator chain:
    |        resilientSupplier = Retry.decorate( retry,
    |                              CircuitBreaker.decorate( cb, supplier  /* the real HTTP call */)
    |                            )
    |
    +-> 5. resilientSupplier.get()  ← EXECUTES THE CHAIN
```

### What Happens During Chain Execution

```sh
resilientSupplier.get()
    |
    v
[RETRY] Starting attempt #1
    |
    v
[CIRCUIT BREAKER] Is CB CLOSED? --NO--> throws CallNotPermittedException (→ §5)
    | YES
    v
[HTTP CALL] httpClient.exchange(url, ...)
    |
    +--- Success (2xx) ------------------------------------------------->
    |                                                                    |
    |                                                CB records SUCCESS  |
    |                                                Retry stops         |
    |                                                Returns response  <-+
    |
    +--- Failure (timeout, ConnectException, 429, 503)
              |
              v
        CB records FAILURE (updates sliding window → §5)
              |
              v
        [RETRY] Is the exception in retry-exceptions? (→ §6)
              |
              +- NO (e.g. 401, 404, 500) → rethrows immediately
              |
              +- YES (429, ConnectException, 503, timeout)
                    |
                    v
              Waits backoff interval (5s on 1st retry, 10s on 2nd)
                    |
                    v
              Retries (back to [CIRCUIT BREAKER])
```

> **Important note**: every individual Retry attempt passes through the CB. If on the 2nd attempt the CB has become OPEN (saturated by concurrent threads), the retry fails with `CallNotPermittedException` without making any HTTP call.

## 5. The Circuit Breaker: How It Works and How It Trips

### The State Machine

The CB has three states. The transition from CLOSED to OPEN occurs when the failure rate exceeds the threshold defined in YAML:

```sh
              Failure Rate > 50%
  +------------------------------------------+
  |                                          v
CLOSED                                     OPEN
  ^                                          |
  |                                          | Waits wait-duration (e.g. 30s)
  |                                          v
  |                       +--------------HALF_OPEN
  |                       |   (probes N test calls)
  |   All succeed         |
  +-----------------------+
       Returns CLOSED       At least one fails → returns OPEN
```

### The Sliding Window

The CB does not use a global counter. It observes a **rolling window of the last N calls**:

```yaml
# external_config — standard API calls
sliding-window-size: 20        # Observes the last 20 calls
minimum-number-of-calls: 10    # Does not evaluate until at least 10 calls are recorded
failure-rate-threshold: 50     # If ≥50% fail → OPENS
wait-duration-in-open-state: 30s
```

**Practical example:**

```sh
Calls: [OK, OK, OK, OK, OK, FAIL, FAIL, FAIL, FAIL, FAIL, FAIL]
                                                             ^
                                         11 total → exceeds minimum (10)
                                         6 failures out of 11 = 54.5% > 50%
                                         → CB OPENS!
```

For infrequent operations (e.g. bulk exports), a smaller window is preferable: since calls are rare, a few failures already constitute a meaningful signal.

```yaml
# external_long_config — heavy, infrequent operations
sliding-window-size: 5
minimum-number-of-calls: 3
failure-rate-threshold: 50
wait-duration-in-open-state: 60s  # Stays open longer to allow recovery
```

### Exceptions That Do NOT Count as Failures

```yaml
ignore-exceptions:
  - HttpClientErrorException.NotFound             # 404 → resource absent, not a server fault
  - HttpClientErrorException.BadRequest           # 400 → caller error, not a server fault
  - HttpClientErrorException.Forbidden            # 403 → authorization issue
  - HttpClientErrorException.UnprocessableEntity  # 422 → semantically invalid input
```

These exceptions are **ignored by the CB**: they count neither as success nor as failure. A 404 is an *expected* outcome — it does not indicate that the remote server is degraded.

**What DOES count as a failure** (and contributes to opening the CB):

- `ConnectException` → server unreachable
- `HttpClientErrorException.TooManyRequests` (429) → server overloaded
- `HttpServerErrorException.ServiceUnavailable` (503) → server unavailable
- `ResourceAccessException` → network timeout
- `HttpClientErrorException.Unauthorized` (401) → invalid credentials
- Any exception not listed in `ignore-exceptions`

### 401 as a Systemic Failure Signal

A 401 (Unauthorized) response may appear to be a caller-side issue, but in multi-tenant systems it can indicate a systemic condition: if a tenant's credentials have expired or been rotated, *every* subsequent call will return 401 until the configuration is updated.

Treating 401 as a failure and allowing the CB to open has a beneficial side effect: subsequent calls with those credentials **fail immediately (0ms)** without reaching the remote server. This prevents unnecessary load on the external API and, for APIs that enforce per-IP rate limiting, it protects against automatic IP bans.

## 6. The Retry: Attempt Logic

### Exponential Backoff

```yaml
retry:
  configs:
    external_config:
      max-attempts: 3
      wait-duration: 5s
      enable-exponential-backoff: true
      exponential-backoff-multiplier: 2
```

```sh
Attempt 1 → FAIL
    Waits 5s   (5 × 2⁰)
Attempt 2 → FAIL
    Waits 10s  (5 × 2¹)
Attempt 3 → FAIL
    → Rethrows the exception to the caller (handled via try/catch in the service)
```

**Before Resilience4j**, this pattern required manual implementation with blocking loops:

```java
// Legacy: manual loop with Thread.sleep — blocks the thread for the entire wait duration
boolean shouldRetry = true;
while (shouldRetry) {
    try {
        response = httpClient.exchange(...);
        shouldRetry = false;
    } catch (HttpClientErrorException.TooManyRequests e) {
        Thread.sleep(5000); // thread blocked!
        shouldRetry = true;
    }
}
```

**With Resilience4j**, the Retry module handles this automatically with exponential backoff, zero manual code, and centralized YAML configuration.

### Retry-Exceptions: What Triggers a Retry

```yaml
retry-exceptions:
  - org.springframework.web.client.HttpClientErrorException.TooManyRequests     # 429: server overloaded
  - org.springframework.web.client.HttpServerErrorException.ServiceUnavailable  # 503: temporarily down
  - java.net.ConnectException                                                   # server unreachable
  - org.springframework.web.client.ResourceAccessException                      # timeout
```

Only exceptions in this list trigger a new attempt. A 401 or 500 is rethrown immediately — retrying them is pointless as they do not stem from a transient network or load issue.

### Cascading Failure: Why Retry Must Be Disabled for Internal Calls

For calls to internal microservices, Retry should be set to `max-attempts: 1` (effectively disabled).

If an internal service is under stress and every caller retries three times, the number of requests that service must handle **multiplies by three**. With N callers each retrying 3 times, the load grows from N to 3N on an already struggling system. This is *cascading failure*: the retry mechanism — designed to help — makes the situation significantly worse until complete collapse.

The Circuit Breaker (§5) handles the fault by opening the circuit; disabled retries prevent further amplification of load during the critical window.

## 7. The Bulkhead Pattern and Multi-Tenant Isolation

### The Problem Without Isolation

In a multi-tenant system with a single shared CB per service:

```sh
Tenant: acme    → shared CB → External API
Tenant: globex  → shared CB → External API
Tenant: contoso → shared CB → External API

If acme has expired credentials (401 on every call):
→ the shared CB accumulates failures and OPENS
→ globex and contoso can no longer reach the API!
```

One tenant's misconfiguration brings down the service for all others.

### The Solution: Dynamic Circuit Breakers per Tenant

Instead of one CB per service, create a CB for each **tenant + service** pair, with a unique instance name:

```sh
CB name = lane_ServiceName_methodName + "-" + tenantId
```

**Examples:**

```sh
EXT_OrderService_getOrders-acme          → CB for acme's getOrders
EXT_OrderService_getOrders-globex        → CB for globex's getOrders
EXT_LONG_ReportService_export-contoso    → CB for contoso's heavy export
INT_InventoryService_getStock-acme       → CB for acme's internal Inventory call
```

The result:

```sh
Tenant: acme    → CB "EXT_OrderService-acme"    [OPEN!]  → API — blocked
Tenant: globex  → CB "EXT_OrderService-globex"  [CLOSED] → API — OK
Tenant: contoso → CB "EXT_OrderService-contoso" [CLOSED] → API — OK
```

`acme`'s problem does not propagate to other tenants.

Circuit Breakers are created **lazily** (on first use) via `CircuitBreakerRegistry` and then reused: the registry holds them in memory for the application's lifetime.

> **The naming convention is critical** both for monitoring (→ [§8](#8-monitoring-circuit-breakers)) and for correct state isolation. A CB created with a generic name that is not scoped to a specific tenant provides no isolation.

## 8. Monitoring Circuit Breakers

### The Data Source: `CircuitBreakerRegistry`

All Circuit Breakers — whether created statically or dynamically — are registered in Resilience4j's `CircuitBreakerRegistry`, which exposes the full list at any time:

```java
circuitBreakerRegistry.getAllCircuitBreakers()
    .forEach(cb -> {
        CircuitBreaker.State  state        = cb.getState();
        CircuitBreaker.Metrics metrics     = cb.getMetrics();
        float failureRate                  = metrics.getFailureRate();        // -1 if window not yet full
        int   failedCalls                  = metrics.getNumberOfFailedCalls();
        int   bufferedCalls                = metrics.getNumberOfBufferedCalls();
        System.out.printf("[%s] state=%s failRate=%.0f%% (%d/%d)%n",
            cb.getName(), state, failureRate, failedCalls, bufferedCalls);
    });
```

### Spring Boot Actuator: the Standard Approach

With `spring-boot-starter-actuator` and the `resilience4j-micrometer` bridge on the classpath, Circuit Breakers are exposed automatically:

```yaml
management:
  endpoints:
    web:
      exposure:
        include: health, circuitbreakers, circuitbreakerevents
  health:
    circuitbreakers:
      enabled: true
```

Available endpoints:

```sh
GET /actuator/health               → aggregated status (UP / DOWN) with per-CB detail
GET /actuator/circuitbreakers      → full list of CBs with state and metrics
GET /actuator/circuitbreakerevents → stream of recent events (open, close, retry, ...)
```

These endpoints integrate natively with Prometheus, Grafana, and any Micrometer-compatible monitoring stack.

### Subscribing to Events at Runtime

To capture the last error per CB for production diagnostics, register a listener on the registry's event publisher:

```java
@PostConstruct
public void init() {
    // Intercept the creation of each new CB
    circuitBreakerRegistry.getEventPublisher().onEntryAdded(event -> {
        CircuitBreaker cb = event.getAddedEntry();

        // Subscribe to that CB's error events
        cb.getEventPublisher().onError(errorEvent -> {
            String msg = errorEvent.getThrowable().getClass().getSimpleName()
                       + ": " + errorEvent.getThrowable().getMessage();
            lastErrors.put(cb.getName(), msg);  // ConcurrentHashMap<String, String>
        });

        cb.getEventPublisher().onStateTransition(transitionEvent ->
            log.warn("CB [{}] {} → {}",
                cb.getName(),
                transitionEvent.getStateTransition().getFromState(),
                transitionEvent.getStateTransition().getToState())
        );
    });
}
```

### Resetting a Circuit Breaker

Useful after resolving an underlying issue (e.g. updated credentials, restored service):

```java
// Reset a single CB — clears the sliding window and returns to CLOSED
circuitBreakerRegistry.circuitBreaker("EXT_OrderService_getOrders-acme").reset();

// Reset all CBs
circuitBreakerRegistry.getAllCircuitBreakers().forEach(CircuitBreaker::reset);
```

In production, these reset operations should be exposed through secured admin endpoints (e.g. under `/actuator` with proper security configuration or a dedicated admin path).

## 9. Managing the HTTP Client: Single-Tenant and Multi-Tenant Approaches

### The `CheckedSupplier` Pattern: What It Is and Why It Is Not HTTP GET

Resilience4j wraps the HTTP call inside a **lambda** (`CheckedSupplier<T>`), a functional interface that supports checked exceptions:

```java
@FunctionalInterface
public interface CheckedSupplier<T> {
    T get() throws Throwable;  // "execute and return" — no relation to HTTP GET
}
```

The `.get()` method simply means "execute this function". There is no `.post()` or `.delete()` because this is not an HTTP API: the HTTP method (`GET`, `POST`, `PUT`...) is specified *inside* the lambda, passed to the underlying HTTP client.

The full chain is assembled as follows:

```java
// 1. The lambda encapsulates the actual HTTP call
CheckedSupplier<ResponseEntity<T>> supplier = () ->
    httpClient.exchange(url, HttpMethod.POST, entity, responseType);
//                           ↑ the HTTP method lives here

// 2. Decorate with CB and Retry — these do not execute the lambda yet
CheckedSupplier<ResponseEntity<T>> resilient =
    Retry.decorateCheckedSupplier(retry,
        CircuitBreaker.decorateCheckedSupplier(cb, supplier));

// 3. .get() triggers the entire chain
ResponseEntity<T> result = resilient.get();
```

### Single-Tenant Approach

In a single-tenant system, one CB per service is sufficient. The CB name is static and can simply reflect the service being called:

```java
CircuitBreaker cb    = circuitBreakerRegistry.circuitBreaker("OrderService", "external_config");
Retry          retry = retryRegistry.retry("OrderService", "external_config");

CheckedSupplier<ResponseEntity<List<OrderDTO>>> supplier = () ->
    httpClient.exchange(url, HttpMethod.GET, entity, new ParameterizedTypeReference<>() {});

try {
    return Retry.decorateCheckedSupplier(retry,
               CircuitBreaker.decorateCheckedSupplier(cb, supplier))
           .get()
           .getBody();
} catch (CallNotPermittedException e) {
    log.error("Circuit open for OrderService");
    return List.of();
} catch (Throwable e) {
    log.error("Error calling OrderService", e);
    return List.of();
}
```

The declarative approach with `@CircuitBreaker` (Spring AOP) is more concise but less flexible — see [§12](#12-dynamic-circuit-breakers-how-the-registry-works-and-when-to-use-annotations) for a detailed comparison.

### Multi-Tenant Approach

In a multi-tenant system, a CB is required for every **tenant + service** pair. CB names become dynamic and depend on the `tenantId` received at runtime:

```java
private <T> ResponseEntity<T> executeWithResilience(
        String tenantId, String serviceName, String configName,
        CheckedSupplier<ResponseEntity<T>> supplier) {

    // e.g. "EXT_OrderService_getOrders-acme"
    String instanceName = serviceName + "-" + tenantId;

    CircuitBreaker cb    = circuitBreakerRegistry.circuitBreaker(instanceName, configName);
    Retry          retry = retryRegistry.retry(instanceName, configName);

    try {
        return Retry.decorateCheckedSupplier(retry,
                   CircuitBreaker.decorateCheckedSupplier(cb, supplier))
               .get();
    } catch (CallNotPermittedException e) {
        log.error("Circuit OPEN | [{}]", instanceName);
        throw e;
    } catch (Throwable e) {
        log.error("Error | [{}] | {}", instanceName, e.getMessage());
        throw sneakyThrow(e);
    }
}
```

Calling services simply pass the `tenantId` and a lambda wrapping the HTTP call:

```java
public List<OrderDTO> getOrders(String tenantId) {
    final String label = "EXT_" + getClass().getSimpleName() + "_getOrders";
    final String url   = baseUrl + "/orders";

    try {
        return executeWithResilience(tenantId, label, "external_config",
                    () -> httpClient.exchange(url, HttpMethod.GET,
                              new HttpEntity<>(buildHeaders(tenantId)),
                              new ParameterizedTypeReference<List<OrderDTO>>() {}))
               .getBody();
    } catch (Throwable e) {
        log.error("{} [tenant: {}] — exception:", label, tenantId, e);
        return new ArrayList<>();  // local fallback
    }
}
```

### The Fallback: Explicit try/catch vs Annotation

**With the programmatic approach** (recommended for multi-tenant), the fallback is the `catch` block:

```java
try {
    return executeWithResilience(...).getBody();
} catch (Throwable e) {
    log.error("{} [tenant: {}] fallback triggered", label, tenantId, e);
    return new ArrayList<>();  // default value — must be local, never a remote call
}
```

Using `catch (Throwable e)` rather than `catch (Exception e)` is important: `CallNotPermittedException` (thrown when the CB is OPEN) is a `RuntimeException`, but `Error` subtypes and other non-`Exception` `Throwable`s would silently escape a narrower catch.

**With the annotation approach** (`@CircuitBreaker(fallbackMethod = ...)`), Spring AOP automatically invokes a fallback method with the same signature plus a trailing `Throwable` parameter:

```java
@CircuitBreaker(name = "OrderService", fallbackMethod = "getOrdersFallback")
public List<OrderDTO> getOrders(String tenantId) { ... }

public List<OrderDTO> getOrdersFallback(String tenantId, Throwable t) {
    log.error("Fallback getOrders [{}]", tenantId, t);
    return new ArrayList<>();
}
```

This approach works well for single-tenant systems but is not suitable for multi-tenant architectures because the CB name is static (→ [§12](#12-dynamic-circuit-breakers-how-the-registry-works-and-when-to-use-annotations)).

## 10. Summary Table: Hystrix vs Resilience4j

| Concept             | Hystrix (legacy)                                                     | Resilience4j (current)                                                                               |
|---------------------|----------------------------------------------------------------------|------------------------------------------------------------------------------------------------------|
| **Circuit Breaker** | `@HystrixCommand`                                                    | `@CircuitBreaker` (AOP) or programmatic via registry                                                 |
| **Fallback**        | `fallbackMethod` in the annotation (automatic)                       | Explicit `try/catch` (→ [§9](#9-managing-the-http-client-single-tenant-and-multi-tenant-approaches)) |
| **Timeout**         | `execution.isolation.thread.timeoutInMilliseconds` in the annotation | Managed by the HTTP client (ConnectTimeout + ReadTimeout)                                            |
| **Retry**           | Not native (required a separate library)                             | Native `Retry` module with exponential backoff (→ [§6](#6-the-retry-attempt-logic))                  |
| **Isolation**       | Separate thread pool per command                                     | Functional decorators on the calling thread (no overhead)                                            |
| **Multi-tenant**    | Not natively supported                                               | Dynamic CBs per tenant+service pair (→ [§7](#7-the-bulkhead-pattern-and-multi-tenant-isolation))     |
| **Configuration**   | Inline annotation parameters (verbose, scattered)                    | Centralized YAML with inheritance                                                                    |
| **Monitoring**      | Hystrix Dashboard (deprecated)                                       | `CircuitBreakerRegistry` + Spring Actuator (→ [§8](#8-monitoring-circuit-breakers))                  |
| **Spring Boot 3**   | Incompatible                                                         | Natively compatible                                                                                  |
| **Window type**     | Time-based                                                           | Count-based sliding window (→ [§5](#5-the-circuit-breaker-how-it-works-and-how-it-trips))            |

## 11. General Best Practices

### Decorator Order: Retry OUTSIDE, CB INSIDE (mandatory)

```sh
Retry [ CircuitBreaker [ HTTP call ] ]   ← CORRECT
CircuitBreaker [ Retry [ HTTP call ] ]   ← INCORRECT
```

If the CB were external to the Retry, an OPEN CB would block the entire chain before the Retry could intervene. The correct order ensures that every individual Retry attempt passes through the CB: if the CB opens during the second attempt, the third is blocked immediately.

### Jitter on Exponential Backoff

Without jitter, all threads that failed at the same moment will wait for exactly the same interval and retry *simultaneously*, recreating the original load spike — a well-known problem called the **Thundering Herd**.

Resilience4j does not expose jitter via YAML; it must be configured programmatically:

```java
RetryConfig config = RetryConfig.custom()
    .maxAttempts(3)
    .intervalBiFunction((attempt, result) -> {
        long base   = 5000L * (long) Math.pow(2, attempt - 1); // 5s, 10s, 20s
        long jitter = (long)(Math.random() * base * 0.3);      // ±30% random spread
        return base + jitter;
    })
    .build();
```

### `automatic-transition-from-open-to-half-open-state`

Default: `false`. With the default, the CB remains OPEN until a new real call arrives after `wait-duration-in-open-state`. In production, if traffic drops (e.g. off-hours), the CB can remain unnecessarily OPEN even after the downstream service has recovered.

Setting this to `true` enables an automatic background transition:

```yaml
circuitbreaker:
  configs:
    external_config:
      automatic-transition-from-open-to-half-open-state: true
```

### `permitted-number-of-calls-in-half-open-state`

Default: `1`. With a single probe in HALF_OPEN, one unlucky result is enough to re-open the CB. Raising this to 3–5 makes the HALF_OPEN → CLOSED transition more statistically reliable:

```yaml
circuitbreaker:
  configs:
    external_config:
      permitted-number-of-calls-in-half-open-state: 3
```

### Sliding Window: `COUNT_BASED` vs `TIME_BASED`

| Type                    | When to use it                                                          |
|-------------------------|-------------------------------------------------------------------------|
| `COUNT_BASED` (default) | Irregular traffic: observes the last N calls regardless of elapsed time |
| `TIME_BASED`            | Steady, predictable volume: observes calls within the last N seconds    |

In multi-tenant systems where not all tenants are active simultaneously, `COUNT_BASED` is generally the better choice: an empty time window for an idle tenant conveys nothing about its actual health.

### Fallbacks Must Not Make Remote Calls

A fallback that calls another external service can itself fail, nesting errors and potentially triggering additional Circuit Breakers. Fallbacks should be **strictly local**:

```java
// Correct: local fallback — no outbound calls
catch (Throwable e) { return new ArrayList<>(); }
catch (Throwable e) { return cachedResult; }
catch (Throwable e) { return ResponseDTO.empty(); }

// Avoid: fallback that calls another system
catch (Throwable e) { return backupService.getFromAlternativeSource(); }
```

### Do Not Mix Annotations and Programmatic Approach

Placing `@CircuitBreaker` on a method that internally delegates to a centralized client with an already-integrated CB results in two nested Circuit Breakers: duplicated logic, divergent state, and confusing observability.

## 12. Dynamic Circuit Breakers: How the Registry Works and When to Use Annotations

### `CircuitBreakerRegistry` Is a Glorified Concurrent Map

```java
CircuitBreaker cb = circuitBreakerRegistry.circuitBreaker("breakerName", "configName");
```

Internally, this call performs the following:

```sh
1. Does a CB named "breakerName" already exist?
   → YES: returns the same instance (same sliding window, same state)
   → NO:  reads "configName" from YAML, creates a new CB, stores it in the internal map
```

The created CB has **persistent in-memory state** (the sliding window is backed by a circular array). Since the registry treats it as a singleton by name, its state survives across calls for the entire application lifetime.

When `CircuitBreaker.decorateCheckedSupplier(cb, supplier)` is called, the result is a new lambda equivalent to:

```java
() -> {
    cb.acquirePermission();           // throws CallNotPermittedException if OPEN
    long start = System.nanoTime();
    try {
        T result = supplier.get();    // executes the actual HTTP call
        cb.onSuccess(elapsed, NANOSECONDS);
        return result;
    } catch (Throwable t) {
        cb.onError(elapsed, NANOSECONDS, t); // updates the sliding window
        throw t;
    }
}
```

### `@CircuitBreaker` Annotation vs Programmatic Approach

The annotation uses **Spring AOP**: Spring intercepts the method call via a proxy and applies the CB logic before the implementation is reached.

| Aspect                | `@CircuitBreaker` (AOP)              | Programmatic Approach               |
|-----------------------|--------------------------------------|-------------------------------------|
| CB name               | Static, hardcoded in source          | Dynamic, computed at runtime        |
| Per-tenant isolation  | NO: one CB shared across all tenants | YES: one CB per tenant+service pair |
| Self-invocation       | Does not work (same bean proxy)      | Always works                        |
| Decorator composition | Only via stacked annotations         | Full control over ordering          |
| Fallback              | Separate method via AOP              | Explicit `try/catch` in the method  |

**The annotation is the right choice when:**

- The system is single-tenant (one CB per service is sufficient)
- The CB name does not depend on runtime parameters
- Zero boilerplate is a priority

**The programmatic approach is required when:**

- The CB name depends on a method parameter (e.g. `tenantId`)
- Explicit multi-decorator composition with guaranteed ordering is needed
- Granular per-tenant monitoring is desired (→ [§8](#8-monitoring-circuit-breakers))

> **Technical note:** even with `@CircuitBreaker`, the CB is registered in `CircuitBreakerRegistry` and is visible via Actuator or the registry API. The limitation is the static name: with 100 tenants, you would have a single shared CB instead of 100 isolated ones.

## 13. RestClient vs RestTemplate: What Changes with Resilience4j

The short answer is: **almost nothing**.

`CheckedSupplier` is a Resilience4j functional interface — it is entirely agnostic of what runs inside the lambda. Replacing `RestTemplate` with `RestClient` requires no changes whatsoever to the resilience logic:

```java
// With RestTemplate
CheckedSupplier<ResponseEntity<T>> supplier = () ->
    restTemplate.exchange(url, method, entity, responseType);

// With RestClient — the Resilience4j chain is identical; only the lambda body changes
CheckedSupplier<ResponseEntity<T>> supplier = () ->
    restClient.method(method).uri(url)
              .headers(h -> h.addAll(entity.getHeaders()))
              .body(entity.getBody())
              .retrieve()
              .toEntity(responseType);
```

The **exception hierarchy** is also identical (`HttpClientErrorException`, `ResourceAccessException`, etc.), so `retry-exceptions` and `ignore-exceptions` defined in YAML continue to work without modification.

The most relevant practical difference is in timeout configuration: `RestClient` requires an explicit `RequestFactory` rather than the builder-level setters available on `RestTemplate`. The runtime behaviour is equivalent.

## Conclusion: the Flow in Five Lines

1. A service delegates to a centralized HTTP client, passing `tenantId`, a `serviceLabel`, and a lambda wrapping the HTTP call.
2. The client retrieves (or lazily creates) a **Circuit Breaker** and a **Retry** scoped to that specific `tenant+service` pair — enforcing multi-tenant isolation.
3. The call is wrapped in the chain `Retry [ CB [ HTTP ] ]`; `resilientSupplier.get()` triggers execution.
4. On a retriable failure, the **Retry** waits with exponential backoff and tries again, passing through the CB on each attempt.
5. When the CB accumulates too many failures (≥50% of the sliding window), it **OPENS** and all subsequent calls fail immediately — without ever contacting the remote service.
