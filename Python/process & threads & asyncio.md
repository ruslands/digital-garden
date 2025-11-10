# Processes, Threads, and Asyncio: Concurrency in Python

## Table of Contents
1. [Introduction](#introduction)
2. [Fundamental Concepts](#fundamental-concepts)
3. [Concurrency vs Parallelism](#concurrency-vs-parallelism)
4. [Multiprocessing](#multiprocessing)
5. [Multithreading](#multithreading)
6. [Asyncio and Coroutines](#asyncio-and-coroutines)
7. [Choosing the Right Approach](#choosing-the-right-approach)
8. [FastAPI Deployment Concepts](#fastapi-deployment-concepts)
9. [Event Loop and Selectors](#event-loop-and-selectors)

---

## Introduction

Understanding concurrency models is crucial for building efficient Python applications. This guide covers processes, threads, and asyncio, helping you choose the right approach for your use case.

**Key Concepts:**
- **Process**: Independent program execution with its own memory space
- **Thread**: Lightweight execution unit within a process, shares memory
- **Coroutine**: Cooperative multitasking within a single thread
- **GIL**: Global Interpreter Lock that affects threading in Python

---

## Fundamental Concepts

### Processes and Threads

**Process:**
- One process might have multiple threads
- Processes don't share memory (isolated memory spaces)
- Each process has its own Python interpreter
- GIL operates with threads, not processes

**Thread:**
- Thread is a stream of program execution, a sequence of operations
- Threads share memory within a process
- Multiple threads can exist in one process
- GIL affects thread execution (only one Python thread runs at a time)

**Coroutines:**
- Coroutines live in threads
- Coroutines work in a single thread
- You can run many coroutines on a single thread
- Cooperative multitasking (programmer controls switching)

### Visual Reference

![[python-2.png]]

**Gunicorn - Process Manager:**

Gunicorn is a process manager. We use Gunicorn because uvicorn doesn't have a built-in process manager.

**Resources:**
- [Python GINO FastAPI Tutorial](https://python-gino.org/docs/en/master/tutorials/fastapi.html)
- [FastAPI Deployment: Server Workers](https://fastapi.tiangolo.com/deployment/server-workers/)
- [FastAPI Deployment Concepts](https://fastapi.tiangolo.com/deployment/concepts/)

---

## Concurrency vs Parallelism

### Definitions

**Concurrency:**
- Two or more tasks can start, run, and complete in overlapping time periods
- Doesn't necessarily mean they'll ever both be running at the same instant
- Example: Multitasking on a single-core machine
- Like two lines of customers ordering from a single cashier (lines take turns)

**Parallelism:**
- Tasks literally run at the same time
- Example: On a multicore processor
- Like two lines of customers ordering from two cashiers (each line gets its own cashier)

### Real-World Analogy

**Asyncio Example:**
- Want to watch a movie with tea and sandwiches
- Asynchronously: Start movie download, start tea brewing, make sandwiches
- All tasks progress without waiting for each other

**Threads Example:**
- Constantly switching between tasks
- Start tea, check movie download, make sandwiches, check tea again
- Operating system controls switching

**Multiprocessing Example:**
- Three people doing three tasks in parallel
- True parallel execution

### Visual Comparison

![[python-3.png]]
![[python-4.png]]
![[python-5.png]]
![[python-6.png]]
![[python-7.png]]

---

## Multiprocessing

### Characteristics

**Pros:**
- True parallel execution
- Bypasses GIL limitations
- Isolated memory spaces (no shared state issues)
- Can utilize multiple CPU cores

**Cons:**
- Higher memory overhead (each process has its own memory)
- Slower inter-process communication
- More complex to implement
- Process creation overhead

### When to Use

**CPU-Bound Tasks:**
- Mathematical computations
- Image processing
- Data analysis
- Machine learning model training

**Rule of Thumb:**
> CPU Bound => Multi Processing

---

## Multithreading

### Characteristics

**Pros:**
- Lower memory overhead than processes
- Faster inter-thread communication (shared memory)
- Good for I/O-bound tasks
- Easier to share data between threads

**Cons:**
- GIL limits true parallelism for CPU-bound tasks
- Thread safety concerns (need locks)
- Context switching overhead
- Complex debugging

### Thread Safety

**Definition:**
Data structures should not be accessed simultaneously by different threads.

**Lack of Thread Safety:**
- Methods/functions don't have protection against multiple threads
- No locks around data to ensure consistency
- Async code isn't thread-safe because it doesn't need to be (runs in single thread)

### When to Use

**I/O-Bound Tasks:**
- Network operations
- File I/O
- Database queries
- Fast I/O with limited connections

**Rule of Thumb:**
> I/O Bound, Fast I/O, Limited Number of Connections => Multi Threading

**Use Case:** Network operations, working with network resources

---

## Asyncio and Coroutines

### Characteristics

**Pros:**
- Works in one process and one thread
- Low system resource usage
- OS independent
- No need for thread-safe code (no locks needed)
- Very high concurrency with little overhead

**Cons:**
- Only works for I/O-bound tasks
- Requires async/await syntax throughout
- Learning curve for async programming
- Not suitable for CPU-bound tasks

### Real-World Performance

**High Concurrency:**
- Coroutines provide very high level of concurrency with very little overhead
- In threaded environments, you typically have at most 30-50 threads before overhead becomes significant
- With asyncio, you can handle thousands of concurrent operations

### When to Use

**I/O-Bound Tasks:**
- Slow I/O operations
- Many concurrent connections
- Web scraping
- API calls
- Database operations (with async drivers)

**Rule of Thumb:**
> I/O Bound, Slow I/O, Many connections => Asyncio

### How Asyncio Works

**Definition:**
Asyncio is essentially threading where not the CPU but you, as a programmer (or your application), decide where and when the context switch happens.

**Key Difference from Threads:**
- **Threads**: Operating system switches running threads preemptively according to its scheduler
- **Coroutines**: Programmer and programming language determine when to switch coroutines
- Tasks are cooperatively multitasked by pausing and resuming functions at set points
- Coroutine switches are cooperative, meaning the programmer controls when a switch happens

### Async/Await Basics

**Important Rules:**
- `await` can only be used inside functions defined with `async def`
- Functions defined with `async def` have to be "awaited"
- Functions with `async def` can only be called inside other `async def` functions

**Coroutine Definition:**
A coroutine is just the fancy term for the thing returned by an `async def` function. Python knows that it is something like a function that:
- Can start and will end at some point
- Might be paused internally whenever there is an `await` inside of it

**Historical Note:**
`yield from` is an old way to wait for asyncio's coroutine. `await` is the modern way.

### Coroutine Concepts

![[python-10.png]]

**Key Definitions:**
- **Awaitable**: Any object which can be awaited on
- **Async function**: A definition that will create a coroutine once called
- **Coroutine**: An object created by calling an async function
- **Coroutine is awaitable**, but **async function is not awaitable**

**Important Rules:**
- Can't run async function in `asyncio.run()`
- Coroutines can be awaited only once
- Task is a wrapper for coroutine

**Tasks:**
You should use a task when you want your coroutine to effectively run in the background.

**Coroutine Benefits:**
- A coroutine is a generator function that can both yield values and accept values from outside
- Benefit: We can pause the execution of a function and resume it later
- For network operations, it makes sense to pause execution while waiting for response

**Futures:**
A future is like Promise objects from JavaScript. It is like a placeholder for a value that will be materialized in the future. While waiting on network I/O, a function can give us a container, a promise that it will fill the container with the value when the operation completes.

**ensure_future:**
You don't need `ensure_future` if you don't need the results. They are good if you need the results or retrieve exceptions that occurred.

**Key Points:**
1. Invoking a coroutine function (`async def`) does NOT run it. It returns a coroutine object, like generator function returns generator objects.
2. `await` retrieves values from coroutines, i.e., "calls" the coroutine
3. `ensure_future`/`create_task` schedule the coroutine to run on the event loop on next iteration (although not waiting them to finish, like a daemon thread)

### Resources

- [FastAPI Async Guide](https://fastapi.tiangolo.com/async/)
- [Python asyncio Task Documentation](https://docs.python.org/3/library/asyncio-task.html)

---

## Choosing the Right Approach

### Decision Matrix

| Task Type | I/O Speed | Connections | Recommended Approach |
| --------- | --------- | ----------- | -------------------- |
| CPU-bound | N/A       | N/A         | Multiprocessing      |
| I/O-bound | Fast      | Limited     | Multithreading       |
| I/O-bound | Slow      | Many        | Asyncio              |

### Summary

- **CPU bound** → **Parallelism** (Multiprocessing)
- **I/O bound** → **Concurrency** (Multithreading or Asyncio)

**With FastAPI:**
- You can take advantage of concurrency (common for web development, similar to Node.js)
- You can also exploit parallelism and multiprocessing for CPU-bound workloads like Machine Learning systems

### Additional Resources

- [Multiprocessing vs Multithreading vs Asyncio](https://stackoverflow.com/questions/27435284/multiprocessing-vs-multithreading-vs-asyncio-in-python-3)
- [Difference between Coroutine and Thread](https://stackoverflow.com/questions/1934715/difference-between-a-coroutine-and-a-thread)

---

## FastAPI Deployment Concepts

### Program and Process

**What is a Program?**
The word "program" is commonly used to describe:
- The code that you write, the Python files
- The file that can be executed by the operating system (e.g., `python`, `python.exe`, or `uvicorn`)
- A particular program while it is running on the operating system (also called a process)

**What is a Process?**
The word "process" is normally used more specifically:
- A particular program while it is running on the operating system
- Doesn't refer to the file, nor to the code
- Refers specifically to the thing being executed and managed by the operating system
- Any program, any code, can only do things when it is being executed (when there's a process running)
- The process can be terminated (or "killed") by you or by the operating system
- Each application running on your computer has some process behind it
- There can be multiple processes of the same program running at the same time

### Replication - Processes and Memory

**Single Process:**
With a FastAPI application, using a server program like Uvicorn, running it once in one process can serve multiple clients concurrently.

**Multiple Processes - Workers:**
- If you have more clients than what a single process can handle
- If you have multiple cores in the server's CPU
- You could have multiple processes running with the same application at the same time
- Distribute all requests among them
- When you run multiple processes of the same API program, they are commonly called **workers**

### Worker Processes and Ports

**Important Constraint:**
Only one process can be listening on one combination of port and IP address in a server.

**Solution:**
To have multiple processes at the same time, there has to be a single process listening on a port that then transmits the communication to each worker process in some way.

### Memory per Process

**Memory Consumption:**
- When the program loads things in memory (e.g., ML model, large file), it consumes RAM
- Multiple processes normally don't share any memory
- Each running process has its own things, variables, and memory
- If you consume a large amount of memory, each process will consume an equivalent amount

**Example:**
If your code loads a Machine Learning model with 1 GB in size:
- One process: consumes at least 1 GB of RAM
- Four processes (4 workers): each consumes 1 GB, total 4 GB of RAM
- If your server only has 3 GB of RAM, this will cause problems

### Multiple Processes - An Example

**Architecture:**
- **Manager Process**: Starts and controls worker processes, listens on port/IP, transmits communication to workers
- **Worker Processes**: Run your application, perform main computations, load variables in RAM
- **Other Processes**: The same machine would probably have other processes running as well

**Resource Usage:**
- CPU usage by each process can vary a lot over time
- Memory (RAM) normally stays more or less stable
- If API does comparable computations every time with many clients, CPU utilization will probably also be stable

### Gunicorn with Uvicorn

**Gunicorn:**
- Mainly an application server using the WSGI standard
- Can serve applications like Flask and Django
- Gunicorn by itself is not compatible with FastAPI (FastAPI uses ASGI standard)

**Solution:**
- Gunicorn supports working as a process manager
- Users can tell it which specific worker process class to use
- Gunicorn would start one or more worker processes using that class
- Gunicorn acts as process manager, listening on port/IP
- Transmits communication to worker processes running the Uvicorn class
- Gunicorn-compatible Uvicorn worker class converts data to ASGI standard for FastAPI

### Replication - Number of Processes

**Cluster-Level Replication:**
If you have a cluster of machines with Kubernetes, Docker Swarm Mode, Nomad, or similar:
- Handle replication at the cluster level instead of using a process manager (like Gunicorn with workers) in each container
- Distributed container management systems have integrated ways of handling replication
- In those cases, build a Docker image and run a single Uvicorn process instead of Gunicorn with Uvicorn workers

---

## Event Loop and Selectors

### Event Loop

**Function:**
- Executes callbacks one by one
- Manages the execution of coroutines
- Handles I/O operations

### Selectors

**Platform-Specific Selectors:**

**Windows:**
- Selector support: 512 sockets
- Proactor scales better on Windows
- I/O Completion Ports available

**Unix:**
- Only selector events available
- Multiple selector implementations:
  - **Kqueue**: For BSD, macOS
  - **epoll**: Linux
  - **/dev/poll**: Solaris
  - **poll**: POSIX-compliant systems
  - **select**: Most basic, available everywhere

### Visual Reference

![[python-8.png]]
![[python-9.png]]

---

## Summary

This guide covered essential concurrency concepts in Python:

### Key Concepts

1. **Processes**: Independent execution with isolated memory
2. **Threads**: Lightweight execution units sharing memory
3. **Coroutines**: Cooperative multitasking in a single thread

### When to Use What

- **CPU-Bound Tasks**: Use multiprocessing for true parallelism
- **I/O-Bound, Fast I/O, Limited Connections**: Use multithreading
- **I/O-Bound, Slow I/O, Many Connections**: Use asyncio

### Concurrency vs Parallelism

- **Concurrency**: Tasks overlap in time (may not run simultaneously)
- **Parallelism**: Tasks run simultaneously (requires multiple cores)

### FastAPI Deployment

- Single process can handle multiple clients concurrently
- Multiple worker processes for scaling
- Gunicorn as process manager with Uvicorn workers
- Consider memory usage per process

### Key Takeaways

- GIL affects threads but not processes
- Asyncio provides high concurrency with low overhead
- Choose the right tool based on your task characteristics
- Understand memory implications of multiple processes
- Event loop and selectors are platform-specific

Understanding these concepts enables you to build efficient, scalable Python applications that make the best use of available resources.
