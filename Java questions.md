# Java Q&A

## Table of Contents

### Data Structures & Algorithms

1. [Arrays & Strings](#arrays--strings)
   - [ArrayList vs LinkedList](#arraylist-vs-linkedlist)
   - [Reverse a String or Array](#reverse-a-string-or-array)
   - [Find Duplicate in Array](#find-duplicate-in-array)
   - [Two Sum Problem](#two-sum-problem)
   - [Rotate Array](#rotate-array)
   - [Anagram Check](#anagram-check)
2. [Map](#map) — HashMap vs TreeMap vs LinkedHashMap
3. [Stacks & Queues](#stacks--queues) — Stack, Queue, Deque, Valid Parentheses
4. [Heaps](#heaps)
5. [Log (Logarithms)](#log)
6. [Power (Exponentiation)](#power)

### Java Language

1. [Java Core Concepts](#java-core-concepts)
   - [Java 8 Features](#what-are-the-main-features-of-java-8) — Lambdas, Streams, Optional, default methods
   - [Java Memory Management](#explain-the-concept-of-java-memory-management) — Heap, Stack, GC
   - [`==` vs `equals()`](#what-is-the-difference-between--and-equals-in-java)
   - [Garbage Collector](#how-does-the-java-garbage-collector-work)
   - [Exception Types](#what-are-the-different-types-of-exceptions-in-java)
   - [Java 11 vs 17 vs 21](#differences-between-java-11-17-and-java-21) — Records, Sealed classes, Virtual Threads

---

## Arrays & Strings

### ArrayList vs LinkedList

| Feature                 | ArrayList                       | LinkedList                        |
|-------------------------|---------------------------------|-----------------------------------|
| Structure               | Dynamic array                   | Doubly linked list                |
| Access Time             | ✅ O(1) for index-based access   | ❌ O(n) for index-based access     |
| Insert/Delete at End    | ✅ Fast (amortized O(1))         | ✅ Fast (O(1))                     |
| Insert/Delete in Middle | ❌ Slower (O(n)) due to shifting | ✅ Fast if you have a reference    |
| Memory Usage            | Less (just data)                | More (data + 2 pointers per node) |
| Use Case                | Random access, more reads       | Frequent add/remove at head/tail  |

### Reverse a String or Array

```Java
public String reverseString(String s) {
    return new StringBuilder(s).reverse().toString();
}
```

```Java
public void reverseArray(int[] arr) {
   int left = 0;
   int right = arr.length - 1;  // Note: length is a property, not a method

   while(left < right) {
      int tmp = arr[left];
      arr[left++] = arr[right];
      arr[right--] = tmp;
   }
}
```

### Find duplicate in array

We use `add(e)` method of `Set`, which returns a `boolean`: `true` if `e` is inserted, `false` otherwhise

```Java
public int findDuplicate(int[] nums) {
   Set<Integer> set = new HashSet<>();
   for(int num : nums) {
      if(!set.add(num)) //We can't add num because it's already there
         return num;
   }
   return -1;
}
```

#### Using HashMap to Find Duplicates

Why `HashMap`?

- Constant time lookup (O(1) average).
- Helps you count occurrences quickly.
- Useful for not just detecting, but tracking frequency.

```Java
public List<Integer> findDuplicates(int[] nums) {
   Map<Integer, Integer> map = new HashMap<>();
   List<Integer> duplicates = new ArrayList<>();

   for(int num : nums) {
      map.put(num, map.getOrDefault(num, 0) + 1);
   }

   for(Map.Entry<Integer, Integer> entry : map.entrySet()) {
       if (entry.getValue() > 1)
            duplicates.add(entry.getKey());
   }
   return duplicates;
}
```

#### Java Map Methods Cheat Sheet

**Basic Operations:**

| Method                        | Description                                   | Example                         | Time Complexity |
|-------------------------------|-----------------------------------------------|---------------------------------|-----------------|
| `put(K key, V value)`         | Insert or update key-value pair               | `map.put("a", 1)`               | O(1) avg        |
| `get(Object key)`             | Get value by key, returns `null` if not found | `map.get("a")` → `1`            | O(1) avg        |
| `remove(Object key)`          | Remove key-value pair, returns value          | `map.remove("a")` → `1`         | O(1) avg        |
| `containsKey(Object key)`     | Check if key exists                           | `map.containsKey("a")` → `true` | O(1) avg        |
| `containsValue(Object value)` | Check if value exists                         | `map.containsValue(1)` → `true` | O(n)            |
| `size()`                      | Get number of entries                         | `map.size()` → `3`              | O(1)            |
| `isEmpty()`                   | Check if map is empty                         | `map.isEmpty()` → `false`       | O(1)            |
| `clear()`                     | Remove all entries                            | `map.clear()`                   | O(n)            |

**Default Value Methods:**

| Method                                | Description                                             | Example                          |
|---------------------------------------|---------------------------------------------------------|----------------------------------|
| `getOrDefault(K key, V defaultValue)` | Get value or return default if key doesn't exist        | `map.getOrDefault("x", 0)` → `0` |
| `putIfAbsent(K key, V value)`         | Insert only if key doesn't exist, returns existing/null | `map.putIfAbsent("a", 5)`        |

**Iteration Methods:**

| Method       | Description                         | Example                                      |
|--------------|-------------------------------------|----------------------------------------------|
| `keySet()`   | Returns a Set view of keys          | `for(String key : map.keySet())`             |
| `values()`   | Returns a Collection view of values | `for(Integer val : map.values())`            |
| `entrySet()` | Returns a Set of key-value pairs    | `for(Map.Entry<K,V> entry : map.entrySet())` |

**Bulk Operations:**

| Method                                   | Description                                | Example                   |
|------------------------------------------|--------------------------------------------|---------------------------|
| `putAll(Map m)`                          | Copy all entries from another map          | `map.putAll(otherMap)`    |
| `replace(K key, V value)`                | Replace value for key (only if key exists) | `map.replace("a", 10)`    |
| `replace(K key, V oldValue, V newValue)` | Replace only if current value matches      | `map.replace("a", 1, 10)` |

**Compute Methods (Java 8+):**

| Method                                | Description                                      | Use Case                        |
|---------------------------------------|--------------------------------------------------|---------------------------------|
| `compute(K key, BiFunction)`          | Compute new value based on key and current value | Update based on key-value logic |
| `computeIfAbsent(K key, Function)`    | Compute value only if key is absent              | Initialize nested structures    |
| `computeIfPresent(K key, BiFunction)` | Compute value only if key exists                 | Update existing values          |
| `merge(K key, V value, BiFunction)`   | Merge new value with existing value              | Combining/aggregating values    |

**Common Interview Patterns:**

```Java
// 1. Frequency Counter
Map<Character, Integer> freq = new HashMap<>();
for(char c : str.toCharArray()) {
    freq.put(c, freq.getOrDefault(c, 0) + 1);
    // OR using merge:
    // freq.merge(c, 1, Integer::sum);
}

// 2. Group By Pattern
Map<String, List<String>> groups = new HashMap<>();
for(String item : items) {
    String key = getGroupKey(item);
    groups.computeIfAbsent(key, k -> new ArrayList<>()).add(item);
}

// 3. Two Sum Pattern (value -> index)
Map<Integer, Integer> map = new HashMap<>();
for(int i = 0; i < nums.length; i++) {
    int complement = target - nums[i];
    if(map.containsKey(complement)) {
        return new int[]{map.get(complement), i};
    }
    map.put(nums[i], i);
}

// 4. Check for Duplicates
Set<Integer> seen = new HashSet<>();
for(int num : nums) {
    if(!seen.add(num)) {
        return true; // duplicate found
    }
}

// 5. Iterate Over Entries
for(Map.Entry<String, Integer> entry : map.entrySet()) {
    System.out.println(entry.getKey() + ": " + entry.getValue());
}

// 6. Using compute for complex updates
map.compute("count", (key, val) -> (val == null) ? 1 : val + 1);

// 7. Using computeIfAbsent for nested maps
Map<String, Map<String, Integer>> nested = new HashMap<>();
nested.computeIfAbsent("user1", k -> new HashMap<>())
        .put("score", 100);

// 8. Using merge for aggregation
Map<String, Integer> scores = new HashMap<>();
scores.merge("player1", 10, Integer::sum); // adds 10 to existing or sets to 10

// 9. GetorDefault to avoid null checks
int count = map.getOrDefault("key", 0);

// 10. Rate limiter pattern
public boolean isUtenteAbilitato(String utenteId) {

  if (!utenti.containsKey(utenteId)) {
    utenti.put(utenteId, new ArrayList<>());
    return true; // Non ci sono richieste precedenti -> utente abilitato
  }

  List<LocalDateTime> richieste = utenti.get(utenteId);

  LocalDateTime now = LocalDateTime.now();
  richieste.removeIf(timestamp -> timestamp.isBefore(now.minusHours(1)));

  int numeroRichieste = richieste.size();

  if (numeroRichieste < LIMITE_RICHIESTE) {
    richieste.add(now);
    return true;
  }

  return false;
}
```

**Quick Tips:**

- Use `getOrDefault()` to avoid null checks when counting
- Use `computeIfAbsent()` for initializing nested collections
- Use `merge()` for simple aggregations (counting, summing)
- Use `entrySet()` when you need both key and value during iteration
- `containsKey()` is O(1) but `containsValue()` is O(n)
- HashMap allows one `null` key and multiple `null` values

Java HashMap Methods Reference: <https://www.w3schools.com/java/java_ref_hashmap.asp>

### Two Sum Problem

Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to the target.

Assumptions:

- Each input has exactly one solution
- You may not use the same element twice

```Java
Input: nums = [2, 7, 11, 15], target = 9  
Output: [0, 1]
```

#### Brute force (O(n²))

```Java
public int[] twoSum(int[] nums, int target) {
    for (int i = 0; i < nums.length; i++) {
        for (int j = i + 1; j < nums.length; j++) {
            if (nums[i] + nums[j] == target) {
                return new int[]{i, j};
            }
        }
    }
    return new int[0]; // if no solution found
}

```

#### Optimal Solution (Using HashMap, O(n))

```Java
public int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>(); // value -> index

    for (int i = 0; i < nums.length; i++) {
        int complement = target - nums[i]; // what number would complete the sum?
        if (map.containsKey(complement)) {
            return new int[]{i, map.get(complement)}; // found the pair
        }
        map.put(nums[i], i);
    }

    return new int[0];
}
```

You go through the array once:

- Time: O(n) – single pass through the array.
- Space: O(n) – for storing elements in the HashMap.

#### What if the array is sorted?

We use two pointers

```Java
public int[] twoSumSorted(int[] nums, int target) {
   int left = 0;
   int right = nums.length - 1;

   while (left < right) {
      int sum = nums[left] + nums[right];

      if(sum == target)
         return new int[]{left, right};
      else if(sum < target)
         left++;   // sum too small, move left pointer right
      else
         right--;  // sum too large, move right pointer left
   }

   return new int[0];
}   
```

### Rotate Array

Given an array, rotate it to the right by k steps, where k is non-negative.

```Java
Input: nums = [1, 2, 3, 4, 5, 6, 7], k = 3  
Output: [5, 6, 7, 1, 2, 3, 4]
```

```Java
public void rotate(int[] nums, int k) {
    int n = nums.length;
    k = k % n; // handle cases where k > n

    reverse(nums, 0, n - 1);      // 1. Reverse entire array
    reverse(nums, 0, k - 1);      // 2. Reverse first k elements
    reverse(nums, k, n - 1);      // 3. Reverse the rest
}

private void reverse(int[] nums, int start, int end) {
    while (start < end) {
        int temp = nums[start];
        nums[start++] = nums[end];
        nums[end--] = temp;
    }
}
```

### Anagram check

#### Check by sorting (O(n log n))

```Java
public boolean isAnagram(String a, String b) {
   if(a.length() != b.length())  // length() is a method for String
      return false;

   char[] a1 = a.toCharArray();  // use parameter names consistently
   char[] a2 = b.toCharArray();
   Arrays.sort(a1);
   Arrays.sort(a2);
   return Arrays.equals(a1, a2);
}
```

Let's break it down:

- converting to char[] → O(n)
- sorting each array → O(n log n)
- comparing arrays → O(n)

👉 Overall: O(n log n)

#### Check by counting

If you're only dealing with lowercase letters, you can use a frequency count (array of size 26) for O(n) time and O(1) space

```Java
public boolean isAnagram(String s, String t) {
    if (s.length() != t.length())
        return false;

    char[] first = s.toLowerCase().toCharArray();  // use parameter names
    char[] second = t.toLowerCase().toCharArray();

    int[] count = new int[26];  // frequency array for 26 lowercase letters
    for (int i = 0; i < first.length; i++) {  // length is a property for arrays
        count[first[i] - 'a']++;   // increment for chars in first string
        count[second[i] - 'a']--;  // decrement for chars in second string
    }

    // If anagrams, all counts should be zero
    for (int c : count) {
        if (c != 0)
            return false;
    }

    return true;
}
```

## Map

### HashMap vs TreeMap vs LinkedHashMap

Feature | HashMap | TreeMap | LinkedHashMap
|-|-|-
Ordering | ❌ No ordering | ✅ Sorted by keys (natural or custom) | ✅ Insertion order preserved
Time Complexity | O(1) for get/put (avg) | O(log n) for get/put | O(1) for get/put
Null keys/values | 1 null key, many null values | ❌ No null keys | ✅ 1 null key, many null values
Backed By | Hash table | Red-Black tree | Hash table + doubly linked list
Use Case | Fast lookup, no order | Sorted map view | Ordered iteration (e.g. LRU cache)

Summary:

- `HashMap`: fast key-value lookup with no order.
- `TreeMap`:  sorted keys.
- `LinkedHashMap`: maintain insertion order or implement something like LRU cache

## Stacks & Queues

### `Stack`

- Last In First Out (LIFO) or First In Last Out (FILO) ordering
- `push(e)` and `pop` operations are performed on `top` element (last inserted)
- `peek` returns the top element of the stack without removing it. If the stack is empty, it throws an exception.

### `Queue`

- First In First Out (FIFO) ordering
- elements can be **inserted** (`add(e)`/`offer(e)`) at the rear(back) of the queue and elements can be **deleted** (`remove`/`poll`) from the front(head) of the queue
- `element`/`peek` return the top element of the stack without removing it. If the stack is empty, `elements` throws an exception while `peek` returns null.

### `Deque` (double ended queue)

Elements can be inserted or deleted either **from both** front(head) or rear(tail) ends

Instead of using the older Stack class (which is synchronized and legacy), Java recommends using Deque (via ArrayDeque) for stack operations.

- `Stack` is based on `Vector`, which is synchronized and generally slower.

- `Deque` (ArrayDeque) is faster, more modern, and **non-thread-safe** (which is good unless you need concurrency).

| Stack Operation | Deque Method (LIFO)       | Description             |
|-----------------|---------------------------|-------------------------|
| `push(x)`       | `push(x)` / `addFirst(x)` | Add to top of stack     |
| `pop()`         | `pop()` / `removeFirst()` | Remove top of stack     |
| `peek()`        | `peek()` / `getFirst()`   | View top of stack       |
| `isEmpty()`     | `isEmpty()`               | Check if stack is empty |

### Valid Parentheses

```Java
public boolean isValid(String s) {
    Deque<Character> stack = new ArrayDeque<>();
    
    for(char c : s.toCharArray()) {
        // Push opening brackets onto stack
        if(c == '(' || c == '[' || c == '{') {
            stack.push(c);
        }
        // For closing brackets, check if they match
        else {
            if(stack.isEmpty()) return false;  // no matching opening bracket
            
            char top = stack.pop();
            if((c == ')' && top != '(') ||
               (c == ']' && top != '[') ||
               (c == '}' && top != '{')) {
                return false;  // mismatched pair
            }
        }
    }
    
    return stack.isEmpty();  // all brackets should be matched
}
```

## Heaps

| Aspect             | Stack Memory                         | Heap Memory                            |
|--------------------|--------------------------------------|----------------------------------------|
| Stores             | Method calls, local primitives, refs | Objects (and their instance variables) |
| Size               | Smaller, managed per thread          | Larger, shared across threads          |
| Lifetime           | Short-lived (until method returns)   | Long-lived (until GC collects it)      |
| Speed              | Very fast (LIFO structure)           | Slower than stack                      |
| Garbage Collected? | ❌ No, auto-removed when method ends  | ✅ Yes, cleaned by garbage collector    |

```Java
void doSomething() {
    int x = 5;               // x is on stack
    String s = new String(); // s (ref) is on stack, actual object on heap
}
```

## Log

```Java
private void logOperazione(String tipo, double importo) {
        String timestamp = LocalDateTime.now().format(DateTimeFormatter.ISO_LOCAL_DATE_TIME);
        String logLine = String.format("[%s] %s: %.2f\n", timestamp, tipo, importo);

        try (FileWriter fw = new FileWriter("log_operazioni.txt", true)) {
            fw.write(logLine);
        } catch (IOException e) {
            System.err.println("Errore durante il logging: " + e.getMessage());
        }
    }
```

## Power

```java
// O(n) solution
public static int power(int base, int exp) {
    int result = 1;

    for (int i = 0; i < exp; i++) {
        result *= base;
    }
    
    return result;
}


// O(log n) solution
public static int power(int base, int exp) {
    if (exp == 0)
        return 1;
    
    if (exp == 1)
        return base;
    

    int half = power(base, exp / 2);

    if (exp % 2 == 0)
        return half * half;
    else
        return half * half * base;
}
```

## Java Core Concepts

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

| Aspect           | Java 11          | Java 17          |
|------------------|------------------|------------------|
| Status           | LTS (until 2026) | LTS (until 2029) |
| Spring Boot      | 2.x              | 2.x and 3.x      |
| Records          | Not available    | Final            |
| Sealed Classes   | Not available    | Final            |
| Pattern Matching | Not available    | instanceof       |
| Text Blocks      | Not available    | Final            |

**From Java 17 to Java 21:**

| Aspect                 | Java 17         | Java 21          |
|------------------------|-----------------|------------------|
| Virtual Threads        | Not available   | Final            |
| Pattern Matching       | instanceof only | switch + records |
| Sequenced Collections  | Not available   | Final            |
| String Templates       | Not available   | Preview          |
| Structured Concurrency | Not available   | Preview          |

**Common Migration Issues:**

1. **Removed APIs**: Some deprecated APIs removed (e.g., `SecurityManager`)
2. **Strong encapsulation**: `--add-opens` may be needed for reflection
3. **Module system**: Some internal APIs require explicit module access
4. **Build tools**: Update Maven/Gradle plugins for compatibility
