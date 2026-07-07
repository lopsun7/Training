# Homework 21 - Messaging Systems, Kafka, SQS, SNS, and RabbitMQ

## 1. Point-to-Point vs Publish-Subscribe Model

### Point-to-Point

In a point-to-point model, a producer sends a message to a queue, and one message is normally consumed by only one consumer.

Example:

```text
Producer -> Queue -> Consumer A
                  -> Consumer B
```

If Consumer A receives one message, Consumer B will not receive the same message. Multiple consumers can share the work, but each individual message is processed once by one consumer in the consumer group.

Pros:

- Good for task distribution and workload balancing.
- Easy to scale consumers horizontally.
- Useful when each message should trigger one job, such as sending one email or processing one order.
- Helps decouple producer and consumer because the producer does not need to know which worker handles the message.

Cons:

- Not suitable when multiple independent systems all need the same event.
- If a message is consumed by one consumer, other consumers do not automatically get a copy.
- If the queue is not configured carefully, retry and dead-letter handling can become messy.

Common examples:

- AWS SQS
- RabbitMQ work queue
- Kafka topic with one consumer group

### Publish-Subscribe

In a publish-subscribe model, a producer publishes an event to a topic, and multiple subscribers can receive the same event.

Example:

```text
Producer -> Topic -> Email Service
                 -> Inventory Service
                 -> Analytics Service
```

Each subscriber receives its own copy of the event.

Pros:

- Good for event broadcasting.
- One event can trigger multiple downstream systems.
- Strong decoupling because the publisher does not need to know all subscribers.
- Useful for event-driven architecture.

Cons:

- More complex to trace because one event can trigger many systems.
- Duplicate processing and idempotency become important.
- If too many subscribers depend on the same event, schema changes must be managed carefully.

Common examples:

- AWS SNS
- Kafka topic with multiple consumer groups
- RabbitMQ fanout/topic exchange

## 2. Why Use a Message System?

We use a message system to decouple services and make the system more scalable, reliable, and asynchronous.

Without a message system, Service A may call Service B directly:

```text
Order Service -> Payment Service -> Email Service -> Inventory Service
```

If one downstream service is slow or down, the whole request can fail or become slow.

With a message system:

```text
Order Service -> Message Queue -> Payment Consumer
                             -> Email Consumer
                             -> Inventory Consumer
```

Benefits:

- Decoupling: producers and consumers do not need to know each other directly.
- Async processing: the producer can return quickly instead of waiting for slow jobs.
- Scalability: consumers can be scaled independently.
- Reliability: messages can be retried if a consumer fails.
- Traffic buffering: queues can absorb spikes when traffic suddenly increases.
- Event-driven design: multiple services can react to the same business event.

Example in an online shopping system:

When a user places an order, the order service can publish an `OrderCreated` event. Payment, inventory, shipping, email, and analytics services can consume the event independently.

## 3. Kafka Architecture

Kafka is a distributed event streaming platform. It is designed for high-throughput, durable, and scalable message processing.

Main components:

- Producer: sends messages to Kafka topics.
- Consumer: reads messages from Kafka topics.
- Topic: a logical message stream, such as `order-created`.
- Partition: a topic is split into partitions for parallelism and scalability.
- Broker: a Kafka server that stores partitions and serves producer/consumer requests.
- Cluster: a group of Kafka brokers.
- Consumer Group: a group of consumers that share work for the same topic.
- Offset: the position of a message inside a partition.
- Replication: partitions can have replicas on multiple brokers for fault tolerance.
- Controller: manages broker and partition leadership.
- ZooKeeper or KRaft: older Kafka versions use ZooKeeper for metadata; newer Kafka can use KRaft mode without ZooKeeper.

Architecture example:

```text
Producer
   |
   v
Kafka Topic: order-created
   |-- Partition 0 -> Broker 1
   |-- Partition 1 -> Broker 2
   |-- Partition 2 -> Broker 3
   |
Consumer Group: payment-service
   |-- Consumer A reads Partition 0
   |-- Consumer B reads Partition 1
   |-- Consumer C reads Partition 2

Consumer Group: analytics-service
   |-- Independently reads the same topic from its own offsets
```

Important ideas:

- Kafka stores messages on disk.
- Consumers track offsets, so they can resume from where they stopped.
- One partition can only be consumed by one consumer in the same consumer group at a time.
- Different consumer groups can independently consume the same data.
- Kafka is pull-based: consumers pull messages from brokers.

## 4. Message Accumulated in Kafka: Consumer Cannot Consume Data on Time

This situation is usually called consumer lag. It means producers are writing faster than consumers are reading.

I would handle it step by step.

