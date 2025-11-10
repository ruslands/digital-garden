# Microservice Patterns: Design Patterns Guide

## Table of Contents
1. [Introduction](#introduction)
2. [Decomposition Patterns](#decomposition-patterns)
3. [Refactoring Patterns](#refactoring-patterns)
4. [Data Management Patterns](#data-management-patterns)
5. [Communication Patterns](#communication-patterns)
6. [UI Composition Patterns](#ui-composition-patterns)
7. [Service Discovery Patterns](#service-discovery-patterns)
8. [Deployment Patterns](#deployment-patterns)
9. [Reliability Patterns](#reliability-patterns)
10. [Observability Patterns](#observability-patterns)
11. [Other Patterns](#other-patterns)
12. [Resources](#resources)

---

## Introduction

**Microservices design patterns** exist to combat microservice issues. They provide proven solutions that make your architecture efficient while making management less costly and troublesome.

**Total Patterns:** 52 patterns

**Purpose:**
- Solve common microservices challenges
- Provide proven architectural solutions
- Improve system efficiency
- Reduce management complexity

**Resources:**
- [26 Main Microservice Patterns (Mail.ru)](https://mcs.mail.ru/blog/26-osnovnyh-patternov-mikroservisnoj-razrabotki)
- [Design Patterns in Microservices (TSH.io)](https://tsh.io/blog/design-patterns-in-microservices-api-gateway-bff-and-more/)
- [Microservices.io Patterns](https://microservices.io/patterns/)

---

## Decomposition Patterns

**Purpose:**
Solutions for decomposition, that is, dividing applications into microservices.

### Decompose By Business Capability

**What It Is:**
Organize services around business capabilities rather than technical layers.

**Benefits:**
- Aligns with business structure
- Clear ownership
- Better domain understanding
- Independent development

**Example:**
- Order Management Service
- Payment Service
- Shipping Service
- Customer Service

### Decompose By Subdomain

**What It Is:**
Use Domain-Driven Design (DDD) to identify bounded contexts and decompose by subdomain.

**Benefits:**
- Domain-driven approach
- Clear boundaries
- Better modeling
- Aligns with DDD principles

**Example:**
- Core Domain: Order Processing
- Supporting Domain: Notification
- Generic Domain: Authentication

---

## Refactoring Patterns

**Purpose:**
Solutions for organizing interaction with Legacy applications and/or their gradual transition to microservices architecture.

### Strangler (Strangler Fig)

**What It Is:**
Gradually replace a legacy system by building new services around it, eventually "strangling" the old system.

**How It Works:**
1. Identify a portion of the legacy system to replace
2. Build a new microservice for that functionality
3. Route traffic to the new service
4. Repeat until the legacy system is fully replaced

**Benefits:**
- Low-risk migration
- Gradual transition
- No big-bang rewrite
- Continuous delivery

**Use Case:**
Large monolithic applications that need to be migrated to microservices.

### Anti-Corruption Layer

**What It Is:**
A layer that translates between different domain models, protecting your new microservices from legacy system's domain model.

**Purpose:**
- Isolate new services from legacy systems
- Translate between different models
- Prevent legacy concepts from corrupting new design

**Benefits:**
- Clean boundaries
- Independent evolution
- Protection from legacy issues

---

## Data Management Patterns

**Purpose:**
Solutions for how microservices interact with databases.

### Database per Service

**What It Is:**
Each service has its own private database.

**Benefits:**
- Data isolation
- Independent scaling
- Technology diversity
- Loose coupling

**Principle:**
No other services are allowed to connect to the database. Other services use only service API + events.

### Shared Database

**What It Is:**
Services share a database (anti-pattern in microservices).

**When to Avoid:**
- Creates tight coupling
- Prevents independent scaling
- Makes deployment complex
- Violates microservices principles

**Note:**
Generally considered an anti-pattern, but may be acceptable in specific scenarios during migration.

### Command-Side Replica

**What It Is:**
Maintain a queryable replica of data in a service that implements a command.

**Purpose:**
- Separate read and write operations
- Optimize for different access patterns
- Improve performance

**Use Case:**
Services that need to both modify data (command) and query it efficiently.

### API Composition

**What It Is:**
Implement queries by invoking the services that own the data and performing an in-memory join.

**How It Works:**
1. API Gateway or composite service receives query
2. Invokes multiple services to get data
3. Performs in-memory join/aggregation
4. Returns combined result

**Benefits:**
- No data duplication
- Services own their data
- Flexible composition

**Drawbacks:**
- Multiple service calls
- Potential performance issues
- Network overhead

### CQRS (Command Query Responsibility Segregation)

**What It Is:**
Implement queries by maintaining one or more materialized views that can be efficiently queried.

**Core Principle:**
Define a view database, which is a read-only replica that is designed to support that query. The application keeps the replica up to date by subscribing to Domain events published by the service that owns the data.

**Foundation:**
This approach is based on the Command-query separation (CQS) principle.

**CQS Principle:**
The main idea of CQS is that in an object, methods can be of two types:
- **Queries**: Methods return a result without changing the object's state. In other words, Query has no side effects.
- **Commands**: Methods change the object's state without returning a value

**CQS Benefits:**
In CQS, Query has one feature. Since Query doesn't change the object's state, methods of the Query type can be well parallelized, distributing the load that falls on read operations.

**Implementation:**
To change the system state, a Command class is created, and to select data, a Query class is created.

**Visual Reference:**
![[architecture-2.png]]

**Resources:**
- [Habr: CQRS Article](https://habr.com/ru/companies/simbirsoft/articles/329970/)

### Domain Event

**What It Is:**
Publish an event whenever data changes.

**Purpose:**
- Notify other services of changes
- Enable eventual consistency
- Decouple services
- Support event-driven architecture

**Benefits:**
- Loose coupling
- Scalability
- Flexibility
- Event sourcing support

### Event Sourcing

**What It Is:**
Persist aggregates as a sequence of events.

**How It Works:**
- Store all changes as events
- Reconstruct state by replaying events
- Event log is the source of truth

**Benefits:**
- Complete audit trail
- Time travel debugging
- Event replay
- Natural event publishing

**Use Cases:**
- Financial systems
- Audit requirements
- Complex business logic
- Event-driven architectures

### Saga

**What It Is:**
Use sagas, which are sequences of local transactions, to maintain data consistency across services.

**Definition:**
A saga represents a set of local transactions. Each local transaction updates the database and publishes a message or event, initiating the next local transaction in the saga. If a transaction fails, for example, due to business rule violations, the saga launches compensating transactions that roll back changes made by preceding local transactions.

**Two Coordination Methods:**

**1. Choreography:**
- Each local transaction publishes domain events that trigger local transactions in other services
- Decentralized coordination
- Services react to events independently

**2. Orchestration:**
- An orchestrator (object) tells the participants what local transactions to execute
- Centralized coordination
- Orchestrator manages the flow

**Comparison:**
Saga is contrasted with 2PC (two-phase commit protocol).

**Resources:**
- [Habr: Saga Pattern Article](https://habr.com/ru/post/427705/)

**2PC (Two-Phase Commit):**
- Distributed transaction protocol
- Can cause blocking
- Not suitable for microservices
- Saga is preferred alternative

---

## Communication Patterns

**Purpose:**
Solutions for external interactions of microservices: with client applications, remote services, etc.

### Direct Communication

**What It Is:**
Services communicate directly with each other.

**Characteristics:**
- Simple implementation
- Direct service-to-service calls
- Can create tight coupling
- May not scale well

### API Gateway

**What It Is:**
A single entry point for all client requests, routing them to appropriate microservices.

**Responsibilities:**
- Request routing
- Protocol translation
- Authentication/authorization
- Rate limiting
- Response aggregation

**Benefits:**
- Single entry point
- Centralized concerns
- Client simplification
- Version management

### Backends for Frontends (BFF)

**What It Is:**
Create separate backend services for different client types (web, mobile, desktop).

**Purpose:**
- Optimize for specific client needs
- Reduce over-fetching
- Improve performance
- Better user experience

**Example:**
- Web BFF: Optimized for web browsers
- Mobile BFF: Optimized for mobile apps
- Admin BFF: Optimized for admin interfaces

---

## UI Composition Patterns

**Purpose:**
Solutions for displaying data from multiple microservices on a single page or screen of the user interface.

### Client-Side UI Composition

**What It Is:**
The client (browser/mobile app) fetches data from multiple services and composes the UI.

**How It Works:**
- Client makes multiple API calls
- Client assembles the UI
- Client handles composition logic

**Benefits:**
- Flexible composition
- Client control
- No server-side rendering needed

**Drawbacks:**
- Multiple round trips
- Client complexity
- Potential performance issues

### Server-Side Page Fragment Composition

**What It Is:**
Server-side composition of page fragments from multiple services.

**How It Works:**
- Server fetches data from multiple services
- Server composes HTML fragments
- Server returns complete page

**Benefits:**
- Single request from client
- Server-side optimization
- Better for SEO
- Reduced client complexity

---

## Service Discovery Patterns

**Purpose:**
Solutions that client applications can use to determine the location of services they need. This is especially important in microservices applications, as they work in virtualized and containerized environments where the number of service instances and their location change dynamically.

**Key Component:**
The key component of microservice discovery is the Service Registry - a database with information about the location of service instances. When instances start and stop, information in the registry is updated. But you can interact with the service registry in two ways, which formed the basis of the patterns described below.

### Client-Side Service Discovery

**What It Is:**
The client queries the service registry to find available instances and performs load balancing itself.

**How It Works:**
1. Client queries service registry
2. Registry returns list of available instances
3. Client selects instance and makes request
4. Client handles load balancing

**Example:**
Netflix Eureka (client-side discovery pattern)

**Benefits:**
- Client control
- Flexible load balancing
- No additional proxy needed

**Drawbacks:**
- Client complexity
- Coupling to registry
- Client must handle failures

### Server-Side Service Discovery

**What It Is:**
A load balancer queries the service registry and routes requests to available instances.

**How It Works:**
1. Client makes request to load balancer
2. Load balancer queries service registry
3. Load balancer routes to available instance
4. Load balancer handles load balancing

**Benefits:**
- Client simplicity
- Centralized load balancing
- Better failure handling

**Drawbacks:**
- Additional infrastructure
- Single point of failure (if not distributed)
- Less client control

---

## Deployment Patterns

**Purpose:**
Solutions for deploying developed microservices.

### Service Instance Per Host

**What It Is:**
Deploy one service instance per host (virtual machine or container).

**Benefits:**
- Isolation
- Independent scaling
- Resource control
- Fault isolation

**Variations:**
- One instance per VM
- One instance per container
- One instance per process

### Blue-Green Deployment

**What It Is:**
Maintain two identical production environments (blue and green). Switch traffic between them for zero-downtime deployments.

**How It Works:**
1. Deploy new version to inactive environment (green)
2. Test the new version
3. Switch traffic from active (blue) to new (green)
4. Keep old version as backup

**Benefits:**
- Zero downtime
- Quick rollback
- Safe deployments
- Production testing

---

## Reliability Patterns

**Purpose:**
Solutions for improving the reliability of applications with microservices architecture.

### Circuit Breaker

**What It Is:**
Prevent cascading failures by stopping requests to a failing service.

**How It Works:**
1. Monitor service health
2. If failures exceed threshold, open circuit
3. Reject requests immediately (fail fast)
4. Periodically attempt to close circuit
5. If service recovers, close circuit

**States:**
- **Closed**: Normal operation
- **Open**: Failing, reject requests
- **Half-Open**: Testing if service recovered

**Benefits:**
- Prevents cascading failures
- Fast failure detection
- System resilience
- Resource protection

**Tools:**
- Hystrix
- Resilience4j
- Custom implementations

### Bulkhead

**What It Is:**
Isolate resources so that a failure in one part doesn't affect others.

**Analogy:**
Like bulkheads in a ship that prevent water from flooding the entire vessel.

**Implementation:**
- Separate thread pools
- Isolated connection pools
- Resource partitioning
- Failure isolation

**Benefits:**
- Fault isolation
- Prevents resource exhaustion
- Better system stability
- Controlled degradation

---

## Observability Patterns

**Purpose:**
Solutions for building monitoring of microservices operation.

### Log Aggregation

**What It Is:**
Collect and centralize logs from all services.

**Benefits:**
- Centralized logging
- Easier debugging
- Better troubleshooting
- Audit trail

**Tools:**
- Fluentd
- Elasticsearch
- Logstash
- Splunk

### Distributed Tracing

**What It Is:**
Track requests as they flow through multiple services.

**Purpose:**
- Understand request flow
- Identify bottlenecks
- Debug distributed systems
- Performance analysis

**Tools:**
- Jaeger
- Zipkin
- OpenTelemetry
- AWS X-Ray

### Health Check

**What It Is:**
Periodically check if a service is healthy and ready to handle requests.

**Types:**
- **Liveness**: Is the service running?
- **Readiness**: Is the service ready to accept traffic?

**Benefits:**
- Automatic failure detection
- Load balancer integration
- Deployment validation
- System health monitoring

---

## Other Patterns

### Cross-Cutting Concerns

**What It Is:**
Concerns that affect multiple services (logging, security, monitoring).

**Solution:**
- Shared libraries
- Service mesh
- API Gateway
- Middleware

### Microservice Chassis

**What It Is:**
A framework that provides common functionality for microservices.

**Purpose:**
- Standardize service structure
- Provide common features
- Reduce boilerplate
- Ensure consistency

**Components:**
- Logging
- Configuration
- Health checks
- Metrics
- Service discovery

### Service Template

**What It Is:**
A standardized template for creating new microservices.

**Benefits:**
- Consistent structure
- Faster development
- Best practices built-in
- Easier onboarding

### Ambassador

**What It Is:**
A helper service that acts as a proxy for the main service, handling cross-cutting concerns.

**Purpose:**
- Offload common functionality
- Service mesh alternative
- Client-side proxy
- Language-agnostic

### Sidecar

**What It Is:**
A helper container that runs alongside the main service container.

**Purpose:**
- Handle cross-cutting concerns
- Service mesh implementation
- Logging, monitoring, security
- Transparent to main service

### Consumer-Driven Contract Testing

**What It Is:**
Tests that verify a service meets the expectations of its consumers.

**Benefits:**
- Catch breaking changes early
- Document API contracts
- Ensure compatibility
- Consumer confidence

### External Configuration

**What It Is:**
Store configuration outside the application code.

**Benefits:**
- Environment-specific configs
- No code changes for config
- Centralized management
- Security (secrets management)

---

## Resources

### Pattern References

- [26 Main Microservice Patterns (Mail.ru)](https://mcs.mail.ru/blog/26-osnovnyh-patternov-mikroservisnoj-razrabotki)
- [Design Patterns in Microservices (TSH.io)](https://tsh.io/blog/design-patterns-in-microservices-api-gateway-bff-and-more/)
- [Microservices.io Patterns](https://microservices.io/patterns/) - Comprehensive pattern catalog

### Related Articles

- [CQRS Article](https://habr.com/ru/companies/simbirsoft/articles/329970/)
- [Saga Pattern Article](https://habr.com/ru/post/427705/)

---

## Summary

Microservices design patterns provide solutions for common challenges:

**Pattern Categories:**
1. **Decomposition**: How to break down applications
2. **Refactoring**: How to migrate from monoliths
3. **Data Management**: How to handle data in microservices
4. **Communication**: How services interact
5. **UI Composition**: How to build user interfaces
6. **Service Discovery**: How to find services
7. **Deployment**: How to deploy services
8. **Reliability**: How to make systems resilient
9. **Observability**: How to monitor systems

**Key Takeaways:**
- **52 total patterns** address various microservices challenges
- Patterns solve specific problems in distributed systems
- Choose patterns based on your requirements
- Combine patterns for comprehensive solutions
- Patterns evolve with technology and best practices

Understanding and applying these patterns helps build efficient, maintainable, and scalable microservices architectures.
