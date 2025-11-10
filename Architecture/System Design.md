# System Design: Message Delivery Guarantees

## Table of Contents
1. [Introduction](#introduction)
2. [Message Delivery Guarantees](#message-delivery-guarantees)
3. [Transactional Outbox Pattern](#transactional-outbox-pattern)
4. [Summary](#summary)

---

## Introduction

In distributed messaging systems, delivery guarantees define how messages are delivered and processed. Understanding these guarantees is crucial for designing reliable systems that balance reliability, performance, and complexity.

**Key Concepts:**
- **At-Most-Once**: Messages delivered zero or one time
- **At-Least-Once**: Messages delivered one or more times
- **Exactly-Once**: Messages delivered exactly one time
- **Transactional Outbox**: Pattern for reliable message delivery

**Trade-offs:**
Each guarantee balances:
- Reliability
- Performance
- Implementation complexity
- System requirements

---

## Message Delivery Guarantees

### 1. At-Most-Once

**Guarantee:**
Messages are delivered zero or one time; no retries on failure.

**How It Works:**
The sender sends a message once without waiting for acknowledgment. If delivery fails (e.g., due to network issues), the message is lost.

**Pros:**
- Low latency and high throughput due to minimal coordination
- Simplest to implement; no retry or deduplication logic needed

**Cons:**
- Messages can be lost, reducing reliability
- Unsuitable for critical operations where loss is unacceptable

**Use Cases:**
Non-critical, high-throughput scenarios like logging, telemetry, or ephemeral updates where occasional loss is tolerable.

**Examples:**
- **Kafka**: Achievable with "fire-and-forget" producers (acks=0)
- **UDP-based systems**: Often at-most-once due to lack of retries
- **AWS SQS**: Not natively supported but approximated with minimal retry policies

---

### 2. At-Least-Once

**Guarantee:**
Every message is delivered one or more times until successfully processed.

**How It Works:**
The sender retries on failure (e.g., no acknowledgment). The receiver may process duplicates, so deduplication (via unique message IDs) or idempotent operations are often required.

**Pros:**
- High reliability; ensures no messages are lost
- Simpler than exactly-once, as it doesn't require full transactional coordination

**Cons:**
- Risk of duplicates, necessitating idempotency or deduplication
- Increased network and processing load due to retries

**Use Cases:**
Critical systems where message loss is unacceptable, such as payment processing or order management (with deduplication).

**Examples:**
- **Kafka**: Default mode for producers without exactly-once semantics; consumers use offsets to track progress
- **RabbitMQ**: Default with consumer acknowledgments
- **AWS SQS**: Standard queues guarantee at-least-once delivery

---

### 3. Exactly-Once

**Guarantee:**
Each message is delivered and processed exactly one time—no duplicates, no misses.

**How It Works:**
- Senders assign unique message IDs
- Receivers track IDs to detect and filter duplicates
- Transactions, checkpoints, or idempotent operations ensure single processing, even with retries

**Pros:**
- Highest reliability; no duplicates or losses
- Ideal for systems requiring strict correctness

**Cons:**
- High latency due to coordination overhead (e.g., transactions or two-phase commits)
- Most complex to implement and maintain
- May reduce throughput compared to other guarantees

**Use Cases:**
Financial transactions, inventory updates, or any system where duplicates or losses cause significant issues.

**Examples:**
- **Kafka**: Supports exactly-once semantics (EOS) since version 0.11.0 using idempotent producers and transactional messaging
- **RabbitMQ**: Achievable with publisher confirms and careful consumer acknowledgment configuration
- **AWS SQS**: FIFO queues with deduplication approximate exactly-once delivery

---

### Comparison Table

| Guarantee     | Reliability | Complexity | Performance | Deduplication Needed | Use Case Example        |
| ------------- | ----------- | ---------- | ----------- | -------------------- | ----------------------- |
| At-Most-Once  | Low         | Low        | High        | No                   | Logging, telemetry      |
| At-Least-Once | Medium      | Medium     | Medium      | Often                | Payments, order systems |
| Exactly-Once  | High        | High       | Low         | Sometimes            | Financial transactions  |

---

### Additional Considerations

**Effective-Once:**
A practical approach where systems aim for exactly-once but rely on idempotency to handle duplicates, reducing complexity. Common in real-world implementations.

**Ordering Guarantees:**
Some systems (e.g., Kafka with partitions, SQS FIFO) ensure in-order processing, which can interact with delivery guarantees.

**System-Specific Support:**

- **Kafka**: Supports all three guarantees (at-most-once with acks=0, at-least-once by default, exactly-once with EOS)
- **AWS SQS**: Standard queues are at-least-once; FIFO queues approximate exactly-once with deduplication
- **RabbitMQ**: Primarily at-least-once; exactly-once possible with careful configuration

**Implementation Techniques:**

- **Idempotency**: Design operations to be safe for repeats (e.g., setting a value to "X")
- **Deduplication**: Store processed message IDs to filter duplicates
- **Transactional Outbox**: Store messages in a database transaction with business operations, then forward reliably
- **Two-Phase Commit**: Coordinate sender and receiver to agree on delivery state

---

### Choosing a Guarantee

**Considerations:**

- **Use Case**: Critical operations require at-least-once or exactly-once; non-critical scenarios can use at-most-once
- **System Support**: Verify native capabilities (e.g., Kafka EOS vs. SQS FIFO)
- **Trade-offs**: Balance reliability, latency, throughput, and implementation complexity

**Best Practice:**
For robust systems, prioritize idempotency and deduplication to simplify achieving at-least-once or exactly-once guarantees. Tools like Kafka's EOS, SQS FIFO, or transactional outboxes are battle-tested solutions.

---

## Transactional Outbox Pattern

### What is Transactional Outbox?

A **Transactional Outbox** is a design pattern used in distributed systems to ensure reliable message delivery, often to achieve **at-least-once** or **exactly-once** delivery guarantees. It addresses the challenge of coordinating database updates and message publishing in a consistent way, preventing message loss or duplication even in the face of failures.

---

### How It Works

#### 1. Store Message with Business Operation

When a business operation updates the database (e.g., creating an order), the system also stores a message describing the event (e.g., "OrderCreated") in an **outbox table** within the same database transaction.

**Key Point:**
This ensures that the database update and message are committed atomically—if the transaction fails, neither is persisted.

#### 2. Reliable Message Forwarding

A separate process (e.g., a background job or event relay) polls the outbox table for new messages.

**Process:**
- Reads unprocessed messages
- Publishes them to a message broker (e.g., Kafka, RabbitMQ) or downstream system
- Once the message is successfully published, the outbox record is marked as processed or deleted (often in another transaction)

#### 3. Deduplication (Optional)

To support **exactly-once** delivery:
- The message includes a unique ID
- Downstream consumers deduplicate based on this ID to avoid processing duplicates
- Alternatively, idempotent operations ensure repeated messages don't cause unintended effects

---

### Benefits

**Consistency:**
Guarantees that a message is published if and only if the corresponding database operation succeeds.

**Reliability:**
Prevents message loss by persisting messages in the database before publishing.

**Simplified Error Handling:**
Failures in message publishing don't affect the database transaction; the outbox relay retries until successful.

**Exactly-Once Support:**
When combined with deduplication or idempotency, it enables exactly-once semantics.

---

### Challenges

**Additional Complexity:**
Requires managing an outbox table and a polling/relay process.

**Latency:**
Slight delay between the database transaction and message publishing due to polling.

**Scalability:**
Polling the outbox table can become a bottleneck in high-throughput systems, requiring optimization (e.g., batching, partitioning).

**Cleanup:**
Outbox table may grow large if processed messages aren't regularly deleted.

---

### Example Workflow

1. **E-commerce app creates an order:**
   - Database transaction:
     - Insert order into `orders` table
     - Insert "OrderCreated" event into `outbox` table with a unique ID
   - Transaction commits

2. **Background process:**
   - Polls the `outbox` table
   - Finds the "OrderCreated" event
   - Publishes it to Kafka

3. **After successful publication:**
   - Process marks the outbox record as processed

4. **Downstream services:**
   - Consume the Kafka message
   - Use the unique ID to deduplicate if needed

---

### Use Cases

**Event-Driven Systems:**
Publishing domain events (e.g., "OrderPlaced", "PaymentProcessed") to trigger downstream actions.

**Microservices:**
Ensuring reliable communication between services without direct coupling.

**Exactly-Once Delivery:**
When paired with deduplication, supports strict delivery guarantees in systems like payment processing or inventory management.

---

### Examples in Practice

**Kafka:**
Often used with the transactional outbox pattern to publish events reliably. Kafka's exactly-once semantics (EOS) can enhance this.

**Debezium:**
A tool that implements the outbox pattern by capturing database changes (including outbox table entries) and streaming them to Kafka.

**AWS SQS:**
Outbox pattern ensures messages are reliably sent to SQS, with FIFO queues for deduplication if exactly-once is needed.

**Custom Implementations:**
Many systems use a database (e.g., PostgreSQL, MySQL) with a simple outbox table and a cron job or daemon to relay messages.

---

### Implementation Tips

**Schema:**
Outbox table typically includes columns for message ID, type, payload, status, and timestamp.

**Polling:**
Use efficient queries (e.g., `SELECT ... FOR UPDATE SKIP LOCKED` in PostgreSQL) to avoid contention.

**Idempotency:**
Ensure downstream consumers can handle duplicates or use unique message IDs for deduplication.

**Monitoring:**
Track outbox processing lag and failures to ensure timely message delivery.

---

## Summary

**Message Delivery Guarantees:**
- **At-Most-Once**: Fastest, simplest, but messages can be lost
- **At-Least-Once**: Reliable, but may have duplicates
- **Exactly-Once**: Most reliable, but most complex

**Transactional Outbox:**
A robust pattern for reliable messaging, especially in microservices or event-driven architectures. It ensures atomicity between database operations and message publishing.

**Key Takeaways:**
- Choose guarantee based on system requirements
- Implement idempotency for at-least-once systems
- Use transactional outbox for reliable message delivery
- Monitor and optimize outbox processing
- Balance reliability, performance, and complexity

Understanding these concepts is essential for designing reliable distributed systems.
