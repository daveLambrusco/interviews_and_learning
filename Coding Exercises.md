# Coding Exercises

Common interview coding exercises with solutions in JavaScript/TypeScript and Java.

## Table of Contents

1. [Map Methods Cheat Sheet (JavaScript/TypeScript)](#map-methods-cheat-sheet-javascripttypescript)
   - [The Map Object](#the-map-object) — set, get, has, delete, size, clear
   - [Iterating a Map](#iterating-a-map) — forEach, for...of, keys(), values(), entries()
   - [Convert Between Map and Object/Array](#convert-between-map-and-objectarray)
   - [Array Higher-Order Methods Cheat Sheet](#array-higher-order-methods-cheat-sheet) — map, filter, reduce, find, some, every, flat
   - [Java Map/HashMap Cheat Sheet](#java-maphashmap-cheat-sheet)
2. [Two Sum (HashMap Approach)](#two-sum-hashmap-approach)
   - [TypeScript Solution O(n)](#typescript-solution-on-using-map)
   - [Java Solution](#java-solution)
   - [Step-by-Step Walkthrough](#step-by-step-walkthrough)
   - [Common Variations](#common-variations)

---

## Map Methods Cheat Sheet (JavaScript/TypeScript)

A quick reference for all `Map` methods and array higher-order functions commonly used in interview coding exercises.

### The Map Object

`Map` holds key-value pairs where keys can be **any type** (unlike plain objects where keys are strings/symbols).

```typescript
const map = new Map<string, number>();

// SET — Add or update an entry (returns the Map itself, chainable)
map.set('a', 1);
map.set('b', 2).set('c', 3);  // Chaining

// GET — Retrieve value by key (returns undefined if not found)
map.get('a');    // 1
map.get('z');    // undefined

// HAS — Check if a key exists (returns boolean)
map.has('a');    // true
map.has('z');    // false

// DELETE — Remove an entry by key (returns true if existed)
map.delete('b'); // true
map.delete('z'); // false

// SIZE — Number of entries (property, not method)
map.size;        // 2

// CLEAR — Remove all entries
map.clear();
map.size;        // 0
```

### Iterating a Map

```typescript
const map = new Map([['x', 10], ['y', 20], ['z', 30]]);

// forEach
map.forEach((value, key) => console.log(`${key}: ${value}`));

// for...of (entries — default iterator)
for (const [key, value] of map) {
  console.log(`${key}: ${value}`);
}

// keys() — Iterator of keys
[...map.keys()];    // ['x', 'y', 'z']

// values() — Iterator of values
[...map.values()];  // [10, 20, 30]

// entries() — Iterator of [key, value] pairs
[...map.entries()]; // [['x', 10], ['y', 20], ['z', 30]]
```

### Convert Between Map and Object/Array

```typescript
// Object → Map
const obj = { a: 1, b: 2 };
const map = new Map(Object.entries(obj));

// Map → Object
const obj2 = Object.fromEntries(map);

// Map → Array of pairs
const arr = [...map]; // [['a', 1], ['b', 2]]

// Array of pairs → Map
const map2 = new Map([['a', 1], ['b', 2]]);
```

### Array Higher-Order Methods Cheat Sheet

```typescript
const nums = [1, 2, 3, 4, 5];

// MAP — Transform each element (returns new array)
nums.map(n => n * 2);           // [2, 4, 6, 8, 10]

// FILTER — Keep elements matching condition
nums.filter(n => n > 3);        // [4, 5]

// REDUCE — Accumulate into single value
nums.reduce((sum, n) => sum + n, 0);  // 15

// FIND — First element matching condition (or undefined)
nums.find(n => n > 3);          // 4

// FINDINDEX — Index of first match (or -1)
nums.findIndex(n => n > 3);     // 3

// SOME — At least one matches? (returns boolean)
nums.some(n => n > 4);          // true

// EVERY — All match? (returns boolean)
nums.every(n => n > 0);         // true

// FLATMAP — Map + flatten one level
[[1, 2], [3, 4]].flatMap(x => x);  // [1, 2, 3, 4]
['hello world', 'foo bar'].flatMap(s => s.split(' '));  // ['hello', 'world', 'foo', 'bar']

// FLAT — Flatten nested arrays
[1, [2, [3]]].flat();           // [1, 2, [3]]
[1, [2, [3]]].flat(Infinity);   // [1, 2, 3]

// SORT — Sort in place (mutates!)
[3, 1, 2].sort((a, b) => a - b); // [1, 2, 3]

// FOREACH — Side effects only (returns undefined)
nums.forEach(n => console.log(n));
```

### Java Map/HashMap Cheat Sheet

```java
Map<String, Integer> map = new HashMap<>();

map.put("a", 1);                      // Add/update
map.get("a");                         // 1
map.getOrDefault("z", 0);             // 0
map.containsKey("a");                 // true
map.containsValue(1);                 // true
map.remove("a");                      // Remove
map.size();                           // Size
map.isEmpty();                        // Check empty
map.putIfAbsent("b", 2);             // Only add if absent
map.computeIfAbsent("c", k -> 3);    // Compute if absent
map.merge("a", 1, Integer::sum);     // Merge (great for counting)

// Iterate
map.forEach((k, v) -> System.out.println(k + ": " + v));
for (Map.Entry<String, Integer> entry : map.entrySet()) {
    entry.getKey();
    entry.getValue();
}

// Streams
map.entrySet().stream()
    .filter(e -> e.getValue() > 1)
    .map(Map.Entry::getKey)
    .collect(Collectors.toList());
```

---

## Two Sum (HashMap Approach)

> **Problem:** Given an array of integers `nums` and an integer `target`, return the indices of the two numbers that add up to `target`. Assume exactly one solution exists and you may not use the same element twice.

### TypeScript Solution (O(n) using Map)

```typescript
function twoSum(nums: number[], target: number): number[] {
  const map = new Map<number, number>(); // value → index

  for (let i = 0; i < nums.length; i++) {
    const complement = target - nums[i];

    if (map.has(complement)) {
      return [map.get(complement)!, i];
    }

    map.set(nums[i], i);
  }

  throw new Error('No solution found');
}

// Example
twoSum([2, 7, 11, 15], 9); // [0, 1] because 2 + 7 = 9
```

### Java Solution

```java
public int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>(); // value → index

    for (int i = 0; i < nums.length; i++) {
        int complement = target - nums[i];

        if (map.containsKey(complement)) {
            return new int[] { map.get(complement), i };
        }

        map.put(nums[i], i);
    }

    throw new IllegalArgumentException("No solution found");
}
```

### Step-by-Step Walkthrough

```md
Input: nums = [2, 7, 11, 15], target = 9

i=0: nums[0]=2, complement=9-2=7, map={} → 7 not in map → map={2:0}
i=1: nums[1]=7, complement=9-7=2, map={2:0} → 2 IS in map! → return [0, 1] ✅

Time: O(n) — single pass
Space: O(n) — map stores up to n elements
```

### Why is this correct?

- We store each number's index as we go (`map.set(nums[i], i)`)
- For each new number, we check if its **complement** (`target - nums[i]`) was already seen
- If yes, we found our pair → return both indices
- We never use the same element twice because we check `map` **before** adding the current element
- One pass through the array = O(n) time complexity

### Common Variations

```typescript
// Return VALUES instead of indices
function twoSumValues(nums: number[], target: number): number[] {
  const seen = new Set<number>();
  for (const num of nums) {
    if (seen.has(target - num)) return [target - num, num];
    seen.add(num);
  }
  throw new Error('No solution');
}

// Return ALL pairs (not just one)
function twoSumAllPairs(nums: number[], target: number): number[][] {
  const map = new Map<number, number[]>();
  const result: number[][] = [];
  
  nums.forEach((num, i) => {
    const complement = target - num;
    if (map.has(complement)) {
      for (const j of map.get(complement)!) {
        result.push([j, i]);
      }
    }
    if (!map.has(num)) map.set(num, []);
    map.get(num)!.push(i);
  });
  
  return result;
}
```
