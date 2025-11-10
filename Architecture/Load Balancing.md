# Load Balancing: Algorithms and Implementation

## Table of Contents
1. [Introduction](#introduction)
2. [Static Load Balancing Algorithms](#static-load-balancing-algorithms)
3. [Dynamic Load Balancing Algorithms](#dynamic-load-balancing-algorithms)
4. [Advanced/Hybrid Approaches](#advancedhybrid-approaches)
5. [How Load Balancers Track Connections](#how-load-balancers-track-connections)
6. [Summary](#summary)

---

## Introduction

Load balancing algorithms distribute incoming network traffic or workload across multiple servers to ensure no single server is overwhelmed, improving responsiveness and availability.

**Main Types:**
- **Static Load Balancing**: Uses predefined rules
- **Dynamic Load Balancing**: Adapts to real-time conditions

**Benefits:**
- Improved performance
- High availability
- Scalability
- Fault tolerance

---

## Static Load Balancing Algorithms

**Definition:**
These use predefined rules and don't adapt to current server loads.

### Round Robin

**How It Works:**
Requests are distributed in a circular order.

**Use Case:**
When all servers have equal capacity.

**Characteristics:**
- Simple implementation
- Equal distribution
- No consideration of server load

**Example:**
Server 1 → Server 2 → Server 3 → Server 1 → ...

---

### Weighted Round Robin

**How It Works:**
Like Round Robin, but servers are assigned weights based on their capacity.

**Use Case:**
When servers have different capabilities.

**Characteristics:**
- Considers server capacity
- Better resource utilization
- Still simple to implement

**Example:**
Server 1 (weight 3) → Server 2 (weight 1) → Server 3 (weight 2) → ...

---

### Least Connection

**How It Works:**
The server with the fewest active connections gets the next request.

**Use Case:**
For long-lived sessions like databases or sticky HTTP sessions.

**Characteristics:**
- Considers current load
- Good for stateful applications
- More complex than Round Robin

**Example:**
If Server 1 has 5 connections, Server 2 has 3, and Server 3 has 7, the next request goes to Server 2.

---

## Dynamic Load Balancing Algorithms

**Definition:**
These adapt to real-time traffic and server performance.

### Least Response Time

**How It Works:**
Directs traffic to the server with the lowest average response time and fewest connections.

**Use Case:**
Web services where latency varies.

**Characteristics:**
- Considers both response time and connections
- Adapts to performance changes
- More complex implementation

**Example:**
Server with 200ms response time and 10 connections is preferred over server with 500ms and 5 connections.

---

### Resource-Based (Adaptive)

**How It Works:**
Uses metrics like CPU, memory usage, or custom health checks.

**Use Case:**
High-performance computing, microservices.

**Characteristics:**
- Most sophisticated
- Considers actual resource usage
- Requires monitoring infrastructure

**Metrics Used:**
- CPU utilization
- Memory usage
- Disk I/O
- Network bandwidth
- Custom health checks

---

### IP Hash

**How It Works:**
Hashes the client IP to determine the server.

**Use Case:**
When session stickiness is required (e.g., gaming or chat apps).

**Characteristics:**
- Ensures same client goes to same server
- Simple implementation
- Not adaptive to load

**Example:**
hash(client_ip) % number_of_servers → server_index

---

## Advanced/Hybrid Approaches

### Load Balancing with Machine Learning

**How It Works:**
Predicts traffic spikes and chooses optimal server.

**Characteristics:**
- Predictive capabilities
- Self-learning
- Complex implementation

**Use Case:**
Useful in cloud-native architectures.

**Benefits:**
- Proactive load distribution
- Better resource utilization
- Adapts to patterns

---

### Cloud-Native Load Balancers

**Examples:**
- AWS ELB (Elastic Load Balancer)
- Google Cloud Load Balancer
- Azure Load Balancer

**Characteristics:**
- Use a mix of the above algorithms
- Handle scaling and failover automatically
- Integrated with cloud services

**Features:**
- Auto-scaling
- Health checks
- SSL termination
- Geographic distribution

---

## Comparison Table

| Algorithm            | Adapts to Load? | Good for Stateful Apps? | Complexity |
| -------------------- | --------------- | ----------------------- | ---------- |
| Round Robin          | ❌               | ❌                       | Low        |
| Least Connections    | ✅               | ✅                       | Medium     |
| Weighted Round Robin | ❌               | ❌                       | Low        |
| IP Hash              | ❌               | ✅                       | Low        |
| Least Response Time  | ✅               | ✅                       | High       |
| Resource-Based       | ✅               | ✅                       | High       |

---

## How Load Balancers Track Connections

### 1. Connection Tracking (for proxy-based LBs)

**How It Works:**
If the load balancer is a **reverse proxy** (e.g., NGINX, HAProxy, Envoy), it **terminates the client connection itself** and opens a new one to the backend server.

**Key Points:**
- The LB **knows exactly how many open connections** it has to each backend
- It can keep a real-time counter for each server
- Most accurate method

**Example:**
HAProxy uses built-in counters for open connections and makes decisions based on that.

**Benefits:**
- Real-time accuracy
- Direct connection management
- No additional infrastructure needed

---

### 2. Health Checks and Metrics (for external/decoupled LBs)

**How It Works:**
Some advanced or distributed LBs (e.g., AWS ELB, Istio, custom-built solutions) **ask the servers** to report their load using:

**Methods:**
- **HTTP endpoints** that return current metrics (e.g., `/status`, Prometheus metrics)
- **Sidecars or agents** that push metrics to a central monitor

**Examples:**
- A server exposes `/metrics` → returns number of current sessions
- A sidecar (like Envoy in Istio) reports current load to the control plane

**Characteristics:**
- Requires server instrumentation
- May have slight delay
- Scalable approach

---

### 3. SNMP / OS-Level Monitoring (low-level LB integrations)

**How It Works:**
In more advanced or older enterprise setups:
- The LB queries the server using protocols like **SNMP**
- Gets real-time data about **network sockets**, **CPU**, **RAM**, etc.

**Use Cases:**
- Enterprise environments
- Legacy systems
- Detailed monitoring requirements

**Characteristics:**
- Low-level access
- Requires SNMP configuration
- More complex setup

---

### 4. Sticky Sessions and Application State

**How It Works:**
In some cases, sticky sessions or session IDs help track which server is handling which clients.

**Characteristics:**
- Less precise than connection counting
- Used more for **session persistence**, not balancing
- Helps maintain state

**Use Cases:**
- Session-based applications
- Stateful services
- User-specific data

---

## Summary Table: Connection Tracking Methods

| LB Type                        | How It Knows Connections     | Notes                           |
| ------------------------------ | ---------------------------- | ------------------------------- |
| Reverse Proxy (NGINX, HAProxy) | Tracks connections directly  | Most accurate and common        |
| Cloud LBs (AWS, GCP)           | Metrics from agents or APIs  | Scalable, sometimes delayed     |
| Service Mesh (Istio)           | Telemetry from sidecars      | Fine-grained, distributed       |
| Custom setups                  | /status APIs or SNMP polling | Requires server instrumentation |

---

## Summary

**Load Balancing Types:**
- **Static**: Simple, predictable, but not adaptive
- **Dynamic**: Adaptive, more complex, better performance

**Algorithm Selection:**
- Choose based on application requirements
- Consider statefulness, server capacity, and performance needs
- Balance complexity with benefits

**Connection Tracking:**
- Reverse proxies track connections directly (most accurate)
- Cloud LBs use metrics and health checks
- Service meshes use distributed telemetry
- Custom solutions may use SNMP or APIs

**Key Takeaways:**
- Static algorithms are simpler but less flexible
- Dynamic algorithms adapt to real-time conditions
- Connection tracking method depends on LB architecture
- Choose algorithm based on application characteristics
- Monitor and adjust based on performance metrics

Understanding load balancing algorithms and connection tracking methods is essential for designing scalable, high-performance systems.
