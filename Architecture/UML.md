# UML: Unified Modeling Language Guide

## Table of Contents
1. [Introduction](#introduction)
2. [UML Diagram Types](#uml-diagram-types)
3. [Class Diagram Components](#class-diagram-components)
4. [System Design Diagrams](#system-design-diagrams)
5. [Tools and Resources](#tools-and-resources)

---

## Introduction

**UML (Unified Modeling Language)** is a standardized modeling language used to visualize, specify, construct, and document the artifacts of software systems. UML provides a common vocabulary for software developers, system architects, and business analysts to communicate about system design.

**Total Diagrams:** UML defines 14 different types of diagrams, each serving a specific purpose in system design and documentation.

**Visual Reference:**
![[architecture-4.png]]

---

## UML Diagram Types

UML diagrams are categorized into two main groups:

### Structural Diagrams
- Class Diagram
- Object Diagram
- Component Diagram
- Composite Structure Diagram
- Deployment Diagram
- Package Diagram
- Profile Diagram

### Behavioral Diagrams
- Use Case Diagram
- Activity Diagram
- State Machine Diagram
- Sequence Diagram
- Communication Diagram
- Interaction Overview Diagram
- Timing Diagram

---

## Class Diagram Components

### Class Types

**Boundary Classes:**
- Classes at the boundary of the system
- The classes that you or other systems interact with
- Represent interfaces between the system and external actors

**Entity Classes:**
- Typical business entities like "person" and "bank account"
- Represent core data and business logic
- Often map to database tables

**Control Classes:**
- Implement business logic or orchestration
- Coordinate interactions between boundary and entity classes
- Handle application flow and processing

### Active vs Passive Classes

**Active Classes:**
- Initiate and control the flow of activity
- Illustrated with a thicker border in UML diagrams
- Represent classes that have their own thread of control

**Passive Classes:**
- Store data and serve other classes
- Do not initiate activity on their own
- Standard border thickness in diagrams

### Common Elements

- **Actor**: Represents external entities (users, systems) that interact with the system
- **Database**: Represents data storage
- **Collections**: Represents data structures
- **Queue**: Represents message queues or task queues

---

## System Design Diagrams

### 1. Context Diagram (Contextual Diagram)

**Purpose:**
The context diagram is the first thing you create when working with a system. If you skip this, it can cost you missed integrations and design errors.

**What It Shows:**
- The highest level view of the system
- People and systems that will interact with your future system
- External dependencies and integrations

**Why It's Important:**
This is the most important representation, providing the highest level of understanding of people and systems that will interact with your system. The context may include some lower-level details, which is normal and acceptable.

**Key Benefits:**
- Identifies all external systems and actors
- Prevents missed integrations
- Provides clear system boundaries
- Helps stakeholders understand system scope

### 2. Container Diagram

**Purpose:**
The container diagram provides a view of what deployable elements make up the server-side and how these components interact with each other.

**What It Shows:**
- Deployable components (applications, databases, file systems)
- How components communicate
- Technology choices for each container

**Key Benefits:**
- Shows the high-level technical building blocks
- Illustrates technology decisions
- Helps understand system architecture at a technical level

### 3. Sequence Diagram

**Purpose:**
While the first two diagrams show how system elements relate to each other, they cannot demonstrate what happens inside the system. Sequence diagrams answer questions like:
- Which components are involved in a process?
- What actions are triggered?
- How do components interact with each other?

**Example:**
When a user registers in your system:
- Which components are involved?
- What actions are triggered?
- How do components interact?

**What It Shows:**
- Temporal sequence of interactions between objects
- Message flow between objects
- Object lifelines and activation periods

**Key Benefits:**
- Shows dynamic behavior of the system
- Illustrates the order of operations
- Helps understand complex interactions
- Useful for debugging and testing

### 4. Deployment Diagram

**Purpose:**
While context, container, and sequence diagrams help understand system composition, connections, and interactions, they cannot answer questions about:
- Availability
- Scalability
- Security

The deployment diagram addresses these concerns.

**What It Shows:**
- Physical infrastructure (servers, networks, devices)
- How software components are deployed
- Network topology and communication paths
- Security zones and boundaries

**Key Benefits:**
- Shows infrastructure requirements
- Illustrates scalability architecture
- Helps plan deployment strategies
- Identifies security considerations

### 5. Use Case Diagram

**Purpose:**
Previous diagrams were mainly technical, while the use case diagram is more business-oriented. It shows how people interact with your system at a very high level.

**What It Shows:**
- Actors (users, external systems)
- Use cases (business capabilities)
- Relationships between actors and use cases

**Key Benefits:**
- Business-focused view of the system
- Identifies functional requirements
- Helps communicate with stakeholders
- Use cases can be considered as business capabilities

---

## Tools and Resources

### PlantUML

**PlantUML** is a tool that allows you to create UML diagrams using a simple text-based syntax.

**Resources:**
- [PlantUML Sequence Diagram](https://plantuml.com/sequence-diagram) - Official documentation for sequence diagrams
- [PlantUML Official Site](http://www.plantuml.com/plantuml/uml/SyfFKj2rKt3CoKnELR1Io4ZDoSa70000) - Online editor and examples
- [Real World PlantUML](https://real-world-plantuml.com) - Practical examples and patterns

### Other Tools

- [SmartDraw Class Diagram](https://www.smartdraw.com/class-diagram/) - Visual class diagram tool

### Articles and Tutorials

- [Habr Article on UML](https://habr.com/ru/post/416077/) - Russian-language tutorial on UML

---

## Best Practices

### When to Use Each Diagram

1. **Start with Context Diagram**: Always create this first to understand system boundaries
2. **Use Container Diagram**: When you need to show technical architecture
3. **Use Sequence Diagram**: When you need to understand dynamic behavior
4. **Use Deployment Diagram**: When planning infrastructure and scalability
5. **Use Use Case Diagram**: When communicating with business stakeholders

### Diagram Creation Order

1. Context Diagram (system boundaries)
2. Container Diagram (technical building blocks)
3. Sequence Diagram (dynamic behavior)
4. Deployment Diagram (infrastructure)
5. Use Case Diagram (business view)

---

## Summary

UML provides a comprehensive set of 14 diagram types for modeling software systems:

- **Structural Diagrams**: Show the static structure of the system
- **Behavioral Diagrams**: Show the dynamic behavior of the system

**Key Diagrams for System Design:**
1. Context Diagram - System boundaries and external interactions
2. Container Diagram - Deployable components and their interactions
3. Sequence Diagram - Temporal interactions and message flow
4. Deployment Diagram - Infrastructure and scalability
5. Use Case Diagram - Business capabilities and user interactions

**Tools:**
- PlantUML for text-based diagram creation
- Various visual tools for interactive diagramming

Understanding and using UML diagrams effectively helps create better-designed systems, improve communication between team members, and document system architecture comprehensively.
