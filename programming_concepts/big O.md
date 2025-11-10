# Big O Notation: Complete Course

## Table of Contents
1. [Introduction](#introduction)
2. [What is Big O Notation?](#what-is-big-o)
3. [Why Big O Matters](#why-matters)
4. [Common Complexity Classes](#complexity-classes)
5. [Analyzing Algorithms](#analyzing-algorithms)
6. [Rules of Big O](#rules)
7. [Space Complexity](#space-complexity)
8. [Practice Problems](#practice-problems)
9. [Advanced Topics](#advanced-topics)

---

## Introduction {#introduction}

Big O notation is the language we use to describe how efficient algorithms are. Whether you're building web apps, analyzing data, or preparing for technical interviews, understanding Big O is essential for writing performant code.

**What you'll learn:**
- How to analyze algorithm efficiency
- Common complexity classes and when they occur
- How to optimize your code
- How to compare different algorithmic approaches

---

## What is Big O Notation? {#what-is-big-o}

Big O notation describes **how the runtime or space requirements of an algorithm grow as the input size increases**.

### The Key Idea

Big O focuses on the **worst-case scenario** and **ignores constants** to give us a high-level understanding of scalability.

**Example:**
```python
def find_item(items, target):
    for item in items:  # Loops through n items
        if item == target:
            return True
    return False
```

- Best case: item is first → O(1)
- Average case: item is in the middle → O(n/2)
- Worst case: item is last or not present → O(n)

We describe this as **O(n)** because we care about the worst case.

### Mathematical Definition

For a function f(n), we say f(n) = O(g(n)) if there exist constants c and n₀ such that:

```
f(n) ≤ c × g(n) for all n ≥ n₀
```

In simpler terms: Big O gives an upper bound on how fast a function grows.

---

## Why Big O Matters {#why-matters}

### The Scalability Problem

Consider searching for a name in a phone book:

| Algorithm | 100 entries | 1,000 entries | 1,000,000 entries |
|-----------|-------------|---------------|-------------------|
| O(1) | 1 step | 1 step | 1 step |
| O(log n) | 7 steps | 10 steps | 20 steps |
| O(n) | 100 steps | 1,000 steps | 1,000,000 steps |
| O(n²) | 10,000 steps | 1,000,000 steps | 1,000,000,000,000 steps |

**The difference is dramatic!** An O(n²) algorithm that works fine on 100 items becomes completely unusable at scale.

### Real-World Impact

- **Web servers:** O(n²) algorithm = server crashes under load
- **Mobile apps:** Poor complexity = battery drain and lag
- **Data processing:** Wrong algorithm = hours instead of seconds
- **Interviews:** Understanding Big O is tested at every major tech company

---

## Common Complexity Classes {#complexity-classes}

From fastest to slowest:

### 1. O(1) - Constant Time
**Runtime doesn't change with input size.**

```python
def get_first(items):
    return items[0]  # Always 1 operation

def hash_lookup(dictionary, key):
    return dictionary[key]  # Always 1 operation
```

**Examples:** Array access, hash table lookup, stack push/pop

---

### 2. O(log n) - Logarithmic Time
**Runtime grows slowly, doubling input only adds one step.**

```python
def binary_search(sorted_array, target):
    left, right = 0, len(sorted_array) - 1
    
    while left <= right:
        mid = (left + right) // 2
        if sorted_array[mid] == target:
            return mid
        elif sorted_array[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1
```

**How it works:** Each step eliminates half the remaining data.
- 1,000 items → ~10 steps
- 1,000,000 items → ~20 steps

**Examples:** Binary search, balanced binary tree operations, finding in sorted data

---

### 3. O(n) - Linear Time
**Runtime grows proportionally with input size.**

```python
def find_max(numbers):
    max_val = numbers[0]
    for num in numbers:  # Visits each element once
        if num > max_val:
            max_val = num
    return max_val

def sum_array(numbers):
    total = 0
    for num in numbers:
        total += num
    return total
```

**Examples:** Simple loops, linear search, traversing an array/linked list

---

### 4. O(n log n) - Linearithmic Time
**Common in efficient sorting algorithms.**

```python
def merge_sort(arr):
    if len(arr) <= 1:
        return arr
    
    mid = len(arr) // 2
    left = merge_sort(arr[:mid])    # Divide: log n levels
    right = merge_sort(arr[mid:])
    
    return merge(left, right)        # Merge: n work per level

def merge(left, right):
    result = []
    i = j = 0
    while i < len(left) and j < len(right):
        if left[i] <= right[j]:
            result.append(left[i])
            i += 1
        else:
            result.append(right[j])
            j += 1
    result.extend(left[i:])
    result.extend(right[j:])
    return result
```

**Examples:** Merge sort, heap sort, quick sort (average case)

---

### 5. O(n²) - Quadratic Time
**Nested loops over the same data.**

```python
def bubble_sort(arr):
    n = len(arr)
    for i in range(n):           # Outer loop: n times
        for j in range(n - i - 1):  # Inner loop: up to n times
            if arr[j] > arr[j + 1]:
                arr[j], arr[j + 1] = arr[j + 1], arr[j]
    return arr

def find_duplicates_naive(arr):
    duplicates = []
    for i in range(len(arr)):
        for j in range(i + 1, len(arr)):  # Compare each pair
            if arr[i] == arr[j]:
                duplicates.append(arr[i])
    return duplicates
```

**Examples:** Bubble sort, selection sort, insertion sort, comparing all pairs

---

### 6. O(2ⁿ) - Exponential Time
**Doubles with each additional input. Usually impractical.**

```python
def fibonacci_recursive(n):
    if n <= 1:
        return n
    return fibonacci_recursive(n - 1) + fibonacci_recursive(n - 2)
    # Each call spawns 2 more calls!

def power_set(s):
    # Generates all subsets of a set
    if not s:
        return [[]]
    
    item = s[0]
    subsets = power_set(s[1:])
    return subsets + [[item] + subset for subset in subsets]
```

**Examples:** Recursive fibonacci, generating all subsets, brute-force solutions

---

### 7. O(n!) - Factorial Time
**The slowest. Only works for tiny inputs.**

```python
def permutations(arr):
    if len(arr) <= 1:
        return [arr]
    
    result = []
    for i in range(len(arr)):
        rest = arr[:i] + arr[i+1:]
        for p in permutations(rest):
            result.append([arr[i]] + p)
    return result

# Traveling salesman brute force
def tsp_brute_force(cities):
    # Try every possible route
    min_distance = float('inf')
    for route in permutations(cities):
        distance = calculate_distance(route)
        min_distance = min(min_distance, distance)
    return min_distance
```

**Examples:** Generating all permutations, traveling salesman (brute force)

---

## Analyzing Algorithms {#analyzing-algorithms}

### Step-by-Step Process

1. **Identify the input size** (usually n)
2. **Count operations** relative to input size
3. **Focus on the dominant term** (drop constants and smaller terms)
4. **Express in Big O notation**

### Example 1: Simple Analysis

```python
def example1(arr):
    print(arr[0])           # O(1)
    
    for item in arr:        # O(n)
        print(item)
    
    for item in arr:        # O(n)
        print(item)
    
    print("Done")           # O(1)

# Total: O(1) + O(n) + O(n) + O(1) = O(2n + 2)
# Simplified: O(n)  ← Drop constants
```

### Example 2: Nested Loops

```python
def example2(arr):
    for i in arr:              # O(n)
        for j in arr:          # O(n)
            print(i, j)        # O(1)

# Total: O(n) × O(n) × O(1) = O(n²)
```

### Example 3: Different Inputs

```python
def example3(arr1, arr2):
    for item in arr1:          # O(n)
        print(item)
    
    for item in arr2:          # O(m)
        print(item)

# Total: O(n + m)  ← Keep both variables
```

### Example 4: Loops with Break

```python
def example4(arr):
    for i in range(len(arr)):
        if arr[i] == 5:
            break
        print(arr[i])

# Worst case: O(n) - if 5 is at the end or not present
```

---

## Rules of Big O {#rules}

### Rule 1: Drop Constants

```python
# O(2n) → O(n)
def rule1_example(arr):
    for item in arr:      # n operations
        print(item)
    for item in arr:      # n operations
        print(item)
    # Total: 2n, but we say O(n)
```

**Why?** Big O cares about growth rate, not exact count. 2n and n grow at the same rate.

### Rule 2: Drop Non-Dominant Terms

```python
# O(n² + n) → O(n²)
def rule2_example(arr):
    for i in arr:              # n times
        for j in arr:          # n times
            print(i, j)        # n² operations
    
    for item in arr:           # n operations
        print(item)

# Total: n² + n, but we say O(n²)
```

**Why?** When n is large, n² dominates. For n=1000: n² = 1,000,000 while n = 1,000 (negligible difference).

### Rule 3: Different Inputs = Different Variables

```python
# O(a + b), NOT O(n)
def compress_arrays(arr1, arr2):
    for item in arr1:     # a operations
        print(item)
    for item in arr2:     # b operations
        print(item)
```

### Rule 4: Worst Case Analysis

Always analyze the worst-case scenario unless otherwise specified.

---

## Space Complexity {#space-complexity}

Big O also describes **memory usage**, not just time.

### O(1) Space - Constant

```python
def sum_array(arr):
    total = 0        # 1 variable
    for num in arr:
        total += num
    return total
# Uses same memory regardless of input size
```

### O(n) Space - Linear

```python
def double_array(arr):
    result = []           # New array
    for num in arr:
        result.append(num * 2)
    return result         # Size grows with input
```

### O(n) Space - Recursion

```python
def factorial(n):
    if n <= 1:
        return 1
    return n * factorial(n - 1)
# Call stack grows to n levels deep
```

### Space-Time Tradeoff

Often you can trade space for time or vice versa:

```python
# Fibonacci: O(2ⁿ) time, O(n) space (call stack)
def fib_slow(n):
    if n <= 1:
        return n
    return fib_slow(n-1) + fib_slow(n-2)

# Fibonacci: O(n) time, O(n) space (memoization)
def fib_memo(n, memo={}):
    if n in memo:
        return memo[n]
    if n <= 1:
        return n
    memo[n] = fib_memo(n-1, memo) + fib_memo(n-2, memo)
    return memo[n]

# Fibonacci: O(n) time, O(1) space (iterative)
def fib_fast(n):
    if n <= 1:
        return n
    a, b = 0, 1
    for _ in range(2, n + 1):
        a, b = b, a + b
    return b
```

---

## Practice Problems {#practice-problems}

### Problem 1: What's the Big O?

```python
def mystery1(arr):
    for i in range(len(arr)):
        print(arr[i])
```

<details>
<summary>Answer</summary>
O(n) - single loop through array
</details>

---

### Problem 2: What's the Big O?

```python
def mystery2(arr):
    for i in range(len(arr)):
        for j in range(len(arr)):
            if i != j:
                print(arr[i], arr[j])
```

<details>
<summary>Answer</summary>
O(n²) - nested loops, both iterate n times
</details>

---

### Problem 3: What's the Big O?

```python
def mystery3(arr):
    if len(arr) < 2:
        return
    mid = len(arr) // 2
    mystery3(arr[:mid])
    mystery3(arr[mid:])
```

<details>
<summary>Answer</summary>
O(n log n) - divides array in half each time (log n levels), but creates new arrays at each level (n total work)
</details>

---

### Problem 4: Optimize This Code

```python
# Find if two arrays have any common elements
def has_common_items(arr1, arr2):
    for i in arr1:
        for j in arr2:
            if i == j:
                return True
    return False
# Current: O(n × m)
```

<details>
<summary>Optimized Solution</summary>

```python
def has_common_items_optimized(arr1, arr2):
    set1 = set(arr1)  # O(n)
    for item in arr2:  # O(m)
        if item in set1:  # O(1) lookup
            return True
    return False
# Improved: O(n + m) time, O(n) space
```
</details>

---

### Problem 5: Find Duplicates

```python
# Find first duplicate in array
def first_duplicate(arr):
    for i in range(len(arr)):
        for j in range(i + 1, len(arr)):
            if arr[i] == arr[j]:
                return arr[i]
    return None
# Current: O(n²)
```

<details>
<summary>Optimized Solution</summary>

```python
def first_duplicate_optimized(arr):
    seen = set()
    for item in arr:
        if item in seen:
            return item
        seen.add(item)
    return None
# Improved: O(n) time, O(n) space
```
</details>

---

## Advanced Topics {#advanced-topics}

### Big Ω (Omega) - Best Case

Big O describes worst case. Big Ω describes **best case**.

```python
def linear_search(arr, target):
    for i, item in enumerate(arr):
        if item == target:
            return i
    return -1

# O(n) - worst case: target at end
# Ω(1) - best case: target at start
```

### Big Θ (Theta) - Average Case

Big Θ describes when best and worst case are the same (tight bound).

```python
def print_all(arr):
    for item in arr:
        print(item)

# Θ(n) - always n operations, regardless of input
```

### Amortized Analysis

Some operations are expensive occasionally but cheap on average.

```python
# Dynamic array (like Python list)
arr = []
for i in range(n):
    arr.append(i)  # Usually O(1), occasionally O(n) when resizing

# Amortized: O(1) per append
# Total: O(n) for n appends
```

### Recursive Complexity

Use the **Master Theorem** or draw a **recursion tree**.

**Example: Binary Search**
```
T(n) = T(n/2) + O(1)
     = O(log n)
```

**Example: Merge Sort**
```
T(n) = 2T(n/2) + O(n)
     = O(n log n)
```

### Common Data Structure Complexities

| Data Structure | Access | Search | Insert | Delete | Space |
|----------------|--------|--------|--------|--------|-------|
| Array | O(1) | O(n) | O(n) | O(n) | O(n) |
| Linked List | O(n) | O(n) | O(1)* | O(1)* | O(n) |
| Hash Table | O(1)** | O(1)** | O(1)** | O(1)** | O(n) |
| Binary Search Tree | O(log n)** | O(log n)** | O(log n)** | O(log n)** | O(n) |
| Heap | - | O(n) | O(log n) | O(log n) | O(n) |

\* At known position  
\*\* Average case; worst case can be O(n)

---

## Key Takeaways

1. **Big O describes scalability**, not exact runtime
2. **Drop constants and non-dominant terms**
3. **Focus on worst-case** unless stated otherwise
4. **Different inputs need different variables**
5. **Space complexity matters** as much as time
6. **Know common patterns**: loops are O(n), nested loops are O(n²), divide-and-conquer is often O(n log n)
7. **Optimization is about choosing better algorithms**, not just faster code

---

## Further Study

**Books:**
- "Introduction to Algorithms" (CLRS)
- "The Algorithm Design Manual" by Skiena
- "Grokking Algorithms" by Bhargava

**Practice:**
- LeetCode
- HackerRank
- CodeSignal
- Project Euler

**Visualization:**
- VisuAlgo.net
- Big-O Cheat Sheet

---

## Quiz Yourself

1. What's the Big O of accessing the middle element of an array?
2. Why is binary search O(log n)?
3. What's faster: O(n log n) or O(n²)?
4. Can an O(n²) algorithm ever be faster than O(n)?
5. What's the space complexity of recursively calculating factorial?

<details>
<summary>Answers</summary>

1. O(1) - arrays allow constant-time index access
2. Each step eliminates half the search space
3. O(n log n) - much faster for large n
4. Yes! For small n, constants matter. O(n²) with small constant might beat O(n) with huge constant.
5. O(n) - call stack grows n levels deep

</details>

---

**Congratulations!** You now understand Big O notation. Keep practicing by analyzing every algorithm you encounter. Soon it will become second nature!
