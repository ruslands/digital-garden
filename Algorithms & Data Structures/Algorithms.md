# Algorithms: Complete Guide

## Table of Contents
1. [Introduction](#introduction)
2. [Naive Approach](#naive-approach)
3. [Subarray/Subsequence Algorithms](#subarraysubsequence-algorithms)
4. [Search Algorithms](#search-algorithms)
5. [Sorting Algorithms](#sorting-algorithms)
6. [String Algorithms](#string-algorithms)
7. [Summary](#summary)

---

## Introduction

Algorithms are step-by-step procedures for solving problems. Understanding different algorithm types and their complexities helps choose the right approach for each problem.

**Key Concepts:**
- **Naive Approach**: Simple, straightforward solutions
- **Optimized Algorithms**: Efficient solutions with better complexity
- **Time Complexity**: How execution time grows with input size
- **Space Complexity**: How memory usage grows with input size

---

## Naive Approach

### What is a Naive Approach?

A "naive approach" in the context of algorithms refers to the simplest, most straightforward way to solve a problem without optimizing for efficiency or considering edge cases deeply. It typically prioritizes clarity and ease of implementation over performance, often resulting in higher time or space complexity compared to more sophisticated methods.

**Characteristics:**
- Simple and intuitive
- Easy to understand and implement
- May not scale well for large inputs
- Often has higher time or space complexity

**When to Use:**
- Teaching and learning
- Small-scale problems
- Prototyping
- When clarity is more important than performance

---

### Naive Approach in Sorting Algorithms

#### 1. Bubble Sort as a Naive Approach

**Why it's naive:**
Bubble sort is often considered a naive sorting method because it blindly compares every pair of adjacent elements and swaps them if they're out of order, repeating this process until no more swaps are needed. It doesn't leverage any advanced techniques to reduce comparisons or take advantage of partially sorted data (without optimization).

**Complexity:**
- **Time Complexity**: O(n²) – it checks every pair repeatedly, even if the array is nearly sorted
- **Space Complexity**: O(1)

**Characteristics:**
- Simple to understand
- Inefficient for large datasets
- No optimization for partially sorted data

---

#### 2. Selection Sort as a Naive Approach

**Why it's naive:**
Selection sort naively scans the entire unsorted portion of the array to find the minimum element in each pass, even if it could potentially skip some comparisons with a smarter strategy. It doesn't adapt to the data's structure.

**Complexity:**
- **Time Complexity**: O(n²) – no optimization for fewer comparisons
- **Space Complexity**: O(1)

**Characteristics:**
- Always performs the same number of comparisons
- Doesn't adapt to data patterns
- Simple but inefficient

---

#### 3. Insertion Sort (Naive in Some Contexts)

**Why it's naive:**
While insertion sort can be efficient for small or nearly sorted arrays, its naive nature comes from shifting elements one by one to insert a new element, without considering batch operations or dividing the problem into more manageable chunks (like Shell sort does).

**Complexity:**
- **Time Complexity**: O(n²) – linear insertions in a growing sorted portion
- **Space Complexity**: O(1)
- **Best Case**: O(n) for nearly sorted arrays

**Characteristics:**
- Good for small datasets
- Efficient for nearly sorted data
- Simple implementation

---

### General Characteristics of a Naive Approach

**Key Features:**
- **Simplicity**: Easy to code and understand (e.g., nested loops for sorting)
- **No Optimization**: Ignores opportunities to reduce work, like divide-and-conquer (e.g., Quicksort) or preprocessing (e.g., Counting Sort)
- **Brute Force**: Often checks every possibility exhaustively rather than finding shortcuts
- **Higher Complexity**: Typically results in O(n²) or worse for problems where optimized solutions achieve O(n log n) or better

---

### Example Beyond Sorting

**Problem:** Finding two numbers in an array that sum to a target value

**Naive Approach:**
- Check every pair of elements with nested loops
- **Complexity**: O(n²)

**Optimized Approach:**
- Use a hash table to store complements while scanning once
- **Complexity**: O(n)

**Comparison:**
- Naive: Check all pairs → O(n²)
- Optimized: Single pass with hash table → O(n)

---

### When Not to Use Naive Approaches

**Optimized Alternatives:**
- **Quicksort, Merge Sort, or Radix Sort** are not considered naive because they use clever strategies (e.g., partitioning, merging, or digit-based grouping) to achieve better performance
- The naive approach is useful for teaching or small-scale problems but is rarely practical for large datasets

---

## Subarray/Subsequence Algorithms

**Total: 4 algorithms**

### 1. Kadane's Algorithm

**Description:**
Finds the maximum sum subarray in an array.

**Complexity:**
- **Time Complexity**: O(n)
- **Space Complexity**: O(1)

**Use Cases:**
- Maximum subarray sum problem
- Stock trading (best time to buy/sell)
- Array analysis

**Key Insight:**
Maintains the maximum sum ending at each position, either extending the previous subarray or starting a new one.

---

### 2. Sliding Window

**Description:**
Uses a fixed or variable window to solve subarray problems (e.g., max sum of k elements).

**Complexity:**
- **Time Complexity**: O(n) typically
- **Space Complexity**: O(1) or O(k) depending on window size

**Use Cases:**
- Maximum/minimum in sliding window
- Longest substring with k distinct characters
- Subarray problems with constraints

**Key Insight:**
Maintains a window that slides through the array, updating the window as it moves.

---

### 3. Longest Increasing Subsequence (LIS)

**Description:**
Finds the longest subsequence (not necessarily contiguous) where elements increase.

**Complexity:**
- **Time Complexity**: O(n log n) with dynamic programming + binary search
- **Space Complexity**: O(n)

**Use Cases:**
- Sequence analysis
- Pattern recognition
- Optimization problems

**Key Insight:**
Uses dynamic programming with binary search to efficiently find the longest increasing subsequence.

---

### 4. Maximum Subarray Product

**Description:**
Finds the contiguous subarray with the largest product.

**Complexity:**
- **Time Complexity**: O(n)
- **Space Complexity**: O(1)

**Use Cases:**
- Array product problems
- Optimization
- Dynamic programming

**Key Insight:**
Tracks both maximum and minimum products at each position (since negative numbers can flip the sign).

---

## Search Algorithms

**Total: 8 algorithms**

### 1. Linear Search (Sequential Search)

**Description:**
Iterates through each element in a list until the target is found or the list ends.

**Complexity:**
- **Time Complexity**: O(n)
- **Space Complexity**: O(1)

**Best For:**
- Small or unsorted datasets
- When data is not sorted
- Simple implementations

**Characteristics:**
- Simple to implement
- Works on any data structure
- No preprocessing required

---

### 2. Binary Search (Logarithmic Search)

**Description:**
Works on sorted arrays by repeatedly dividing the search range in half.

**Complexity:**
- **Time Complexity**: O(log n)
- **Space Complexity**: O(1) iterative, O(log n) recursive

**Best For:**
- Large, sorted datasets
- When data is sorted
- Fast lookup requirements

**Characteristics:**
- Very efficient
- Requires sorted data
- Divide-and-conquer approach

---

### 3. Interpolation Search

**Description:**
An improvement over binary search for uniformly distributed sorted arrays. Instead of always dividing the search space in half, it estimates the position of the target based on the values at the bounds (using interpolation).

**Complexity:**
- **Time Complexity**: O(log log n) on average for uniform data; O(n) in the worst case (e.g., exponential distribution)
- **Space Complexity**: O(1)

**Best For:**
- Searching in large, uniformly distributed sorted datasets (e.g., phone books, numerical databases)

**Characteristics:**
- Faster than binary search for uniform data
- Degrades to O(n) for non-uniform data
- Uses value-based position estimation

---

### 4. Ternary Search

**Description:**
A divide-and-conquer algorithm to find the maximum or minimum of a unimodal function/array (single peak or valley).

**How It Works:**
Divides the search space into three parts using two points (m1, m2), compares values, and eliminates one-third per iteration.

**Complexity:**
- **Time Complexity**: O(log₃ n) for discrete arrays; O(log n) for continuous functions with precision ε
- **Space Complexity**: O(1) iterative, O(log n) recursive

**Use Case:**
Finding peaks in unimodal arrays (e.g., [1, 3, 5, 4, 2]) or optimizing unimodal functions (e.g., distance minimization).

**Comparison:**
Slower than Binary Search (two comparisons vs. one) but suited for extremum problems.

---

### 5. Fibonacci Search

**Description:**
A comparison-based search algorithm that uses Fibonacci numbers to find an element in a sorted array by narrowing the search space.

**How It Works:**
Divides the array using Fibonacci numbers, compares the target with an element at a Fibonacci index, and eliminates a portion of the array iteratively.

**Complexity:**
- **Time Complexity**: O(log n), where n is the array size; specifically tied to the Fibonacci sequence's growth (~log base φ, golden ratio)
- **Space Complexity**: O(1) iterative, O(log n) recursive due to call stack

**Use Case:**
Searching in sorted arrays (e.g., [1, 3, 5, 7, 9]) when minimizing comparisons or memory writes is key (similar to Binary Search).

**Comparison:**
Similar to Binary Search but uses addition/subtraction of Fibonacci numbers instead of division, potentially reducing computational overhead on some systems.

---

### 6. Jump Search

**Description:**
Works on sorted arrays by "jumping" ahead by fixed steps (typically √n) and then performing a linear search within the identified block to find the target.

**Complexity:**
- **Time Complexity**: O(√n)
- **Space Complexity**: O(1)

**Best For:**
Sorted arrays where binary search might be overkill, and memory access is costly (e.g., in systems with expensive random access).

**Characteristics:**
- Middle ground between linear and binary search
- Good for systems with expensive memory access
- Simpler than binary search

---

### 7. Exponential Search

**Description:**
Finds a range where the element might be in a sorted array, then uses binary search.

**Complexity:**
- **Time Complexity**: O(log n)
- **Space Complexity**: O(1)

**Use Cases:**
- Searching in unbounded or infinite sorted arrays
- When the target is near the beginning
- Combining with binary search

**How It Works:**
1. Start with a small range
2. Double the range until the target is within bounds
3. Use binary search in the identified range

---

### 8. Depth-First Search (DFS)

**Description:**
Explores as far as possible along each branch before backtracking, typically using a stack (explicitly or via recursion).

**Complexity:**
- **Time Complexity**: O(V + E) (for graphs, where V is vertices and E is edges)
- **Space Complexity**: O(V) for recursion stack or explicit stack

**Best For:**
- Graph traversal
- Pathfinding
- Topological sorting
- Maze solving

**Characteristics:**
- Uses stack (recursion or explicit)
- Explores deep before wide
- Can find paths (not necessarily shortest)

---

### 9. Breadth-First Search (BFS)

**Description:**
Explores all neighbors at the present depth before moving to nodes at the next depth level, using a queue.

**Complexity:**
- **Time Complexity**: O(V + E)
- **Space Complexity**: O(V) for queue

**Best For:**
- Shortest path in unweighted graphs
- Level-order traversal
- Finding minimum steps
- Social network analysis

**Characteristics:**
- Uses queue
- Explores wide before deep
- Finds shortest paths in unweighted graphs

---

### Comparison with Popular Algorithms

**Jump Search:**
A middle ground between linear and binary search, balancing simplicity and efficiency.

**Interpolation Search:**
Shines when data distribution is predictable, outperforming binary search in such cases but degrading otherwise.

---

## Sorting Algorithms

**Total: 24 algorithms**

### Efficient Sorting Algorithms

#### 1. Quicksort (Быстрая сортировка)

**Explanation:**
Quicksort picks a "pivot" element from the array and partitions the other elements into two groups: those less than the pivot and those greater than it. It then recursively sorts the sub-arrays. The choice of pivot (e.g., first element, random, or median) affects performance.

**Time Complexity:**
- **Average**: O(n log n) – efficient partitioning leads to balanced sub-arrays
- **Worst**: O(n²) – occurs with already sorted or reverse-sorted arrays and a bad pivot choice (e.g., smallest or largest element)
- **Best**: O(n log n)

**Space Complexity:**
- **Average**: O(log n) for recursion stack
- **Worst**: O(n) for recursion stack

**Characteristics:**
- In-place sorting
- Not stable
- Divide-and-conquer approach

---

#### 2. Merge Sort (Сортировка слиянием)

**Explanation:**
Merge sort divides the array into two halves, recursively sorts each half, and then merges the sorted halves back together by comparing elements. It's a stable, divide-and-conquer algorithm.

**Time Complexity:**
- **Average and Worst**: O(n log n) – consistently splits the array into halves (log n levels) and merges them (n operations per level)
- **Best**: O(n log n)

**Space Complexity:**
- **O(n)** – requires extra space for merging

**Characteristics:**
- Stable sorting
- Predictable performance
- Requires extra memory

---

#### 3. Heapsort (Пирамидальная сортировка)

**Explanation:**
Heapsort uses a binary heap data structure. It first builds a max-heap from the array (where the parent is greater than its children), then repeatedly extracts the maximum element and rebuilds the heap.

**Time Complexity:**
- **Average and Worst**: O(n log n) – building the heap takes O(n), and extracting n elements while maintaining the heap takes O(n log n)
- **Best**: O(n log n)

**Space Complexity:**
- **O(1)** – in-place sorting

**Characteristics:**
- In-place but not stable
- Guaranteed O(n log n) performance
- Uses heap data structure

---

#### 4. Counting Sort (Сортировка подсчётом)

**Explanation:**
Counting sort is not comparison-based. It counts the occurrences of each element in the array and uses this information to place elements in their correct positions. It works best for integers within a known range.

**Time Complexity:**
- **Average and Worst**: O(n + k) – where n is the number of elements and k is the range of input values (max - min + 1)
- **Best**: O(n + k)

**Space Complexity:**
- **O(k)** – needs extra space for the counting array

**Characteristics:**
- Not comparison-based
- Very fast for small integer ranges
- Requires known range

---

#### 5. Radix Sort (Поразрядная сортировка)

**Explanation:**
Radix sort processes the array digit by digit (e.g., from least significant to most significant). It uses a stable sorting algorithm (like counting sort) for each digit. It's efficient for large datasets of integers or strings.

**Time Complexity:**
- **Average and Worst**: O(n * k) – where n is the number of elements and k is the number of digits (or maximum length of elements)
- **Best**: O(n * k)

**Space Complexity:**
- **O(n + k)** – depends on the stable subroutine sort used

**Characteristics:**
- Requires a stable subroutine sort
- Good for integers and strings
- Linear time for fixed-width data

---

### Simple Sorting Algorithms

#### 6. Bubble Sort (Сортировка пузырьком)

**Explanation:**
Bubble sort repeatedly steps through the array, compares adjacent elements, and swaps them if they're in the wrong order. Larger elements "bubble up" to the end.

**Time Complexity:**
- **Average and Worst**: O(n²) – compares and swaps elements in nested loops
- **Best**: O(n) – when the array is already sorted and no swaps are needed (with an optimization flag)

**Space Complexity:**
- **O(1)**

**Characteristics:**
- Simple but inefficient for large datasets
- Stable sorting
- Easy to understand

---

#### 7. Selection Sort (Сортировка выбором)

**Explanation:**
Selection sort repeatedly finds the minimum element from the unsorted portion of the array and places it at the beginning. It divides the array into sorted and unsorted parts.

**Time Complexity:**
- **Average and Worst**: O(n²) – always performs n iterations, each scanning the remaining unsorted portion
- **Best**: O(n²)

**Space Complexity:**
- **O(1)**

**Characteristics:**
- In-place but not stable
- Simple implementation
- Always performs the same number of comparisons

---

#### 8. Insertion Sort (Сортировка вставками)

**Explanation:**
Insertion sort builds a sorted portion of the array one element at a time by taking an element from the unsorted part and inserting it into the correct position in the sorted part.

**Time Complexity:**
- **Average and Worst**: O(n²) – shifts elements in the sorted portion for each insertion
- **Best**: O(n) – nearly sorted arrays require minimal shifts

**Space Complexity:**
- **O(1)**

**Characteristics:**
- Efficient for small or partially sorted arrays
- Stable sorting
- Adaptive (performs well on nearly sorted data)

---

#### 9. Shell Sort (Сортировка Шелла)

**Explanation:**
Shell sort is an optimization of insertion sort. It compares and swaps elements that are far apart (using a gap), reducing the gap size with each pass until it becomes insertion sort (gap = 1).

**Time Complexity:**
- **Average**: O(n^1.3) – depends on the gap sequence (e.g., Knuth's sequence); not fully quadratic
- **Worst**: O(n²) – with a poor gap sequence
- **Best**: O(n log n)

**Space Complexity:**
- **O(1)**

**Characteristics:**
- Better than simple insertion sort for larger arrays
- Gap sequence affects performance
- In-place sorting

---

#### 10. Bucket Sort (Блочная сортировка)

**Explanation:**
Bucket sort divides the range of input values into "buckets," distributes elements into these buckets, sorts each bucket (often with another algorithm like insertion sort), and concatenates the results. It works well for uniformly distributed data.

**Time Complexity:**
- **Average**: O(n + k) – where n is the number of elements and k is the number of buckets
- **Worst**: O(n²) – if all elements fall into one bucket and a quadratic sort is used
- **Best**: O(n + k)

**Space Complexity:**
- **O(n + k)** – for buckets and elements

**Characteristics:**
- Efficient for uniformly distributed data
- Requires knowledge of data distribution
- Can degrade to O(n²) in worst case

---

### Other Sorting Algorithms

11. **Cocktail Sort (Сортировка перемешиванием)** – Bidirectional bubble sort
12. **Gnome Sort (Гномья сортировка)** – Similar to insertion sort
13. **Tree Sort (Сортировка с помощью двоичного дерева)** – Uses binary search tree
14. **Comb Sort (Сортировка расчёской)** – Improvement over bubble sort
15. **Smoothsort (Плавная сортировка)** – Adaptive heap sort variant
16. **Introsort (Интроспективная сортировка)** – Hybrid of quicksort, heapsort, and insertion sort
17. **Patience Sorting (Терпеливая сортировка)** – Card game-based sorting
18. **Stooge Sort** – Recursive sorting algorithm
19. **Bogosort (глупая сортировка, stupid sort)** – Randomly permutes until sorted
20. **Permutation Sort (Сортировка перестановкой)** – Generates all permutations
21. **Bead Sort (Гравитационная сортировка)** – Physical sorting method
22. **Pancake Sorting (Блинная сортировка)** – Reverses prefixes
23. **Topological Sort (Топологическая сортировка)** – For directed acyclic graphs
24. **External Sort (Внешняя сортировка)** – For data that doesn't fit in memory

---

## String Algorithms

### Knuth-Morris-Pratt (KMP) Search

**Description:**
A string-searching algorithm that efficiently finds occurrences of a pattern within a text by avoiding unnecessary backtracking. It preprocesses the pattern to create a "partial match" table (or failure function).

**Complexity:**
- **Time Complexity**: O(n + m) (where n is the text length and m is the pattern length)
- **Space Complexity**: O(m)

**Best For:**
- Exact string matching
- Text processing
- Pattern searching in large datasets (e.g., bioinformatics, plagiarism detection)

**Key Insight:**
Preprocesses the pattern to avoid re-checking characters that have already been matched, eliminating unnecessary backtracking.

**How It Works:**
1. Preprocess pattern to build failure function
2. Use failure function to skip characters that can't match
3. Search text in linear time

---

## Summary

**Algorithm Categories:**
- **Naive Approaches**: Simple but often inefficient (O(n²) or worse)
- **Subarray/Subsequence**: 4 algorithms for array problems
- **Search Algorithms**: 8 algorithms for finding elements
- **Sorting Algorithms**: 24 algorithms for ordering data
- **String Algorithms**: Pattern matching and text processing

**Key Takeaways:**
- Choose algorithms based on problem requirements
- Consider time and space complexity
- Naive approaches are good for learning but not for production
- Optimized algorithms use clever strategies (divide-and-conquer, preprocessing, etc.)
- Understand when to use each algorithm type

**Complexity Comparison:**
- **O(1)**: Constant time (best)
- **O(log n)**: Logarithmic (very good)
- **O(n)**: Linear (good)
- **O(n log n)**: Linearithmic (acceptable)
- **O(n²)**: Quadratic (slow for large data)
- **O(2ⁿ)**: Exponential (very slow)
- **O(n!)**: Factorial (extremely slow)

Understanding these algorithms and their complexities is essential for efficient problem-solving in computer science.
