# Java Algorhitms and Structures Q&A

- [Java Algorhitms and Structures Q\&A](#java-algorhitms-and-structures-qa)
  - [Arrays \& Strings](#arrays--strings)
    - [ArrayList vs LinkedList](#arraylist-vs-linkedlist)
    - [Reverse a String or Array](#reverse-a-string-or-array)
    - [Find duplicate in array](#find-duplicate-in-array)
      - [Using HashMap to Find Duplicates](#using-hashmap-to-find-duplicates)
        - [**TODO**: Study Java HashMap Methods (https://www.w3schools.com/java/java\_ref\_hashmap.asp)](#todo-study-java-hashmap-methods-httpswwww3schoolscomjavajava_ref_hashmapasp)
    - [Two Sum Problem](#two-sum-problem)
      - [Brute force (O(n²))](#brute-force-on)
      - [Optimal Solution (Using HashMap, O(n))](#optimal-solution-using-hashmap-on)
      - [What if the array is sorted?](#what-if-the-array-is-sorted)
    - [Rotate Array](#rotate-array)
    - [Anagram check](#anagram-check)
      - [Check by sorting (O(n log n))](#check-by-sorting-on-log-n)
      - [Check by counting](#check-by-counting)
  - [Map](#map)
    - [HashMap vs TreeMap vs LinkedHashMap](#hashmap-vs-treemap-vs-linkedhashmap)
  - [Stacks \& Queues](#stacks--queues)
    - [`Stack`](#stack)
    - [`Queue`](#queue)
    - [`Deque` (double ended queue)](#deque-double-ended-queue)
    - [Valid Parentheses](#valid-parentheses)
  - [Heaps](#heaps)
  - [Log](#log)


## Arrays & Strings

### ArrayList vs LinkedList

Feature | ArrayList | LinkedList
|-|-|-|
Structure | Dynamic array | Doubly linked list
Access Time | ✅ O(1) for index-based access | ❌ O(n) for index-based access
Insert/Delete at End | ✅ Fast (amortized O(1)) | ✅ Fast (O(1))
Insert/Delete in Middle | ❌ Slower (O(n)) due to shifting | ✅ Fast if you have a reference
Memory Usage | Less (just data) | More (data + 2 pointers per node)
Use Case | Random access, more reads | Frequent add/remove at head/tail

### Reverse a String or Array 

```Java
public String reverseString(String s) {
    return new StringBuilder(s).reverse().toString();
}
```

```Java
public void reverseArray(int[] arr) {
   int left = 0;
   int right = arr.length() - 1;

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
public List<int> findDuplicate(int[] nums) {
   Map<Integer, Integer> map = new HashMap<>();
   List<Integer> duplicates = new ArrayList<>();

   for(int num : nums) {
      map.put(num, map.getOrDefault(num, 0) + 1);
   }

   for(Map<Integer, Integer> entry : map.entrySet()) {
       if (entry.getValue() > 1)
            duplicates.add(entry.getKey());
   }
   return duplicates;
}
```

##### **TODO**: Study Java HashMap Methods (https://www.w3schools.com/java/java_ref_hashmap.asp)

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
   int right = nums.length() - 1;

   while (left < right) {
      int sum = nums[left] + nums[right];

      if(sum == target)
         return new int[]{left, right};
      else if(sum < target)
         left++;
      else
         right--;
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
    k = k % nums.length; // handle cases where k > n

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
   if(a.length != b.length)
      return false;

   char[] a1 = string1.toCharArray();
   char[] a2 = string2.toCharArray();
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

    char[] first = stringOne.toLowerCase().toCharArray(); 
    char[] second = stringTwo.toLowerCase().toCharArray();

    int[] count = new int[26];
    for (int i = 0; i < first.length(); i++) {
        count[first[i] - 'a']++;
        count[second[i] - 'a']--;
    }

    for (int c : count) {
        if (c != 0)
            return false;
    }

    return true;
}
```

## Map

### HashMap vs TreeMap vs LinkedHashMap

Feature | HashMap | TreeMap | LinkedHashMap |
|-|-|-|-|
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

| Stack Operation | Deque Method (LIFO)        | Description             |
|------------------|----------------------------|--------------------------|
| `push(x)`        | `push(x)` / `addFirst(x)`  | Add to top of stack      |
| `pop()`          | `pop()` / `removeFirst()`  | Remove top of stack      |
| `peek()`         | `peek()` / `getFirst()`    | View top of stack        |
| `isEmpty()`      | `isEmpty()`                | Check if stack is empty  |



### Valid Parentheses

```Java
public boolean isValid(String s) {
    Deque<Character> stack = new ArrayDeque<>();

    for(chat c : s.toCharArray()) {
        if(c == '(' || c == '[' || c == '{')
            stack.push(c);
        else if(stack.isEmpty() || stack.pop() != c)
            return false;
    }
    
    return stack.isEmpty();
}
```

## Heaps

Aspect | Stack Memory | Heap Memory
| - | -| -
Stores | Method calls, local primitives, refs | Objects (and their instance variables)
Size | Smaller, managed per thread | Larger, shared across threads
Lifetime | Short-lived (until method returns) | Long-lived (until GC collects it)
Speed | Very fast (LIFO structure) | Slower than stack
Garbage Collected? | ❌ No, auto-removed when method ends | ✅ Yes, cleaned by garbage collector

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