# Software Architecture: Fundamentals and Patterns

## Table of Contents
1. [Introduction](#introduction)
2. [Architecture Patterns](#architecture-patterns)
3. [Design Methodologies](#design-methodologies)
4. [Communication Patterns](#communication-patterns)
5. [Architecture Frameworks and Languages](#architecture-frameworks-and-languages)
6. [Modeling Tools and Notations](#modeling-tools-and-notations)
7. [Architect Roles](#architect-roles)
8. [Resources](#resources)

---

## Introduction

**Visual Reference:**
![[architecture-1.png]]

Software architecture patterns provide proven solutions for organizing and structuring software systems. Understanding these patterns helps architects make informed decisions about system design.

**Key Concepts:**
- Architecture patterns define high-level structure
- Design patterns solve specific design problems
- Frameworks provide structured approaches
- Tools support visualization and documentation

---

## Architecture Patterns

### Layered Architecture

**Definition:**
Layered architecture is classified into four distinct layers: presentation, business, persistence, and database. A request must pass through the layer right below it to go to the next layer.

**Layers:**
1. **Presentation Layer**: User interface
2. **Business Layer**: Business logic
3. **Persistence Layer**: Data access
4. **Database Layer**: Data storage

**Benefits:**
- Clear separation of concerns
- Easy to understand
- Well-established pattern

**Drawbacks:**
- Can lead to performance issues
- Tight coupling between layers
- Difficult to scale individual layers

### Client-Server

**Definition:**
A distributed architecture where clients request services from servers.

**Characteristics:**
- Centralized server
- Multiple clients
- Request-response model

**Use Cases:**
- Web applications
- Database systems
- File sharing

### Event-Driven Architecture

**Definition:**
A software architecture paradigm promoting the production, detection, consumption of, and reaction to events.

**Characteristics:**
- Asynchronous communication
- Event producers and consumers
- Decoupled components

**Benefits:**
- High scalability
- Loose coupling
- Real-time processing

**Use Cases:**
- Real-time systems
- Microservices
- IoT applications

### Microkernel

**Definition:**
Consists of two types of components – a core system and several plug-in modules. While the core system works on minimal functionality to keep the system operational, the plug-in modules are independent components with specialized processing.

**Components:**
- **Core System**: Minimal functionality
- **Plug-in Modules**: Specialized processing

**Benefits:**
- Extensibility
- Modularity
- Separation of core and extensions

**Use Cases:**
- Operating systems
- IDEs
- Application frameworks

### Microservices

**Definition:**
An architectural approach where applications are built as a collection of small, independent services.

**Characteristics:**
- Service independence
- Technology diversity
- Distributed deployment

**Benefits:**
- Independent scaling
- Technology flexibility
- Fault isolation

**Resources:**
- See [Microservices.md](./Microservices.md) for detailed information

### Space-Based Architecture

**Definition:**
A distributed architecture pattern that uses in-memory data grids to handle scalability and performance.

**Characteristics:**
- In-memory data storage
- Distributed processing
- High performance

**Use Cases:**
- High-throughput systems
- Real-time processing
- Distributed caching

### Service-Oriented Architecture (SOA)

**Definition:**
An architectural style that supports service orientation. It is applied in the field of software design where services are provided to other components by application components, through a communication protocol over a network.

**Key Principle:**
By designing an application as a set of loosely coupled components, we initially get the ability to scale any component.

**Characteristics:**
- Autonomous components with loose coupling
- Communication via standard protocols (e.g., REST)
- Service reusability

**Relationship to Microservices:**
Microservices architecture is a logical continuation (or subset) of SOA. It is based on increasing the number of services instead of increasing the functionality of a specific service (reflection of the Single Responsibility principle in architecture) and deep integration with continuous processes.

### Other Architecture Patterns

**Master-Slave:**
- One master coordinates multiple slaves
- Used in database replication, distributed computing

**Pipe-Filter:**
- Data flows through a series of filters
- Each filter processes and passes data to the next

**Broker:**
- Mediates communication between components
- Decouples components

**Peer-to-Peer:**
- Equal nodes communicate directly
- No central server

**Event Bus:**
- Central event distribution mechanism
- Publishers and subscribers

**Message Bus:**
- Communication infrastructure
- Message routing and transformation

**Model View Controller (MVC):**
- Separates presentation, business logic, and data
- Widely used in web frameworks

**Model View Presenter (MVP):**
- Similar to MVC with presenter layer
- Better testability

**Model View and View Model (MVVM):**
- Data binding between view and view model
- Used in modern UI frameworks

**Blackboard:**
- Shared knowledge base
- Multiple components contribute to solution

**Interpreter:**
- Interprets domain-specific language
- Rule-based processing

**Serverless:**
- Functions as a service
- Event-driven, auto-scaling

**Static Content Hosting:**
- CDN-based content delivery
- Static file serving

**Publisher-Subscriber:**
- Decoupled messaging pattern
- Event-driven communication

**Sharding:**
- Horizontal data partitioning
- Distributed data storage

**Circuit Breaker:**
- Fault tolerance pattern
- Prevents cascading failures

**Command Query Responsibility Segregation (CQRS):**
- Separates read and write operations
- Optimized data access

**Resources:**
- [Software Architecture Patterns](https://www.simform.com/blog/software-architecture-patterns/)

---

## Design Methodologies

### Domain-Driven Design (DDD)

**Definition:**
A software design approach focusing on modeling software to match a domain according to input from that domain's experts.

**Key Concept:**
The structure and language of software code (class names, class methods, class variables) should match the business domain.

**Example:**
If a software processes loan applications, it might have classes like `LoanApplication` and `Customer`, and methods such as `AcceptOffer` and `Withdraw`.

**Benefits:**
- Better domain understanding
- Clearer code
- Business-aligned design

### Behavior Driven Development (BDD)

**Definition:**
An agile software development process that encourages collaboration among developers, quality assurance testers, and customer representatives in a software project.

**Characteristics:**
- User story focus
- Executable specifications
- Collaboration emphasis

**Benefits:**
- Better communication
- Clear requirements
- Testable specifications

---

## Communication Patterns

### Connection-Oriented Communication

**Definition:**
A network communication mode in telecommunications and computer networking, where a communication session or a semi-permanent connection is established before any useful data can be transferred, enabling the ability to ensure that data is delivered in the correct order to the upper communication layer.

**Characteristics:**
- Connection establishment required
- Ordered data delivery
- Reliable transmission

**Examples:**
- TCP protocol
- Telephone calls
- Database connections

**Alternative:**
Connectionless communication, for example the datagram mode communication used by the IP and UDP protocols, where data may be delivered out of order, since different network packets are routed independently, and may be delivered over different paths.

### Enterprise Service Bus (ESB)

**Definition:**
An Enterprise Service Bus (ESB) is fundamentally an architecture. It is a set of rules and principles for integrating numerous applications together over a bus-like infrastructure.

**Characteristics:**
- Centralized integration
- Message routing
- Protocol transformation
- Service orchestration

**ESB Products:**
ESB products enable users to build this type of architecture, but vary in the way that they do it and the capabilities that they offer.

**Use Cases:**
- Enterprise integration
- Service composition
- Legacy system integration

### Message-Oriented Middleware (MOM)

**Definition:**
Software or hardware infrastructure supporting sending and receiving messages between distributed systems.

**Benefits:**
- Allows application modules to be distributed over heterogeneous platforms
- Reduces the complexity of developing applications that span multiple operating systems and network protocols
- Creates a distributed communications layer that insulates the application developer from the details of the various operating systems and network interfaces

**Characteristics:**
- Asynchronous messaging
- Decoupled communication
- APIs that extend across diverse platforms and networks

**Use Cases:**
- Distributed systems
- Microservices communication
- Event-driven architectures

---

## Architecture Frameworks and Languages

### ArchiMate

**Definition:**
ArchiMate (originally from Architecture-Animate) is an open and independent enterprise architecture modeling language to support the description, analysis and visualization of architecture within and across business domains in an unambiguous way.

**Purpose:**
- Standardized modeling
- Cross-domain communication
- Architecture visualization

**Resources:**
- [ArchiMate Tool](https://www.archimatetool.com)

### TOGAF

**Definition:**
The Open Group Architecture Framework (TOGAF) is the most used framework for enterprise architecture today that provides an approach for designing, planning, implementing, and governing an enterprise information technology architecture.

**Characteristics:**
- High-level approach to design
- Comprehensive methodology
- Industry standard

**Use Cases:**
- Enterprise architecture
- IT strategy
- Digital transformation

### Other Frameworks and Languages

**Architecture Frameworks:**
- **TOGAF**: Enterprise architecture framework
- **ArchiMate**: Enterprise architecture modeling language
- **C4 Model**: Software architecture model
- **IT4IT**: IT management framework
- **ITIL**: IT service management
- **COBIT**: IT governance framework

**Design Tools:**
- Architecture design - Mulesoft Architect
- Design tools (ARIS or equivalent)

**Resilience Patterns:**
- Patterns for ensuring fault tolerance
- Circuit breaker, retry, timeout patterns

**Integration Patterns:**
- REST, GraphQL, gRPC
- API design patterns
- Service communication patterns

### Process Frameworks

**APQC Process Classification Framework:**
- [APQC Process Classification Framework](http://koptelov.info/apqc/)
- Standard process taxonomy

**eTOM (Enhanced Telecoms Operations Map):**
- [eTOM](http://koptelov.info/publikatsii/etom/)
- Telecommunications operations framework

**SCOR (Supply Chain Operations Reference):**
- Supply chain management framework

**EMMMv (Exploration, Mining, Metals and Minerals vertical):**
- Industry-specific framework

**FEA (Federal Enterprise Architecture):**
- Government enterprise architecture framework

---

## Modeling Tools and Notations

### UML (Unified Modeling Language)

**Definition:**
UML – унифицированный язык моделирования (Unified Modeling Language) – это система обозначений, которую можно применять для объектно-ориентированного анализа и проектирования. Его можно использовать для визуализации, спецификации, конструирования и документирования программных систем.

**Purpose:**
- Object-oriented analysis and design
- System visualization
- Specification and documentation

**Use Cases:**
- Software design
- System documentation
- Requirements analysis

### EPC (Event-driven Process Chain)

**Definition:**
Нотация моделирования EPC (Event-driven Process Chain) ориентирована на построение алгоритмов взаимодействия в процессе выполнения конкретной работы.

**Purpose:**
- Business process modeling
- Workflow design
- Process optimization

### BPMN (Business Process Model and Notation)

**Definition:**
Business Process Model and Notation (BPMN) is a graphical representation for specifying business processes in a business process model.

**Purpose:**
- Business process documentation
- Process standardization
- Workflow visualization

### IDEF0

**Definition:**
Методология IDEF0 (Integrated DEFinition) – это совокупность методов, правил и процедур, предназначенных для построения функциональной модели предметной области. Функциональная модель IDEF0 отображает функциональную структуру объекта, т.е. производимые им действия и связи между этими действиями.

**Purpose:**
- Functional modeling
- System analysis
- Process documentation

### ER Model (Entity-Relationship Model)

**Definition:**
ER-модель (от англ. Entity-Relationship model, модель «сущность — связь») — модель данных, позволяющая описывать концептуальные схемы предметной области.

**Purpose:**
- Database design
- Conceptual modeling
- Data structure design

**Usage:**
ER-модель используется при высокоуровневом (концептуальном) проектировании баз данных. С её помощью можно выделить ключевые сущности и обозначить связи, которые могут устанавливаться между этими сущностями.

**ER Diagrams:**
ER-диаграммы часто используются в сочетании с диаграммами DFD, которые схематично показывают движение потоков информации в рамках процесса или системы.

### Modeling Notations Summary

**Notations:**
- **UML**: Unified Modeling Language for software systems
- **EPC**: Event-driven Process Chain for workflows
- **BPMN**: Business Process Model and Notation
- **IDEF0**: Functional modeling methodology
- **ER Model**: Entity-Relationship model for databases
- **Flowchart**: Block diagrams for processes

**Notations Definition:**
Нотации – графические модели, которые используются, чтобы фиксировать бизнес-процессы, анализировать их и оптимизировать.

### Modeling Tools

**Architecture Tools:**
- [Archimate Tool](https://www.archimatetool.com)
- [Structurizr](https://structurizr.com)
- [C4 Model](https://c4model.com)

**Diagram Icons:**
- [Stencil Town](https://stenciltown.omnigroup.com/categories/all/)

**Documentation Tools:**
- [AsciiDoctor](https://asciidoctor.org)
- [Inkscape](https://inkscape.org/ru/)

**Database Design:**
For visual design of database structure, we use the tool [DbSchema](https://www.dbschema.com/) (which actually outputs structure description in [2bass](https://github.com/CourseOrchestra/2bass)). However, placing ER diagrams in documentation is quite a headache.

**Modeling Software:**
- **Eclipse Papyrus**: Powerful tool for maintaining models, almost with code synchronization
- **Rational Rose**: Enterprise modeling tool
- **ARIS**: Business process modeling
- **Enterprise Architect (Sparx Systems)**: UML modeling tool

**PlantUML Integration:**
Including PlantUML diagrams in asciidoc text is convenient.

**5 Diagrams:**
- [5 Diagrams Article](https://m.habr.com/ru/company/epam_systems/blog/538018/)

**Data Storage:**
Persistent data storage, such as relational databases and NoSQL databases, binary file storage such as images, music and video, big data storage, and so on.

---

## Architect Roles

### Solution Definition

**Definition:**
Решение - Это продукт или комплекс продуктов, который решает конкретную техническую или бизнес-задачу.

**Architect Types:**
- архитектор программного обеспечения (Software Architect)
- IT-архитектор (IT Architect)
- архитектор информационных систем (Information Systems Architect)
- Solution и Enterprise архитектор (Solution and Enterprise Architect)

### Architecture Excellence Initiative

**EPAM Global Architecture Community:**
Architecture Excellence Initiative – глобальному архитектурному сообществу EPAM

**Training:**
обучение в Solution Architecture School

### Key Knowledge Areas

**SOLID Principles:**
- Single Responsibility
- Open/Closed
- Liskov Substitution
- Interface Segregation
- Dependency Inversion

**Object-Oriented Patterns:**
- Creational patterns
- Structural patterns
- Behavioral patterns

**Components:**
- Principles of cohesion and component compatibility

**Architecture:**
- Horizontal levels and vertical slices
- Clean architecture principles
- Best practices

**Service-Oriented Architecture (SOA):**
- SOA concepts
- Service design
- Integration patterns

### Business Analysis

**Standards and Methodologies:**
- Business analysis standards and methodologies
- BABOK (Business Analysis Body of Knowledge)
- ГОСТы. Профессиональный стандарт «Бизнес-аналитик»
- Business analysis in software development methodologies

**Data Analysis:**
- MS Excel / Tableau / Power BI / Cognos

**ITSM and ITIL:**
- IT ideology ITSM and ITIL library: business analysis as a service

**Best Practices:**
- Best practices for managing business analysis department

**Business Process Modeling:**
- Business analysis methods. Business process modeling
- IDEF family. IDEF0
- eEPC notation
- BPMN notation
- Flowchart notation

**Requirements:**
Сбором и описанием требований— выявление, сбор, формализация и документирование требований к бизнес-процессу и ПО (UML, EPC, BPMN, ER, модель данных).

### AS IS Model

**Definition:**
AS IS - модель "как есть", модель существующего состояния организации.

**Purpose:**
- Current state analysis
- Gap identification
- Improvement planning

### Product Backlog

**Definition:**
Бэклог продукта (product backlog) — это упорядоченный набор элементов, очередь задач, перечень всех функций, которые заинтересованные люди хотят получить от продукта. Этот список содержит краткие описания всех желаемых возможностей продукта.

**Management:**
Product manager или product owner представляют бэклог команде и управляют им, описывает его главные элементы во время митинга по планированию спринта. Описание бэклога следует производить на простом и доступном языке, без технических спецификаций, чтобы оно было понятно каждому в команде. Любые изменения и требования по продукту должны быть своевременно отражены в этой очереди задач.

**Sprint Backlog:**
Бэклог спринта — это список определенных задач по воплощению в жизнь выбранных элементов бэклога продукта. Это список для оптимизации, которой команда займется в ближайший спринт, а также описание, каким образом они эту оптимизацию будут реализовывать.

### SCRUM Roles

**Roles:**
- **Client**: Idea, resources
- **Business Analyst**: Uses backlog - list of tasks (epics) that need to be done for the product to function. Epics split to stories.

### Precedence Diagram Method (PDM)

**Definition:**
Precedence Diagram Method (PDM) is a project planning technique that allows project managers to map out all the tasks in a project to plan the order in which they will be executed.

**Purpose:**
- Task sequencing
- Dependency visualization
- Project planning

### GTD Principles

**Getting Things Done:**
Основные принципы GTD очень просты. В основе лежат три привычки, к которым должен приучить себя каждый эффективный человек.

1. Записывать свои дела (Write down your tasks)
2. Быстро решать, что делать дальше (Quickly decide what to do next)
3. Делать то, что записал (Do what you wrote down)

### Customer Journey and Use Cases

**Customer Journey Map:**
- User experience mapping
- Touchpoint analysis
- Journey optimization

**Use Case Diagrams:**
- System functionality visualization
- Actor interactions
- Requirements documentation

---

## Resources

### Articles and Blogs

- [Architecture Article](https://habr.com/ru/post/496760/)
- [SOLID Principles](https://zen.yandex.ru/media/codeblog/solid-v-obektnoorientirovannom-programmirovanii-5b39d9175a1a2301f38e0d63)
- [Solution Architect Books](https://javarevisited.blogspot.com/2018/02/5-must-read-books-to-become-software-architect-solution.html?m=1)
- [EPAM Solution Architect](https://dev.by/news/solution-architect-v-epam)
- [EPAM Architecture Blog](https://habr.com/ru/company/epam_systems/blog/442944/?_ga=2.262525196.1492089757.1611253623-868867144.1604997058)
- [Architecture Blog](https://m.habr.com/ru/company/dododev/blog/539360/)

### Podcasts

- [DRY RMR Podcast](https://soundcloud.com/dry-rmr)

### Books

- [Software Architecture in Practice](https://www.amazon.com/Software-Architecture-Practice-Edition-Engineering/dp/0321815734)
- [Designing Software Architectures](https://www.amazon.com/Designing-Software-Architectures-Practical-Engineering/dp/0134390784/ref=sr_1_1?ie=UTF8&qid=1497300541&sr=8-1&keywords=designing+software+architectures)
- [Software Systems Architecture](https://www.amazon.com/Software-Systems-Architecture-Stakeholders-Perspectives/dp/032171833X/ref=pd_bxgy_b_img_z)
- [DevOps for Software Architects](https://www.amazon.com/DevOps-Software-Architects-Perspective-Engineering/dp/0134049845)

### Organizations and Communities

- [Solution Architecture GitHub](https://github.com/unlight/solution-architecturel)
- [SEI CMU](https://www.sei.cmu.edu)
- [IASA Global](https://iasaglobal.org)
- [Architectis](https://architectis.je)

### Tools and Platforms

- [C4 Model](https://c4model.com)
- [Structurizr](https://structurizr.com)
- [Archimate Tool](https://www.archimatetool.com)
- [Stencil Town](https://stenciltown.omnigroup.com/categories/all/)
- [AsciiDoctor](https://asciidoctor.org)
- [Inkscape](https://inkscape.org/ru/)
- [DbSchema](https://www.dbschema.com/)

---

## Summary

Software architecture encompasses:

- **Architecture Patterns**: Layered, Client-Server, Event-Driven, Microservices, SOA, and more
- **Design Methodologies**: DDD, BDD, and others
- **Communication Patterns**: Connection-oriented, ESB, MOM
- **Frameworks**: TOGAF, ArchiMate, C4, ITIL, COBIT
- **Modeling Tools**: UML, BPMN, EPC, IDEF0, ER models
- **Architect Roles**: Software, IT, Solution, Enterprise architects

**Key Takeaways:**
- Choose patterns based on system requirements
- Use appropriate frameworks for enterprise architecture
- Model systems using standard notations
- Understand different architect roles and responsibilities
- Leverage tools for visualization and documentation

Understanding these fundamentals is essential for effective software architecture design and implementation.
