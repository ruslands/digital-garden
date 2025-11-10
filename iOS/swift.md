# Swift Programming Language: Fundamentals

## Table of Contents
1. [Introduction](#introduction)
2. [Getting Started](#getting-started)
3. [Variables and Constants](#variables-and-constants)
4. [Type System](#type-system)
5. [Optionals](#optionals)
6. [Access Modifiers](#access-modifiers)
7. [Swift Actors](#swift-actors)
8. [Structs](#structs)

---

## Introduction

Swift is a powerful and intuitive programming language developed by Apple for iOS, macOS, watchOS, and tvOS development. It combines the performance and efficiency of compiled languages with the simplicity and expressiveness of modern scripting languages.

**Key Features:**
- Type-safe and memory-safe
- Modern syntax with clear, expressive code
- Supports both object-oriented and functional programming paradigms
- Built-in support for concurrency with async/await and actors
- Strong interoperability with Objective-C

---

## Getting Started

### Checking Swift Version

To check the installed Swift version, use the following command:

```bash
swift --version
```

**Note**: Swift can execute in the global environment without needing a `main()` function, making it suitable for scripting and interactive development.

---

## Variables and Constants

Swift provides two ways to declare values: `let` for constants and `var` for variables.

### Constants (`let`)

**Description**: Constants are immutable values that cannot be changed after they are declared.

**Syntax**:
```swift
let apples = 23
```

**Characteristics**:
- Immutable/constant
- Must be initialized when declared
- Cannot be reassigned after initialization
- Use when the value will not change

### Variables (`var`)

**Description**: Variables are mutable values that can be changed after they are declared.

**Syntax**:
```swift
var apples = 23
```

**Characteristics**:
- Mutable
- Can be reassigned after initialization
- Use when the value may change

### Type Annotations

Swift supports both type inference and explicit type annotations.

**Explicit Type Definition**:
```swift
let hello: String = "Hi"
```

**Type Inference** (Swift automatically infers the type):
```swift
let hello = "Hi"  // Swift infers this as String
```

**When to Use Explicit Types**:
- When you want to be explicit about the type
- When working with complex types
- When the inferred type might not be what you expect

---

## Type System

Swift has a strong, static type system that helps catch errors at compile time.

**Key Characteristics**:
- **Type Safety**: Swift ensures type correctness at compile time
- **Type Inference**: Swift can infer types from context
- **Type Checking**: Compiler checks types before code runs

---

## Optionals

### What are Optionals?

Optionals in Swift represent values that might be absent. They are a fundamental part of Swift's type system that helps prevent null pointer exceptions.

**Syntax**:
```swift
let hello: String? = "Hi"
```

**The `?` Symbol**: The `?` means the variable is optional and allows it to contain a `nil` value.

### Optional Types

An optional type can be either:
- A value of the specified type
- `nil` (no value)

**Example**:
```swift
var name: String? = "John"  // Optional String with value
name = nil                  // Optional String with no value
```

### Working with Optionals

**Optional Binding**:
```swift
if let name = optionalName {
    print("Name is \(name)")
} else {
    print("Name is nil")
}
```

**Optional Chaining**:
```swift
let length = optionalString?.count
```

**Nil Coalescing Operator**:
```swift
let name = optionalName ?? "Unknown"
```

**Why Optionals Matter**:
- Prevents null pointer exceptions
- Makes nil handling explicit
- Encourages safe code practices
- Compiler enforces optional handling

---

## Access Modifiers

Swift provides several access control levels to restrict access to code.

### Common Access Modifiers

**`private`**: 
- Most restrictive
- Accessible only within the same source file
- Use for implementation details

**`lazy`**:
- Not an access modifier, but a property modifier
- Delays initialization until first access
- Useful for expensive computations or properties that might not be used

**Combined Usage**:
```swift
private lazy var expensiveProperty: SomeType = {
    // Expensive initialization
    return SomeType()
}()
```

**Other Access Modifiers**:
- `internal`: Default access level (accessible within the same module)
- `fileprivate`: Accessible within the same source file
- `public`: Accessible from other modules
- `open`: Like public, but also allows subclassing and overriding

---

## Swift Actors

### What are Actors?

Swift Actors are a concurrency feature introduced in Swift 5.5 that provide a safe way to work with mutable state in concurrent code.

**Key Features**:
- **Isolation**: Actors isolate their mutable state
- **Thread Safety**: Only one task can access an actor's mutable state at a time
- **Serial Execution**: Methods and properties are executed serially
- **Prevents Data Races**: Compiler enforces actor isolation

**When to Use Actors**:
- When you need to share mutable state between concurrent tasks
- When you want to prevent data races
- When you need thread-safe access to shared resources

**Example**:
```swift
actor Counter {
    private var value = 0
    
    func increment() {
        value += 1
    }
    
    func getValue() -> Int {
        return value
    }
}
```

**Usage**:
```swift
let counter = Counter()
await counter.increment()
let value = await counter.getValue()
```

---

## Structs

### What are Structs?

A **Struct** (short for "structure") is a lightweight data structure that groups together related data and functionality. It's similar to a class but has some key differences.

### Key Characteristics of Structs

#### 1. Value Type

**Description**: Structs are value types, meaning they are copied when passed around in code.

**Implications**:
- When you assign a struct to a new variable, a copy is made
- Changes to one instance don't affect another
- More predictable behavior in concurrent code

**Example**:
```swift
struct Point {
    var x: Int
    var y: Int
}

var point1 = Point(x: 0, y: 0)
var point2 = point1  // point2 is a copy of point1
point2.x = 10        // point1.x is still 0
```

**Contrast with Classes**: Classes are reference types and are passed by reference. Changes to one instance affect all references to that instance.

#### 2. No Inheritance

**Description**: Structs don't support inheritance. You cannot subclass a struct, unlike classes which can be subclassed.

**Implications**:
- Simpler type hierarchy
- No need to worry about inheritance chains
- Use protocols for polymorphism instead

**Example**:
```swift
// This is NOT allowed:
// struct ChildStruct: ParentStruct { }  // Error!

// Use protocols instead:
protocol Drawable {
    func draw()
}

struct Circle: Drawable {
    func draw() {
        // Implementation
    }
}
```

#### 3. Immutable by Default

**Description**: Properties of a struct are immutable (cannot be changed) by default if the struct instance is declared as a constant (using `let`).

**Behavior**:
- If declared with `let`, all properties are immutable
- If declared with `var`, properties marked with `var` can be changed
- Properties marked with `let` are always immutable

**Example**:
```swift
struct Person {
    let name: String
    var age: Int
}

let person = Person(name: "John", age: 30)
// person.name = "Jane"  // Error: cannot assign to 'name'
// person.age = 31        // Error: 'person' is a let constant

var mutablePerson = Person(name: "John", age: 30)
// mutablePerson.name = "Jane"  // Error: 'name' is a let property
mutablePerson.age = 31          // OK: 'age' is var and instance is var
```

**Making Properties Mutable**: You can make properties mutable (changeable) by:
1. Declaring the instance as a variable (using `var`)
2. Marking properties with `var` instead of `let`

### When to Use Structs

**Use Structs When**:
- You need a simple data container
- You want value semantics (copying behavior)
- You don't need inheritance
- You're working with simple data types
- You want thread-safe behavior (value types are inherently safer in concurrent code)

**Use Classes When**:
- You need reference semantics
- You need inheritance
- You need identity comparison (===)
- You're working with complex object hierarchies

### Struct vs Class Summary

| Feature           | Struct                          | Class                             |
| ----------------- | ------------------------------- | --------------------------------- |
| **Type**          | Value type                      | Reference type                    |
| **Inheritance**   | No                              | Yes                               |
| **Copy Behavior** | Copied on assignment            | Referenced on assignment          |
| **Mutability**    | Immutable by default (if `let`) | Mutable (if properties are `var`) |
| **Identity**      | No identity comparison          | Identity comparison (===)         |
| **Memory**        | Stack allocation (usually)      | Heap allocation                   |
| **Thread Safety** | Safer (value semantics)         | Requires synchronization          |

---

## Best Practices

1. **Prefer `let` over `var`**: Use constants whenever possible to make your code more predictable
2. **Use Explicit Types When Needed**: While type inference is powerful, sometimes explicit types improve readability
3. **Handle Optionals Safely**: Always use optional binding, optional chaining, or nil coalescing
4. **Choose Structs for Simple Data**: Use structs for data containers and value types
5. **Use Actors for Shared Mutable State**: When you need thread-safe access to mutable state in concurrent code
6. **Leverage Type Safety**: Let Swift's type system catch errors at compile time

---

## Summary

Swift provides a powerful and safe programming environment with:
- Strong type system with optionals
- Value types (structs) and reference types (classes)
- Modern concurrency features (actors)
- Clear syntax for variables and constants
- Comprehensive access control

Understanding these fundamentals is essential for effective iOS development with Swift.
