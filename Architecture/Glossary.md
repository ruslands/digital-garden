# Architecture Glossary: Key Concepts and Patterns

## Table of Contents
1. [Introduction](#introduction)
2. [Orchestration vs Choreography](#orchestration-vs-choreography)
3. [API Architectures and Communication Protocols](#api-architectures-and-communication-protocols)
4. [Distributed Systems Concepts](#distributed-systems-concepts)
5. [Sharding](#sharding)
6. [Summary Tables](#summary-tables)

---

## Introduction

This glossary provides definitions and explanations of key architectural concepts, patterns, and technologies used in modern distributed systems and microservices architectures.

**Purpose:**
- Understand fundamental architectural patterns
- Learn about communication protocols
- Explore distributed systems concepts
- Make informed architectural decisions

---

## Orchestration vs Choreography

### Definitions

**Orchestration:**
Centralized control of service workflows by a single coordinator. Tools like **Apache Airflow** (for data pipelines) and **Camunda** (for business process management) manage the full process flow explicitly.

**Choreography:**
Decentralized, event-driven coordination where services react independently to events without a central controller. Common tools include **Apache Kafka**, **RabbitMQ**, **NATS**, and cloud event buses like **AWS EventBridge** and **Azure Event Grid**.

**Simple Analogy:**
- **Orchestration = Central conductor managing everything**
- **Choreography = Independent actors responding to shared events**

### Tools Comparison

| **Approach**      | **Tool**             | **Description**                                    | **Use Case / Domain**                   |
| ----------------- | -------------------- | -------------------------------------------------- | --------------------------------------- |
| **Orchestration** | Apache Airflow       | Workflow orchestration for data pipelines (DAGs)   | Data engineering, ETL, ML pipelines     |
| **Orchestration** | Camunda              | BPMN-based business process management engine      | Business workflows, human tasks         |
| **Choreography**  | Apache Kafka         | Distributed event streaming platform               | Real-time event-driven microservices    |
| **Choreography**  | RabbitMQ             | Message broker with pub/sub and routing            | Asynchronous messaging between services |
| **Choreography**  | NATS                 | Lightweight messaging system for cloud-native apps | Pub/sub messaging, microservices        |
| **Choreography**  | AWS EventBridge      | Serverless event bus on AWS                        | Cloud event-driven architectures        |
| **Choreography**  | Azure Event Grid     | Event routing service on Azure                     | Cloud event-driven applications         |
| **Choreography**  | Google Cloud Pub/Sub | Global messaging service for event-driven systems  | Cloud pub/sub for microservices         |
| **Choreography**  | Apache Pulsar        | Distributed pub/sub with geo-replication           | Scalable event streaming                |
| **Choreography**  | Temporal.io          | Workflow + event-driven orchestration              | Hybrid orchestration + choreography     |
| **Choreography**  | Solace, Red Hat AMQ  | Enterprise event mesh platforms                    | Event brokers for hybrid cloud          |

---

## API Architectures and Communication Protocols

### Overview

Different API architectures and communication protocols serve different purposes in distributed systems. Understanding their characteristics helps choose the right approach for your use case.

### Protocol Comparison

| **API Architecture / Protocol** | **Description**                                                                 | **Key Features**                                                          | **Statefulness**               | **Example Use Cases**                             |
| ------------------------------- | ------------------------------------------------------------------------------- | ------------------------------------------------------------------------- | ------------------------------ | ------------------------------------------------- |
| **REST**                        | Representational State Transfer; stateless HTTP API design                      | Stateless, uses HTTP verbs (GET, POST, etc.), JSON/XML payloads           | Stateless                      | Public web APIs, mobile app backends              |
| **SOAP**                        | Simple Object Access Protocol; XML-based messaging protocol                     | Stateful or stateless, strict contract via WSDL, built-in WS-\* standards | Can be Stateful or Stateless   | Enterprise systems, legacy SOAP services          |
| **RPC**                         | Remote Procedure Call; client calls server functions directly                   | Simple request-response, usually blocking calls                           | Usually Stateless              | Distributed computing, microservices              |
| **gRPC**                        | Modern RPC framework using HTTP/2 and Protocol Buffers                          | Binary serialization, low latency, supports bi-directional streaming      | Usually Stateless              | High-performance microservices, real-time systems |
| **GraphQL**                     | Query language for APIs allowing clients to specify exactly what data they need | Single endpoint, flexible queries, reduces overfetching                   | Stateless                      | Complex data fetching, mobile and web clients     |
| **WebSocket**                   | Full-duplex communication protocol over a single TCP connection                 | Persistent connection, real-time bi-directional communication             | Stateful                       | Chat apps, real-time notifications                |
| **WebHook**                     | HTTP callbacks triggered by events                                              | Server sends data to client when event occurs, asynchronous               | Stateless                      | Payment gateways, CI/CD pipelines                 |
| **MQTT**                        | Lightweight publish-subscribe messaging protocol, designed for IoT              | Low bandwidth, supports QoS levels                                        | Stateful (connection-oriented) | IoT devices, telemetry data                       |
| **AMQP**                        | Advanced Message Queuing Protocol; message-oriented middleware                  | Reliable messaging, routing, queuing                                      | Stateful                       | Enterprise messaging, financial transactions      |

### Additional Notes

**gRPC vs RPC:**
gRPC is an improved version of RPC that uses HTTP/2 for multiplexing, binary serialization for efficiency, and supports advanced streaming features like bi-directional and server-side streaming.

**Stateless vs Stateful:**

- **Stateless:** Each API call is independent; the server does **not** store client session info between calls. Example: REST APIs.
- **Stateful:** Server maintains client state between calls, often using persistent connections or sessions. Example: WebSocket, MQTT.

---

## Distributed Systems Concepts

### 1. Distributed Logs

#### Concept

A **distributed log** is a **fault-tolerant, append-only** data structure that is replicated across multiple servers/nodes.

**Key Characteristics:**
- Think of it as a **shared, ordered list of events or messages** that all participants can read from or write to
- It forms the backbone of many systems, including **Apache Kafka**, **Pulsar**, and **distributed databases** like **CockroachDB** or **etcd**

#### Key Features

- **Append-only**: You only add to the end of the log
- **Ordered**: All messages/events have a clear sequence
- **Replicated**: Copies exist across nodes for reliability
- **Use cases**: Event sourcing, messaging queues, audit logs, stream processing

#### Real-World Analogy

Think of a **flight recorder (black box)** in a plane that logs all events during a flight. Now imagine 100 planes writing their logs into a **single, shared recorder**, and all maintenance crews can read from it at the same time. That's a **distributed log**.

#### Example: Apache Kafka

Kafka is a distributed log system.

```plaintext
Producer ---> Kafka ---> Consumer
           (writes)     (reads)

Log Partition:
[0] order_id=1001
[1] order_id=1002
[2] order_id=1003
```

**How It Works:**
- **Producer** writes events to a topic (e.g., `orders`)
- **Kafka** stores them in order
- **Consumers** read from the topic at their own pace

#### Why It's Useful

- Event sourcing in microservices
- Activity feeds (Twitter, LinkedIn)
- Data pipelines (logs, analytics)

#### Distributed Nature

- Logs are **partitioned** and **replicated**
- Each **partition** is an ordered list
- Multiple **brokers** (servers) store partitions

---

### 2. RedLock (Redis Distributed Lock)

#### Concept

**RedLock** is an **algorithm proposed by Redis' creator**, Salvatore Sanfilippo (antirez), for implementing **distributed locks** using Redis in a **fault-tolerant way**.

#### Problem It Solves

When you need a **mutual exclusion mechanism** (i.e., only one process can perform an operation at a time) in a **distributed system**, a simple Redis lock isn't safe due to possible failures.

#### Real-World Analogy

Imagine 5 security guards (Redis servers). You need at least **3 out of 5** to say "yes" before you're allowed into a room. This prevents someone else from sneaking in while you're busy checking.

#### How RedLock Works

1. It involves **5 Redis nodes** (preferably on different machines)
2. A client tries to acquire a lock by writing a unique value to a key **on a majority (3 out of 5) of these nodes**
3. The lock has a **timeout** to prevent deadlocks
4. If the client succeeds in acquiring the lock in most nodes within a short time window, it considers the lock acquired

#### Basic Lock (NOT Distributed)

```python
SET lock_key "123" NX PX 10000  # NX = Only if not exists, PX = 10 sec expiry
```

**Problem:** If Redis crashes? You lose the lock.

#### RedLock Flow (Distributed)

1. Try to set the lock **on 5 Redis nodes**
2. If you get **at least 3 successful locks** within a time window, you have the lock
3. Release the lock on all nodes when done

#### Key Rules

- Use **unique values** to avoid confusion during unlock
- Use **timeouts** to prevent deadlocks

#### Libraries

- `redlock-py` (Python)
- `Redisson` (Java)
- `node-redlock` (Node.js)

#### Criticism

The RedLock algorithm has been **controversial**. Some experts suggest using **consensus-based** systems (e.g., etcd, ZooKeeper) for safer distributed locking.

---

### 3. Consistent Hashing

#### Concept

**Consistent Hashing** is a technique to **distribute keys/data across nodes** in a way that **minimizes reorganization** when nodes are added or removed.

#### Problem It Solves

In a regular hash map, changing the number of buckets (nodes) invalidates many key assignments. Consistent hashing avoids this.

#### Real-World Analogy

You have 4 lockers (servers). You assign users to lockers by hashing their names. If one locker disappears or you add a new one, you **don't want to reassign all users—just a few**.

#### How It Works

- Keys and nodes are placed on a **circular ring** using a hash function
- A key is assigned to the **next node on the ring in clockwise direction**
- When a node is added/removed, **only a small subset of keys** need to be reassigned

#### Regular Hashing (BAD)

```plaintext
hash(key) % N

If N = 4:
- hash("user1") % 4 → Node 2
- hash("user2") % 4 → Node 0

If N becomes 5 (new node):
- Almost all keys are reassigned.
```

#### Consistent Hashing (GOOD)

Place **nodes** and **keys** on a **hash ring**. A key is assigned to the **first node clockwise** from its hash.

```plaintext
Hash Ring:
[user1_hash] → Node A
[user2_hash] → Node B
[user3_hash] → Node A
```

If a node is removed/added, **only nearby keys** are affected.

#### Use Cases

- **Distributed caches** (e.g., memcached)
- **Sharding** in distributed databases
- **Load balancing**
- **CDNs** (content delivery networks)

#### With Virtual Nodes

To avoid imbalance (some servers getting more keys), we use **virtual nodes**—each server is placed multiple times on the ring.

---

## Sharding

### When to Consider Sharding

**You Should Consider Sharding When:**

#### 1. Your Dataset Is Too Big for a Single Machine

- If your **database size exceeds the storage, memory, or CPU capacity** of a single server
- Examples:
  - Hundreds of millions of rows in a single table
  - Your backups take too long or don't fit anymore

#### 2. You're Hitting Performance Bottlenecks

- **Read/write throughput** is maxing out
- Disk I/O or query times are degrading
- One big table is becoming a **hotspot** (e.g., `orders` table with 1000+ inserts/sec)

#### 3. High Availability or Geo-distribution Requirements

- You need **regional sharding** to keep data close to users (e.g., US data in one shard, EU data in another)
- You want to **scale out horizontally** instead of upgrading a single machine vertically

#### 4. Tenant Isolation in Multi-Tenant SaaS

If you serve multiple clients (tenants), sharding per tenant can improve:
- Isolation
- Scalability
- Custom backup/restore per tenant

### When Not to Shard

**Don't shard if:**
- You can still **scale vertically** (add more CPU/RAM/disk)
- Your workload fits in **read replicas + caching** (e.g., PgBouncer + Redis)
- You haven't tried **partitioning** (built-in in PostgreSQL)
- Complexity is not worth it yet

### Sharding Approaches in PostgreSQL

| Method                          | Description                         | Tools                              |
| ------------------------------- | ----------------------------------- | ---------------------------------- |
| **Manual sharding**             | You manage where each row goes      | Your own logic/code                |
| **Citus**                       | Extension for horizontal scaling    | [Citus](https://www.citusdata.com) |
| **Foreign Data Wrappers (FDW)** | Query across databases              | `postgres_fdw`, `multicorn`        |
| **Logical Partitioning**        | Use PostgreSQL's table partitioning | Native Postgres                    |

### Example Use Case

Imagine a SaaS app with 500,000 customers:
- Each customer has thousands of orders
- Total `orders` table = 2 billion rows
- Queries like `SELECT * FROM orders WHERE customer_id = ?` are slow

**Sharding by `customer_id`** across 10 Postgres servers could:
- Split the load
- Reduce index size per shard
- Improve performance

### Decision Checklist

| Question                                                  | Yes? Then Sharding Might Be Needed |
| --------------------------------------------------------- | ---------------------------------- |
| Is your dataset huge (100GB+)?                            | ✅                                  |
| Are you hitting resource limits on CPU/RAM?               | ✅                                  |
| Do you need low-latency access per region?                | ✅                                  |
| Do you have natural partitioning keys (e.g., tenant\_id)? | ✅                                  |
| Are queries mostly isolated by a specific key?            | ✅                                  |

---

## Summary Tables

### Distributed Systems Concepts

| Concept            | Description                                          | Use Cases                                     |
| ------------------ | ---------------------------------------------------- | --------------------------------------------- |
| Distributed Logs   | Append-only, ordered, replicated logs across servers | Kafka, messaging, event sourcing              |
| RedLock            | Redis-based algorithm for distributed locking        | Ensuring mutual exclusion in distributed apps |
| Consistent Hashing | Smart key distribution across changing nodes         | Caching, load balancing, sharding             |

### Concept Comparison

| Concept                | What It Is                                     | Real-Life Tool       | Analogy                             |
| ---------------------- | ---------------------------------------------- | -------------------- | ----------------------------------- |
| **Distributed Logs**   | Append-only, ordered log across servers        | Kafka, Pulsar        | Black box for many planes           |
| **RedLock**            | Safe locking with multiple Redis nodes         | Redis + Redlock lib  | Need 3 of 5 guards to enter a room  |
| **Consistent Hashing** | Smart key-to-node assignment w/ low disruption | Cassandra, Memcached | Users assigned to lockers on a ring |

---

## Summary

This glossary covers essential architectural concepts:

- **Orchestration vs Choreography**: Centralized vs decentralized workflow management
- **API Protocols**: REST, SOAP, RPC, gRPC, GraphQL, WebSocket, and more
- **Distributed Logs**: Append-only, ordered, replicated event streams
- **RedLock**: Fault-tolerant distributed locking with Redis
- **Consistent Hashing**: Efficient key distribution across nodes
- **Sharding**: Horizontal scaling strategy for large datasets

**Key Takeaways:**
- Choose orchestration for centralized control, choreography for decentralized systems
- Select API protocols based on statefulness, performance, and use case requirements
- Use distributed logs for event sourcing and messaging
- Implement distributed locks carefully, considering alternatives
- Apply consistent hashing for load balancing and caching
- Consider sharding when vertical scaling is insufficient

Understanding these concepts is essential for designing scalable, reliable distributed systems.
