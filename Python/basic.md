# Python Fundamentals: A Comprehensive Guide

## Table of Contents
1. [Resources and References](#resources-and-references)
2. [Core Concepts](#core-concepts)
3. [Development Environment](#development-environment)
4. [Global Interpreter Lock (GIL)](#global-interpreter-lock-gil)
5. [Object-Oriented Programming](#object-oriented-programming)
6. [Advanced Features](#advanced-features)
7. [System Integration](#system-integration)

---

## Resources and References

### Official Documentation
- [Python Glossary](https://docs.python.org/3/glossary.html) - Official Python glossary with definitions of key terms

### Decorators
- [Decorators with Parameters](https://stackoverflow.com/questions/5929107/decorators-with-parameters) - Stack Overflow discussion on parameterized decorators
- [Real Python: Primer on Python Decorators](https://realpython.com/primer-on-python-decorators/#functions) - Comprehensive guide to decorators

### UML Class Diagrams
- [UML Class Diagram Tutorial](https://www.visual-paradigm.com/guide/uml-unified-modeling-language/uml-class-diagram-tutorial/) - Guide to creating and understanding UML class diagrams

### GIL Resources
- [GIL в Python: зачем он нужен и как с этим жить](https://www.youtube.com/watch?v=AWX4JnAnjBE&t=141s) - Video explanation of Python's Global Interpreter Lock

---

## Core Concepts

### Bit Length Calculation

To determine the minimum number of bits needed to store a number, you need to take its logarithm base 2 and add 1 to the result.

**Mathematical approach:**
```python
import math

# Calculate log base 2 of a number
print(math.log(10, 2))  # (x, base)
```

**Python built-in method:**
```python
n = 10
print(n.bit_length())  # Returns the number of bits needed
```

**Explanation:** The `bit_length()` method returns the number of bits needed to represent the integer in binary, excluding the sign and leading zeros. This is more efficient than calculating the logarithm manually.

### Basic Arithmetic Operations

```python
# Integer division (floor division)
20 // 6  # Returns 3 (discards remainder)

# Modulo operation (remainder)
5 % 4    # Returns 1
```

**Key differences:**
- `//` performs floor division, returning the largest integer less than or equal to the result
- `%` returns the remainder after division
- Both operations are useful for various algorithms and mathematical computations

### Variable Management

```python
# View active variables in the current namespace
dir()  # Shows all active variables

# Delete a variable
del var_name  # Removes the variable from the namespace
```

**Use cases:**
- `dir()` is useful for debugging and introspection
- `del` helps free memory and clean up namespaces
- Both are particularly useful in interactive Python sessions

### Alternative Python Implementations

**Nim, PyPy, Cython** - These can be used as alternatives or extensions to standard Python:
- **PyPy**: A fast, compliant alternative Python interpreter with JIT compilation
- **Cython**: Allows writing C extensions for Python, improving performance
- **Nim**: A statically typed compiled language that can interoperate with Python

---

## Development Environment

### Virtual Environments

Virtual environments allow you to create isolated Python environments for different projects, preventing dependency conflicts.

**Creating a virtual environment:**

```bash
# Using virtualenv (if installed separately)
virtualenv -p python3 venv

# Using Python's built-in venv module (recommended)
python3 -m venv venv
```

**Activating the virtual environment:**

```bash
# On Unix/macOS
. venv/bin/activate

# On Windows
venv\Scripts\activate
```

**Installing dependencies:**

```bash
# Install packages from requirements.txt one by one
cat requirements.txt | xargs -n 1 pip3 install

# Or use pip directly
pip3 install -r requirements.txt
```

**Uninstalling all packages:**

```bash
# Uninstall all packages in the current environment
pip freeze | xargs pip uninstall -y
```

**Best practices:**
- Always use virtual environments for project isolation
- Keep a `requirements.txt` file with all dependencies
- Use `pip freeze > requirements.txt` to generate the requirements file

### Jupyter Notebook Extensions

**Variable Inspector Extension:**

```python
# Install Jupyter contrib extensions
!pip install jupyter_contrib_nbextensions

# Install the extensions
!jupyter contrib nbextension install --sys-prefix

# Enable the variable inspector
!jupyter nbextension enable varInspector/main
```

**Converting Notebooks to Scripts:**

```bash
# Convert a Jupyter notebook to a Python script
!jupyter nbconvert --to script 'my-notebook.ipynb'
```

**Explanation:** The variable inspector extension allows you to view all variables and their values in your Jupyter notebook, making debugging easier. Converting notebooks to scripts is useful for version control and production deployment.

---

## Global Interpreter Lock (GIL)

### Understanding Processes, Threads, and Concurrency

#### Historical Context

**Early Computing:**
- Processors and memory: All information, including cursor position on screen, is stored in memory
- Early computers could only execute one program at a time

**Evolution of Multitasking:**

1. **Process Abstraction**
   - Introduced to solve the single-program limitation
   - Each process has its own memory space

2. **Cooperative Multitasking**
   - All programs loaded into memory simultaneously
   - Each program runs for some time, then transfers control to another program
   - **Problem:** Programs didn't transfer control quickly enough and corrupted memory

3. **Preemptive Multitasking**
   - Operating system controls process switching
   - Still had various problems with resource management

4. **Threads**
   - Multiple threads can work on a single core
   - Operating system quickly switches between threads
   - **Problem:** Threads share memory, but the main issue isn't shared memory access
   - The real problem: threads sleep and wake up at unexpected times, causing access violations

### What is the GIL?

**Definition:**
> A global interpreter lock (GIL) is a mechanism used in computer-language interpreters to synchronize the execution of threads so that only one native thread can execute at a time. An interpreter that uses GIL always allows exactly one thread to execute at a time, even if run on a multi-core processor.

**In Simple Terms:**
The Global Interpreter Lock (GIL) ensures that only one Python thread can execute at a time in a Python program.

### Why Does GIL Exist?

**Purpose:**
- Needed for thread-safe memory management
- However, threads don't actually work simultaneously—they execute sequentially
- So what does GIL actually do?

**The Real Function:**
- GIL doesn't protect threads from simultaneous access
- Instead, it protects against a second thread unexpectedly waking up and changing structures in the interpreter's memory
- It prevents threads from waking up at unexpected times

### How GIL Works

**The Mechanism:**

1. **Python cannot prevent threads from sleeping** - this is controlled by the operating system
2. **Python's solution:** Make threads wait for GIL instead of waking unexpectedly
3. All threads sleep and wait for GIL, ensuring controlled execution

**Version Differences:**

- **Before Python 3.2:** Thread switching occurred based on "ticks"
- **Python 3.2+:** Thread switching occurs based on milliseconds (every 5ms)

**What is a Tick?**
- A tick is an interpreter instruction (e.g., `a=1` or `for i in range(0,100000)`)
- Each tick represented one interpreter operation

**Summary:**
- Under normal conditions, threads sleep and wake up frequently and unexpectedly at the OS's command
- GIL prevents all Python threads except the current one from waking unexpectedly

### GIL Release Mechanism

**When GIL is Released:**

Python uses GIL only to protect Python code. When Python calls:
- Operating system functions (e.g., network data reception)
- External library functions (e.g., NumPy, SciPy)

These are not Python code and cannot modify interpreter internals. Python releases GIL during these operations and reacquires it when the function returns. This is called "raising/lowering GIL."

**Example:**
```python
# When Python calls a system function or external library:
# 1. GIL is released
# 2. Function executes (potentially blocking)
# 3. GIL is reacquired when function returns
```

### GIL on Different Processors

#### Single-Core Processors

- **GIL doesn't interfere** with threads and provides benefits
- Code unrelated to the interpreter sleeps and wakes as needed without waiting
- Python code sleeps and wakes carefully to prevent damage

#### Multi-Core Processors

**Challenges:**
- If you have multiple cores and want to maximize Python code execution speed, you have a rare case
- **Solutions:**
  1. **Use optimized libraries:** NumPy, SciPy release GIL during long operations (matrix work)
  2. **Use libraries for calculations:** Let optimized C libraries handle computation
  3. **Use processes:** GIL doesn't affect processes, only threads

### Processes vs Threads

**Key Differences:**

- **Process:** Can contain one or more threads; all threads within a process share resources
- **Thread:** A sequence of program execution, a sequence of operations
- **Coroutines:** Execute within threads

**Important:** GIL only affects threads within a single process. Multiple processes can run in parallel without GIL interference.

### GIL and System Scheduler

**How They Work Together:**

1. Two threads are running
2. First thread acquires GIL because it's doing a blocking task
3. System scheduler puts the first thread to sleep
4. Second thread wakes up, sees GIL is held by first thread, and goes back to sleep

**GIL Switching Rules:**

- A thread cannot hold GIL indefinitely if it's doing nothing
- **Before Python 3.3:** GIL switches automatically every 100 machine code instructions
- **Python 3.3+:** GIL switches every 5 milliseconds

### Physical vs Logical Errors

**Important Distinction:**

- **GIL protects the interpreter from crashes** (physical errors)
- **GIL does NOT protect against logical errors** in your code

**Python Tools for Logical Protection:**

Python provides tools for logical code protection:
- `threading.Lock` - Mutual exclusion lock
- `threading.Semaphore` - Counter-based synchronization
- `threading.Event` - Event-based synchronization
- `threading.Queue` - Thread-safe queue for data exchange

**Conclusion:** GIL is harmless for most use cases. It protects the interpreter while you use threading primitives to protect your application logic.

### GIL vs Other Languages

**Comparison with Node.js (PHP, Perl):**

- Node.js doesn't use threads for network operations
- These languages use processes instead of threads for parallelism
- This is a different concurrency model

### Profiling Tools

- Intel provides utilities for profiling threads
- These tools help identify performance bottlenecks in multi-threaded applications

---

## Object-Oriented Programming

### UML Class Diagrams

UML (Unified Modeling Language) class diagrams are used to visualize the structure of classes and their relationships.

**Visibility Modifiers:**

Visibility modifiers are shown before class attributes and methods:
- `+` denotes **public** attributes or operations
- `-` denotes **private** attributes or operations
- `#` denotes **protected** attributes or operations

**Parameter Directionality:**

Each parameter in a method can have a direction description: `in`, `out`, `inout`.

**Examples:**

- **Method1** uses `p1` as an **input parameter**: the value of `p1` is used by the method, but the method does not modify `p1`
- **Method2** takes `p2` as an **input/output parameter**: the value of `p2` is used by the method and receives the method's output value, but the method may also modify `p2`
- **Method3** uses `p3` as an **output parameter**: the parameter serves as storage for the method's output value

**Class Definition:**

A class represents a concept that encapsulates:
- **State (attributes)**: Each attribute has a type
- **Behavior (operations)**: Each operation has a signature
- The class name is the only mandatory information

**Visual Reference:**
![[python-1.png]]

### Type Hints

**Best Practice for Optional Types:**

```python
# Preferred way
name: Union[str, None]

# Instead of (deprecated)
name: Optional[str]
```

**Explanation:** `Union[str, None]` is the preferred modern syntax for optional types. `Optional[str]` is still valid but considered less explicit.

**Ellipsis in Type Hints:**

The `...` (ellipsis) means "this is required", in contrast to:
- `None`: not required
- A literal value like `"Foo"`: means `"Foo"` is the default value

**Example:**
```python
def function(param: str = ...) -> None:
    # param is required (must be provided)
    pass

def function(param: str = None) -> None:
    # param is optional (can be omitted)
    pass

def function(param: str = "default") -> None:
    # param has a default value of "default"
    pass
```

### Static Methods

**Definition:**

A `staticmethod` is a method that knows nothing about the class or instance it was called on. It just receives the arguments that were passed, with no implicit first argument (unlike instance methods which receive `self` or class methods which receive `cls`).

**Why Use Static Methods?**

`staticmethod` isn't useless—it's a way of:
- Putting a function into a class (because it logically belongs there)
- Indicating that it does not require access to the class or instance

**Example:**
```python
class MathUtils:
    @staticmethod
    def add(a: int, b: int) -> int:
        return a + b

# Can be called without creating an instance
result = MathUtils.add(5, 3)  # Returns 8
```

**When to Use:**
- When a function is logically related to a class but doesn't need access to class or instance data
- For utility functions that are conceptually part of a class
- To organize related functions together

---

## Advanced Features

### Yield and Generators

**What is `yield`?**

`yield` is a keyword used like `return`, except the function returns a **generator** instead of a value.

**When to Use:**

It's handy when you know your function will return a huge set of values that you will only need to read once. Generators are memory-efficient because they produce values on-demand rather than storing them all in memory.

**Key Concept:**

When you call a function with `yield`, **the code in the function body does not run immediately**. The function only returns a generator object—this is a bit tricky!

**How It Works:**

1. **First call:** When a `for` loop calls the generator object created from your function:
   - It runs the code from the beginning until it hits `yield`
   - Returns the first value of the loop

2. **Subsequent calls:** Each subsequent call:
   - Runs another iteration of the loop you wrote in the function
   - Returns the next value
   - Continues until the generator is empty

3. **Generator exhaustion:** The generator becomes empty when:
   - The function runs without hitting `yield`
   - The loop has come to an end
   - An `if/else` condition is no longer satisfied

**Example:**
```python
def number_generator(n):
    for i in range(n):
        yield i * 2

# Usage
for num in number_generator(5):
    print(num)  # Prints 0, 2, 4, 6, 8

# Generator object
gen = number_generator(5)
print(next(gen))  # 0
print(next(gen))  # 2
```

**Benefits:**
- Memory efficient: values are generated on-demand
- Lazy evaluation: computation happens only when needed
- Can represent infinite sequences
- Useful for processing large datasets

---

## System Integration

### Shebang Lines

**Python Script Shebang:**

```python
#!/usr/bin/python
```

**Explanation:** The shebang line (`#!`) tells the system which interpreter to use to execute the script. This allows scripts to be run directly without explicitly calling the interpreter.

**Usage:**
```bash
# Make script executable
chmod +x script.py

# Run directly
./script.py
```

### Mathematical Expressions in Markdown

**LaTeX Math Notation:**

```markdown
$$c = \sqrt{a^2+b^2}$$
```

**Explanation:** This uses LaTeX syntax for mathematical expressions. The `$$` delimiters create a block-level math equation. Many Markdown processors (like those in Jupyter notebooks) support LaTeX rendering.

### Shell Script Wrapper

**Simple Shell Script for Command Execution:**

```bash
#!/bin/sh

set -e

exec "$@"
```

**Line-by-Line Explanation:**

1. **`#!/bin/sh`**
   - Specifies that the script should be run with `sh` (standard Unix shell)
   - This is the shebang line for shell scripts

2. **`set -e`**
   - Tells the interpreter to exit immediately if any command returns a non-zero exit code (indicating an error)
   - This is a best practice that helps prevent unintended consequences of failed commands
   - Ensures the script fails fast on errors

3. **`exec "$@"`**
   - `exec` replaces the current process with the specified command
   - `$@` is a special variable that expands to all arguments passed to the script
   - This effectively runs the command directly without the overhead of an additional shell process
   - The script process is replaced by the command process

**Use Cases:**
- Creating lightweight wrappers around commands
- Entrypoint scripts in Docker containers
- Simplifying command execution in scripts
- Reducing process overhead

**Example Usage:**
```bash
# Save as 'run-python.sh'
# Make executable: chmod +x run-python.sh
# Usage: ./run-python.sh python3 script.py
```

---

## Summary

This guide covers fundamental Python concepts including:

- **Core operations**: Bit calculations, arithmetic, variable management
- **Development tools**: Virtual environments, Jupyter extensions
- **Concurrency**: Deep dive into GIL and threading
- **OOP concepts**: UML diagrams, type hints, static methods
- **Advanced features**: Generators and yield
- **System integration**: Shell scripts and shebang lines

Each section provides practical examples and explanations to help you understand and apply these concepts effectively.
