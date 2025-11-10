# Management and Technical Terms Glossary

## Table of Contents
1. [Financial and Business Terms](#financial-and-business-terms)
2. [Requirements and Documentation](#requirements-and-documentation)
3. [Process Modeling and Notation](#process-modeling-and-notation)
4. [User Experience and Design](#user-experience-and-design)
5. [Technical Terms](#technical-terms)
6. [Customer Satisfaction Metrics](#customer-satisfaction-metrics)
7. [Service Level Metrics](#service-level-metrics)
8. [Requirements Analysis](#requirements-analysis)
9. [Continuous Integration and Deployment](#continuous-integration-and-deployment)
10. [Project Management](#project-management)
11. [Risk Management](#risk-management)
12. [Quality and Completion Criteria](#quality-and-completion-criteria)

---

## Financial and Business Terms

### ВГО (Внутригрупповые Операции)

**Definition:** Финансовые операции между компаниями, входящими в одну группу компаний.

**Purpose:** Financial transactions between companies within the same corporate group.

---

### Spinoff

**Definition:** The creation of an independent company through the sale or distribution of new shares of an existing business or division of a parent company.

**Purpose:** The spun-off companies are expected to be worth more as independent entities than as parts of a larger business.

---

## Requirements and Documentation

### BRD (Business Requirements Document)

**Definition:** It clearly defines everything that a project entails.

**Purpose:** Comprehensive document outlining all project requirements from a business perspective.

---

### FRD (Functional Requirement Document)

**Definition:** Document that specifies the functional requirements of a system.

**Purpose:** Describes what the system should do from a functional perspective.

---

### SRS (Software Requirements Specification)

**Definition:** A document that describes what the software will do and how it will be expected to perform.

**Purpose:** Detailed specification of software behavior and performance expectations.

---

### ARD (Architecture Decision Record)

**Definition:** A document that captures an important architecture decision made along with its context and consequences.

**Purpose:** Documents why architectural decisions were made and their impact.

---

## Process Modeling and Notation

### BPMN (Business Process Model and Notation)

**Definition:** Standard notation for modeling business processes.

**Purpose:** Visual representation of business processes for analysis and improvement.

---

### CMMN (Case Management Model and Notation)

**Definition:** Notation for modeling case management processes.

**Purpose:** Represents unstructured, knowledge-intensive processes.

---

### DMN (Decision Model and Notation)

**Definition:** Standard notation for modeling business decisions.

**Purpose:** Visual representation of business decision logic.

---

### Flowable Tools

**Tools:** BPMN, CMMN, DMN

**Purpose:** Suite of tools for process and decision modeling.

---

### IDEF (Integrated DEFinition)

**Definition:** Нотаций проектирования IDEF (Integrated DEFinition)

**Purpose:** Set of modeling languages for system design and analysis.

---

### EPC (Event-Driven Process Chain)

**Definition:** Most often used to model higher-level business processes.

**Comparison:** BPMN would be most often better for lower-level process modeling.

**Purpose:** High-level business process modeling.

---

## User Experience and Design

### UserFlow / ScreenFlow

**Tools:**
- **[Overflow](https://overflow.io/)** - "User flows done right" - Easy import from Sketch or Figma, can create flow for 100 screens in an hour
- **Miro, Flowmapp** - Other tools for team user flow design

**Types of User Flow:**

1. **Task Flow**
   - **Definition:** Это простое представление того, что пользователь делает на каждом шаге для выполнения цели или задачи. По сути это классическая блок-схема, определяющая эту последовательность.
   - **Purpose:** Simple representation of user steps to complete a goal or task

2. **Wire Flow (Lo-Fi)**
   - **Definition:** Это объединение блок-схемы и wireframes. Wireframe – это низкодетализированный набросок дизайна экрана, упор в котором делается не на визуальную составляющую, а на расположение элементов, структуру и содержание экрана.
   - **Details:** В wire flow вместо элементов блок-схемы представлены схематичные макеты экранов, с которыми взаимодействует пользователь на пути достижения цели. Не нужно зацикливаться на визуальных деталях и отрисовывать каждую кнопку и иконку. Чаще всего акцент делается на элементах навигации в дизайне каждой отдельной страницы.
   - **Purpose:** Combines flowchart and wireframes, focuses on structure and navigation

3. **Screen Flow (Hi-Fi)**
   - **Definition:** Здесь речь идет о детально проработанных экранах, которые понятны как пользователям, так и разработчикам. Обычно делается акцент на элементах навигации и некоторых нюансах поведения. Его можно использовать как регламентирующий документ для утверждения дизайна макетов.
   - **Details:** Screen flow можно по сути назвать прототипом, если ему добавить интерактивность. Его особенностью является высокая точность или идеальное пиксельное соответствие: в нем учитывается физический размер экрана и представляются все визуальные и типографические детали продукта. Элементы screen flow – это фактически макеты экранов готового приложения.
   - **Purpose:** Detailed screens that serve as regulatory documents for design approval

---

### Customer Journey Map

**Definition:** A customer journey map is a visual representation of a customer's experience with your brand. These visuals tell a story about how a customer moves through each phase of interaction and experiences each phase.

**Purpose:** Understand and improve customer experience across all touchpoints.

---

### Use Case vs User Story vs Customer Journey Map

**Differences by Degree of Detail:**

1. **User Story**
   - **Definition:** Поверхностное описание сценария. Я хочу сделать это и получить такой результат
   - **Characteristics:** Normally, and purposely, more vague. Provides a simplified, abridged description in layman's terms of what a feature should help the user do. Leaves it more open to interpretation and encourages more creativity and discussion.

2. **Use Case**
   - **Definition:** Описывает детальнее взаимодействие с системой
   - **Characteristics:** Covers more ground by showing how the user should interact with the system and how the system should reciprocate. Goes into further detail about how the individual steps in a feature's process.

3. **Customer Journey Map**
   - **Definition:** Описывает весь флоу взаимодействие пользователя с системой при этом на карте отображается эмоциональная составляющая пользователя
   - **Characteristics:** Shows the entire flow of user interaction with the system, displaying the emotional component of the user experience.

---

## Technical Terms

### GCP (Google Cloud Platform)

**Definition:** Google's cloud computing platform.

**Purpose:** Provides cloud infrastructure and services.

---

### BLOB (Binary Large Object)

**Definition:** A collection of binary data stored as a single entity.

**Common Uses:** Typically images, audio or other multimedia objects, though sometimes binary executable code is stored as a blob.

---

### CPQ (Configure Price Quote)

**Definition:** CPQ is a sales tool for companies to quickly and accurately generate quotes for orders.

**Purpose:** Streamlines the quoting process for complex products.

---

### OTAP (Over the Air Programming)

**Definition:** Refers to various methods of distributing new software, configuration settings, and even updating encryption keys to devices like mobile phones, set-top boxes, electric cars or secure voice communication equipment (encrypted 2-way radios).

**Key Feature:** One central location can send an update to all users, who are unable to refuse, defeat, or alter that update, and the update applies immediately to everyone on the channel.

**Note:** A user could 'refuse' OTA, but the 'channel manager' could also 'kick them off' the channel automatically.

---

### XSD (XML Schema Definition)

**Definition:** A World Wide Web Consortium (W3C) recommendation that specifies how to formally describe the elements in an Extensible Markup Language (XML) document.

**Purpose:** Defines the structure and data types of XML documents.

---

### Data Governance

**Definition:** A collection of processes, roles, policies, standards, and metrics that ensure the effective and efficient use of information in enabling an organisation to achieve its goals.

**Purpose:** Ensures data quality, security, and compliance.

---

### ETL (Extract, Transform, Load)

**Definition:** A data integration process that combines data from multiple data sources into a single, consistent data store that is loaded into a data warehouse or other target system.

**Stands for:**
- **Extract** - Get data from source
- **Transform** - Clean and format data
- **Load** - Load into target system

---

### ERP (Enterprise Resource Planning)

**Definition:** Refers to a type of software that organizations use to manage day-to-day business activities such as accounting, procurement, project management, risk management and compliance, and supply chain operations.

**Purpose:** Integrated management of business processes.

---

## Customer Satisfaction Metrics

### CSAT vs NPS vs CES

**Three Key Metrics:**

1. **NPS (Net Promoter Score)**
   - **Definition:** Widely used market research metric that typically takes the form of a single survey question asking respondents to rate the likelihood that they would recommend a company, product, or a service to a friend or colleague.
   - **Purpose:** Measures customer loyalty

2. **CSAT (Customer Satisfaction Score)**
   - **Definition:** Measures customer satisfaction with a product or service
   - **Purpose:** Immediate satisfaction indicator

3. **CES (Customer Effort Score)**
   - **Definition:** Measures how easy it was for customers to accomplish their goal
   - **Purpose:** Ease of use indicator

---

## Service Level Metrics

### SLI (Service Level Indicator)

**Definition:** Это метрики времени, такие как задержка запроса, пропускная способность (запросы в секунду) или число сбоев на запрос. Они, как правило, агрегируются во времени, а затем преобразуются в ставку, среднее или процент по отношению к пороговому значению.

**Examples:**
- Request latency
- Throughput (requests per second)
- Error rate per request

**Purpose:** Time-based metrics that measure service performance.

---

### SLO (Service Level Objective)

**Definition:** Это целевые показатели совокупного успеха SLI в течение определенного периода времени (например, за «последние 30 дней» или «за этот квартал»), согласованные заинтересованными сторонами.

**Purpose:** Target metrics for aggregate SLI success over a specific time period, agreed upon by stakeholders.

---

### SLA (Service Level Agreement)

**Definition:** Это обещание провайдера услуги потребителю, касающееся доступности и последствий ситуаций, когда он не в состоянии обеспечить согласованный уровень сервиса.

**Purpose:** Provider's promise to the consumer regarding availability and consequences when service levels are not met.

---

## Requirements Analysis

### Requirements Hierarchy

**Three Levels of Software Requirements:**

1. **Business Requirements (Бизнес-требования)**
   - High-level business goals and objectives

2. **User Requirements (Требования пользователей)**
   - **Definition:** Описывают цели и задачи, которые пользователям позволит решить система.
   - Goals and tasks that the system will allow users to solve

3. **Functional Requirements (Функциональные требования)**
   - **Definition:** Определяют функциональность ПО, которую разработчики должны построить, чтобы пользователи смогли выполнить свои задачи в рамках бизнес-требований.
   - **Documentation:** Функциональные требования документируются в спецификации требований к ПО (software requirements specification, SRS), где описывается так полно, как необходимо, ожидаемое поведение системы.
   - Functionality that developers must build

4. **Non-functional Requirements (Нефункциональные требования)**
   - Quality attributes, performance, security, etc.

---

### Business Rules

**Definition:** Бизнес-правила (business rules) включают корпоративные политики, правительственные постановления, промышленные стандарты и вычислительные алгоритмы.

**Key Point:** Бизнес-правила не являются требованиями к ПО, потому что они находятся снаружи границ любой системы ПО. Однако они часто налагают ограничения, определяя, кто может выполнять конкретные варианты использования, или диктовать, какими функциями должна обладать система, подчиняющаяся соответствующим правилам.

**Purpose:** Corporate policies, government regulations, industry standards, and computational algorithms that constrain the system.

---

### Requirements Traceability

**Definition:** Трассировка требований - позволяет описывать и отслеживать связи между различными артефактами требований — бизнес-требованиями, системными требованиями в различных формах (в том числе в виде вариантов использования), а в широком смысле и артефактами процесса разработки вообще.

**Purpose:** 
- Key component of managing changing requirements
- Allows determining the scope of impact when requirements change
- Reduces risk of project delays and budget overruns due to underestimated impact of requirement changes

**Benefits:**
- Tracks connections between business requirements, system requirements, and development artifacts
- Helps manage changing requirements
- Reduces project risks

---

## Continuous Integration and Deployment

### Continuous Integration (CI)

**Definition:** Developers practicing continuous integration merge their changes back to the main branch as often as possible. The developer's changes are validated by creating a build and running automated tests against the build.

**Purpose:** Early detection of integration issues.

---

### Continuous Delivery

**Definition:** Предполагает частую доставку обновлений на «боевую» систему. Продукт поддерживается в актуальной версии, а любые ошибки при обновлении проще отследить, так как при каждом релизе объем изменений невелик.

**Purpose:** Frequent delivery of updates to production, with small change volumes for easier error tracking.

---

### Continuous Deployment

**Definition:** Continuous deployment goes one step further than continuous delivery. With this practice, every change that passes all stages of your production pipeline is released to your customers. There's no human intervention, and only a failed test will prevent a new change to be deployed to production.

**Purpose:** Fully automated deployment to production.

---

## Project Management

### Critical Path Method

**Definition:** Метод критического пути - это метод планирования операций, в основе которого лежит математический алгоритм.

**Model Components:**
- Список всех операций, необходимых для выполнения проекта (List of all operations needed)
- Зависимости между этими операциями (Dependencies between operations)
- Период времени, необходимый для выполнения каждой операции (Duration of each operation)

**Purpose:** Определить наиболее длительную последовательность операций, необходимую для завершения проекта, а также самые ранние и самые поздние моменты начала и окончания каждой операции.

**Key Concepts:**
- **Критические операции** - Operations on the longest path
- **Операции с временным резервом** - Operations with time buffer (can be moved without affecting project duration)

**Reference:** [Wrike - Critical Path Method](https://www.wrike.com/ru/blog/metod-kriticheskogo-puti-pri-upravlenii-proektami-prosto-kak-dvazhdy-dva/)

---

### Project Constraints (Тройственность Ограничений)

**Definition:** Тройственность ограничений продукта: ограниченность описывает баланс между содержанием проекта, стоимостью, временем и качеством.

**Components:**
- **Scope** (Содержание проекта)
- **Cost** (Стоимость)
- **Time** (Время)
- **Quality** (Качество)

**Purpose:** Understanding the balance between project constraints.

---

### Project Portfolio Management

**Definition:** Портфель проектов - это общее представление обо всех проектах организации, выполняемых для достижения стратегических бизнес-целей. Это могут быть все проекты компании, отдельного подразделения или отдела. Множество проектов и программ, объединенных для удобств управления.

**Purpose:** Unified view of all projects for strategic business goal achievement.

---

### Focus Factor

**Definition:** Фокус фактор: понимается некий коэффициент, отражающий отношение производительности существующей команды к производительности «идеальной» команды.

**Purpose:** Measures team productivity relative to ideal team performance.

---

### Theory of Constraints

**Definition:** Теория ограничений - это методология управления системами в различных видах деятельности, суть которой заключается в поиске ключевых ограничений, определяющих успех и эффективность всей системы в целом.

**Key Principle:** «Цепь не сильнее, чем ее самое слабое звено» или «Скорость эскадры определяется скоростью ее самого медленного корабля».

**Explanation:** Вся система не может быть быстрее и мощнее, чем ее самое слабое место. Это место и называется ограничением, и как минимум, одно ограничение есть в любой системе.

**System Model:** Любая система состоит из набора процессов или последовательных задач, как на конвейере. Каждый процесс имеет определенную мощность или пропускную способность. При этом один из процессов всегда отстает, поскольку имеет определенное ограничение, которое становится «бутылочным горлышком» всей системы.

**Result:** Мощность и скорость системы всегда будет равняться ее самому слабому звену в цепи задач.

---

## Risk Management

### Risk Management

**Definition:** Управление рисками - процесс принятия и выполнения управленческих решений, направленных на снижение вероятности возникновения неблагоприятного результата и минимизацию возможных потерь проекта, вызванных его реализацией.

**Purpose:** Reduce probability of adverse outcomes and minimize potential project losses.

---

### Risk Register

**Definition:** Реестр рисков является одним из способов представления и хранения информации об опасных событиях и риске. В реестр риска обычно включают основные виды опасностей, применяемые методы оценки и снижения риска и мероприятия по предупреждению, снижению и обработке риска в конкретном бизнес-процессе.

**Purpose:** Document and track risks, mitigation strategies, and risk treatment activities.

---

### Positive Risks (Opportunities)

**Definition:** Позитивные риски (шансы): Риск антоним шанса

**Purpose:** Recognize that not all risks are negative; some represent opportunities.

---

### RAT (Riskiest Assumption Test)

**Definition:** An approach that is used for testing ideas and hypotheses. The main goal is to gather the feedback of users as quickly as possible and check the idea feasibility before the product is launched.

**Purpose:** Test the riskiest assumptions related to your future product before full launch.

---

### Six Sigma

**Definition:** A set of techniques and tools for process improvement.

**Purpose:** Improve process quality and reduce defects.

---

## Development Methodologies

### FDD (Feature-Driven Development)

**Definition:** An iterative and incremental software development process. It is a lightweight or Agile method for developing software.

**Purpose:** Agile development focused on feature delivery.

---

### SDD (Specification-Driven Development)

**Definition:** Development approach driven by specifications.

**Reference:** [TestRigor - Specification-Driven Development](https://blog.testrigor.com/how-specification-driven-development-works/)

**Purpose:** Ensures development follows detailed specifications.

---

### Jobs-to-be-Done Theory

**Definition:** Provides a framework for defining, categorizing, capturing, and organizing all your customers' needs.

**Purpose:** Understand customer needs from a jobs-to-be-done perspective rather than just features.

---

## Quality and Completion Criteria

### Definition of Done (DoD)

**Definition:** The definition of done is an agreed-upon set of things that must be true before any product backlog item is considered complete.

**Purpose:** Ensures consistent quality standards across all work items.

---

### Conditions of Satisfaction (CoS)

**Definition:** Are specific to a given product backlog item and define what must be true for that particular product backlog item to be considered done.

**Purpose:** Item-specific completion criteria.

---

### Acceptance Criteria (AC)

**Definition:** Almost same as CoS.

**Purpose:** Criteria that must be met for a work item to be accepted.

---

### Bus Factor

**Definition:** A measurement of the risk resulting from information and capabilities not being shared among team members, derived from the phrase "in case they get hit by a bus".

**Also Known As:** Bus problem, lottery factor, truck factor, beer truck factor, bus/truck number, or lorry factor.

**Key Concept:** Similar to the much older idea of key person risk, but considers the consequences of losing key technical experts, versus financial or managerial executives (who are theoretically replaceable at an insurable cost).

**Requirement:** Personnel must be both key and irreplaceable to contribute to the bus factor; losing a replaceable or non-key person would not result in a bus-factor effect.

**Purpose:** Identify knowledge concentration risks in the team.

---

## Additional Concepts

### Miller's Law / Miller's Magic Number

**Definition:** Кошелёк Миллера/закон Миллера — закономерность, обнаруженная американским учёным-психологом Джорджем Миллером, согласно которой кратковременная человеческая память, как правило, не может запомнить и повторить более 7 ± 2 элементов.

**Purpose:** Understanding cognitive limits for effective communication and design.

---

### Sales Action

**Definition:** Sales-related actions and activities.

---

### Window of Enthusiasm/Opportunity

**Definition:** Окно энтузиазма/возможностей

**Purpose:** Time period when opportunities or enthusiasm are at their peak.

---

### Experience Mixing

**Definition:** Перемешивание опыта: Оптимальное количество время работы в компании это 3-4 года

**Purpose:** Optimal tenure in a company is 3-4 years for experience diversity.

---

### Bleeding Edge Technology

**Definition:** A type of technology released to the public even though it has not been thoroughly tested and may be unreliable.

**Purpose:** Cutting-edge but potentially unstable technology.

---

## Summary

| Category               | Key Terms                                            | Count |
| ---------------------- | ---------------------------------------------------- | ----- |
| **Requirements**       | BRD, FRD, SRS, ARD                                   | 4     |
| **Process Modeling**   | BPMN, CMMN, DMN, IDEF, EPC                           | 5     |
| **UX/Design**          | UserFlow, ScreenFlow, Customer Journey Map           | 3     |
| **Technical**          | GCP, BLOB, CPQ, OTAP, XSD, ETL, ERP                  | 7     |
| **Metrics**            | NPS, CSAT, CES, SLI, SLO, SLA                        | 6     |
| **CI/CD**              | CI, Continuous Delivery, Continuous Deployment       | 3     |
| **Project Management** | Critical Path, Constraints, Portfolio, Focus Factor  | 4     |
| **Risk Management**    | Risk Register, RAT, Six Sigma, Theory of Constraints | 4     |
| **Quality**            | DoD, CoS, AC, Bus Factor                             | 4     |

---

## Best Practices

1. **Use appropriate documentation** - Choose BRD, FRD, or SRS based on project needs
2. **Model processes** - Use BPMN for lower-level, EPC for higher-level processes
3. **Track requirements** - Implement requirements traceability for change management
4. **Measure service levels** - Use SLI, SLO, and SLA for service quality
5. **Manage knowledge** - Monitor bus factor to reduce knowledge concentration risk
6. **Define completion** - Use DoD, CoS, and AC for clear completion criteria
