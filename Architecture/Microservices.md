# Microservices: Architecture Guide

## Table of Contents
1. [Introduction](#introduction)
2. [Core Principles](#core-principles)
3. [Essential Processes](#essential-processes)
4. [Service Mesh](#service-mesh)
5. [Database Strategy](#database-strategy)
6. [Service Communication](#service-communication)
7. [API Gateway](#api-gateway)
8. [Deployment and Infrastructure](#deployment-and-infrastructure)
9. [Development Tools](#development-tools)
10. [Key Concepts](#key-concepts)
11. [Resources](#resources)

---

## Introduction

**Microservices architecture** is an approach to building applications as a collection of small, independent services that communicate over well-defined APIs. Each service is responsible for a specific business capability and can be developed, deployed, and scaled independently.

**Key Benefits:**
- Independent deployment and scaling
- Technology diversity (polyglot)
- Fault isolation
- Team autonomy
- Faster development cycles

**Resources:**
- [HTTP Security](https://m.habr.com/ru/post/503284/)
- [Single sign-on, Authorization](https://habr.com/en/company/dataart/blog/262817)
- [Microservices Article](https://habr.com/en/post/438186/)

---

## Core Principles

### 1. Unified Communication Protocol

**Principle:**
Define a single protocol that all microservices will use to communicate.

**Why It Matters:**
- Ensures consistency across services
- Simplifies integration
- Reduces complexity
- Makes debugging easier

**Common Choices:**
- REST API
- gRPC
- Message queues (AMQP, MQTT, STOMP)

### 2. Standardized Service Template

**Principle:**
Create a unified microservice template for different languages (Python, Go, PHP).

**Benefits:**
- Consistent structure across services
- Faster onboarding
- Easier maintenance
- Standardized patterns

**Components:**
- Project structure
- Configuration management
- Logging standards
- Error handling
- Testing setup

### 3. Client-Based Communication

**Principle:**
All microservices communicate only through clients (libraries), not direct API calls via curl.

**Why:**
- Type safety
- Better error handling
- Easier testing
- Version management
- Abstraction of implementation details

**Service Maker Pattern:**
Service Maker creates a client library for its service, so other services don't work directly with the API through curl requests.

---

## Essential Processes

To make microservices architecture effective, you need to establish many processes:

### Observability

**Logging:**
- Tools: Fluentd, Elasticsearch
- Centralized log aggregation
- Structured logging
- Log analysis and search

**Distributed Tracing:**
- Tool: Jaeger
- Request tracing across services
- Performance monitoring
- Dependency mapping

**Error Aggregation:**
- Tool: Sentry
- Centralized error tracking
- Error alerting
- Error analytics

**Monitoring:**
- Tool: Grafana
- Service health monitoring
- Performance metrics
- Dashboards and alerts

### Infrastructure

**Event Stream Processing:**
- Status, messages, events from Kubernetes
- Real-time event processing
- Event-driven architecture

**Service Mesh:**
- Tool: Netramesh (custom solution)
- Service connectivity control
- Traffic management
- Security policies

**Rate Limiting / Circuit Breaker:**
- Tool: Hystrix (example)
- Prevents cascading failures
- Protects services from overload
- Resilience patterns

### Development and Operations

**Continuous Integration:**
- Tools: TeamCity, Jenkins
- Automated builds
- Automated testing
- Intelligent CI server

**Communication and Notifications:**
- Tools: Slack, email
- Team communication
- Alert notifications
- Status updates

**Task Tracking:**
- Tool: Jira
- Issue management
- Project tracking
- Workflow management

**Documentation:**
- API documentation
- Service documentation
- Architecture documentation
- Tools: Sphinx, Swagger, RAML

---

## Service Mesh

### What is a Service Mesh?

**Definition:**
A service mesh is a dedicated infrastructure layer for handling service-to-service communication in a microservices architecture. It helps manage and facilitate communication between microservices by providing a set of networking services that include discovery, load balancing, encryption, authentication, and authorization.

**Core Idea:**
The main idea of the service mesh approach is to add another infrastructure layer on top of the network that allows us to do anything with inter-service interaction.

**How It Works:**
Most implementations work as follows: an additional sidecar container with a transparent proxy is added to each microservice, through which all incoming and outgoing traffic of the service passes. This is the place where we can:
- Perform client-side load balancing
- Apply security policies
- Introduce rate limiting
- Collect important information about service interactions in production

### Key Features

1. **Service Discovery:**
   - Automatically locates and registers services within the mesh
   - Enables dynamic service discovery as new services are added or removed

2. **Load Balancing:**
   - Distributes incoming traffic across multiple instances of a service
   - Ensures optimal resource utilization and high availability

3. **Traffic Management:**
   - Enables fine-grained control over how traffic is routed between services
   - Supports canary releases, A/B testing, and blue-green deployments

4. **Security:**
   - Provides encryption, authentication, and authorization mechanisms
   - Secures communication between services
   - Important in microservices where communication occurs over untrusted networks

5. **Observability:**
   - Offers tools for monitoring and logging
   - Provides insights into performance and health of services

6. **Fault Tolerance:**
   - Implements circuit breaking and retry logic
   - Handles and recovers from failures gracefully
   - Improves overall system resilience

### Service Mesh Solutions

**Popular Implementations:**
- **Istio**: Open-source service mesh platform, integrates with Kubernetes
- **Linkerd**: Lightweight service mesh
- **Consul Connect**: Service mesh for Consul
- **Netramesh**: Custom solution (Open Source), more resource-efficient than Istio

**Comparison:**
Initially, Istio was tried, but it uses too many resources, which becomes expensive at scale. Netramesh was developed as a custom solution that:
- Consumes several times fewer resources than Istio
- Doesn't do everything Istio can do
- Is used in production
- Available as Open Source

### Three-Layer Infrastructure

**A. Upper Layer - Service Mesh:**
- Service-to-service communication
- Traffic management
- Security policies

**B. Middle Layer - Kubernetes:**
- Deployment and operation of microservices
- Container orchestration
- Resource management

**C. Lower Layer - Bare Metal:**
- Physical infrastructure
- No cloud or OpenStack
- Direct hardware control

---

## Database Strategy

### Database per Service

**Core Principle:**
Each microservice application owns its database. No other services are allowed to connect to the database. Other services use only service API + events.

**Why:**
- Data isolation
- Independent scaling
- Technology diversity
- Loose coupling

**Implementation:**
All microservices have their own database. Accordingly, any access to data can only be obtained through the application API.

### Polyglot Persistence

**Concept:**
In practice, it may be useful to use an approach called [Polyglot Persistence](http://martinfowler.com/bliki/PolyglotPersistence.html) - choosing storage that is most suitable for the tasks of a specific service.

**Example:**
In a project, MongoDB was used as the main database for each service for simplicity. However, different services might benefit from:
- Relational databases (PostgreSQL, MySQL)
- Document databases (MongoDB)
- Key-value stores (Redis)
- Time-series databases (InfluxDB)
- Graph databases (Neo4j)

**Benefits:**
- Choose the best tool for each service
- Optimize for specific use cases
- Better performance
- Cost efficiency

### NoSQL Considerations

**Disadvantages:**
- Not good for too many updates
- ACID isn't guaranteed (can't use critical data, e.g., transactions with such databases)
- Joins are hard

**When to Use:**
- High read/write throughput
- Flexible schema requirements
- Horizontal scaling needs
- Document or key-value data models

---

## Service Communication

### Communication Patterns

**Synchronous Communication:**
- REST API requests
- gRPC calls
- Direct service-to-service calls

**Asynchronous Communication:**
- Message queues (AMQP, MQTT, STOMP)
- Event streams
- Pub/sub patterns

**Best Practice:**
Common practice is to combine different interaction methods:
- **Synchronous GET requests** for retrieving information
- **Asynchronous requests** using a queue server for create/update operations

**Eventual Consistency:**
This approach moves us into the world of [eventual consistency](http://martinfowler.com/articles/microservice-trade-offs.html#consistency) - one of the important aspects of distributed systems that we have to live with.

### Service Discovery

**What It Is:**
Service discovery allows automatic determination of network addresses for available application instances that can dynamically change due to scaling, failures, and updates.

**Key Component:**
The key component here is the Registry service.

**Examples:**
- **Netflix Eureka** (client-side discovery pattern)
- **Consul**
- **Zookeeper**
- **Etcd**

**Client-Side Discovery:**
Eureka is an example of a client-side discovery pattern, meaning the client must request addresses of available instances and perform load balancing between them independently.

### API Types

**REST API vs Thrift API:**
- REST: HTTP-based, JSON/XML, widely supported
- Thrift: Binary protocol, more efficient, requires code generation

**Protocols:**
- AMQP (Advanced Message Queuing Protocol)
- MQTT (Message Queuing Telemetry Transport)
- STOMP (Simple Text Oriented Messaging Protocol)

---

## API Gateway

### What is an API Gateway?

**Definition:**
Traditionally, an additional layer is proposed before a collection of microservices - the so-called API Gateway, which solves several problems at once. API Gateway is a smart proxy server between users and any number of services (APIs), hence the name.

### Why API Gateway is Needed

The need for this layer appears immediately when transitioning to the microservices pattern:

1. **Single Address:**
   - Much more convenient than hundreds (Netflix has more than 600) of individual API addresses

2. **Centralized Authentication:**
   - Logical to check user data (token) in a single place, at the "entrance"

3. **Rate Limiting:**
   - Convenient to implement request limits in a single place

4. **Flexibility:**
   - The entire system becomes more flexible - you can change the internal structure every day
   - Supporting old API versions becomes trivial

5. **Response Manipulation:**
   - Can cache or mutate responses

6. **Response Aggregation:**
   - For user convenience (or front-end developers), can combine responses from different services
   - Facebook has long offered this capability

### API Gateway Responsibilities

**Core Functions:**
- Request routing
- Request composition
- Protocol translation
- Load balancing
- Caching
- Access control
- API metering
- Monitoring

**Implementation:**
The API Gateway is responsible for tasks such as load balancing, caching, access control, API metering, and monitoring, and can be implemented effectively using NGINX.

**How It Works:**
- All requests from clients first go through the API Gateway
- It then routes requests to the appropriate microservice
- The API Gateway often handles a request by invoking multiple microservices and aggregating the results
- It can translate between web protocols such as HTTP and WebSocket and web-unfriendly protocols that are used internally

### API Gateway Models

**Proxy Model:**
- Uses NGINX as an API Gateway
- Single entry point
- Simple routing

**Router Mesh Model:**
- Uses an additional NGINX server as a hub for inter-process communication
- Centralized routing
- Better for complex routing

**Fabric Model:**
- Uses one NGINX server per microservice
- Controls HTTP traffic
- Optionally implements SSL/TLS between microservices
- Breakthrough capability for security

### API Gateway Solutions

- **API Umbrella** (Lua)
- **Kong** (Lua)
- **AWS API Gateway** (paid service from Amazon)
- **NGINX** (custom implementation)

### Authentication in API Gateway

**Common Pattern:**
Yes, there is one common user database, based on which OAuth2 tokens are issued. In each request to a microservice, there is an X-User header with the user identifier, meaning the microservice always knows that any request to it has passed authentication. The microservice remains to do authorization based on any of its internal logic. This is the traditional way for this architecture - this is what books recommend.

---

## Deployment and Infrastructure

### Container Orchestration

**Kubernetes:**
- Deployment and operation of microservices
- Infrastructure layer separation
- Monitoring everything that happens with microservices

**Helm:**
- Kubernetes package manager
- Helm charts for application deployment
- Configuration management

**Hazelcast:**
- In-memory data grid
- Distributed caching
- Clustering

### Deployment Models

**Service Instance Per Host:**
- One service instance per host/container
- Isolation
- Independent scaling

**Blue-Green Deployment:**
- Zero-downtime deployments
- Two identical production environments
- Switch traffic between environments

### Load Balancing

**Purpose:**
A load balancer distributes requests across available instances.

**Routing:**
The primary key of a row or identity of a customer is used to route the request to a particular server.

**Consistent Hashing:**
- Hashing requests from clients and sending them to the assigned server
- Formula: `Hash(request) -> m%n` where m is hash, n is number of servers, m%n is remainder
- K hash functions: take server id and hash it 1 or k times, then assign servers on clock diagram
- One server will serve at k places

### Platform as a Service (PaaS)

**Definition:**
A PaaS provides developers with an easy way to deploy and manage their microservices.

**Custom PaaS:**
Another way to automate the deployment of microservices is to develop what is essentially your own PaaS. One typical starting point is to use a clustering solution, such as Kubernetes, in conjunction with a container technology such as Docker.

**Serverless:**
AWS Lambda is a convenient way to deploy microservices.

**Cluster Managers:**
You might use a cluster manager such as Kubernetes or Marathon to manage your containers.

### Architecture Tiers

**Four-Tier Architecture vs Three-Tier Architecture:**
- Traditional three-tier: Presentation, Application, Data
- Four-tier adds: Service layer or API Gateway layer

---

## Development Tools

### API Development

**Postman:**
- API testing and development
- Request/response testing
- Collection management

**RAML and Swagger:**
- API definition languages
- API documentation
- Code generation

**API Definition:**
API definition is aimed at machine consumption instead of human consumption of APIs. API definitions can also be imported into a mock server for virtual API testing.

### Testing

**Mock Objects:**
When testing code, the mock object approach is often used. These are objects that have the same interface as the components being used, but their behavior is completely defined in the test, and their use allows avoiding the full infrastructure setup required for the application to run.

**Fault Injection Testing:**
- Testing system resilience
- Simulating failures
- Validating error handling

**Hoverfly.io:**
- Service virtualization
- API mocking
- Testing tool

### Version Control

**Git Tag:**
- Version marking
- Release management

**Git Flow:**
- Branching strategy
- Feature development workflow
- Release management

**Bitbucket/GitFlow:**
- Repository hosting
- Workflow management

### Documentation

**Sphinx:**
- Documentation generation
- API documentation
- Technical documentation

**Event Schema:**
- Event definition
- Schema management
- Last alert tools

### Configuration

**app.toml:**
- Application configuration
- TOML format
- Service settings

**Polyglot:**
- Multiple programming languages
- Technology diversity
- Service-specific languages

---

## Key Concepts

### Inter-Process Communication (IPC)

**Definition:**
IPC: Inter Process Communications

**IDL (Interface Definition Language):**
- Defines service interfaces
- Language-agnostic contracts
- Code generation

**Communication Types:**
Each service has a well-defined boundary in the form of a remote procedure call (RPC)-driven or message-driven API.

### Event-Driven Architecture

**Event Log:**
- Events of interest
- Event sourcing
- Audit trail

**API vs Event Streams:**
- Synchronous: API calls
- Asynchronous: Event streams
- Choose based on use case

### Caching Strategies

**Write-Through:**
- If any update from user, update cache first, then database
- Ensures cache consistency
- Slower writes

**Write-Back:**
- Update cache after database is updated
- This mechanism is used for critical data
- Faster writes, risk of data loss

### Data Storage

**CLOB (Character Large Object):**
- Large text data storage
- Database field type

**BLOB (Binary Large Object):**
- Binary data storage
- Images can be stored as FILE or BLOB
- Efficient binary storage

### Protocols

**XMPP Protocol:**
- Extensible Messaging and Presence Protocol
- Real-time messaging
- Chat applications

**WebSockets:**
- For chatting applications
- Real-time bidirectional communication
- Persistent connections

**WebSocket Connection:**
To establish a WebSocket connection, the client and server upgrade from the HTTP protocol to the WebSocket protocol during their initial handshake. WebSockets sent to the scrap heap of history Comet and all add-ons built on top of it - Bayeux, LongPolling, MultiPart, and so on.

**Resources:**
- [WebSocket Quantum](http://websocket.org/quantum.html)
- Kaazing WebSocket Gateway

### Upstream/Downstream

**Definition:**
Upstream and downstream describe the flow of a message: all messages flow from upstream to downstream.

**In Practice:**
- If you are looking at a request, then the client is upstream, and the server is downstream
- In contrast, if you are looking at a response, then the client is downstream, and the server is upstream

**Example:**
This means all requests for `/` go to any of the servers listed under upstream XXX, with a preference for port 8000.

### Single Point of Failure (SPOF)

**Definition:**
A single point of failure (SPOF) in computing is a critical point in the system whose failure can take down the entire system. A lot of resources and time is spent on removing single points of failure in an architecture/design.

**Mitigation:**
- Redundancy
- Load balancing
- Failover mechanisms
- Distributed systems

### Database Index

**Definition:**
A database index is a data structure that improves the speed of data retrieval operations on a database table at the cost of additional writes and storage space to maintain the index data structure.

**Trade-offs:**
- Faster reads
- Slower writes
- Additional storage

### Terminology

**Latency:**
- Задержка (delay)
- Time between request and response
- Critical for performance

**Retention Policy:**
- Политика хранения (storage policy)
- Data retention rules
- Lifecycle management

**I/O Calls:**
- Input/Output operations
- Performance bottleneck
- Optimization target

### Two-Phase Commit (2PC)

**Definition:**
2PC - two-phase commit protocol

**Purpose:**
- Distributed transaction coordination
- ACID guarantees across services
- Complex and can cause blocking

**Alternatives:**
- Saga pattern
- Eventual consistency
- Compensating transactions

### Technology Stack Examples

**Elixir/Phoenix/Postgres:**
- Functional programming
- Real-time applications
- High concurrency

**Angular/Hapi/MongoDB:**
- Frontend: Angular
- Backend: Hapi.js
- Database: MongoDB

**Language Selection:**
- **Web 2.0 sites**: Python is good (frameworks, ORMs, templating engines, lots of experience)
- **Network, highly concurrent, or low-latency daemons**: Erlang
- **Computational tasks**: Go works well, also for concurrent network things, gradually catching up to Erlang. May find application in online games if garbage collector doesn't become an obstacle.

---

## ASGI and Python Web Frameworks

### ASGI (Asynchronous Server Gateway Interface)

**Definition:**
ASGI (Asynchronous Server Gateway Interface) is a spiritual successor to WSGI, intended to provide a standard interface between async-capable Python web servers, frameworks, and applications.

**Comparison:**
Where WSGI provided a standard for synchronous Python apps, ASGI provides one for both asynchronous and synchronous apps, with a WSGI backwards-compatibility implementation and multiple servers and application frameworks.

**WSGI:**
WSGI (Web Server Gateway Interface) is a standard for interaction between a Python program running on the server side and the web server itself, for example Apache.

**High-Level View:**
At a very high level, ASGI is an interface for communication between applications and servers. But in reality, everything is a bit more complicated.

**Model:**
ASGI relies on a simple model: when a client connects to the server, an application instance is created. Then incoming data is passed to the application and all data that it returns is sent back.

**Visual Reference:**
![[architecture-3.png]]

**Function Signature:**
The signature of this function is exactly what the "I" in "ASGI" means: the interface that an application must implement so that the server can call it.

**Parameters:**
- **scope**: A dictionary containing information about the incoming request. Its content differs for [HTTP](https://asgi.readthedocs.io/en/latest/specs/www.html#connection-scope) and [WebSocket](https://asgi.readthedocs.io/en/latest/specs/www.html#id1) connections
- **receive**: An asynchronous function for receiving ASGI event messages
- **send**: An asynchronous function for sending ASGI event messages

### Middleware

**Definition:**
For example, we can create ASGI middleware (i.e., an application that wraps another application) to display the time it took to execute a request.

**Django Middleware:**
Middleware allows processing requests from the browser before they reach Django views, as well as responses from views before they return to the browser.

**Processing Order:**
Middleware is applied in the same order it is added to the list in Django settings. When the browser sends a request, it is processed like this:

```
Browser -> M_1 -> M_2 -> ... -> M_N -> View
```

The view receives the request, performs some operations, and returns a response. On the way back to the browser, the response passes through each middleware again, but in reverse order:

```
Browser <- M_1 <- M_2 <- ... <- M_N <- View
```

**Implementation:**
There are 2 ways to create middleware: as a function and as a class.

**Resources:**
- [Django Middleware Guide](https://webdevblog.ru/nachalo-raboty-s-middleware-v-django/)

### ASGI Benefits

**Speed:**
The asynchronous nature of ASGI applications and servers makes them [really fast](https://www.techempower.com/benchmarks/#section=data-r18&hw=ph&test=fortune&l=zijzen-f&w=zik0zh-zik0zj-e7&d=b) (at least for Python) - we're talking about 60k-70k requests per second (assuming Flask and Django achieve only 10-20k in a similar situation).

**Capabilities:**
ASGI servers and platforms provide access to essentially parallel functions (WebSocket, Server-Sent Events, HTTP/2) that cannot be implemented with synchronous code and WSGI.

### ASGI Frameworks

**Starlette:**
- Oriented towards modern GraphQL
- ASGI framework
- High performance

**FastAPI:**
- Focuses on REST API services
- Swagger documentation for methods
- Modern Python web framework

**Comparison:**
Starlette is oriented towards modern GraphQL, while FastAPI cares about those who build REST.

### GraphQL

**Definition:**
GraphQL is a syntax that describes how to request data, and is mainly used by the client to load data from the server.

**Three Main Characteristics:**
- Allows the client to specify exactly what data it needs
- Facilitates data aggregation from multiple sources
- Uses a type system to describe data

**Facebook's Solution:**
Facebook came up with a conceptually simple solution: instead of having many "dumb" endpoints, it's better to have one "smart" endpoint that can work with complex queries and give data the form the client requests.

**Building Blocks:**
In practice, a GraphQL API is built on three main building blocks:
- **Schema** (schema)
- **Queries** (queries)
- **Resolvers** (resolvers)

---

## API Management

### What is API Management?

**Purpose:**
In short, we can say that the purpose of an API Management system is to turn an API into a product. That is, to endow a purely technical entity with real business content.

**Capabilities:**
Most API Management systems can do the following:
- Organize access to APIs
- Define API usage policies (request completeness, frequency, and limit per unit of time)
- Provide API documentation tools
- Organize paid access to APIs with policy consideration

---

## Webhooks

### What is a Webhook?

**Definition:**
A webhook is a mechanism for notifying about events occurring in the system through callback functions.

**How It Works:**
In typical APIs, to get information from the server, you need to constantly poll it. In the case of webhooks, the server says to the client: "Now you won't have to call. I'll call you when something new appears."

**Process:**
Using a webhook, the client "subscribes" to receive notifications in predefined cases. This approach is more convenient for both the information provider and the client. Webhooks use HTTP, so no additional infrastructure is required.

**Implementation:**
A webhook makes an HTTP request to your application (most often a [POST](https://ru.wikipedia.org/wiki/POST_\(HTTP\)) request in [JSON](https://ru.wikipedia.org/wiki/JSON) format), and the interpretation of the request depends on the receiving side. Webhooks are a kind of "reverse APIs" - the interface for interacting with the request must be built on the client side.

**Resources:**
- [Habr: Webhooks Article 1](https://habr.com/en/post/482936/)
- [Habr: Webhooks Article 2](https://habr.com/en/post/454440/)
- [Habr: Webhooks Article 3](https://habr.com/ru/post/478620/)
- [Habr: Webhooks Article 4](https://habr.com/en/post/326986/)

---

## Microservice Structure

### Standard Structure

A typical microservice has the following structure:

| Directory/File        | Description                        |
| --------------------- | ---------------------------------- |
| **app/**              |                                    |
| ├── Backs             | Backend controllers                |
| ├── Class             |                                    |
| ├── Components        |                                    |
| ├── Constants         | Constants                          |
| ├── Controllers       | Controllers                        |
| ├── Errors            | Common errors                      |
| ├── GqlSchemas        |                                    |
| ├── Helpers           | Helper functions                   |
| ├── Libs              | Library connections                |
| ├── Middleware        | Middleware                         |
| ├── Routes            |                                    |
| ├── Schemas           |                                    |
| ├── Tasks             | Delayed task execution             |
| └── App.js            | Application initialization         |
| **docs/**             |                                    |
| ├── .gitkeep          |                                    |
| └── DOCS.md           |                                    |
| **file/**             |                                    |
| └── storage            |                                    |
| **tests/**             |                                    |
| **translations/**      |                                    |
| └── locales            |                                    |
| **.dockerignore**      | Needed for image optimization      |
| **.eslintrc**          |                                    |
| **.foreverignore**     |                                    |
| **.gitignore**         |                                    |
| **.gitlab-ci.yml**     |                                    |
| **.npmrc**             |                                    |
| **.nvmrc**             |                                    |
| **Dockerfile**         |                                    |
| **README.md**          |                                    |
| **app.js**             |                                    |
| **healthcheck.js**     |                                    |
| **i18next-parser.config.js** |                                    |
| **package-lock.json** |                                    |
| **package.json**       |                                    |

---

## Continuous Integration

### What is CI?

**Definition:**
Continuous Integration (CI) consists of the following: all changes made to the code are merged in a central repository (the operation is called "merging"). Merging happens several times a day, and after each merge in a specific project, automatic build and testing are triggered.

**Auto Deploy:**
Auto deploy on new commit on master.

**Benefits:**
- Early error detection
- Faster feedback
- Consistent builds
- Automated testing

---

## Code Quality

### Linters

**Purpose:**
Linters help bring code to a unified style and avoid errors.

**Example:**
Pylint (for Python)

**Main Task:**
The most important task of linters is to bring code to uniformity.

**Benefits:**
- Consistent code style
- Error prevention
- Best practices enforcement
- Team collaboration

---

## Python Codecs and Tokenization

**Codecs System:**
Remember what you need to do so that Python code is perceived as UTF-8? Well, this is not magic, it's a large, well-thought-out system. It's called "codecs."

**Tokenization:**
Thanks to the presence of such a system, we can get access to Python's TOKENIZER, to the code reader! We can control how code is understood by the compiler in pyc and the interpreter!

**Use Cases:**
- Custom encoding/decoding
- Code analysis
- Syntax processing
- Language implementation

---

## Resources

### Articles and Guides

- [HTTP Security](https://m.habr.com/ru/post/503284/)
- [Single sign-on, Authorization](https://habr.com/en/company/dataart/blog/262817)
- [Microservices Article](https://habr.com/en/post/438186/)
- [Microservices Implementation](https://habr.com/en/post/280786/)
- [CNCF Landscape](https://landscape.cncf.io)
- [Avito Microservices Blog 1](https://m.habr.com/en/company/avito/blog/454780/)
- [Avito Microservices Blog 2](https://habr.com/ru/company/avito/blog/449974/?_ga=2.150031039.800850705.1570780524-1883241389.1564903935)
- [Yandex Money Microservices](https://habr.com/en/company/yamoney/blog/328092/)
- [Starlette Mock Service](https://www.mattlayman.com/blog/2019/starlette-mock-service/)
- [Webhooks Articles](https://habr.com/en/post/482936/), [2](https://habr.com/en/post/454440/), [3](https://habr.com/ru/post/478620/), [4](https://habr.com/en/post/326986/)

### Concepts

- [Polyglot Persistence](http://martinfowler.com/bliki/PolyglotPersistence.html)
- [Eventual Consistency](http://martinfowler.com/articles/microservice-trade-offs.html#consistency)

---

## Summary

Microservices architecture provides a powerful approach to building scalable, maintainable applications:

- **Core Principles**: Unified protocols, standardized templates, client-based communication
- **Essential Processes**: Observability, infrastructure, CI/CD, documentation
- **Service Mesh**: Manages service-to-service communication
- **Database Strategy**: Database per service, polyglot persistence
- **API Gateway**: Single entry point, routing, authentication
- **Deployment**: Kubernetes, containers, orchestration
- **Development Tools**: Testing, documentation, version control

**Key Takeaways:**
- Each service owns its database
- Services communicate through APIs and events
- Service mesh provides infrastructure layer
- API Gateway simplifies client interactions
- Observability is crucial for microservices
- Choose tools based on scale and requirements

Understanding these concepts and patterns is essential for successfully implementing and operating microservices architectures.
