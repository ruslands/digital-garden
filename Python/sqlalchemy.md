# SQLAlchemy: Database ORM Guide

## Table of Contents
1. [Introduction](#introduction)
2. [Column Options](#column-options)
3. [Relationship Loading Strategies](#relationship-loading-strategies)
4. [Connection Pooling](#connection-pooling)
5. [Query Result Methods](#query-result-methods)

---

## Introduction

SQLAlchemy is a powerful Python SQL toolkit and Object-Relational Mapping (ORM) library. It provides a full suite of well-known enterprise-level persistence patterns and is designed for efficient and high-performing database access.

This guide covers key SQLAlchemy concepts including column definitions, relationship loading strategies, connection pooling, and query result handling.

---

## Column Options

### default vs server_default

**The Problem:**

When adding a new column (e.g., `created_at` with `NOT NULL`) to an existing table that already contains data, using `default` will not work properly.

**Why default Fails:**

If you use `default`, during database migration/upgrade, an error will occur saying "cannot insert NULL value to existing data in the table." This causes significant problems if you want to maintain your data, even just for testing.

**Why server_default Works:**

When using `server_default`, during database upgrade, the database will insert the current DateTime value to all previous existing testing data automatically.

**Conclusion:**

In this case, only `server_default` will work for adding NOT NULL columns to existing tables with data.

**Example:**

```python
from sqlalchemy import Column, DateTime, func
from datetime import datetime

class User(Base):
    __tablename__ = 'users'
    
    id = Column(Integer, primary_key=True)
    
    # ❌ This will fail if table has existing rows
    # created_at = Column(DateTime, nullable=False, default=datetime.utcnow)
    
    # ✅ This works - database handles the default
    created_at = Column(
        DateTime, 
        nullable=False, 
        server_default=func.now()  # Database-level default
    )
```

**Key Differences:**

- **`default`**: Python-level default, applied by SQLAlchemy before insert
- **`server_default`**: Database-level default, applied by the database server

### Other Column Options

#### foreign_key

Defines a foreign key relationship to another table.

```python
from sqlalchemy import Column, Integer, ForeignKey

class Post(Base):
    __tablename__ = 'posts'
    
    id = Column(Integer, primary_key=True)
    user_id = Column(Integer, ForeignKey('users.id'))
```

#### nullable

Determines if a column can contain NULL values.

```python
# Column can be NULL
name = Column(String(100), nullable=True)

# Column cannot be NULL
email = Column(String(255), nullable=False)
```

#### index

Creates an index on the column for faster queries.

```python
# Single column index
email = Column(String(255), index=True)

# Or create composite indexes separately
```

---

## Connection Pooling

### pool_timeout

**Definition:**

`pool_timeout` is the maximum number of seconds to wait when retrieving a new connection from the pool. After the specified amount of time, an exception will be thrown.

**Example:**

```python
from sqlalchemy import create_engine

engine = create_engine(
    'postgresql://user:pass@localhost/dbname',
    pool_timeout=30,  # 30 seconds
)
```

**Use Case:**

Prevents indefinite waiting when all connections in the pool are busy. Useful for detecting connection pool exhaustion issues.

### pool_recycle

**Definition:**

`pool_recycle` is the maximum number of seconds a connection can persist. Connections that live longer than the specified amount of time will be re-established.

**Example:**

```python
engine = create_engine(
    'postgresql://user:pass@localhost/dbname',
    pool_recycle=1800,  # 30 minutes
)
```

**Why It's Important:**

Database servers often close idle connections after a certain period. `pool_recycle` ensures connections are refreshed before the database closes them, preventing "connection lost" errors.

**Best Practice:**

Set `pool_recycle` to be less than your database server's connection timeout (typically 30 minutes or 1 hour).

---

## Relationship Loading Strategies

The primary forms of relationship loading in SQLAlchemy control when and how related objects are loaded from the database.

### lazy='select' (Default)

**Description:**

Lazy loading option. This form of loading emits a SELECT statement at attribute access time to lazily load a related reference on a single object at a time.

**Characteristics:**

- Default loading style for all `relationship()` constructs
- Loads related objects only when accessed
- Can lead to N+1 query problems

**Example:**

```python
class User(Base):
    __tablename__ = 'users'
    posts = relationship('Post', lazy='select')

# Accessing posts triggers a query
user = session.query(User).first()
print(user.posts)  # SELECT query executed here
```

**When to Use:**

- When you rarely need related objects
- When memory usage is a concern
- For one-to-many relationships where you don't always need the collection

### lazy='selectin'

**Description:**

Select IN loading option. This form of loading emits a second (or more) SELECT statement which assembles the primary key identifiers of the parent objects into an IN clause, so that all members of related collections/scalar references are loaded at once by primary key.

**Characteristics:**

- Loads all related objects in a single additional query
- Uses IN clause with primary keys
- More efficient than multiple lazy loads

**Example:**

```python
class User(Base):
    __tablename__ = 'users'
    posts = relationship('Post', lazy='selectin')

# All posts loaded in one query after initial query
users = session.query(User).all()
for user in users:
    print(user.posts)  # Already loaded, no additional queries
```

**When to Use:**

- When you know you'll need related objects for multiple parent objects
- To avoid N+1 query problems
- When working with collections

### lazy='joined'

**Description:**

Joined loading option. This form of loading applies a JOIN to the given SELECT statement so that related rows are loaded in the same result set.

**Characteristics:**

- Uses SQL JOIN to load everything in one query
- Can create large result sets with duplicates
- Most efficient for single object queries

**Example:**

```python
class User(Base):
    __tablename__ = 'users'
    posts = relationship('Post', lazy='joined')

# Single query with JOIN
user = session.query(User).filter(User.id == 1).first()
print(user.posts)  # Already loaded via JOIN
```

**When to Use:**

- When you always need related objects
- For single object queries
- When you want to minimize database round trips

### lazy='raise' / lazy='raise_on_sql'

**Description:**

Raise loading option. This form of loading is triggered at the same time a lazy load would normally occur, except it raises an ORM exception to guard against the application making unwanted lazy loads.

**Characteristics:**

- Prevents accidental lazy loading
- Forces explicit eager loading
- Useful for debugging and performance optimization

**Example:**

```python
class User(Base):
    __tablename__ = 'users'
    posts = relationship('Post', lazy='raise')

user = session.query(User).first()
try:
    print(user.posts)  # Raises exception
except Exception as e:
    print("Lazy loading prevented!")
```

**When to Use:**

- To prevent N+1 query problems
- In production code where lazy loading should be explicit
- For performance-critical applications

### lazy='subquery'

**Description:**

Subquery loading option. This form of loading emits a second SELECT statement which re-states the original query embedded inside of a subquery, then JOINs that subquery to the related table to load all members of related collections/scalar references at once.

**Characteristics:**

- Uses subquery instead of JOIN
- Can be more efficient than joined loading for collections
- Avoids duplicate rows in result set

**When to Use:**

- When joined loading creates too many duplicate rows
- For one-to-many relationships with many related objects
- When you need all related objects for multiple parents

### lazy='write_only'

**Description:**

Write-only loading. This collection-only loader style produces an alternative attribute instrumentation that never implicitly loads records from the database. Instead, it only allows `WriteOnlyCollection.add()`, `WriteOnlyCollection.add_all()`, and `WriteOnlyCollection.remove()` methods.

**Characteristics:**

- Never loads the collection automatically
- Querying requires explicit `WriteOnlyCollection.select()` method
- Prevents accidental loading of large collections

**When to Use:**

- For very large collections (millions of records)
- When you only need to add/remove items, not read them
- To prevent memory issues with large datasets

### lazy='dynamic'

**Description:**

Dynamic loading. This is a legacy collection-only loader style which produces a `Query` object when the collection is accessed, allowing custom SQL to be emitted against the collection's contents.

**Characteristics:**

- Returns a query object instead of a list
- Allows filtering and pagination
- Legacy feature - superseded by "write_only" collections

**Note:**

Dynamic loaders will implicitly iterate the underlying collection in various circumstances, making them less useful for managing truly large collections. Dynamic loaders are superseded by "write only" collections.

**When to Use:**

- Legacy code compatibility
- When you need query-like access to collections
- For pagination of related objects

### lazy='noload'

**Description:**

No loading option. This loading style turns the attribute into an empty attribute (`None` or `[]`) that will never load or have any loading effect.

**Characteristics:**

- Attribute is always empty
- Never queries the database
- Useful for write-only scenarios

**When to Use:**

- Implementing "write-only" attributes
- When you never need to read the relationship
- For performance optimization in specific scenarios

---

## Query Result Methods

SQLAlchemy provides various methods to retrieve results from queries. Here's a comprehensive reference:

| Method                 | Description                                      |
| ---------------------- | ------------------------------------------------ |
| `all()`                | Return all results as a list                     |
| `chunks()`             | Return results in chunks (for large result sets) |
| `close()`              | Close the result set                             |
| `columns`              | Access column information                        |
| `fetchall()`           | Fetch all rows (similar to `all()`)              |
| `fetchmany(size)`      | Fetch a specific number of rows                  |
| `fetchone()`           | Fetch a single row                               |
| `first()`              | Return the first result or None                  |
| `freeze()`             | Create a frozen copy of the result               |
| `keys()`               | Get column names                                 |
| `mappings()`           | Return results as dictionaries                   |
| `merge()`              | Merge results with session                       |
| `one()`                | Return exactly one result (raises if 0 or >1)    |
| `one_or_none()`        | Return one result or None (raises if >1)         |
| `partitions(size)`     | Partition results into chunks                    |
| `scalar()`             | Return the first column of the first row         |
| `scalar_one()`         | Return scalar, must be exactly one result        |
| `scalar_one_or_none()` | Return scalar or None (raises if >1)             |
| `scalars()`            | Return scalar values from all rows               |
| `unique()`             | Apply unique filtering to results                |

### Common Usage Examples

```python
from sqlalchemy.orm import Session

# Get all results
users = session.query(User).all()

# Get first result
user = session.query(User).first()

# Get exactly one (raises if not exactly one)
user = session.query(User).filter(User.id == 1).one()

# Get one or None (raises if more than one)
user = session.query(User).filter(User.id == 999).one_or_none()

# Get scalar value
count = session.query(func.count(User.id)).scalar()

# Get results as dictionaries
users_dict = session.query(User).mappings().all()

# Process in chunks (for large datasets)
for chunk in session.query(User).yield_per(100):
    process_chunk(chunk)
```

---

## Summary

SQLAlchemy provides powerful tools for database interaction:

- **Column Options**: Use `server_default` for database-level defaults, especially when adding NOT NULL columns to existing tables
- **Connection Pooling**: Configure `pool_timeout` and `pool_recycle` for production environments
- **Relationship Loading**: Choose the right `lazy` strategy based on your access patterns to optimize performance
- **Query Methods**: Use appropriate result methods (`all()`, `first()`, `scalar()`, etc.) based on your needs

Understanding these concepts helps you build efficient, scalable database applications with SQLAlchemy.
