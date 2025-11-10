# Design Patterns: Complete Guide

## Table of Contents
1. [Introduction](#introduction)
2. [Behavioral Patterns](#behavioral-patterns)
3. [Creational Patterns](#creational-patterns)
4. [Structural Patterns](#structural-patterns)
5. [Pattern Comparisons](#pattern-comparisons)
6. [Resources](#resources)

---

## Introduction

**Source:** [refactoring.guru](https://refactoring.guru)

**What is a Design Pattern?**
A design pattern is a frequently occurring solution to a specific problem when designing software architecture.

**Best Practice:**
The best way to use patterns is to remember them, and then learn to recognize those places in your architectures and existing applications where they are appropriate to apply. Thus, instead of program code, you reuse someone else's experience.

**Pattern Categories:**
- **Behavioral Patterns** (10 patterns): Focus on effective interaction between objects
- **Creational Patterns** (5 patterns): Focus on object creation
- **Structural Patterns** (7 patterns): Focus on building object and class structures

---

## Behavioral Patterns

**Purpose:**
Responsible for effective interaction between objects and simplify communication between them.

**Mnemonic:** CCIMMOSSTV (10 patterns)

### 1. Chain of Responsibility (Цепочка обязанностей)

**What It Does:**
Allows passing requests sequentially through a chain of handlers. Each subsequent handler decides whether it can handle the request itself and whether to pass the request further along the chain.

**Use Case:**
- Multiple handlers that can process a request
- Handler selection at runtime
- Decoupling sender and receiver

### 2. Command (Команда)

**What It Does:**
Turns requests into objects, allowing them to be passed as arguments when calling methods, queued, logged, and supports operation cancellation.

**Use Case:**
- Undo/redo functionality
- Queuing requests
- Logging requests
- Parameterizing objects with operations

### 3. Iterator (Итератор)

**What It Does:**
Provides the ability to sequentially traverse elements of composite objects without revealing their internal representation.

**Use Case:**
- Traversing collections
- Hiding collection implementation
- Supporting multiple traversal algorithms

### 4. Mediator (Посредник)

**What It Does:**
Allows reducing coupling between many classes by moving these connections into one mediator class.

**Use Case:**
- Complex communication between objects
- Reducing dependencies
- Centralized control

### 5. Memento (Снимок)

**What It Does:**
Allows saving and restoring past states of objects without revealing details of their implementation.

**Use Case:**
- Undo functionality
- State snapshots
- Checkpoint/restore

### 6. Observer (Наблюдатель)

**What It Does:**
Creates a subscription mechanism that allows some objects to monitor and react to events occurring in other objects.

**Use Case:**
- Event-driven systems
- Model-view separation
- Publish-subscribe systems

### 7. State (Состояние)

**What It Does:**
Allows objects to change behavior depending on their state. From the outside, it appears that the object's class has changed.

**Use Case:**
- State machines
- Behavior based on state
- Avoiding large conditional statements

### 8. Strategy (Стратегия)

**What It Does:**
Defines a family of similar algorithms and places each of them in its own class, after which algorithms can be interchanged directly during program execution.

**Use Case:**
- Multiple ways to perform a task
- Algorithm selection at runtime
- Replacing conditionals with polymorphism

### 9. Template Method (Шаблонный метод)

**What It Does:**
Defines the skeleton of an algorithm, shifting responsibility for some of its steps to subclasses. The pattern allows subclasses to override algorithm steps without changing its overall structure.

**Use Case:**
- Code reuse
- Defining algorithm structure
- Framework design

### 10. Visitor (Посетитель)

**What It Does:**
Allows adding new operations to a program without changing the classes of objects on which these operations can be performed.

**Use Case:**
- Adding operations to object structures
- Separating algorithms from objects
- Open/Closed Principle

---

## Creational Patterns

**Purpose:**
Responsible for creating objects, increasing flexibility and code reuse.

**Mnemonic:** ABFPS (5 patterns)

### 1. Abstract Factory (Абстрактная фабрика)

**What It Does:**
Creates families of related objects.

**Use Case:**
- Creating sets of compatible objects
- Ensuring product compatibility
- Platform-specific implementations

**Visual Analogy:**
"Complex meal (button + checkbox) from one restaurant"

### 2. Builder (Строитель)

**What It Does:**
Step-by-step construction of complex objects.

**Use Case:**
- Complex object construction
- Multiple configuration options
- Step-by-step assembly

**Visual Analogy:**
"LEGO constructor - step by step you assemble an object"

### 3. Factory Method (Фабричный метод)

**What It Does:**
Defines a common interface for creating objects in a superclass, allowing subclasses to change the type of objects created (delegates object creation to subclasses).

**Use Case:**
- Deferring object creation to subclasses
- Framework extensibility
- Class selection at runtime

**Visual Analogy:**
"Ordering one dish from the menu, but the chef chooses the recipe"

### 4. Prototype (Прототип)

**What It Does:**
Allows copying objects without delving into the details of their class implementation.

**Applicability:**
- When your code should not depend on the classes of copied objects. This often happens if your code works with objects supplied from outside through some common interface. You cannot bind to their classes, even if you wanted to, since their specific classes are unknown. The prototype pattern provides the client with a common interface for working with all prototypes. The client does not need to depend on all classes of copied objects, only on the cloning interface.
- When you have a lot of subclasses that differ in initial field values. Someone could create all these classes to be able to easily generate objects with a certain configuration. The prototype pattern suggests using a set of prototypes instead of creating subclasses to describe popular object configurations. Thus, instead of generating objects from subclasses, you will copy existing prototype objects in which the internal state is already configured. This will avoid explosive growth in the number of classes in the program and reduce its complexity.

**Use Case:**
- Expensive object creation
- Object copying
- Dynamic configuration

**Visual Analogy:**
"Photocopy of an object (you can change the name, but the rest is preserved)"

### 5. Singleton (Одиночка)

**What It Does:**
Guarantees that a class has only one instance.

**Applicability:**
- When there should be a single instance of some class in the program, accessible to all clients (for example, shared access to a database from different parts of the program). Singleton hides from clients all ways to create a new object, except for a special method. This method either creates an object or returns an existing object if it has already been created.
- When you want to have more control over global variables. Unlike global variables, Singleton guarantees that no other code will replace the created class instance, so you are always sure of the presence of only one singleton object. However, at any time you can expand this restriction and allow any number of singleton objects by changing the code in one place (the getInstance method).

**Use Case:**
- Single instance requirement
- Global access point
- Resource management

**Visual Analogy:**
"One president in a country - there cannot be a second one"

### Creational Patterns Comparison Table

| Pattern              | Purpose                                                           | When to Use                                                         | Example in Python                       | Key Features                                      |
| -------------------- | ------------------------------------------------------------------ | -------------------------------------------------------------------------- | ------------------------------------- | --------------------------------------------------------- |
| **Abstract Factory** | Creates families of related objects without specifying concrete classes | When you need to ensure compatibility of created objects                  | `GUIFactory -> WinFactory/MacFactory` | Family of products, unified interface                     |
| **Builder**          | Step-by-step assembly of complex objects                                    | When you need to create an object with different configurations                      | `HouseBuilder`                        | Flexible step-by-step assembly, isolation of complex logic          |
| **Factory Method**   | Delegates object creation to subclasses                              | When you don't know in advance which classes need to be created                      | `Dialog.create_button()`              | One product, subclasses decide what to create               |
| **Prototype**        | Clones already created objects                                      | When object creation is expensive or complex, but you can copy an already ready one | `Sheep.clone()`                       | Fast creation of copies, especially with deep copying |
| **Singleton**        | Guarantees that a class has only one instance                 | When you need centralized management, for example, logger or configuration | `SingletonMeta` (often with metaclass) | One object for the entire project                                |

### Evolution Path

Many architectures start with applying the Factory Method (simpler and extensible through subclasses) and evolve towards Abstract Factory, Prototype, or Builder (more flexible, but also more complex).

### Factory Method vs Abstract Factory

| Criterion                      | Factory Method (Фабричный метод)                                          | Abstract Factory (Абстрактная фабрика)                                           |
|-------------------------------|----------------------------------------------------------------------------|----------------------------------------------------------------------------------|
| **Goal**                      | Provides interface for creating **one** object                             | Provides interface for creating **family of related objects**            |
| **What it creates**              | One type of product                                                         | Several interrelated products                                             |
| **Who creates**               | Subclasses, overriding creation method                                   | Concrete factories implementing common interface                                |
| **Adding new products**| Requires creating new subclass                                         | Requires creating new factory                                                  |
| **Product relationship**           | Products not necessarily related                                           | Products of one factory are coordinated by style, platform, etc.                   |
| **Example**                    | Dialog window creates buttons for different platforms (Windows, Mac)             | UI factory creates both button and checkbox for one platform                       |
| **Pros**                     | + Simplicity, easy to add new object types                           | + Guarantee of compatibility of products of one family                            |
| **Cons**                    | – Doesn't solve problem of coordination of different products                     | – More code, harder to extend                                                |
| **When to use**        | When you need to delegate creation of **one object**                      | When objects are used **together** and must be compatible               |

#### Example Difference

**Factory Method:**
```python
class Dialog:
    def create_button(self):
        return Button()

class WinDialog(Dialog):
    def create_button(self):
        return WinButton()
```

**Abstract Factory:**
```python
class GUIFactory:
    def create_button(self): ...
    def create_checkbox(self): ...

class WinFactory(GUIFactory):
    def create_button(self): return WinButton()
    def create_checkbox(self): return WinCheckbox()
```

### Factory Method vs Builder vs Prototype

| Criterion                    | **Factory Method**                                  | **Builder**                                               | **Prototype**                                      |
|-----------------------------|------------------------------------------------------|------------------------------------------------------------|---------------------------------------------------|
| **Goal**                    | Delegate object creation to subclass             | Step-by-step assembly of complex object                          | Creating new object by cloning        |
| **Suitable for**            | Choosing appropriate subclass                        | Configuration of complex objects with many settings        | Fast copying of objects                     |
| **What it creates**             | One object from subclass                            | One object, assembled piece by piece                           | Copy of existing object                       |
| **Configuration flexibility**   | Low                                              | High — can set different parameters step by step         | Medium — depends on base object             |
| **Uses inheritance**| Yes                                                  | Optional                                              | No — uses cloning                     |
| **Example application**       | UI factory creates buttons for different OS                | Building house with choice of floors, doors, windows and roof      | Cloning objects with state preservation     |
| **When to use**      | When you need to choose object at runtime    | When object is complex, has many components              | When creation from scratch is expensive or complex           |
| **Implementation simplicity**     | Simple                                             | Medium                                                    | Simple (if `clone()` is correctly implemented)     |

#### Brief Metaphors

- **Factory Method** → "Choose car model from dealer"
- **Builder** → "Assemble car manually from scratch: engine, body, color..."
- **Prototype** → "Copy already assembled car and change license plate"

#### Example Scenarios

| Scenario                                  | Suitable Pattern     | Why?                                              |
|-------------------------------------------|------------------------|------------------------------------------------------|
| User chooses platform (Win/Mac) | Factory Method         | Different buttons — different subclasses                    |
| Creating complex report                  | Builder                | Complex structure with headers, footer and data |
| Copying object with style preservation   | Prototype              | Need to quickly clone object                     |

---

## Structural Patterns

**Purpose:**
Responsible for building structures of objects and classes, simplifying the implementation of relationships between them.

**Mnemonic:** ABCDFFP (7 patterns)

### 1. Adapter (Адаптер)

**What It Does:**
Brings the interface of one class to the interface expected by the client. Adapts the interface of one class to another.

**Use Case:**
- Integrating incompatible interfaces
- Wrapping legacy code
- Interface translation

### 2. Bridge (Мост)

**What It Does:**
Separates abstraction and its implementation so that they can change independently.

**Use Case:**
- Runtime binding
- Platform independence
- Separating interface and implementation

### 3. Composite (Компоновщик)

**What It Does:**
Allows grouping many objects into a tree structure, and then working with it as if it were a single object.

**Use Case:**
- Tree structures
- Part-whole hierarchies
- Uniform treatment of individual and composite objects

### 4. Decorator (Декоратор)

**What It Does:**
Adds new responsibilities to objects without changing their structure. Adds functionality to objects without changing their class.

**Use Case:**
- Dynamic behavior extension
- Adding features without subclassing
- Wrapper pattern

### 5. Facade (Фасад)

**What It Does:**
Provides a unified interface to a set of interfaces in a subsystem. Provides a simple interface to a complex subsystem.

**Use Case:**
- Simplifying complex systems
- Hiding subsystem complexity
- Providing a single entry point

### 6. Flyweight (Легковес)

**What It Does:**
Saves memory by reusing already created objects (suitable for large numbers of similar objects).

**Use Case:**
- Large number of similar objects
- Memory optimization
- Shared state management

### 7. Proxy (Заместитель)

**What It Does:**
Replaces another object and controls access to it or for optimization.

**Use Case:**
- Lazy loading
- Access control
- Caching
- Remote proxies

### Structural Patterns Comparison Table

| Pattern              | Purpose                                                                 | When to Use                                                                   | Example in Python                       | Key Features                                               |
|----------------------|----------------------------------------------------------------------------|----------------------------------------------------------------------------------------|---------------------------------------|--------------------------------------------------------------------|
| **Adapter (Адаптер)** | Brings interface of one class to interface expected by client        | When you need to use incompatible classes together                            | `Adapter(EuropeanSocket).voltage()`   | Changes interface without changing original class               |
| **Facade (Фасад)**    | Provides unified interface to subsystem                      | When you need to simplify work with complex system                                       | `ComputerFacade().start()`            | Hides subsystem complexity, convenient for client use   |
| **Composite (Компоновщик)** | Treats groups of objects as single object                         | When you need object hierarchy (tree), and need to work equally with nodes and leaves| `Composite.add(Leaf)`                 | Recursive structure, same interface for all components  |
| **Decorator (Декоратор)** | Adds new responsibilities to objects without changing their code             | When you need to dynamically extend object behavior                                 | `LoggingDecorator(EmailSender)`       | Wrapper around object, doesn't break main code                  |
| **Proxy (Заместитель)** | Controls access to another object                                    | When you need accessible interface to heavy or protected object                   | `LazyImage('cat.jpg')`                | Placeholder, caching, access control                   |
| **Bridge (Мост)**     | Separates abstraction and implementation, allowing them to change independently      | When you need to scale abstractions and implementations independently                        | `Remote(Control).operate()`           | Divides object into 2 independent hierarchies: abstraction and implementation   |
| **Flyweight (Легковес)** | Saves memory through shared use of identical objects | When you need to create very many similar objects                                   | `FlyweightFactory.get_char('a')`      | Common (internal) states stored once                   |

---

## Resources

**Source:**
- [refactoring.guru](https://refactoring.guru) - Comprehensive design patterns resource

---

## Summary

Design patterns provide proven solutions to common software design problems:

**Pattern Categories:**
- **Behavioral (10)**: Focus on object interaction and communication
- **Creational (5)**: Focus on object creation mechanisms
- **Structural (7)**: Focus on object composition and relationships

**Key Takeaways:**
- Patterns are reusable solutions, not code
- Learn to recognize when to apply patterns
- Start simple (Factory Method) and evolve to more complex patterns
- Choose patterns based on your specific problem
- Patterns work together - combine them as needed

Understanding and applying design patterns helps create more maintainable, flexible, and robust software architectures.
