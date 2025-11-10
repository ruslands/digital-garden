# System Design Interview: Complete Checklist

## Table of Contents
1. [Introduction](#introduction)
2. [Requirements Gathering](#requirements-gathering)
3. [Data Modeling](#data-modeling)
4. [Network and RPS Calculations](#network-and-rps-calculations)
5. [API Design](#api-design)
6. [System Components](#system-components)
7. [Business Metrics](#business-metrics)
8. [Bottlenecks and Solutions](#bottlenecks-and-solutions)
9. [Scaling Strategies](#scaling-strategies)
10. [Testing and Monitoring](#testing-and-monitoring)
11. [Quick Reference](#quick-reference)

---

## Introduction

**Interview Strategy:**
На интервью сразу перечислить план действий (At the interview, immediately list the action plan).

**Key Concepts to Remember:**
- `set` - hash map
- `tuple` - изменяем (mutable)
- `lb` - алгоритм выбора сервиса (load balancer - service selection algorithm)
- PostgreSQL - how replica works
- noGIL - 3.13, 3.14 (Python without Global Interpreter Lock)
- multithreading is faster than multiprocessing (in certain scenarios)

**Purpose:**
This checklist helps systematically approach system design interviews by ensuring all critical aspects are considered.

---

## Requirements Gathering

### Functional Requirements

**Core Questions:**
- Что именно нужно построить? (What exactly needs to be built?)
- Какова **цель** системы? (основной use case) (What is the **goal** of the system? (main use case))
- Кто **пользователи**? (внутренние/внешние, масштабы) (Who are the **users**? (internal/external, scale))
- Какие **ключевые действия** пользователь должен выполнять? (What **key actions** should the user perform?)

**Examples of Key Actions:**
- регистрация (registration)
- авторизация (authorization)
- загрузка файлов (file upload)
- подписка (subscription)
- поиск (search)
- лайки (likes)
- и т.д. (etc.)

**Additional Considerations:**
- Какие дополнительные функции? (What additional features?)
- Есть ли роли пользователей? (админ, гость, премиум) (Are there user roles? (admin, guest, premium))
- Какие CRUD-операции над основными сущностями (What CRUD operations on main entities)

---

### Non-Functional Requirements (NFR)

#### 1. Criticality & Business Impact

**Questions:**
- Is the system Mission Critical / Business Critical?
- What is the business impact of downtime or failure? (e.g., financial, reputational, regulatory)

**Considerations:**
- Financial losses
- Reputation damage
- Regulatory compliance
- Customer impact

---

#### 2. Performance & Scalability

**Expected Load:**
- Concurrent users / peak users
- Requests Per Second (RPS) — both read (RRPS) and write (WRPS)
  - *Current spec: 1100 RPS total (clarify read/write split)*
- Ожидаемая нагрузка: *Сколько пользователей / RPS / хранения данных в год?* (Expected load: *How many users / RPS / data storage per year?*)

**Latency Requirements:**
- Target response time: 200–300ms (P95 or P99?)
- Acceptable degradation under load?
- Получить требования latency - 200-300ms (Get latency requirements - 200-300ms)

**Throughput:**
- WRPS + RRPS (already noted — ensure split is defined)
- Получить требования throughput (WRPS+RRPS) - 1100 RPS (Get throughput requirements (WRPS+RRPS) - 1100 RPS)

**Read vs Write Patterns:**
- Read Heavy or Write heavy? (e.g., 80% read, 20% write)
- Кол-во запрос на создание/запись/удаление/редактирование (Number of requests for create/write/delete/edit)

**Rate Limiting:**
- Per user/IP/token?
- Configurable by tariff/plan?
- Rate limit (может быть ограничен тарифом) (Rate limit (may be limited by tariff))

**Data Volume & Growth:**
- Estimated annual data growth (GB/TB/year)
- Number of rows per major table (current + projected)
- Average record size
- Объем данных в БД, сколько строк в таблицах (Data volume in DB, how many rows in tables)

**TTL (Time-To-Live):**
- TTL for data or cache? (Specify per entity if needed)
- TTL? (Time-To-Live?)

---

#### 3. Reliability, Availability & Fault Tolerance

**Availability (SLA):**
- Target uptime: 99.9% (3.65d downtime/year) or 99.99% (52.6m/year)?
- Measurement window (monthly? annually?)
- Надёжность / Доступность (SLA: 99.9%? 99.99%?) (Reliability / Availability (SLA: 99.9%? 99.99%?))

**Fault Tolerance:**
- Max acceptable downtime per incident
- Failover mechanism (active-active, active-passive?)

**Disaster Recovery (DRP):**
- **RTO (Recovery Time Objective)**: e.g., < 1 hour
- **RPO (Recovery Point Objective)**: e.g., < 5 minutes (data loss tolerance)
- Geographic redundancy? Multi-region?
- DRP - recovery - RTO | RPO

---

#### 4. Data Management & Integrity

**Changelog / Audit Trail:**
- Required? (Yes/No)
- What entities? Who changed what and when?
- Retention period for audit logs?
- Нужен ли Changelog (Is Changelog needed)

**Data Consistency Model:**
- Strong consistency? Eventual? Read-after-write?

**Backup Strategy:**
- Frequency, retention, encryption, test restore process

---

#### 5. Security & Compliance

**Authentication & Authorization:**
- Required? (Yes/No)
- Model: RBAC, ABAC, ACL?
- SSO / MFA support?
- Нужна ли аутентификация, авторизация (RBAC, ACL, ABAC) (Is authentication, authorization needed (RBAC, ACL, ABAC))

**Regulatory & Legal Compliance:**
- GDPR, HIPAA, CCPA, PCI-DSS, etc.
- Data residency / localization requirements (e.g., data must stay in EU)
- Data anonymization / pseudonymization needs?
- Правовые/регуляторные: *GDPR, HIPAA, локализация данных* (Legal/regulatory: *GDPR, HIPAA, data localization*)

**Encryption:**
- At rest? In transit? (TLS 1.3+)
- Key management (BYOK? KMS?)

---

#### 6. Operational & Observability

**Monitoring & Alerting:**
- Key metrics: CPU, memory, latency, error rates, queue depth
- Alert thresholds and escalation paths
- Мониторинг, логгирование, метрики (Monitoring, logging, metrics)

**Logging:**
- Structured logging (JSON)?
- Retention period, PII masking

**Tracing:**
- Distributed tracing (e.g., OpenTelemetry, Jaeger)?

**Health Checks:**
- Liveness / Readiness probes for containers/infra

---

#### 7. Analytics & Business Metrics

**Requirements:**
- Real-time dashboards? Batch reporting?
- Key business KPIs to track (e.g., conversion rate, DAU, error rate by feature)
- Integration with analytics platforms (e.g., Mixpanel, Amplitude, internal BI)
- Аналитика, бизнес метрики (Analytics, business metrics)

---

#### 8. Technical Constraints & Environment

**Tech Stack:**
- Languages, frameworks, databases, message queues
- Технические: *Языки, стек, ограничения API, протоколы, бюджеты, время* (Technical: *Languages, stack, API constraints, protocols, budgets, time*)

**API Constraints:**
- Protocols: REST, GraphQL, gRPC?
- Rate limits, payload size limits, auth methods

**Deployment & Infrastructure:**
- Cloud provider? On-prem? Hybrid?
- Containerization (Docker/K8s)? Serverless?
- Budget & Timeline:
  - CAPEX/OPEX constraints
  - Go-live deadline, phased rollout?

---

#### 9. Usability & Accessibility (Often Overlooked!)

**Accessibility Compliance:**
- WCAG 2.1 AA? Screen reader support?

**Multi-language / Localization Support:**
- UI/UX localization? Date/number/currency formats?

**User Experience Benchmarks:**
- Page load time, interaction latency, mobile responsiveness

---

#### 10. Maintainability & Extensibility

**Documentation Requirements:**
- API docs, architecture diagrams, runbooks

**Extensibility:**
- Plugin architecture? Webhooks? Event-driven?

**Upgrade & Rollback Strategy:**
- Zero-downtime deployments? Canary releases?

---

#### 11. Support & SLAs

**Support Hours:**
- 24/7? Business hours?

**Incident Response SLA:**
- e.g., P1 = 15min response, 1hr resolution

**Customer Support Channels:**
- Email, chat, phone, ticketing

---

### Missing Points Checklist

✅ **Missing Points Added:**
- Data consistency model
- Backup strategy
- Encryption (at rest/in transit)
- Accessibility & localization
- Maintainability & documentation
- Support SLAs
- Business impact of failure
- Health checks & tracing
- Deployment model (cloud/on-prem)
- Upgrade/rollback strategy

**Note:**
This list is now comprehensive, logically grouped, and ready for stakeholder review or inclusion in an SRS (Software Requirements Specification) document.

---

## Data Modeling

### Data Model Construction

**Steps:**
- Построить модель данных (Build data model)
- Для каждого поля определить is Unique, is None (For each field determine is Unique, is None)
- Расчет объема одного объекта (пример 300 кб) (учитывай размер символов латиница/кириллица) (Calculate volume of one object (example 300 KB) (consider character size Latin/Cyrillic))
- Расчет объема всех записей (пример 3 ТБ) (Calculate volume of all records (example 3 TB))
- Определить кол-во строк в таблице (100 млн) (Determine number of rows in table (100 million))

### Example Data Model

| Поле        | Тип         | isUnique | isNullable | Размер (байт) |
| ----------- | ----------- | -------- | ---------- | ------------- |
| id          | UUID        | ✅        | ❌          | 16            |
| username    | String(30)  | ✅        | ❌          | 30            |
| email       | String(50)  | ✅        | ❌          | 50            |
| bio         | Text(500)   | ❌        | ✅          | 500           |
| avatar\_url | String(200) | ❌        | ✅          | 200           |
| created\_at | Timestamp   | ❌        | ❌          | 8             |
| updated\_at | Timestamp   | ❌        | ❌          | 8             |

### Size Calculation

**Итоговый размер одной строки (Total size of one row):**

```
= 16 (UUID) 
+ 30 (username) 
+ 50 (email) 
+ 500 (bio) 
+ 200 (avatar_url) 
+ 8 + 8 (timestamps)
= 812 байт ≈ 1 КБ (с учётом индексов и overhead)
```

**Расчет объема всех записей (Calculation of total record volume):**

- Размер одной строки ≈ **1 КБ**
- Кол-во строк: **100 млн**
- **100 млн × 1 КБ = 100 ГБ**

> Примечание: Если храним бинарные данные (например, аватар), или есть много индексов, добавляем запас.
> Например, с overhead и резервом: **≈ 300 ГБ**

### Character Encoding Reference

**UTF (Unicode Transformation Format)** — это способ закодировать символы Unicode в байты. Unicode — это глобальный стандарт, включающий **буквы, цифры, знаки, символы всех языков мира, эмодзи и прочее**.

#### Encoding Comparison

| Кодировка  | Размер символа | Преимущества                                            | Недостатки                                                                  |
| ---------- | -------------- | ------------------------------------------------------- | --------------------------------------------------------------------------- |
| **ASCII**  | **1 байт**     | Только английские буквы, цифры, спецсимволы (до `0x7F`) |                                                                             |
| **UTF-8**  | 1–4 байта      | Эффективна для английского текста                       | Символы других языков занимают больше. латиница — 1, кириллица/юникод — 2–4 |
| **UTF-16** | 2–4 байта      | Оптимальна для многих мировых языков                    | Менее читаема, возможен BOM. Базовые символы — 2, редкие/эмодзи — 4         |
| **UTF-32** | всегда 4 байта | Простота (1 символ = 1 код)                             | Очень неэффективно по памяти                                                |

#### Character Encoding Examples

| Символ | Значение   | UTF-8 (байты)             | UTF-16 (байты)                            | UTF-32 (байты)         |
| ------ | ---------- | ------------------------- | ----------------------------------------- | ---------------------- |
| `A`    | Английская | `0x41` (1 байт)           | `0x0041` (2 байта)                        | `0x00000041` (4 байта) |
| `Я`    | Кириллица  | `0xD0 0xAF` (2 байта)     | `0x042F` (2 байта)                        | `0x0000042F` (4 байта) |
| `你`   | Китайский  | `0xE4 0xBD 0xA0` (3)      | `0x4F60` (2 байта)                        | `0x00004F60` (4 байта) |
| `😀`    | Эмодзи     | `0xF0 0x9F 0x98 0x80` (4) | `0xD83D 0xDE00` (4 байта, surrogate pair) | `0x0001F600` (4 байта) |

#### Encoding Recommendations

| Сценарий                               | Рекомендуемая кодировка            |
| -------------------------------------- | ---------------------------------- |
| Веб-сайты, API, HTML                   | **UTF-8** (стандарт в интернете) ✅ |
| Внутренние системы Windows             | UTF-16 (например, .NET, Java)      |
| Если важна простота (1 символ = 1 код) | UTF-32                             |

#### Example Text Encoding

**Фраза: `"Привет 😀"`**

**В UTF-8:**
- `П` → 2 байта
- `р` → 2 байта
- …
- `😀` → 4 байта
  ➡️ Всего: **16 байт**

**В UTF-16:**
- Каждая кириллическая буква → 2 байта
- `😀` → 4 байта (суррогатная пара)
  ➡️ Всего: **16 байт**

**В UTF-32:**
- Каждый символ → 4 байта
  ➡️ 7 символов × 4 = **28 байт**

**Useful Tips:**
- **JSON strings:** учитывай экранирование (`\n`, `\"` и т.п.) (consider escaping)
- **Base64 (для бинарных данных):** +33% к размеру исходного файла (+33% to original file size)

---

## Network and RPS Calculations

### Traffic Calculation Example

**Assumptions:**
- **WRPS (запись)** = 100 запросов в секунду (write requests per second)
- **RRPS (чтение)** = 1000 запросов в секунду (read requests per second)

### Inbound Traffic (входящие запросы)

**Write Requests (WRPS):**
- 100 × 1 КБ = **100 КБ/сек**

**Read Requests:**
- Small GET requests, e.g., 200 bytes per request
- 1000 × 0.2 КБ = **200 КБ/сек**

**Total Inbound:**
- **≈ 300 КБ/сек**

### Outbound Traffic (ответы)

**Write Response:**
- Usually short (status, ID): ~200 bytes
- 100 × 0.2 КБ = **20 КБ/сек**

**Read Response:**
- 1 КБ per record
- 1000 × 1 КБ = **1000 КБ/сек**

**Total Outbound:**
- **≈ 1020 КБ/сек ≈ 1 МБ/сек**

### Summary Table

| Метрика            | Значение    |
| ------------------ | ----------- |
| Размер объекта     | ~1 КБ       |
| Кол-во объектов    | 100 млн     |
| Общий объем данных | ~100–300 ГБ |
| WRPS               | 100         |
| RRPS               | 1000        |
| Inbound Traffic    | ~300 КБ/сек |
| Outbound Traffic   | ~1 МБ/сек   |

---

## API Design

**Considerations:**
- Стандартный CRUD (Standard CRUD)
- Нестандартные API (Non-standard APIs)
- api/v1/ (API versioning)

**API Types:**
- REST
- GraphQL
- gRPC

---

## System Components

### Core Components

**Client Application:**
- Клиентское приложение (web/mobile) (Client application (web/mobile))

**Backend Service:**
- Backend Service (CQRS - разбить сервисы на чтение и запись) (CQRS - split services into read and write)

**API Gateway:**
- API Gateway (LB, auth, TLS, rate limit)
- Load balancer health checker (Проверяет жив ли apigateway, если нет то обновляет dns) (Checks if API Gateway is alive, if not updates DNS)
- DNS у каждого API Gateway/LB свой адрес в DNS (Each API Gateway/LB has its own DNS address)

**Authentication Service:**
- Auth service (OAuth / JWT / RBAC)

**Database:**
- СУБД (SQL / NoSQL) (DBMS (SQL / NoSQL))

**Communication Protocols:**
- REST, GraphQL или gRPC?

**Cache:**
- Кэш (Redis, Memcached) (Cache (Redis, Memcached))
- Стратегия кэширования LRU/MRU/LFU/TTL/Two-tiered data (Caching strategy LRU/MRU/LFU/TTL/Two-tiered data)

**Message Queues:**
- Очереди (Kafka, SQS) (Queues (Kafka, SQS))

**Content Delivery:**
- CDN / Static Content

**Traffic Management:**
- Rate Limiter / Load Balancer

---

## Business Metrics

**Considerations:**
- Определить какие метрики будем собирать (Determine which metrics to collect)
- Distributed Log (Kafka) / queue в PostgreSQL или Clickhouse (Distributed Log (Kafka) / queue in PostgreSQL or Clickhouse)
- OLAP / OLTP лучше разнести (OLAP / OLTP better to separate)

**Metrics Types:**
- User metrics (DAU, MAU)
- Business metrics (conversion rate, revenue)
- Technical metrics (error rate, latency)
- Feature-specific metrics

---

## Bottlenecks and Solutions

### Potential Bottlenecks

**Questions to Consider:**
- Где могут быть bottlenecks? (Where can bottlenecks be?)
- Проблема celebrity (Celebrity problem - hot keys/partitions)
- Индепотентность (Idempotency)
- Определить точки отказа (Identify failure points)

### Solutions

**Caching:**
- Кэширование? (Caching?)

**Sharding:**
- Шардирование? По какому ключу (шардирование уменьшить нагрузку на экземпляр бд) (Sharding? By which key (sharding reduces load on DB instance))

**Rate Limiting:**
- Rate Limiting?

**Backpressure:**
- Backpressure? (Handling overload)

**Resilience Patterns:**
- Retrying / Circuit Breaker?

**Idempotency:**
- Ensure operations can be safely retried

---

## Scaling Strategies

### Scaling Approaches

**Horizontal vs Vertical Scaling:**
- Horizontal vs vertical scaling
- Load balancer
- Replication
- Partitioning (Sharding)
- CDN

**Scaling Components:**
- **Horizontal Scaling**: Add more servers
- **Vertical Scaling**: Increase server capacity
- **Load Balancer**: Distribute traffic
- **Replication**: Copy data across servers
- **Partitioning (Sharding)**: Split data across servers
- **CDN**: Distribute static content

---

## Testing and Monitoring

### Monitoring Tools

**Monitoring:**
- Prometheus, Grafana

**Logging:**
- ELK stack, Loki

**Tracing:**
- Jaeger, OpenTelemetry

**Health Checks:**
- Liveness / Readiness probes
- Health check endpoints

**Alerting:**
- Alert thresholds
- Escalation paths
- Notification channels

---

## Quick Reference

### Key Concepts

- **set** - hash map
- **tuple** - изменяем (mutable)
- **lb** - алгоритм выбора сервиса (load balancer)
- **PostgreSQL** - how replica works
- **noGIL** - 3.13, 3.14 (Python without GIL)
- **multithreading is faster than multiprocessing** (in certain scenarios)

### Interview Checklist

**1. Requirements Gathering:**
- Functional requirements
- Non-functional requirements (11 categories)

**2. Data Modeling:**
- Build data model
- Calculate sizes
- Consider encoding

**3. Network Calculations:**
- Calculate inbound/outbound traffic
- Estimate bandwidth needs

**4. API Design:**
- Choose protocol (REST/GraphQL/gRPC)
- Design endpoints
- Versioning strategy

**5. System Components:**
- Identify all components
- Design interactions
- Plan deployment

**6. Business Metrics:**
- Define metrics
- Plan collection
- Design analytics

**7. Bottlenecks:**
- Identify potential issues
- Design solutions
- Plan for scale

**8. Scaling:**
- Choose scaling strategy
- Plan for growth
- Design for resilience

**9. Monitoring:**
- Set up observability
- Define alerts
- Plan logging

---

## Summary

This comprehensive checklist covers all aspects of system design interviews:

- **Requirements**: Functional and non-functional (11 categories)
- **Data Modeling**: Structure, sizing, encoding
- **Network**: Traffic calculations, bandwidth planning
- **API Design**: Protocols, endpoints, versioning
- **Components**: Architecture, services, infrastructure
- **Metrics**: Business and technical metrics
- **Bottlenecks**: Identification and solutions
- **Scaling**: Horizontal, vertical, strategies
- **Monitoring**: Observability, logging, alerting

**Key Takeaways:**
- Start with requirements gathering
- Calculate data volumes and network traffic
- Design for scale from the beginning
- Plan for failures and bottlenecks
- Set up comprehensive monitoring
- Consider all non-functional requirements

Use this checklist systematically during system design interviews to ensure comprehensive coverage of all critical aspects.
