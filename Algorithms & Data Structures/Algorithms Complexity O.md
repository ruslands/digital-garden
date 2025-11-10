# Algorithm Complexity: Big O Notation Guide

## Table of Contents
1. [Introduction](#introduction)
2. [Time Complexity](#time-complexity)
3. [Space Complexity](#space-complexity)
4. [Asymptotic Analysis](#asymptotic-analysis)
5. [Big O Notation](#big-o-notation)
6. [Complexity Examples](#complexity-examples)
7. [String Operations Complexity](#string-operations-complexity)
8. [List Operations Complexity](#list-operations-complexity)
9. [Resources](#resources)
10. [Summary](#summary)

---

## Introduction

**Resource:** [uLearn Complexity Course](https://ulearn.me/Course/complexity)

Пространственная и временная сложность — это ключевые понятия в анализе алгоритмов, которые помогают оценить их эффективность (Spatial and temporal complexity are key concepts in algorithm analysis that help evaluate their efficiency).

**Purpose:**
- Evaluate algorithm efficiency
- Compare different algorithms
- Predict performance on large inputs
- Make informed design decisions

---

## Time Complexity

**Временная сложность (time complexity)** описывает, сколько времени требуется алгоритму для выполнения в зависимости от размера входных данных (обычно обозначается как n). Она измеряется в терминах количества элементарных операций и выражается с помощью нотации "О-большое" (Big O).

### Common Time Complexities

| Complexity     | Name                                              | Description                                                            | Example                                 |
| -------------- | ------------------------------------------------- | ---------------------------------------------------------------------- | --------------------------------------- |
| **O(1)**       | Константное время (Constant time)                 | Не зависит от размера входных данных                                   | Доступ к элементу массива по индексу    |
| **O(log n)**   | Логарифмическое время (Logarithmic time)          | Растет медленно, типично для бинарного поиска или операций с деревьями | Бинарный поиск, операции с деревьями    |
| **O(n)**       | Линейное время (Linear time)                      | Растет пропорционально размеру входных данных                          | Один цикл по массиву                    |
| **O(n log n)** | Линейно-логарифмическое время (Linearithmic time) | Типично для эффективных алгоритмов сортировки                          | QuickSort или MergeSort                 |
| **O(n²)**      | Квадратичное время (Quadratic time)               | Растет как квадрат размера данных                                      | Вложенные циклы в сортировке пузырьком  |
| **O(2ⁿ)**      | Экспоненциальное время (Exponential time)         | Очень быстрое увеличение, характерно для рекурсивных алгоритмов        | Задачи о подмножествах                  |
| **O(n!)**      | Факториальное время (Factorial time)              | Чрезвычайно быстрое увеличение, встречается в задачах полного перебора | Задача коммивояжера методом грубой силы |

**Visual Reference:**
![[0-notation.jpeg]]

---

## Space Complexity

**Пространственная сложность (space complexity)** оценивает, сколько памяти (или дополнительного пространства) требуется алгоритму для работы, также в зависимости от n.

**Components:**
- **Фиксированная часть**: память, не зависящая от входных данных (например, переменные)
- **Переменная часть**: память, зависящая от n (например, массивы, рекурсивные вызовы)

**Examples:**
- **O(1)**: константная память (например, несколько переменных)
- **O(n)**: линейная память (например, массив размером n)
- **O(n²)**: квадратичная память (например, двумерная матрица)

---

## Asymptotic Analysis

### Function Complexity

**Функция сложности (Complexity Function):**
это функция, которая показывает, **сколько операций** выполняет алгоритм в зависимости от размера входных данных `n`. Она помогает формализовать и сравнить эффективность разных алгоритмов.

### Asymptotic Analysis Definition

**Асимптотический анализ (Asymptotic Analysis):**
метод описания предельного поведения функций, показывает поведение функции сложности при **больших значениях n**, игнорируя константы и менее значимые члены.

**Основные обозначения (Main Notations):**
- **O(f(n))** — верхняя граница (наихудший случай) (upper bound, worst case)
- **Ω(f(n))** — нижняя граница (лучший случай) (lower bound, best case)
- **Θ(f(n))** — точная оценка (и верхняя, и нижняя граница) (tight bound, both upper and lower)

---

### Why Use Asymptotic Estimation?

**For real code, computational complexity can be an extremely complex function! :-)**

**Challenges:**
- Easy to make mistakes in calculations and forget some terms
- Many reasons why it doesn't make sense to calculate the exact form of the complexity function

**Reasons:**
1. Some "elementary" operations, such as sin or square root, are more expensive, and the complexity function doesn't account for this
2. The cost of elementary operations is different on different processors
3. The cost of an elementary operation depends on the execution context (caching, context switching, processor instruction pipeline, ...)
4. When code is written in a high-level language (C#, Python, JavaScript, ...) there is hidden cost: memory allocation, garbage collection, JIT compilation, etc.)

**Solution:**
Instead of calculating boring complex formulas, engineers use so-called **asymptotic estimation** of the complexity function.

**How to Get Asymptotic Estimation:**
If briefly and roughly, you can get an asymptotic estimate like this: **discard all terms in the complexity function except the one with the fastest growth rate. Then discard all constants. What you get will be the asymptotic complexity estimate.**

**Why It Matters:**
Usually, we're interested in algorithm runtime on large data, when the algorithm can slow down significantly. Asymptotic estimation answers the question of how much performance degrades as input size grows.

---

## Big O Notation

### Notation Usage

For complexity assessment, engineers usually use so-called **O-notation**. For example, they say "the complexity of this algorithm is O(n²)" (pronounced: O of n squared), and this one is Θ(n log(n)) (pronounced: theta of n log n).

**Brief Explanation:**
- **O(n²)** means that the fastest-growing term in the complexity function is n to a power of no more than 2
- **Θ(n²)** means that the power is exactly 2

**Interpretation:**
- **"f = O(g)"** can be interpreted as: "function f(n) grows no faster than function g(n) as n grows"
- **"f = Θ(g)"** means: "f grows as fast as g (if we ignore differences in constant factors)"

**Preference:**
As you can see, Θ is more precise and, accordingly, more preferable than Big O. However, people very often use the Big O symbol in the sense of Θ, especially during informal conversations, especially in writing.

**Practical Approach:**
In a quick analysis of an algorithm, we don't need the complexity function, just its estimate. Therefore, the boring and meticulous calculation of all coefficients turns into a light and pleasant activity — just simplify and discard the unnecessary!

---

### Example Analysis

**Code:**
```cpp
int i = 1;
while (i < n)
{
  for(int j=0; j<n-2; j++)
    count++;
  i++;
}
```

**Analysis:**
1. Find the "hottest" operation — the one that executes the most times. Most often, such an operation is inside the most nested loop. In our case, it's `count++`.
2. Notice that `count++` executes slightly less than n² times. Not exactly n², because i starts at 1, not zero, and the inner loop goes to n-2. However, an experienced engineer won't even try to account for these constants — they already know that the result will be a quadratic estimate.
3. If you're not sure yet — check. In this case, for example, the exact number of `count++` executions is (n-1)*(n-2) = Θ(n²).
4. A couple of such checks should be enough to develop intuition and then not even start messing with these constants, but immediately declare that `count++` will execute Θ(n²) times.
5. Just in case, check if there's another operation in the code that would execute more than Θ(n²) times. In our case, there is no such operation. All other operations won't change the asymptotics, so they can even be ignored. So the complexity estimate of this code is Θ(n²). Done!

---

### Performance Table

We evaluate algorithm complexity to make sure it won't "slow down". Asymptotic estimation gives us an approximate idea of at what task size (that same n on which the complexity function depends) we can expect acceptable execution speed.

**Visual Reference:**
![[o-notaion-2.png]]

**Zones:**
- **Green zone**: Values less than 10 million — approximately how many elementary operations can execute "quickly" on modern computers for human reaction speed
- **Orange zone**: If your algorithm runs on data of such size that your complexity falls into the orange zone — you'll have to wait
- **Red zone**: You won't get results the same day

**Key Observations:**
- There's a cardinal difference between O(n) and O(n²) and especially O(n³)
- Factorial and exponential are applicable only for the smallest values of n
- For processing multi-million arrays, algorithms with O(n²) or slower are no longer suitable

---

### What is Asymptotics?

**Асимптотика (Asymptotics)** in the context of algorithms is a way of describing **how execution time or memory usage** of an algorithm changes as **input data size** `n` **increases**.

**Why Do We Need Asymptotics?**
- Allows **comparing algorithms by efficiency**
- Helps understand **whether an algorithm will work fast on large data**
- Ignores details like specific machines, languages, and constants — analysis "in general"

---

### Main Notations

| Обозначение | Что означает                        | Пример                                      |
| ----------- | ----------------------------------- | ------------------------------------------- |
| **O(f(n))** | Верхняя граница (наихудший случай)  | `O(n²)` — не медленнее, чем `n²`            |
| **Ω(f(n))** | Нижняя граница (лучший случай)      | `Ω(n)` — минимум как `n` операций           |
| **Θ(f(n))** | Точная оценка (и верхняя, и нижняя) | `Θ(n log n)` — и не быстрее, и не медленнее |

---

### Complexity Examples (Best to Worst)

| Обозначение      | Название                       | Пример алгоритма                     |
| ---------------- | ------------------------------ | ------------------------------------ |
| `O(1)`           | Константная                    | Доступ к элементу массива по индексу |
| `O(log n)`       | Логарифмическая                | Бинарный поиск                       |
| `O(n)`           | Линейная                       | Один проход по массиву               |
| `O(n log n)`     | Линейно-логарифмическая        | Быстрая сортировка, Merge Sort       |
| `O(n²)`          | Квадратичная                   | Вложенные циклы                      |
| `O(2ⁿ)`, `O(n!)` | Экспоненциальная/факториальная | Очень неэффективные алгоритмы        |

**Remember:**
**Asymptotics is a way to talk about algorithm performance growth as data volume increases, without being distracted by implementation details.**

---

## Complexity Examples

### Example 1: Linear Loop

**Code:**
```cpp
for (int i = 0; i < n; i++)
    count++;
```

**Elementary Operations Count:**
- `1` — for `int i = 0`
- `n + 1` — for condition check `i < n`
- `2n` — for `i++` (equivalent to `i = i + 1`, which is two operations: addition and assignment)
- `2n` — for `count++` (increment consists of read, increment, and write)

**Total Complexity Function:**
**C(n) = 2 + 5n**

**Asymptotic Complexity:**
For complexity function `C(n) = 2 + 5n`:
- **O(n)** — linear complexity
- **Ω(n)** — linear complexity
- **Θ(n)** — exact linear complexity

This algorithm has **linear asymptotic complexity**, as the number of operations grows proportionally to `n`.

---

### Example 2: Nested Loops

**Code:**
```cpp
int i = 0;
while (i < n)
{
    for (int j = 0; j < n; j++)
        count++;
    i++;
}
```

**Operations Count:**
- `i = 0` — executes 1 time
- `i < n` — while loop condition, executes `n + 1` times
- `i++` — executes `n` times, and since `i += 1` is two operations (addition and assignment), total `2n` operations

**Nested Loop `for` Analysis:**
Executes **n times**, and each time:
- `j = 0` — 1 operation
- `j < n` — condition check, executes `n + 1` times
- `j++` — executes `n` times, 2 operations each (addition + assignment) → `2n` operations
- `count++` — executes `n` times, 2 operations each (addition + assignment) → `2n` operations

**Total Complexity Function:**
```text
C(n) = 1                          // initialization i
     + (n + 1)                    // while condition checks
     + 2n                         // i++ increments
     + n * (                      // for loop executes n times
         1                        // j = 0
         + (n + 1)                // j < n condition checks
         + 2n                     // j++
         + 2n                     // count++
       )
```

**Simplified:**
```text
C(n) = 1 + (n + 1) + 2n + n(1 + (n + 1) + 2n + 2n)
     = 1 + n + 1 + 2n + n(1 + n + 1 + 2n + 2n)
     = 2 + 3n + n(2 + 5n)
     = 2 + 3n + 2n + 5n²
     = 2 + 5n + 5n²
```

**Result:**
**C(n) = 5n² + 5n + 2**

**Asymptotic Complexity:**
Since the leading term `5n²` dominates for large `n`, we get:

**O(n²)** — quadratic complexity

---

### Example 3: Loop with Step

**Code:**
```cpp
int count = 0;
for (int i = n / 10; i >= 0; i -= 100)
    if (i % 3 == 0) count++;
```

**Loop Analysis:**
- **Start**: `i = n / 10`
- **Condition**: `i >= 0`
- **Step**: `i -= 100`

On each step, `i` decreases by 100 until it becomes less than 0. How many iterations total?

**Number of Iterations:**
```text
i = n / 10
i -= 100 on each iteration
```

Number of steps ≈ `n / 1000`
(because `n / 10` is divided by 100 on each step)

> That is:
> `number of iterations ≈ n / 1000 = O(n)`

**BUT!** Division by a **constant** (1000) in asymptotics **is not considered**. We always ignore constant factors in Big-O.

**Operations in Loop Body:**
```cpp
if (i % 3 == 0) count++;
```
This is a **constant operation**, i.e., executes in `O(1)` on each iteration.

**Total Complexity:**
The loop executes `O(n)` times (with step 100), and inside — `O(1)`.

**Result:**
> **Asymptotic complexity: `O(n)` — linear.**

---

### Example 4: Inverse Linear Complexity

**Code:**
```cpp
var count = 0;
for (int i = 0; i < 1000000; i += n)
    count++;
```

**What Happens:**
- `i` starts at `0`
- Increases by `n` on each step: `i += n`
- Loop continues while `i < 1,000,000`
- Loop body — one constant operation: `count++`

**Number of Iterations:**
Number of iterations ≈ `1,000,000 / n`

That is:
- If `n = 1`, loop executes **1,000,000 times**
- If `n = 2`, then **500,000 times**
- If `n = 1000000`, then **1 time**
- If `n > 1,000,000`, then **0 times**

**Asymptotic Complexity:**
Since the number of iterations is **inversely proportional to `n`**, and inside — `O(1)` operation, we get:

> **O(1,000,000 / n)**

But since 1,000,000 is a **constant**, in asymptotics we discard it.

**Final Asymptotic Complexity:**
> **O(1 / n)** — *inverse linear complexity*

⚠️ However: in Big-O terms **this is considered O(1)**, because as **`n` grows**, execution time **doesn't grow, but rather decreases**, and **O(1)** means "doesn't depend on `n` or decreases".

**Conclusion:**
- Number of iterations: **proportional to `1 / n`**
- Asymptotics: **O(1)** (constant complexity, because for large `n` the loop executes less and less often)

---

### Example 5: Logarithmic Loop

**Code:**
```cpp
var count = 0;
for (int i = 1; i < n; i *= 2)
    count++;
```

**What Does the Loop Do?**
- `i` starts at 1
- On each iteration, multiplied by 2: `i *= 2`
- Loop executes while `i < n`

**Number of Iterations:**
On each iteration, `i` takes values:
```
1, 2, 4, 8, 16, ..., while i < n
```

This is a **geometric progression** with base 2. The number of iterations equals the number of doublings until `i < n`.

That is:
```text
i = 2^k < n  ⇒  k < log₂(n)
```

Therefore:
> Number of iterations: **≈ log₂(n)**

**Asymptotic Complexity:**
- Loop body: `count++` — this is an `O(1)` operation
- Iterations: `≈ log₂(n)`

Therefore:
> **Final complexity: `O(log n)`** — **logarithmic**

**Conclusion:**
A loop where the variable increases in geometric progression (by multiplication) usually gives **logarithmic complexity**, and this is very efficient for large `n`.

---

### Example 6: Logarithmic Loop with n²

**Code:**
```cpp
var count = 0;
for (int i = 1; i < n * n; i *= 3)
    count++;
```

**What Does the Loop Do?**
- Start: `i = 1`
- Step: `i *= 3` (i.e., `i` grows in **geometric progression** with base `3`)
- Condition: `i < n²`

**Number of Iterations:**
On each iteration, `i` takes values:
```
1, 3, 9, 27, ..., while i < n²
```

The number of iterations corresponds to the number of times we need to multiply `1` by `3` to reach `n²`.

This means:
```text
i = 3^k < n²
⇒ k < log₃(n²) = 2 * log₃(n)
```

**Asymptotic Complexity:**
Since:
- Iterations ≈ `2 * log₃(n)`
- Constants (like `2` or logarithm base) in Big-O are not considered

Then:
> **Final complexity: `O(log n)`**

**Conclusion:**
- Loop with `i *= 3` until `n²` executes approximately `log₃(n²)` times
- Asymptotic complexity:
  ✅ **`O(log n)` — logarithmic**

---

### Example 7: Square Root Loop

**1. `for (int i = 0; i * i < n; i++) z++;`**

**Condition:** `i * i < n` → loop executes while `i < √n`

- Number of iterations: approximately `√n`
- Loop body: `z++` — `O(1)` operation

**Complexity: `O(√n)`**

---

**2. `for (int i = 0; i < n * n; i++) z++;`**

Regular linear loop up to `n²`

- Number of iterations: `n²`
- Loop body: `z++` — `O(1)` operation

**Complexity: `O(n²)`**

---

**3. `for (int i = 1; i % 7 != 0; i++) z++;`**

- Condition: `i % 7 != 0`
- Loop terminates when `i` is divisible by 7 → that is, after **6 steps**

**Complexity: `O(1)`** (constant number of iterations)

---

**4. `for (int i = 1; i < n * n; i *= 10) z++;`**

`i` increases 10 times each iteration: `1, 10, 100, 1000, ..., < n²`

- This is a geometric progression with base 10
- Number of iterations: `log₁₀(n²) = 2 * log₁₀(n)`
- Logarithm with any base in Big-O is `O(log n)`

**Complexity: `O(log n)`**

---

### Summary Table of Loop Complexities

| №   | Loop                               | Complexity |
| --- | ---------------------------------- | ---------- |
| 1   | `for (i = 0; i * i < n; i++)`      | `O(√n)`    |
| 2   | `for (i = 0; i < n * n; i++)`      | `O(n²)`    |
| 3   | `for (i = 1; i % 7 != 0; i++)`     | `O(1)`     |
| 4   | `for (i = 1; i < n * n; i *= 10)`  | `O(log n)` |
| 5   | `for (i = 1; i < n; i *= 2)`       | `O(log n)` |
| 6   | `for (i = 1; i < n * n; i *= 3)`   | `O(log n)` |
| 7   | `for (i = 0; i < 1000000; i += n)` | `O(1)`     |
| 8   | `for (i = 0; i < n; i += n)`       | `O(1)`     |

---

### Example 8: String Search Function

**Code:**
```csharp
int FindSpace(string s)
{
    int i = 0;
    while (i < s.Length && s[i] != ' ')
        i++;
    return i >= s.Length ? -1 : i;
}
```

This function **searches for the first space** `' '` in string `s`.

**How It Works:**
- Starts from the beginning of the string
- Checks each character until:
  - it encounters a space, or
  - it reaches the end of the string
- Returns:
  - index of the first space if found,
  - `-1` if no space exists

**Time Complexity Analysis:**
Let `n = s.Length`.

- In the worst case (if **there are no spaces in the string**), the loop executes `n` times
- In the best case (if **space is the first character**), the loop executes 0 times

Therefore:
- **Best case**: `O(1)`
- **Worst case**: `O(n)`
- **Asymptotic in general case**:
  ✅ **`O(n)` — linear time complexity**

**Space Complexity Analysis:**
- No allocation of additional data structures
- Only one variable `int i`

**Space Complexity: `O(1)`**

**Summary:**

| Показатель                 | Оценка |
| -------------------------- | ------ |
| Временная сложность        | `O(n)` |
| Пространственная сложность | `O(1)` |

---

## String Operations Complexity

### C# String Operations Analysis

Here are time complexity estimates for each of the specified string operations in C# (and similarly in most languages with immutable strings, including Java and Python):

#### 1. `s[i]` — Getting the i-th character in a string

- **Complexity:** `O(1)`
- **Reason:** String is stored as a character array, index access is a constant-time operation

#### 2. `s.Length` — Getting string length

- **Complexity:** `O(1)`
- **Reason:** String length is stored as an object field, access to which occurs in constant time

#### 3. `s.Substring(startIndex, count)` — Getting a substring

- **Complexity:** `O(count)`
- **Reason:** A new string is created, into which `count` characters are copied. Time depends on substring length

#### 4. `s.IndexOf("a")` — Finding first occurrence of a character

- **Complexity:** `O(n)`
- **Reason:** Potentially need to traverse the entire string `s`, length `n`, in the worst case

#### 5. `s.Replace("abc", "def")` — Replacing all occurrences of a string

- **Complexity:** `O(n * m)` in the worst case
  where `n` is the length of string `s`, `m` is the number of replacements
- **Reason:** Need to traverse the string, find all occurrences of `"abc"` (which itself is `O(n)`), and create a new string with replacements. Copying is another `O(n)`

#### 6. `s.Replace("a", v)` — Replacing all occurrences of one character

- **Complexity:** `O(n + k * |v|)`
  where `k` is the number of occurrences of `"a"`, `|v|` is the length of string `v`
- **If |v| is bounded above as `O(n)`**, then overall complexity is **`O(n)`**
- **Reason:** String is fully traversed, and each replacement creates a new part of the string

### String Operations Summary Table

| Операция                    | Сложность  |
| --------------------------- | ---------- |
| `s[i]`                      | `O(1)`     |
| `s.Length`                  | `O(1)`     |
| `s.Substring(start, count)` | `O(count)` |
| `s.IndexOf("a")`            | `O(n)`     |
| `s.Replace("abc", "def")`   | `O(n * m)` |
| `s.Replace("a", v)`         | `O(n)`     |

---

## List Operations Complexity

### Python List.remove() Complexity

**Question:** What is the time complexity of the `remove()` method for a list in the case when the element exists in the list?

**Answer:** **O(n)**

Where **n** is the length of the list.

**Why:**
1. **Finding the element** (`x`) requires traversing the list to the first match — this takes **O(n)** in the worst case
2. **Removing the element** may also require shifting all subsequent elements one position to the left — also **O(n)** in the worst case

Thus, even if the element is found early, due to the need to shift elements, the overall complexity remains **O(n)**.

**Example:**
```python
lst = [1, 2, 3, 4, 5]
lst.remove(3)  # Time complexity O(n)
```

---

### Python List.insert() Complexity

**Question:** What is the time complexity of adding an element at a position in the middle of a list using the `insert()` method?

**Answer:** **O(n)**

**Explanation:**
- List in Python is implemented as an **array**
- When inserting in the middle (or at the beginning), all elements **after** the specified index need to be **shifted one position to the right** to make room for the new element
- This shift takes **O(n)** time in the worst case, where **n is the length of the list**

**Example:**
```python
lst = [1, 2, 3, 4, 5]
lst.insert(2, 99)  # Insert in the middle (between 2 and 3)
```

In this example, Python first finds the needed position, then shifts `[3, 4, 5]` one position to the right, and inserts `99`. All of this takes linear time.

### List Operations Comparison by Position

| Операция                       | Сложность             |
| ------------------------------ | --------------------- |
| `append(x)` (в конец)          | O(1) амортизированная |
| `insert(0, x)` (в начало)      | O(n)                  |
| `insert(n//2, x)` (в середину) | O(n)                  |
| `remove(x)`                    | O(n)                  |
| `pop()` (с конца)              | O(1)                  |
| `pop(i)` (по индексу)          | O(n)                  |

**Note:**
If you need to frequently insert/delete elements in the middle of a list, it's better to use **`collections.deque`** or another data type.

---

## Resources

- [uLearn Complexity Course](https://ulearn.me/Course/complexity)

---

## Summary

**Time and Space Complexity:**
- **Time Complexity**: How execution time grows with input size
- **Space Complexity**: How memory usage grows with input size

**Asymptotic Analysis:**
- **O(f(n))**: Upper bound (worst case)
- **Ω(f(n))**: Lower bound (best case)
- **Θ(f(n))**: Tight bound (exact estimate)

**Common Complexities:**
- **O(1)**: Constant (best)
- **O(log n)**: Logarithmic (very good)
- **O(n)**: Linear (good)
- **O(n log n)**: Linearithmic (acceptable)
- **O(n²)**: Quadratic (slow for large data)
- **O(2ⁿ)**: Exponential (very slow)
- **O(n!)**: Factorial (extremely slow)

**Key Takeaways:**
- Use asymptotic estimation to simplify complexity analysis
- Ignore constants and lower-order terms
- Focus on the fastest-growing term
- Consider both time and space complexity
- Understand operation complexities for different data structures

Understanding complexity analysis is essential for writing efficient algorithms and making informed design decisions.
