# GraphQL: Query Language and Runtime Guide

## Table of Contents
1. [Introduction](#introduction)
2. [GraphQL Service](#graphql-service)
3. [Type System](#type-system)
4. [Relay Connections](#relay-connections)
5. [Resources](#resources)

---

## Introduction

**GraphQL** is a query language for APIs and a runtime for executing those queries by using a type system you define for your data. GraphQL provides a more efficient, powerful, and flexible alternative to REST.

**Key Benefits:**
- Clients request exactly the data they need
- Single endpoint for all operations
- Strongly typed schema
- Introspection capabilities
- Real-time subscriptions support

**Resources:**
- [GraphQL Official Documentation](https://graphql.org/)
- [Apollo GraphQL](https://www.apollographql.com/) - Popular GraphQL implementation
- [GraphQL Rules](https://graphql-rules.onrender.com/rules/naming-fields-args/) - Best practices and naming conventions
- [GraphQL Rules GitHub](https://github.com/graphql-rules/graphql-rules) - Community-driven rules
- [Schema Design Articles](https://github.com/nodkz/conf-talks/tree/master/articles/graphql/schema-design) - Schema design patterns and best practices
- [Nodkz GitHub](https://github.com/nodkz) - GraphQL community resources

---

## GraphQL Service

A **GraphQL service** is an API that uses GraphQL to define its schema and handle queries. It consists of:

- **Type definitions**: Define the structure of your data
- **Resolvers**: Functions that fetch the actual data
- **Schema**: The complete type system

**Key Characteristics:**
- Single endpoint (typically `/graphql`)
- Self-documenting through introspection
- Type-safe queries and responses

---

## Type System

GraphQL has a strong type system. **Object types, scalars, and enums are the only kinds of types you can define in GraphQL**.

### Nullability and Lists

Understanding nullability and lists is crucial in GraphQL:

| Syntax       | Meaning                                   | Possible Values                                  |
| ------------ | ----------------------------------------- | ------------------------------------------------ |
| `id: UUID!`  | Cannot be null                            | `UUID` only                                      |
| `[String]`   | Nullable list of nullable strings         | `[]`, `[null]`, `null`                           |
| `[String]!`  | Non-nullable list of nullable strings     | `[]`, `[null]` (list cannot be null)             |
| `[String!]`  | Nullable list of non-nullable strings     | `[]`, `null` (strings cannot be null)            |
| `[String!]!` | Non-nullable list of non-nullable strings | `[]` only (neither list nor strings can be null) |

**Key Points:**
- `!` means the field cannot be null
- `[]` means it's a list
- Combinations control both list and element nullability

### Scalars

**Built-in Scalars:**

- **Int**: A signed 32-bit integer
- **Float**: A signed double-precision floating-point value
- **String**: A UTF-8 character sequence
- **Boolean**: `true` or `false`
- **ID**: The ID scalar type represents a unique identifier, often used to refetch an object

**Custom Scalars:**

You can define a new scalar with constraints, such as:
- Character count limits
- Character type restrictions
- Format validation
- Custom serialization/deserialization

**Example:**
```graphql
scalar Email
scalar Date
scalar URL
```

### Enums

**Definition:**
Enums are a special kind of scalar that is restricted to a particular set of allowed values.

**Example:**
```graphql
enum Episode {
  NEWHOPE
  EMPIRE
  JEDI
}
```

**Use Cases:**
- Status fields (e.g., `ACTIVE`, `INACTIVE`, `PENDING`)
- Categories with fixed options
- State machines

### Interfaces

**Definition:**
An interface is an abstract type that includes a certain set of fields that a type must include to implement the interface. Think of it as a base class or contract.

**Characteristics:**
- Defines a set of fields that implementing types must have
- Allows polymorphism in GraphQL queries
- Types can implement multiple interfaces

**Example:**
```graphql
interface Character {
  name: String!
  appearsIn: [Episode!]!
}
```

**Use Case:**
When multiple types share common fields but have different additional fields.

### Types

**Definition:**
Types are the basic building blocks of a GraphQL schema. A type can implement an interface, inheriting its fields and adding its own parameters.

**Example:**
```graphql
type Character {
  name: String!
  appearsIn: [Episode!]!
}

type Human implements Character {
  id: ID!
  name: String!
  friends: [Character]
  appearsIn: [Episode]!
  starships: [Starship]
  totalCredits: Int
}
```

**Key Points:**
- `Human` implements `Character`, so it must have `name` and `appearsIn`
- `Human` adds additional fields like `id`, `friends`, `starships`, and `totalCredits`
- `friends` returns a list of `Character` (could be `Human`, `Droid`, etc.)

### Union Types

**Definition:**
Union types allow a field to return one of several possible types.

**Example:**
```graphql
union SearchResult = Human | Droid | Starship
```

**Use Case:**
When a field can return different types that don't share a common interface.

### Input Types

**Definition:**
Input types allow you to pass complex objects as arguments, particularly valuable for mutations where you might want to pass in a whole object to be created.

**Why They Exist:**
So far, we've only talked about passing scalar values, like enums or strings, as arguments into a field. Input types enable passing complex objects.

**Example:**
```graphql
input ReviewInput {
  stars: Int!
  commentary: String
}
```

**Use Cases:**
- Mutations (creating/updating resources)
- Complex filtering
- Batch operations

---

## Relay Connections

**Relay Connections** is a specification for pagination in GraphQL, providing a standardized way to handle paginated data.

**Reference:**
- [Relay Connections Specification](https://relay.dev/graphql/connections.htm)

### Arguments

**Pagination Arguments:**

- **`first`**: Used for forward slicing, returns n rows from the beginning
- **`after`**: Used for forward pagination. You pass in a cursor, and the server returns items after that cursor
- **`last`**: Used for backward slicing, returns n rows from the end
- **`before`**: Used for backward pagination, returns items before the specified cursor

**Additional Arguments:**

- **`filter`**: Apply filtering criteria to the connection
- **`sorting`**: Specify sorting order for the results

### Fields

**Connection Structure:**

**`edges`**: An array of edge objects, each containing:
- **`cursor`**: A unique identifier for this position in the connection
- **`<object>`**: The actual data object

**`nodes`**: A convenience field that returns just the objects (without cursor wrapper)
- **`<object>`**: The actual data object

**`pageInfo`**: Metadata about the current page:
- **`startCursor`**: Cursor of the first item in this page
- **`endCursor`**: Cursor of the last item in this page
- **`hasNextPage`**: Boolean indicating if there are more edges available, or if we've reached the end of this connection
- **`hasPreviousPage`**: Boolean indicating if there are previous pages

**`totalCount`**: The total number of items in the connection (optional, can be expensive to compute)

### Example Response

```json
{
  "data": {
    "protoCollections": {
      "totalCount": 121,
      "pageInfo": {
        "endCursor": "MTIx",
        "hasNextPage": false,
        "hasPreviousPage": false,
        "startCursor": "MQ"
      },
      "nodes": [{
        "displayName": "",
        "externalCollectionId": "12782",
        "id": "a9be6708-bfe2-485f-a38b-baf09edd0cac",
        "isActive": true,
        "type": "MASTER_HIERARCHY"
      }],
      "edges": [{
        "cursor": "MQ",
        "node": {
          "displayName": "",
          "externalCollectionId": "12782",
          "id": "a9be6708-bfe2-485f-a38b-baf09edd0cac",
          "isActive": true,
          "type": "MASTER_HIERARCHY"
        }
      }]
    }
  }
}
```

**Key Points:**
- `nodes` provides direct access to objects (simpler)
- `edges` provides cursors along with objects (more flexible)
- `pageInfo` enables efficient pagination navigation
- `totalCount` gives overall context (use sparingly due to performance)

---

## Best Practices

### Naming Conventions

- Use PascalCase for types, interfaces, and enums
- Use camelCase for fields and arguments
- Use descriptive, clear names
- Follow GraphQL naming rules: [GraphQL Rules - Naming](https://graphql-rules.onrender.com/rules/naming-fields-args/)

### Schema Design

- Design for your use cases, not for your database
- Use interfaces for shared behavior
- Leverage unions for flexible return types
- Use input types for complex mutations
- Consider pagination from the start

### Performance

- Use `totalCount` sparingly (can be expensive)
- Implement proper pagination
- Consider field-level resolvers
- Use DataLoader for N+1 query problems

---

## Summary

GraphQL provides a powerful, type-safe way to build APIs:

- **Type System**: Scalars, enums, interfaces, types, unions, and input types
- **Nullability**: Control with `!` and list syntax
- **Pagination**: Relay Connections specification for standardized pagination
- **Flexibility**: Clients request exactly what they need

**Key Takeaways:**
- GraphQL is strongly typed and self-documenting
- Understanding nullability and lists is crucial
- Relay Connections provide a standard pagination pattern
- Use interfaces and unions for flexible schemas
- Input types enable complex mutations

GraphQL simplifies API development by providing a single endpoint with a flexible, type-safe query language that gives clients exactly the data they need.