### Step 1: Check Metrics

First, I would check:

- consumer lag
- producer throughput
- consumer throughput
- partition count
- consumer group size
- processing time per message
- broker CPU, memory, disk I/O, and network
- error/retry rate in consumers

### Step 2: Scale Consumers

If the topic has enough partitions, I can add more consumers in the same consumer group.

Example:

```text
Topic has 12 partitions -> consumer group can use up to 12 active consumers
```

But if the topic only has 3 partitions, adding 10 consumers will not help much because only 3 consumers can actively read at the same time.

### Step 3: Increase Partition Count

If the bottleneck is parallelism, I can increase the number of partitions. More partitions allow more consumers to process messages in parallel.

I need to be careful because increasing partitions can affect message ordering. Kafka only guarantees order inside one partition, not across the whole topic.

### Step 4: Optimize Consumer Processing

I would check whether the consumer is slow because of:

- slow database writes
- external API calls
- synchronous processing
- large message payloads
- too many retries
- inefficient code

Possible fixes:

- batch database writes
- use async processing carefully
- cache repeated lookups
- optimize SQL queries
- avoid slow downstream calls in the consumer path
- tune consumer configs such as `max.poll.records` and `fetch.min.bytes`

### Step 5: Use Retry and Dead Letter Topic

If some bad messages keep failing, they can block progress. I would move repeatedly failing messages to a dead letter topic and continue processing healthy messages.

Example:

```text
order-created -> order-created-retry -> order-created-dlt
```

### Step 6: Temporarily Reduce Producer Rate

If the system is under extreme load, I can apply backpressure or rate limiting on producers. This protects Kafka and consumers from becoming worse.

### Step 7: Check Retention Risk

If lag is too large and messages are close to expiration, I must either increase retention temporarily or speed up consumption quickly. Otherwise old messages may be deleted before consumers read them.

## 5. How Does Kafka Deal with Expired Data?

Kafka stores messages based on retention policies. It does not delete messages immediately after they are consumed. Instead, it deletes or compacts data based on topic configuration.

Common retention settings:

- `retention.ms`: how long Kafka keeps messages.
- `retention.bytes`: maximum size allowed for a partition log.
- `cleanup.policy=delete`: delete old log segments.
- `cleanup.policy=compact`: keep the latest value for each key.
- `cleanup.policy=delete,compact`: combine both strategies.

How expiration works:

Kafka stores data in log segments. When a segment becomes old enough or the partition exceeds the size limit, Kafka deletes eligible old segments. Deletion is based on retention configuration, not consumer acknowledgment.

Important point:

If a consumer is too slow and the data expires before it reads the messages, Kafka will delete those messages. The consumer may then fail with an offset out-of-range issue or skip to the next available offset depending on configuration.

## 6. Data Volume in Kafka

Kafka data volume means how much data is produced, stored, and consumed over time.

Things to calculate:

- message size
- messages per second
- retention time
- replication factor
- compression ratio
- number of topics and partitions

Example:

```text
Average message size = 1 KB
Messages per second = 10,000
Retention = 7 days
Replication factor = 3
```

Daily raw data:

```text
1 KB * 10,000 msg/sec * 86,400 sec = 864 GB per day
```

Seven-day raw data:

```text
864 GB * 7 = 6,048 GB = about 6 TB
```

With replication factor 3:

```text
6 TB * 3 = about 18 TB storage
```

If compression reduces data by 50%, storage may be closer to:

```text
9 TB
```

So when designing Kafka capacity, I would consider both throughput and retention storage.

## 7. How to Calculate Partition Number

Partition count controls Kafka parallelism. More partitions allow higher throughput and more consumers in the same consumer group, but too many partitions increase broker overhead.

A practical way to estimate partition count:

```text
partitions = max(
  required_producer_throughput / throughput_per_partition,
  required_consumer_throughput / throughput_per_partition
)
```

Example:

```text
Required write throughput = 100 MB/s
One partition can handle about 10 MB/s
Minimum partitions for producer throughput = 100 / 10 = 10
```

If consumers need parallelism:

```text
Need 20 consumers working in parallel
Minimum partitions = 20
```

Then I choose:

```text
max(10, 20) = 20 partitions
```

Other factors:

- Number of consumers in the same consumer group.
- Required ordering guarantee.
- Expected future traffic growth.
- Broker count and replication factor.
- Rebalancing overhead.
- File handles and metadata overhead.

Important rule:

Kafka only guarantees order within a partition. If strict ordering is required for each order ID, I should use order ID as the message key so all events for the same order go to the same partition.

## 8. AWS SQS vs RabbitMQ vs Kafka

