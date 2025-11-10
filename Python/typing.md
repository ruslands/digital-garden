# Python Type Hints: Advanced Typing Concepts

## Table of Contents
- [Python Type Hints: Advanced Typing Concepts](#python-type-hints-advanced-typing-concepts)
  - [Table of Contents](#table-of-contents)
  - [Introduction](#introduction)
  - [TypeVar](#typevar)
  - [NewType](#newtype)
  - [cast](#cast)
  - [Generic](#generic)
  - [Final](#final)
  - [Summary](#summary)

---

## Introduction

Python's `typing` module provides powerful tools for creating type hints and generic types. These features help improve code clarity, enable better IDE support, and allow static type checkers like `mypy` to catch potential errors before runtime.

This guide covers advanced typing concepts that go beyond basic type annotations, focusing on tools for creating reusable and flexible type definitions.

---

## TypeVar

**What is TypeVar?**

`TypeVar` is used as part of the process of creating your own custom [generic functions or classes](https://mypy.readthedocs.io/en/stable/generics.html).

**Purpose:**

`TypeVar` is a variable you can use in type signatures so you can refer to the same unspecified type more than once. This allows you to create generic functions and classes that work with multiple types while maintaining type safety.

**When to Use:**

- When creating generic functions that should work with multiple types
- When you need to ensure the same type is used in multiple places
- When creating reusable type-safe code that doesn't depend on a specific type

**Example:**

```python
from typing import TypeVar, List

# Define a type variable
T = TypeVar('T')

def first_item(items: List[T]) -> T:
    """Return the first item from a list, preserving the type."""
    return items[0]

# Usage
numbers: List[int] = [1, 2, 3]
first_number: int = first_item(numbers)  # Type checker knows this is int

strings: List[str] = ['a', 'b', 'c']
first_string: str = first_item(strings)  # Type checker knows this is str
```

**Key Benefits:**

- Type safety: Ensures consistent types throughout your function
- Reusability: One function works with multiple types
- IDE support: Better autocomplete and type checking

---

## NewType

**What is NewType?**

`NewType` is for when you want to declare a distinct type without actually doing the work of creating a new type or worrying about the overhead of creating new class instances.

**Purpose:**

`NewType` creates a distinct type that is treated as a subtype of the original type by static type checkers, but at runtime it's identical to the original type. This provides type safety without runtime overhead.

**When to Use:**

- When you want to distinguish between different uses of the same base type
- When you need type safety but don't want runtime overhead
- When you want to prevent accidentally mixing different semantic types

**Example:**

```python
from typing import NewType

# Create distinct types from the same base type
UserId = NewType('UserId', int)
ProductId = NewType('ProductId', int)

def get_user(user_id: UserId) -> dict:
    """Get user by ID."""
    return {"id": user_id, "name": "John"}

def get_product(product_id: ProductId) -> dict:
    """Get product by ID."""
    return {"id": product_id, "name": "Widget"}

# Usage
user_id: UserId = UserId(123)
product_id: ProductId = ProductId(456)

# Type checker will catch this error:
# get_user(product_id)  # Error: Expected UserId, got ProductId

# But at runtime, they're just ints:
print(user_id + product_id)  # Works fine: 579
```

**Key Benefits:**

- Type safety: Prevents mixing semantically different types
- Zero runtime overhead: No performance cost
- Clear intent: Makes code more self-documenting

---

## cast

**What is cast?**

`cast` is a function that tells type checkers to treat a value as a specific type, without performing any runtime conversion.

**Purpose:**

`cast` is used when you know more about a type than the type checker can infer, or when you need to work around limitations in type inference.

**When to Use:**

- When you know the type but the type checker doesn't
- When working with dynamic code or external libraries
- When you need to satisfy type checker requirements without runtime changes

**Example:**

```python
from typing import cast, Dict, Any

def process_data(data: Any) -> Dict[str, int]:
    """Process data that we know is a Dict[str, int]."""
    # We know data is Dict[str, int], but type checker sees Any
    result = cast(Dict[str, int], data)
    return result

# Usage
unknown_data: Any = {"a": 1, "b": 2}
typed_data = process_data(unknown_data)  # Type checker treats as Dict[str, int]
```

**Important Notes:**

- `cast` does NOT perform runtime conversion
- It only affects static type checking
- Use sparingly - if you need it often, consider improving your type annotations

---

## Generic

**What is Generic?**

`Generic` is a base class for creating generic classes that can work with multiple types.

**Purpose:**

`Generic` allows you to create classes that are parameterized by one or more types, similar to how `List[T]` or `Dict[K, V]` work.

**When to Use:**

- When creating reusable container classes
- When building libraries that need to work with multiple types
- When you want type-safe generic data structures

**Example:**

```python
from typing import Generic, TypeVar, List

T = TypeVar('T')

class Stack(Generic[T]):
    """A generic stack implementation."""
    
    def __init__(self) -> None:
        self._items: List[T] = []
    
    def push(self, item: T) -> None:
        """Add an item to the stack."""
        self._items.append(item)
    
    def pop(self) -> T:
        """Remove and return the top item."""
        return self._items.pop()
    
    def is_empty(self) -> bool:
        """Check if stack is empty."""
        return len(self._items) == 0

# Usage
int_stack: Stack[int] = Stack()
int_stack.push(1)
int_stack.push(2)
value: int = int_stack.pop()  # Type checker knows this is int

str_stack: Stack[str] = Stack()
str_stack.push("hello")
text: str = str_stack.pop()  # Type checker knows this is str
```

**Key Benefits:**

- Type safety: Ensures consistent types in generic containers
- Reusability: One class works with multiple types
- Better IDE support: Autocomplete and type checking work correctly

---

## Final

**What is Final?**

`Final` is used to indicate that a variable, attribute, or method should not be reassigned, overridden, or modified.

**Purpose:**

`Final` provides a way to declare constants and prevent accidental modifications, improving code safety and clarity.

**When to Use:**

- For constants that should never change
- For class attributes that shouldn't be overridden
- For methods that shouldn't be overridden in subclasses

**Example:**

```python
from typing import Final

# Module-level constant
MAX_RETRIES: Final[int] = 3
API_BASE_URL: Final[str] = "https://api.example.com"

class Config:
    """Configuration class with final attributes."""
    
    # Class attribute that shouldn't be changed
    VERSION: Final[str] = "1.0.0"
    
    def __init__(self) -> None:
        # Instance attribute that shouldn't be reassigned
        self.database_url: Final[str] = "postgresql://localhost/mydb"

# Usage
config = Config()
# Type checker will warn about this:
# config.database_url = "new_url"  # Error: Cannot assign to Final

# This is also an error:
# MAX_RETRIES = 5  # Error: Cannot assign to Final
```

**Key Benefits:**

- Prevents accidental modifications
- Makes code intent clear
- Helps catch bugs early with type checkers

---

## Summary

These advanced typing features provide powerful tools for creating type-safe, reusable Python code:

- **TypeVar**: Create generic functions and classes that work with multiple types
- **NewType**: Create distinct types without runtime overhead
- **cast**: Help type checkers understand types when inference fails
- **Generic**: Build reusable generic classes
- **Final**: Declare constants and prevent modifications

Using these tools effectively can significantly improve code quality, maintainability, and developer experience.
