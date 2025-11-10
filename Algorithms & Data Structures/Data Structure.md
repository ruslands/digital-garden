# Data Structures: Complete Reference Guide

## Table of Contents
1. [Introduction](#introduction)
2. [Python Data Types](#python-data-types)
3. [Linked Lists](#linked-lists)
4. [Heap](#heap)
5. [Binary Tree vs Binary Search Tree](#binary-tree-vs-binary-search-tree)
6. [Complete Binary Tree](#complete-binary-tree)
7. [Trees](#trees)
8. [Arrays](#arrays)
9. [Lists](#lists)
10. [Hash-Based Structures](#hash-based-structures)
11. [Graphs](#graphs)
12. [Resources](#resources)
13. [Summary](#summary)

---

## Introduction

Data structures are ways of organizing and storing data in computer memory. Understanding different data structures and their properties helps choose the right structure for each problem.

**Key Concepts:**
- **Time Complexity**: Operation performance
- **Space Complexity**: Memory usage
- **Access Patterns**: Random vs sequential access
- **Mutability**: Mutable vs immutable structures
- **Ordering**: Ordered vs unordered structures

---

## Python Data Types

В Python существует **четыре** встроенных типа данных: списки (list), кортежи (tuple), словари (dictionary) и множества (set) (In Python there are **four** built-in data types: lists, tuples, dictionaries, and sets).

### Comparison Table

| Feature / Aspect               | **List**                           | **Tuple**                        | **Set**                                       | **Dict (Dictionary)**                    |
| ------------------------------ | ---------------------------------- | -------------------------------- | --------------------------------------------- | ---------------------------------------- |
| **Mutable**                    | ✅ Yes                              | ❌ No                             | ✅ Yes                                         | ✅ Yes                                    |
| **Ordered**                    | ✅ Yes (since Python 3.7)           | ✅ Yes                            | ❌ No (unordered)                              | ✅ Yes (since Python 3.7)                 |
| **Indexed Access**             | ✅ Yes (via `list[i]`)              | ✅ Yes (via `tuple[i]`)           | ❌ No indexing                                 | ✅ Yes (via `dict[key]`)                  |
| **Duplicates Allowed**         | ✅ Yes                              | ✅ Yes                            | ❌ No                                          | Keys ❌, Values ✅                         |
| **Hashable (can be dict key)** | ❌ No                               | ✅ Yes (if all elements hashable) | ✅ Yes (if immutable)                          | ❌ N/A (dict is not hashable)             |
| **Elements Types**             | Any, including mixed types         | Any, including mixed types       | Only hashable (e.g., no lists)                | Keys must be hashable                    |
| **Syntax**                     | `[1, 2, 3]`                        | `(1, 2, 3)`                      | `{1, 2, 3}`                                   | `{'a': 1, 'b': 2}`                       |
| **Memory efficient**           | ❌ Less than tuple                  | ✅ More efficient than list       | ✅ Very efficient for large datasets           | ✅ Efficient for fast lookups             |
| **Use in loops**               | ✅ Commonly used                    | ✅ Fast iteration                 | ✅ Fast `in` checks                            | ✅ Fast access via keys                   |
| **Suitable for...**            | Changing data, appending/removing  | Immutable records                | Unique items, set operations                  | Fast lookups, mapping relations          |
| **Add/Update**                 | `append()`, `insert()`, `extend()` | ❌ Immutable                      | `add()`, `update()`                           | `dict[key] = value`, `update()`          |
| **Remove**                     | `remove()`, `pop()`, `clear()`     | ❌ Immutable                      | `remove()`, `discard()`, `pop()`, `clear()`   | `pop()`, `popitem()`, `clear()`, `del`   |
| **Query**                      | `count()`, `index()`               | `count()`, `index()`             | `in`, `issubset()`, `issuperset()`, `union()` | `keys()`, `values()`, `items()`, `get()` |
| **Sort/Reverse**               | `sort()`, `reverse()`              | ❌ Immutable                      | ❌ No sort                                     | ❌ Not applicable (unordered structure)   |
| **Copy**                       | `copy()`                           | Use slicing or constructor       | `copy()`                                      | `copy()`                                 |

---

### Operation Complexity Table (Big O)

| Операция   | **List**                                       | **Tuple**         | **Set**                      | **Dict**           |
| ---------- | ---------------------------------------------- | ----------------- | ---------------------------- | ------------------ |
| **Add**    | `O(1)` в конец<br>`O(n)` в начало или середину | ❌ Нельзя изменить | `O(1)` *в среднем*           | `O(1)` *в среднем* |
| **Delete** | `O(n)` (по значению или индексу)               | ❌ Нельзя удалить  | `O(1)` *в среднем*           | `O(1)` *в среднем* |
| **Search** | `O(n)` (линейный поиск)                        | `O(n)`            | `O(1)` *в среднем*           | `O(1)` *по ключу*  |
| **Update** | `O(1)` по индексу                              | ❌ Нельзя изменить | ❌ Не поддерживается напрямую | `O(1)` по ключу    |
| **Access** | `O(1)` по индексу                              | `O(1)` по индексу | ❌ Нельзя по индексу          | `O(1)` по ключу    |

**Notes:**
- `List`: добавление в конец — быстро (`append`), но вставка в начало или середину требует сдвига элементов (adding to end is fast, but inserting at beginning or middle requires shifting elements)
- `Tuple`: **неизменяемый тип**, любые попытки изменить — вызовут ошибку (immutable type, any attempts to change will cause an error)
- `Set`: оптимизирован для **проверки наличия элемента** (`x in set`) и удаления (optimized for checking element presence and deletion)
- `Dict`: ключевая особенность — **доступ по ключу за константное время** (key feature is constant-time access by key)

---

### Common Commands

| Операция | `List`           | `Set`           | `Dict`                  |
| -------- | ---------------- | --------------- | ----------------------- |
| Добавить | `list.append(x)` | `set.add(x)`    | `dict[key] = value`     |
| Удалить  | `list.remove(x)` | `set.remove(x)` | `del dict[key]`         |
| Поиск    | `x in list`      | `x in set`      | `key in dict`           |
| Изменить | `list[i] = x`    | ❌               | `dict[key] = new_value` |

---

### Random Access

**Произвольный доступ (random access)** у `list` в Python — это способность **мгновенно обратиться к любому элементу по его индексу**, без необходимости проходить список от начала (the ability to instantly access any element by its index, without needing to traverse the list from the beginning).

**Example:**
```python
my_list = ['apple', 'banana', 'cherry', 'date']
print(my_list[0])  # 'apple'
print(my_list[2])  # 'cherry'
print(my_list[-1]) # 'date'
```

You can immediately get **any element**, knowing its index — regardless of list length.

**Why It's Important:**
- **Access time** to an element by index — **O(1)** (very fast, doesn't depend on list size)
- This distinguishes `list` from, for example, **linked lists** (in other languages), where accessing an element requires traversal from the beginning

**Which Structures Support Random Access?**

| Тип     | Произвольный доступ (`obj[i]`) | Комментарий                    |
| ------- | ------------------------------ | ------------------------------ |
| `list`  | ✅ Да                           | Быстро и удобно                |
| `tuple` | ✅ Да                           | То же самое, но неизменяемый   |
| `set`   | ❌ Нет                          | Нет индексов, нет порядка      |
| `dict`  | ❌ Нет                          | Доступ по ключу, не по позиции |

---

### Sequential Access - Linked Lists

**Reference:** [List of Data Structures](https://en.wikipedia.org/wiki/List_of_data_structures)

**Связанный список (Linked list)** — это структура данных, в которой элементы (называются **узлы**, *nodes*) **связаны между собой с помощью ссылок** (a data structure where elements (called **nodes**) are **linked together using references).

**Node Structure:**
Each node contains:
1. **Данные** (значение элемента) (Data - element value)
2. **Ссылку на следующий узел** (в односвязном списке) или и на следующий, и на предыдущий (в двусвязном списке) (Reference to next node (in singly linked list) or both next and previous (in doubly linked list))

**Example of Singly Linked List:**
```plaintext
[1 | ➡ ] → [2 | ➡ ] → [3 | ➡ ] → None
```

Each node knows only:
- its data (`1`, `2`, `3`)
- and **references** the next node

---

### Types of Linked Lists

| Вид списка      | Описание                                           |
| --------------- | -------------------------------------------------- |
| **Односвязный** | Каждый узел знает только следующего                |
| **Двусвязный**  | Каждый узел знает предыдущего и следующего         |
| **Циклический** | Последний узел ссылается на первый, образуя кольцо |

---

### Linked List Characteristics

| Характеристика                 | Значение                                                             |
| ------------------------------ | -------------------------------------------------------------------- |
| 🔁 **Динамический размер**      | Увеличивается/уменьшается без перераспределения памяти               |
| 🐢 **Медленный доступ**         | Чтобы добраться до `n`-го элемента, нужно пройти все предыдущие узлы |
| ➕ **Вставка/удаление**         | Быстрая (O(1)), если известен нужный узел                            |
| 📦 **Использует больше памяти** | Каждый узел хранит **данные + ссылку(и)**                            |

---

### Why Python Doesn't Use Linked Lists

Python **не использует связанный список как встроенный тип данных** (doesn't use linked list as a built-in data type), because:

- `list` is already well optimized
- `list` supports **random access** (O(1)), while linked list doesn't
- Memory in Python is managed differently (through objects and garbage collection)

**But you can implement a linked list manually**, for example:

```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

# Creating a list: 1 → 2 → 3
n1 = Node(1)
n2 = Node(2)
n3 = Node(3)
n1.next = n2
n2.next = n3
```

**In Python, you can implement:**
- **Очереди, стеки, деревья, графы и хэш-таблицы** (queues, stacks, trees, graphs, and hash tables)
- **Связанный список** (linked list)

---

## Heap

A **heap** is a specialized tree-based data structure that satisfies the **heap property**. It's commonly used to implement priority queues, efficiently find the minimum or maximum element, and support certain sorting algorithms (like Heapsort). Heaps are typically represented as arrays for simplicity and efficiency, even though they are conceptually binary trees.

---

### Types of Heaps

#### 1. Max-Heap

**Heap Property:**
The value of each node is **greater than or equal to** the values of its children.

**Characteristics:**
- The largest element is at the root
- Example: `[10, 7, 9, 5, 6]` (root 10 is the maximum)

#### 2. Min-Heap

**Heap Property:**
The value of each node is **less than or equal to** the values of its children.

**Characteristics:**
- The smallest element is at the root
- Example: `[3, 5, 4, 7, 6]` (root 3 is the minimum)

---

### Key Characteristics

**Binary Tree:**
A heap is a complete binary tree, meaning all levels are fully filled except possibly the last, which is filled from left to right.

**Array Representation:**
For a node at index `i`:
- Left child: `2i + 1`
- Right child: `2i + 2`
- Parent: `⌊(i-1)/2⌋`

**Example:**
Array `[10, 7, 9, 5, 6]` represents a max-heap tree:
    ```
        10
       /  \
      7    9
     / \
    5   6
    ```

**Height:**
The height of a heap with `n` elements is `O(log n)`, due to its complete binary nature.

---

### Operations on a Heap

#### 1. Heapify

**Description:**
Adjusts a subtree to maintain the heap property after insertion or deletion.

**Time Complexity:**
O(log n) for a single heapify operation

**Example:**
In a max-heap, if a child is larger than its parent, swap them and recurse downward.

---

#### 2. Build Heap

**Description:**
Converts an unsorted array into a heap by heapifying all non-leaf nodes.

**Time Complexity:**
O(n) – surprisingly efficient due to the structure of a complete binary tree

**Example:**
Array `[4, 10, 3, 5, 1]` → Max-Heap `[10, 5, 3, 4, 1]`

---

#### 3. Insert

**Description:**
Adds a new element at the end of the heap (last position in the array) and "bubbles it up" to restore the heap property.

**Time Complexity:**
O(log n) – proportional to the height of the tree

---

#### 4. Extract Max/Min

**Description:**
Removes and returns the root (max in max-heap, min in min-heap), replaces it with the last element, and heapifies the root downward.

**Time Complexity:**
O(log n)

---

#### 5. Peek

**Description:**
Returns the root (max or min) without removing it.

**Time Complexity:**
O(1)

---

### Applications

- **Priority Queue**: Heaps efficiently manage elements with priorities (e.g., task scheduling)
- **Heapsort**: A sorting algorithm that builds a max-heap and repeatedly extracts the maximum to sort in O(n log n)
- **Graph Algorithms**: Used in Dijkstra's algorithm or Prim's algorithm to efficiently select the next minimum edge or distance
- **Median Maintenance**: A pair of heaps (min-heap and max-heap) can track the median of a stream of numbers in O(log n) per insertion

---

### Example: Max-Heap Operations

Start with an empty max-heap and insert: `[5, 3, 7, 1, 9]`.  

- Insert 5: `[5]`  
- Insert 3: `[5, 3]`  
- Insert 7: `[7, 3, 5]` (7 > 5, so it bubbles up)  
- Insert 1: `[7, 3, 5, 1]`  
- Insert 9: `[9, 7, 5, 1, 3]` (9 > 7, bubbles up to root)  
- Extract Max: Remove 9, replace with 3, heapify → `[7, 3, 5, 1]`

---

### Heap vs. Binary Search Tree (BST)

| Feature  | **Heap**                                                   | **Binary Search Tree (BST)**                      |
| -------- | ---------------------------------------------------------- | ------------------------------------------------- |
| Type     | Complete binary tree (array-based usually)                 | Binary tree (pointer-based)                       |
| Property | Max-Heap: Parent ≥ children<br>Min-Heap: Parent ≤ children | Left child < Parent < Right child (for all nodes) |

#### Structure & Order

| Feature        | **Heap**                                    | **BST**                                      |
| -------------- | ------------------------------------------- | -------------------------------------------- |
| Shape property | Always a complete binary tree               | No shape restriction (can become unbalanced) |
| Order property | Heap order (only between parent & children) | In-order traversal gives sorted order        |

#### Operations & Time Complexities

| Operation               | **Heap**                 | **BST** (average) | **BST** (worst case) |
| ----------------------- | ------------------------ | ----------------- | -------------------- |
| Insertion               | `O(log n)`               | `O(log n)`        | `O(n)`               |
| Deletion (root/min/max) | `O(log n)`               | `O(log n)`        | `O(n)`               |
| Search                  | `O(n)`                   | `O(log n)`        | `O(n)`               |
| Access min/max          | `O(1)` (min in min-heap) | `O(log n)`        | `O(n)`               |

> ✅ **BST** allows efficient search.
> ✅ **Heap** allows fast access to the highest (or lowest) priority item.

#### Use Cases

| Feature         | **Heap**                                         | **BST**                                 |
| --------------- | ------------------------------------------------ | --------------------------------------- |
| Priority Queues | ✔️ Ideal (e.g., scheduling, Dijkstra's algorithm) | ❌ Not optimal                           |
| Sorted data     | ❌ Must extract all to get sorted list            | ✔️ In-order traversal yields sorted data |
| Searching       | ❌ Inefficient (no global order)                  | ✔️ Efficient (if balanced)               |

#### Variants

- **Heap:** Min-Heap, Max-Heap, Binary Heap, Fibonacci Heap, Binomial Heap
- **BST:** AVL Tree, Red-Black Tree, Splay Tree (all self-balancing)

#### Summary Table

| Feature         | Heap                        | Binary Search Tree         |
| --------------- | --------------------------- | -------------------------- |
| Maintains order | Partial (only parent-child) | Total (sorted by keys)     |
| Fast search     | ❌                           | ✔️                          |
| Fast min/max    | ✔️                           | ❌ (unless augmented)       |
| Shape           | Complete                    | Can be unbalanced          |
| Use in sorting  | Heap Sort (O(n log n))      | Tree Sort (O(n log n) avg) |

---

### Code Example (Python - Min-Heap using heapq)

Python's `heapq` module implements a min-heap:

```python
import heapq

# Initialize an array and convert to heap
arr = [5, 3, 7, 1, 9]
heapq.heapify(arr)  # O(n)
print(arr)  # [1, 3, 7, 5, 9] - min-heap

# Insert
heapq.heappush(arr, 2)  # O(log n)
print(arr)  # [1, 3, 2, 5, 9, 7]

# Extract min
min_val = heapq.heappop(arr)  # O(log n)
print(min_val, arr)  # 1 [2, 3, 7, 5, 9]
```

---

## Binary Tree vs. Binary Search Tree (BST)

### Basic Definitions

| Feature    | **Binary Tree**                                     | **Binary Search Tree (BST)**                          |
| ---------- | --------------------------------------------------- | ----------------------------------------------------- |
| Definition | A tree where each node has **at most two children** | A binary tree that maintains an **ordering property** |
| Node rule  | No specific order among nodes                       | Left child < Parent < Right child                     |

---

### Structure Rules

| Feature            | **Binary Tree**     | **BST**                                           |
| ------------------ | ------------------- | ------------------------------------------------- |
| Number of children | 0, 1, or 2 per node | Same                                              |
| Order requirement  | ❌ No ordering       | ✔️ Must follow BST ordering                        |
| Balanced?          | ❌ Not necessarily   | ❌ Not necessarily (unless self-balanced like AVL) |

---

### Examples

#### Binary Tree (random structure, no order)

```
        A
      /   \
     B     C
    /       \
   D         E
```

No numeric order, just structure.

---

#### Binary Search Tree (with values)

Insert: 30 → 20 → 40 → 10 → 25

```
        30
       /  \
     20    40
    /  \
  10   25
```

Satisfies:
- Left < Root < Right at all nodes

---

### Operations & Use Cases

| Feature     | **Binary Tree**                                           | **BST**                                  |
| ----------- | --------------------------------------------------------- | ---------------------------------------- |
| Used for    | General data structure (e.g., parsing expressions, trees) | Fast searching, insertion, deletion      |
| Search Time | `O(n)` (no order to guide search)                         | `O(log n)` average (if balanced)         |
| Traversals  | Preorder, Inorder, Postorder                              | Same, plus **inorder gives sorted data** |

---

### Summary Table

| Feature           | **Binary Tree**             | **Binary Search Tree**           |
| ----------------- | --------------------------- | -------------------------------- |
| Max 2 children    | ✔️                           | ✔️                                |
| Ordered           | ❌ No order                  | ✔️ Left < Node < Right            |
| Used for          | General data representation | Efficient search, insert, delete |
| Inorder traversal | Not sorted                  | Sorted                           |
| Search complexity | `O(n)`                      | `O(log n)` average (if balanced) |

---

## Complete Binary Tree

A **complete tree** (specifically, a **complete binary tree**) is a special type of binary tree with the following properties:

### Complete Binary Tree Definition

> A binary tree is **complete** if:
>
> * All levels are **completely filled** except possibly the **last** level.
> * In the last level, all nodes are as **far left as possible**.

---

### Example of a Complete Binary Tree

```
        1
      /   \
     2     3
    / \   /
   4   5 6
```

This is a **complete binary tree** because:
- All levels above the last are full (levels 0 and 1)
- The last level (nodes 4, 5, 6) is filled from left to right

---

### Not a Complete Binary Tree

```
        1
      /   \
     2     3
    /     /
   4     6
```

This is **not complete** because:
- Node 5 is missing before 6, so the last level isn't left-filled

---

### Why It Matters

- Heaps (min-heap or max-heap) are always implemented as complete binary trees
- Being complete allows **efficient array-based storage** (no gaps in array)
- It guarantees **logarithmic height** (good for performance)

---

## Trees

### Binary Trees (22 Types)

Binary trees are trees where each node has **at most two children**.

#### By Structure:

1. **Full Binary Tree** – Every node has 0 or 2 children
2. **Complete Binary Tree** – All levels filled, except possibly the last (filled left to right)
3. **Perfect Binary Tree** – All internal nodes have two children and all leaves are at the same level
4. **Balanced Binary Tree** – Tree height is minimized
5. **Degenerate (Skewed) Tree** – Each parent has only one child
   - **Left-skewed**
   - **Right-skewed**

#### By Ordering:

6. **Binary Search Tree (BST)** – Left < Node < Right
7. **Balanced BST** – Any height-balanced version of BST

#### Self-Balancing Binary Search Trees:

8. **AVL Tree** – Self-balancing BST (balance factor -1, 0, +1)
9. **Red-Black Tree** – BST with color rules for balancing
10. **Splay Tree** – Recently accessed elements move to root
11. **Treap** – BST with heap priorities
12. **Scapegoat Tree** – Maintains balance using size
13. **Tango Tree** – For searching in dynamic BSTs (used in competitive BST research)
14. **Weight-Balanced Tree**

#### Others:

15. **Threaded Binary Tree** – Uses NULL pointers to point to in-order predecessor/successor
16. **Expression Tree** – Leaves are operands, internal nodes are operators
17. **Cartesian Tree** – Inorder traversal is the original sequence, satisfies heap property
18. **Binary Trie (Prefix Tree)** – A trie where each node has at most 2 children (used in IP routing)
19. **Segment Tree** – Used for range queries
20. **Fenwick Tree (Binary Indexed Tree)** – Used for cumulative frequency tables
21. **Decision Tree** – Used in machine learning
22. **Morse Code Tree** – Binary tree representing Morse code paths

---

### B-Trees (9 Types)

B-Trees are **multi-way balanced search trees**, optimized for systems that read/write large blocks of data.

1. **B-Tree** – General balanced M-ary search tree
2. **B+ Tree** – All values at leaves, internal nodes store keys only
3. **B* Tree** – Improves space utilization over B+ tree by redistributing keys before splitting
4. **Binary B-Tree** – B-Tree with only two children per node (rarely used)
5. **B^ε Tree** – Designed for efficient I/O operations
6. **Cache-oblivious B-Tree** – Adapts well to different levels of memory hierarchy
7. **T-Tree** – BST + B-Tree hybrid used in main-memory databases
8. **H-Tree** – Recursive structure, not a search tree but used in VLSI
9. **UB-Tree (Universal B-Tree)** – Used for multidimensional indexing

---

### Heaps (18 Types)

Heaps are specialized tree-based structures that satisfy the **heap property**: parent is ordered with respect to children (min or max).

#### Binary Heaps:

1. **Min-Heap** – Parent ≤ children
2. **Max-Heap** – Parent ≥ children
3. **Binary Heap** – General implementation using array

#### Binomial Family:

4. **Binomial Heap** – Collection of binomial trees
5. **Lazy Binomial Heap**
6. **Strict Binomial Heap**

#### Fibonacci Family:

7. **Fibonacci Heap** – Excellent amortized time for decrease-key
8. **Strict Fibonacci Heap**
9. **Relaxed Fibonacci Heap**
10. **Randomized Fibonacci Heap**

#### Pairing Family:

11. **Pairing Heap** – Simple and efficient
12. **Strict Pairing Heap**

#### Other Heaps:

13. **Skew Heap** – Self-adjusting heap
14. **Treap** – BST + Heap (randomized priorities)
15. **Interval Heap** – Double-ended priority queue
16. **Min-Max Heap** – Supports both min and max in `O(1)`
17. **Soft Heap** – Allows some corruption for better performance
18. **Quake Heap** – Designed for decrease-key efficiency

---

### Tree Categories Summary

| Category     | Count | Examples (short)                                       |
| ------------ | ----- | ------------------------------------------------------ |
| Binary Trees | 22    | BST, AVL, Red-Black, Full, Complete, Expression Tree   |
| B-Trees      | 9     | B-Tree, B+ Tree, B* Tree, T-Tree, UB-Tree              |
| Heaps        | 18    | Min/Max Heap, Fibonacci, Binomial, Treap, Pairing Heap |

---

### Bit-slice Trees (12 types)

These trees are optimized for searching and sorting bitstrings or fixed-width integers.

- Binary Trie
- Patricia Trie (Radix Tree)
- Crit-bit Tree
- LC-Trie (Level-compressed Trie)
- XBWT (eXtended Burrows–Wheeler Transform)
- Burst Trie
- Adaptive Radix Tree (ART)
- Hash Array Mapped Trie (HAMT)
- Compressed Trie
- Ternary Search Tree
- Van Emde Boas Tree
- Bitwise Trie

---

### Multi-way Trees (14 types)

Designed to handle more than two children per node, often used in databases and file systems.

- B-tree
- B+ tree
- B* tree
- 2–3 Tree
- 2–3–4 Tree
- (a,b)-Tree
- M-ary Tree
- K-ary Tree
- T-tree
- Quad Tree
- Octree
- Fusion Tree
- Segment Tree
- Interval Tree

---

### Space-partitioning Trees (28 types)

Used for organizing spatial data in computational geometry, graphics, and GIS.

- KD-tree
- Quadtree
- Octree
- BSP Tree (Binary Space Partitioning)
- R-tree
- R+ tree
- R* tree
- X-tree
- M-tree
- PH-tree
- VP-tree (Vantage-Point Tree)
- Metric Tree
- Cover Tree
- Ball Tree
- Bounding Volume Hierarchy (BVH)
- AABB Tree
- Loose Octree
- Region Quadtrees
- Point Quadtrees
- MX-CIF Quadtrees
- k-d-b Tree
- Hilbert R-tree
- Priority R-tree
- SS-tree
- SR-tree
- Slim-tree
- QuadKD Tree
- Hybrid Trees (e.g., Quad-BSP Trees)

---

### Application-specific Trees (9 types)

Designed for use in specific algorithms or applications.

- Syntax Tree (Abstract Syntax Tree, Parse Tree)
- Expression Tree
- Decision Tree
- Game Tree (Minimax Tree)
- Huffman Tree
- Merkle Tree
- Suffix Tree
- Treap
- Cartesian Tree

---

### Trie / Digital / Prefix Trees (Typically 3–5 types)

Efficient for string search, autocomplete, and IP routing.

- Standard Trie
- Patricia Trie (also called Radix Trie)
- Compressed Trie
- Suffix Trie
- Ternary Search Tree (also sometimes grouped here)

---

## Arrays (19 types)

| **Name**                  | **Description**                                                                  | **Examples / Use Cases**                                                                      |
| ------------------------- | -------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| **Array**                 | Fixed-size, contiguous memory structure with indexed elements.                   | `int arr[5] = {1,2,3,4,5}` in C; storing scores, sensor readings.                             |
| **Bit Array**             | Array of bits (0 or 1), space-efficient representation of boolean values.        | Flags, bloom filters, efficient memory storage of true/false.                                 |
| **Bit Field**             | Set of adjacent bits within a single variable used to store flags or small ints. | Hardware registers, embedded systems, memory packing in C structs.                            |
| **Bitboard**              | 64-bit board representation (each bit = 1 square) used in games like chess.      | Chess engines like Stockfish, checkers, and Go representations.                               |
| **Bitmap**                | Maps bits to indicate presence/absence or to store image pixels.                 | File systems (free space tracking), monochrome image storage, font rendering.                 |
| **Circular Buffer**       | Array-like buffer that wraps around when the end is reached (FIFO).              | Audio/video streaming, keyboard buffers, producer-consumer queues.                            |
| **Control Table**         | Table of parameters or states for controlling hardware or software behavior.     | CPU control registers, microcontroller config tables, GUI behavior tables.                    |
| **Image**                 | 2D (or 3D) array of pixels or voxels.                                            | JPEG, PNG image files; digital cameras; medical imaging (CT, MRI).                            |
| **Dope Vector**           | Metadata structure storing array bounds, dimensions, and strides.                | Fortran, Ada arrays; used in compilers for multidimensional array access.                     |
| **Dynamic Array**         | Resizable array that grows/shrinks automatically (uses extra capacity).          | C++ `vector`, Python `list`, Java `ArrayList`.                                                |
| **Gap Buffer**            | Array with a "gap" to optimize text insertion/deletion near cursor.              | Text editors like Emacs, custom line editors.                                                 |
| **Hashed Array Tree**     | Tree of arrays with hashed indexing to improve memory and resizing efficiency.   | Alternative to dynamic arrays in memory-constrained or large-scale systems.                   |
| **Lookup Table**          | Precomputed array used to speed up repeated calculations.                        | Trig functions, CRC checksums, font rendering, gaming (e.g., light maps).                     |
| **Matrix**                | 2D array used for storing data in rows and columns.                              | Mathematics, linear algebra, image processing, spreadsheets.                                  |
| **Parallel Array**        | Multiple arrays where each holds a field of the same indexed record.             | Game engines (position[], velocity[], health[]), physics simulations, data-oriented design.   |
| **Sorted Array**          | Array where elements are maintained in sorted order.                             | Binary search, autocomplete lists, sorted logs.                                               |
| **Sparse Matrix**         | Matrix with mostly zero (or null) entries; stored efficiently.                   | Graph algorithms, machine learning (e.g., NLP term-frequency matrices), scientific computing. |
| **Iliffe Vector**         | Array of pointers to arrays, often used for jagged (non-rectangular) matrices.   | Multidimensional arrays in Java, C (jagged arrays).                                           |
| **Variable-length Array** | Arrays whose size is determined at runtime rather than compile-time.             | C99 `int arr[n];`, stack-based buffers, embedded systems.                                     |

---

## Lists (14 types)

| **Name**                                                     | **Description**                                                                | **Examples / Use Cases**                                                      |
| ------------------------------------------------------------ | ------------------------------------------------------------------------------ | ----------------------------------------------------------------------------- |
| **Doubly Linked List**                                       | Nodes have pointers to both next and previous nodes.                           | Browser history (back/forward), undo-redo stacks, LRU cache.                  |
| **Array List**                                               | Dynamic array that resizes as needed; indexable in O(1).                       | Java `ArrayList`, Python `list`, C++ `vector`; general-purpose storage.       |
| **Linked List**                                              | Each node points only to the next; allows fast insertions/deletions.           | Queue implementations, OS scheduling, polynomial representations.             |
| **Association List**                                         | List of key-value pairs (often unsorted); used for small maps.                 | Lisp property lists, Python dictionaries (internally as fallback).            |
| **Self-Organizing List**                                     | List that reorders elements based on access patterns (e.g., move-to-front).    | Cache lists, autocomplete systems, adaptive algorithms.                       |
| **Skip List**                                                | Probabilistic layered list structure allowing fast search (O(log n)).          | Alternative to balanced trees in databases and key-value stores.              |
| **Unrolled Linked List**                                     | Linked list where each node contains an array (block) of elements.             | Text editors, large datasets requiring fewer node allocations.                |
| **VList**                                                    | Persistent list allowing versioning with logarithmic access.                   | Functional programming, immutable data structures in OCaml, Haskell.          |
| **Conc-tree List**                                           | Tree-based, parallel-friendly structure for concatenating lists efficiently.   | Functional programming, concurrency-safe list operations (e.g., Scala).       |
| **XOR Linked List**                                          | Uses XOR of next and previous pointers to save space (1 pointer/node).         | Space-limited applications, rarely used due to pointer arithmetic complexity. |
| **Zipper**                                                   | Functional structure maintaining focus on one part of the list/tree.           | Editor buffers, navigating trees or JSON structures in functional languages.  |
| **Doubly Connected Edge List**<br> *(also called Half-Edge)* | Stores geometric mesh info: edges, vertices, and faces with cross-references.  | Computational geometry, 3D modeling, CAD software.                            |
| **Difference List**                                          | Representation of lists as functions to enable efficient appends.              | Used in Prolog and functional languages for efficient concatenation.          |
| **Free List**                                                | List of memory blocks available for allocation (linked list of unused memory). | Memory management systems, object pools, garbage collectors.                  |

### List Categories

| **Category**             | **Use Cases**                               |
| ------------------------ | ------------------------------------------- |
| **Sequential Access**    | Linked List, Doubly Linked List, Array List |
| **Associative Data**     | Association List, Self-Organizing List      |
| **Optimized Search**     | Skip List, Self-Organizing List, Zipper     |
| **Functional/Immutable** | VList, Conc-tree, Difference List, Zipper   |
| **Space Efficiency**     | XOR Linked List, Unrolled List, Free List   |
| **Specialized Geometry** | DCEL (Half-Edge) for 3D geometry and CAD    |

---

## Hash-Based Structures (16 types)

### 1. Bloom Filter (Фильтр Блума)

  Это вероятностная структура данных, используемая для проверки принадлежности элемента множеству. Она компактна и эффективна по памяти, но допускает ложные срабатывания (false positives), хотя никогда не пропускает элементы, которые точно есть в множестве (нет false negatives). Работает с помощью нескольких хэш-функций, которые устанавливают биты в битовом массиве.

**This is a probabilistic data structure used to check element membership in a set. It's compact and memory-efficient, but allows false positives, though it never misses elements that are definitely in the set (no false negatives). Works using multiple hash functions that set bits in a bit array.**

---

### 2. Count–min Sketch (Скетч "счётчик-минимум")

  Вероятностная структура для оценки частоты появления элементов в потоке данных. Использует несколько хэш-функций и таблицу счётчиков, чтобы приблизительно подсчитывать, сколько раз элемент встретился, с возможностью переоценки, но не недооценки.

**Probabilistic structure for estimating frequency of elements in a data stream. Uses multiple hash functions and a counter table to approximately count how many times an element appeared, with possibility of overestimation but not underestimation.**

---

### 3. Distributed Hash Table (Распределённая хэш-таблица)

  Это децентрализованная структура данных, которая распределяет пары "ключ-значение" по узлам сети. Позволяет эффективно искать и хранить данные в распределённых системах, таких как пиринговые сети (например, BitTorrent).

**This is a decentralized data structure that distributes key-value pairs across network nodes. Allows efficient searching and storing of data in distributed systems, such as peer-to-peer networks (e.g., BitTorrent).**

---

### 4. Double Hashing (Двойное хэширование)

  Метод разрешения коллизий в хэш-таблицах. Использует две хэш-функции: первая определяет начальную позицию, а вторая задаёт шаг для поиска свободного слота при коллизии, что уменьшает кластеризацию.

**Method for resolving collisions in hash tables. Uses two hash functions: the first determines the initial position, and the second sets the step for finding a free slot during collision, which reduces clustering.**

---

### 5. Dynamic Perfect Hash Table (Динамическая совершенная хэш-таблица)

  Хэш-таблица, которая гарантирует отсутствие коллизий и обеспечивает доступ за O(1), даже при динамическом добавлении элементов. Использует сложные алгоритмы для адаптации к изменяющемуся набору ключей.

**Hash table that guarantees no collisions and provides O(1) access, even with dynamic element addition. Uses complex algorithms to adapt to changing key sets.**

---

### 6. Hash Array Mapped Trie (Хэш-массивное отображённое дерево)

  Структура, сочетающая хэш-таблицу и дерево (trie). Ключи сначала хэшируются, а затем распределяются по узлам дерева, что обеспечивает компактное хранение и быстрый доступ с меньшим количеством коллизий.

**Structure combining hash table and tree (trie). Keys are first hashed, then distributed across tree nodes, providing compact storage and fast access with fewer collisions.**

---

### 7. Hash List (Хэш-список)

  Простая структура, где данные организованы в список, а доступ к ним осуществляется через хэш-функцию. Часто используется для проверки целостности данных, например, в цепочках хэшей.

**Simple structure where data is organized in a list, and access is through a hash function. Often used for data integrity checking, for example, in hash chains.**

---

### 8. Hash Table (Hash Map) (Хэш-таблица)

  Хэш-таблица — это структура данных, которая используется для хранения пар "ключ-значение" и обеспечивает быстрый доступ к значениям по их ключам. Она основана на использовании хэш-функции, которая преобразует ключ в числовой индекс, указывающий, где в памяти будет храниться соответствующее значение.

**Hash table is a data structure used to store key-value pairs and provides fast access to values by their keys. It's based on using a hash function that converts a key into a numeric index indicating where in memory the corresponding value will be stored.**

**How It Works:**
1. **Хэш-функция (Hash Function)**: Принимает ключ (например, строку, число или другой объект) и вычисляет для него уникальный индекс (хэш). Например, хэш-функция может суммировать коды символов строки и брать остаток от деления на размер таблицы (Takes a key and computes a unique index for it)
2. **Хранение (Storage)**: Значение сохраняется в массиве по вычисленному индексу (Value is stored in array at computed index)
3. **Получение (Retrieval)**: Чтобы найти значение, хэш-функция снова применяется к ключу, и система сразу обращается к нужному индексу (To find value, hash function is applied again to key, and system immediately accesses needed index)

**Особенности (Features):**
- **Коллизии (Collisions)**: Иногда разные ключи могут давать одинаковый хэш (например, из-за ограниченного размера таблицы). Для их разрешения используются методы, такие как цепочки (список значений на одном индексе) или открытая адресация (поиск следующего свободного места) (Sometimes different keys can give same hash. Methods like chaining or open addressing are used to resolve)
- **Скорость (Speed)**: В среднем операции вставки, поиска и удаления в хэш-таблице выполняются за время O(1), что делает её очень эффективной (On average, insert, search, and delete operations take O(1) time, making it very efficient)
- **Пример в Python (Example in Python)**: Встроенный тип данных `dict` (словарь) является реализацией хэш-таблицы (Built-in `dict` type is a hash table implementation)

**Example:**
 ```python
# Словарь как хэш-таблица (Dictionary as hash table)
 hash_table = {"имя": "Алексей", "возраст": 25}
 print(hash_table["имя"])  # Вывод: Алексей
 ```

Here, the key "имя" is converted to an index using an internal hash function, and the value "Алексей" is quickly found.

**Use Cases:**
Хэш-таблицы широко применяются там, где важна скорость доступа к данным, например, в базах данных, кэшах или для подсчёта частоты элементов (Hash tables are widely used where data access speed is important, e.g., in databases, caches, or for counting element frequency).

---

### 9. Hash Tree (Хэш-дерево, или дерево Меркла)

  Древовидная структура, где листья содержат хэши данных, а внутренние узлы — хэши своих дочерних узлов. Используется для проверки целостности и аутентичности больших наборов данных (например, в блокчейнах).

**Tree structure where leaves contain data hashes, and internal nodes contain hashes of their child nodes. Used for checking integrity and authenticity of large datasets (e.g., in blockchains).**

---

### 10. Hash Trie (Хэш-три)

  Вариант дерева (trie), где ключи хэшируются для распределения по узлам. Обеспечивает эффективный поиск и вставку, часто используется в системах с большими наборами ключей.

**Variant of tree (trie) where keys are hashed for distribution across nodes. Provides efficient search and insertion, often used in systems with large key sets.**

---

### 11. Koorde

  Распределённая хэш-таблица, основанная на алгоритме Chord и графе де Брёйна. Обеспечивает маршрутизацию и поиск данных в распределённых системах с меньшим числом переходов между узлами.

**Distributed hash table based on Chord algorithm and de Bruijn graph. Provides routing and data search in distributed systems with fewer hops between nodes.**

---

### 12. Prefix Hash Tree (Префиксное хэш-дерево)

  Структура, похожая на дерево Меркла, но оптимизированная для работы с префиксами ключей. Используется для проверки целостности данных с иерархической организацией.

**Structure similar to Merkle tree but optimized for working with key prefixes. Used for checking integrity of hierarchically organized data.**

---

### 13. Rolling Hash (Скользящий хэш)

  Метод вычисления хэша для последовательности данных, где хэш обновляется при сдвиге "окна" по данным. Применяется, например, в алгоритмах поиска подстрок (алгоритм Рабина-Карпа).

**Method for computing hash for a data sequence, where hash is updated when "window" shifts over data. Applied, for example, in substring search algorithms (Rabin-Karp algorithm).**

---

### 14. MinHash

  Техника для оценки схожести множеств. Преобразует элементы в сигнатуры с помощью хэш-функций, позволяя быстро сравнивать большие наборы данных, например, в задачах кластеризации или поиска дубликатов.

**Technique for estimating set similarity. Transforms elements into signatures using hash functions, allowing fast comparison of large datasets, e.g., in clustering or duplicate finding tasks.**

---

### 15. Quotient Filter (Коэффициентный фильтр)

  Компактная вероятностная структура для проверки принадлежности элемента множеству. Похожа на фильтр Блума, но поддерживает слияние фильтров и более точное управление ложными срабатываниями.

**Compact probabilistic structure for checking element membership in a set. Similar to Bloom filter but supports filter merging and more precise control of false positives.**

---

### 16. Ctrie (Конкурентное хэш-три)

  Потокобезопасная реализация хэш-три, не использующая блокировки. Обеспечивает атомарные операции в многопоточных средах, сохраняя высокую производительность и масштабируемость.

**Lock-free thread-safe implementation of hash trie. Provides atomic operations in multithreaded environments while maintaining high performance and scalability.**

---

## Graphs (14 types)

| **Name**                                   | **Description**                                                                             | **Examples / Use Cases**                                                  |
| ------------------------------------------ | ------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| **Graph**                                  | A set of **nodes (vertices)** and **edges** connecting them. Can be directed or undirected. | Social networks, maps, neural networks, dependency resolution.            |
| **Adjacency List**                         | A collection where each vertex stores a list of adjacent vertices.                          | Memory-efficient graph representation; e.g., Facebook friend suggestions. |
| **Adjacency Matrix**                       | 2D matrix where `M[i][j]` indicates presence (and weight) of edge i → j.                    | Dense graphs, quick edge lookups; e.g., road networks, game AI.           |
| **Graph-Structured Stack (GSS)**           | Stack-like structure with graph connections used in parsing ambiguous grammars.             | GLR parsers, computational linguistics.                                   |
| **Scene Graph**                            | Tree or DAG where nodes represent spatial entities and transformations.                     | 3D graphics engines, game engines, AR/VR scenes (e.g., Unity, Blender).   |
| **Decision Tree**                          | Tree where internal nodes are decisions, leaves are outcomes.                               | Machine learning classification, business rules.                          |
| **Binary Decision Diagram (BDD)**          | DAG that represents boolean functions efficiently with binary variables.                    | Formal verification, logic optimization, circuit design.                  |
| **Zero-Suppressed Decision Diagram (ZDD)** | Variant of BDD optimized for sparse combinations of variables.                              | Data mining, set systems, itemset mining.                                 |
| **And-Inverter Graph (AIG)**               | DAG using only AND gates and inverters to represent logic circuits.                         | Hardware synthesis, model checking, FPGA optimization.                    |
| **Directed Graph (Digraph)**               | Graph where edges have a direction (from → to).                                             | Web links, citation networks, process flows.                              |
| **Directed Acyclic Graph (DAG)**           | Directed graph with **no cycles**.                                                          | Task scheduling, build systems (e.g., Make), version control (Git DAG).   |
| **Propositional DAG (PDAG)**               | DAG for propositional logic where identical sub-formulas are shared.                        | SAT solvers, logic compilers, expression optimization.                    |
| **Multigraph**                             | Graph where **multiple edges** between the same pair of vertices are allowed.               | Transportation systems (multiple routes), network routing.                |
| **Hypergraph**                             | A generalization where **edges (hyperedges)** can connect more than two vertices.           | Database modeling, VLSI circuit design, document clustering.              |

### Graph Categories

| **Structure**                 | **Main Use Case**                 |
| ----------------------------- | --------------------------------- |
| **Basic Graphs**              | Graph, Directed Graph, Multigraph |
| **Graph Representations**     | Adjacency List, Adjacency Matrix  |
| **Parsing / Compilation**     | GSS, And-Inverter Graph, PDAG     |
| **AI & ML**                   | Decision Tree, BDD, ZDD           |
| **3D / Game Dev**             | Scene Graph                       |
| **Scheduling / Dependencies** | DAG, PDAG, Git, Makefile systems  |
| **Advanced Modeling**         | Hypergraph                        |

---

### Graph Representation

Graphs can be represented in multiple ways depending on the type of graph and the operations you want to perform.

#### 1. Adjacency List (using dictionary of lists)

```python
graph = {
    'A': ['B', 'C'],
    'B': ['A', 'D'],
    'C': ['A', 'D'],
    'D': ['B', 'C']
}
```

🔹 **Use Case:** Most common for sparse graphs. Efficient space-wise.

---

#### 2. Adjacency Matrix (2D list)

```python
# Nodes: A, B, C
nodes = ['A', 'B', 'C']
matrix = [
    [0, 1, 1],  # A → B, C
    [1, 0, 0],  # B → A
    [1, 0, 0]   # C → A
]
```

🔹 **Use Case:** Good for dense graphs or when constant-time edge lookup is needed.

---

#### 3. Edge List (list of tuples)

```python
edges = [
    ('A', 'B'),
    ('A', 'C'),
    ('B', 'D'),
    ('C', 'D')
]
```

🔹 **Use Case:** Simple storage, often used when input is from files.

---

#### 4. Using `networkx` Library (for advanced graph operations)

```python
import networkx as nx

G = nx.DiGraph()  # or nx.Graph() for undirected
G.add_edge('A', 'B')
G.add_edge('A', 'C')
G.add_edge('B', 'D')
G.add_edge('C', 'D')

print(list(G.nodes))
print(list(G.edges))
```

🔹 **Use Case:** Ideal for visualization, shortest paths, and graph algorithms.

---

### Graph Representation Comparison

| **Representation** | **Best For**                 | **Pros**                    | **Cons**                     |
| ------------------ | ---------------------------- | --------------------------- | ---------------------------- |
| Adjacency List     | Sparse graphs                | Memory efficient, intuitive | Slower edge lookups          |
| Adjacency Matrix   | Dense graphs                 | Fast edge lookup            | High memory for large graphs |
| Edge List          | Input/output, simple storage | Minimal structure           | Slower for most operations   |
| `networkx` Graph   | Complex graphs & algorithms  | Rich features, easy to use  | External dependency          |

---

## Resources

- [List of Data Structures](https://en.wikipedia.org/wiki/List_of_data_structures)

---

## Summary

**Data Structure Categories:**
- **Python Built-in Types**: List, Tuple, Set, Dict (4 types)
- **Linked Lists**: Singly, Doubly, Circular (3 main types)
- **Heaps**: Min-Heap, Max-Heap, and variants (18 types)
- **Trees**: Binary Trees (22), B-Trees (9), Heaps (18), and more
- **Arrays**: 19 different array types
- **Lists**: 14 different list types
- **Hash-Based Structures**: 16 types including hash tables, Bloom filters, etc.
- **Graphs**: 14 types with various representations

**Key Takeaways:**
- Choose data structures based on access patterns and operations needed
- Understand time and space complexity for each structure
- Consider mutability, ordering, and indexing requirements
- Use appropriate representations for graphs (adjacency list vs matrix)
- Python provides optimized built-in types, but custom structures can be implemented

**Selection Guide:**
- **Fast lookups**: Use hash tables (dict, set)
- **Ordered data**: Use lists or sorted arrays
- **Priority operations**: Use heaps
- **Tree structures**: Use for hierarchical data
- **Graph structures**: Use for relationships and networks

Understanding these data structures and their properties is essential for efficient algorithm design and problem-solving.