| Feature | AWS SQS | RabbitMQ | Kafka |
| --- | --- | --- | --- |
| Type | Managed queue service | Message broker | Distributed event streaming platform |
| Main model | Point-to-point queue | Queue, routing, pub-sub via exchanges | Log-based pub-sub with consumer groups |
| Hosting | Fully managed by AWS | Self-managed or managed service | Self-managed, MSK, or Confluent |
| Message retention | Limited retention window | Usually until consumed/expired | Retention-based log storage |
| Ordering | FIFO queue supports ordering | Queue order is possible but depends on setup | Ordered within partition |
| Throughput | Good, easy scaling | Good for traditional messaging | Very high throughput |
| Routing | Simple | Strong routing with exchanges | Topic and partition based |
| Replay old messages | Not the main use case | Not the main use case | Strong support through offsets |
| Consumer model | Polling queue | Push or pull style depending client | Pull-based consumers |
| Best for | Simple async jobs, decoupling AWS services | Complex routing, traditional message broker use cases | Event streaming, analytics, logs, high-scale pipelines |

### AWS SQS

SQS is best when I want a simple, fully managed queue and I do not want to manage servers.

Pros:

- Fully managed and highly available.
- Easy to integrate with AWS Lambda, ECS, EC2, SNS.
- Good for background jobs and decoupling services.
- Supports dead-letter queues.
- Standard queue scales very well.

Cons:

- Not designed for long-term event replay.
- Standard queues can deliver duplicates and do not guarantee strict ordering.
- FIFO queues support ordering but with more limits than standard queues.
- Routing features are simpler than RabbitMQ.

### RabbitMQ

RabbitMQ is best when I need flexible routing patterns and traditional message broker features.

Pros:

- Supports exchanges such as direct, topic, fanout, and headers.
- Good for command/task messaging.
- Strong routing flexibility.
- Mature ecosystem.

Cons:

- Usually needs more operational work than SQS.
- Not ideal for very large event replay use cases.
- Scaling and clustering require careful setup.

### Kafka

Kafka is best when I need durable event streams, high throughput, and replay.

Pros:

- Very high throughput.
- Supports replay by offset.
- Good for event sourcing, analytics, logs, and streaming pipelines.
- Multiple consumer groups can read the same topic independently.
- Durable storage with replication.

Cons:

- More complex to operate.
- Partition planning matters.
- Message ordering is only within a partition.
- Not always the simplest choice for small background jobs.

## 9. AWS SNS vs SQS

AWS SNS and SQS are often used together, but they solve different problems.

### SNS

SNS is a publish-subscribe notification service. A publisher sends one message to a topic, and SNS fans it out to multiple subscribers.

Subscribers can include:

- SQS queues
- Lambda functions
- HTTP endpoints
- email
- SMS

SNS is push-based. It pushes messages to subscribers.

Best for:

- event broadcasting
- fanout
- notifications
- sending one event to multiple systems

### SQS

SQS is a queue service. Producers send messages to a queue, and consumers poll the queue.

SQS is pull-based. Consumers ask SQS for messages.

Best for:

- background jobs
- task queue
- buffering traffic spikes
- decoupling one producer from one worker group

### SNS + SQS Together

A common architecture is:

```text
Order Service -> SNS Topic: order-created
                      |-> SQS Queue: payment-service
                      |-> SQS Queue: inventory-service
                      |-> SQS Queue: email-service
```

This gives both fanout and reliable queueing.

Why this is useful:

- SNS broadcasts the event.
- Each service gets its own SQS queue.
- If one service is down, its messages remain in its own queue.
- Services do not block each other.
- Each consumer can scale independently.

## 10. Interview-Style Summary

If I explain this in an interview, I would say:

Messaging systems help decouple services, smooth traffic spikes, and make processing asynchronous. Point-to-point is good when one message should be processed by one worker, while publish-subscribe is good when one event should be broadcast to multiple independent systems.

Kafka is a distributed log-based event streaming platform. Producers write to topics, topics are split into partitions, brokers store the data, and consumers read messages by offset. Kafka is great for high-throughput event streaming and replay, but it requires careful partition and retention planning.

If Kafka messages accumulate, I would check consumer lag, scale consumers, increase partitions if needed, optimize slow consumer logic, use retry and dead-letter topics, and temporarily control producer rate. Kafka deletes expired data based on retention policies, not based on whether a consumer has read it.

For AWS tools, I would use SQS for simple queues and background jobs, SNS for fanout notifications, SNS plus SQS for reliable publish-subscribe, RabbitMQ for flexible broker routing, and Kafka for durable high-throughput event streams with replay.
