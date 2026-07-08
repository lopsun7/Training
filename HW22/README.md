# Homework 22 - Kafka Integration and Messaging Questions

## Code Implementation

Project repository:

- Spring Boot project: `git@github.com:lopsun7/student-management-system.git`
- Kafka implementation branch/current branch: `main`

What I implemented:

- Integrated Spring Kafka into the Student Management System.
- Added a Docker Kafka cluster with 3 brokers.
- Configured the topic `student-events` with 3 partitions and replication factor 3.
- Implemented a producer that publishes `STUDENT_CREATED` events after a student is created.
- Implemented a consumer with consumer group `student-management-events`.
- Configured listener concurrency as `3`, so the consumer group can process partitions in parallel.
- Added validation endpoints to publish test events and inspect consumed events.
- Added unit tests and an embedded Kafka integration test to validate real produce and consume behavior.

Important files:

- `docker-compose.kafka.yml`
- `KafkaTopicConfig.java`
- `StudentEvent.java`
- `KafkaStudentEventPublisher.java`
- `StudentEventConsumer.java`
- `KafkaValidationController.java`
- `KafkaStudentEventIntegrationTest.java`

Run Kafka locally:

```bash
docker compose -f docker-compose.kafka.yml up -d
```

Run the Spring Boot app with Kafka enabled:

```bash
KAFKA_ENABLED=true \
SPRING_PROFILES_ACTIVE=h2 \
KAFKA_BOOTSTRAP_SERVERS=localhost:9092,localhost:9093,localhost:9094 \
./mvnw spring-boot:run
```

Get token:

```bash
TOKEN=$(curl -s -X POST http://localhost:8080/api/v1/auth/token \
  -H "Content-Type: application/json" \
  -d '{"username":"steven","password":"password123"}' \
  | sed -n 's/.*"accessToken":"\([^"]*\)".*/\1/p')
```

Create a student. This will produce a Kafka message:

```bash
curl -X POST http://localhost:8080/api/v1/students \
  -H "Authorization: Bearer ${TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{
    "firstName": "Steven",
    "lastName": "Zhao",
    "email": "steven.kafka@example.com",
    "course": "Kafka"
  }'
```

Check consumed Kafka events:

```bash
curl http://localhost:8080/api/v1/kafka/consumed-events \
  -H "Authorization: Bearer ${TOKEN}"
```

Validate topic partition and replica setup:

```bash
docker exec kafka-1 kafka-topics \
  --bootstrap-server kafka-1:29092 \
  --describe \
  --topic student-events
```

Expected topic setup:

```text
PartitionCount: 3
ReplicationFactor: 3
```

Test validation:

```bash
./mvnw test
```

Verified result:

```text
Tests run: 50, Failures: 0, Errors: 0, Skipped: 0
JaCoCo instruction coverage: 93.33%
```

## Question List

### 1. Point-to-Point vs Publish-Subscribe Model

Point-to-point means one message is usually handled by one consumer. A producer sends a message to a queue, and workers compete for messages. It is good for workload distribution, background jobs, and task processing. The main advantage is simple horizontal scaling, but the limitation is that other independent systems do not automatically receive the same message.

Publish-subscribe means one event can be delivered to multiple subscribers. A producer publishes to a topic, and many consumer groups or subscribers can receive the same event. It is good for event-driven systems, like order events triggering payment, inventory, email, and analytics. The advantage is strong decoupling, but the tradeoff is more complexity around tracing, schema changes, duplicate processing, and idempotency.

### 2. Why Message System?

We use a message system to decouple services, absorb traffic spikes, and make slow work asynchronous. For example, after an order is created, the order service should not wait synchronously for email, analytics, inventory, and shipping. It can publish an event and return faster. Consumers can process the event independently and retry if something fails.

Message systems also improve scalability because producers and consumers can scale separately. They improve reliability because messages can be persisted and retried instead of being lost when a downstream service is temporarily unavailable.

### 3. Kafka Architecture

Kafka is a distributed event streaming platform. The main components are producers, topics, partitions, brokers, consumers, consumer groups, offsets, and replicas.

A producer writes messages to a topic. A topic is split into partitions. Partitions are stored across brokers. Each partition has one leader and can have replicas on other brokers for fault tolerance. Consumers read messages from partitions and commit offsets so they know where to continue next time.

In one consumer group, each partition is consumed by only one consumer at a time. This is how Kafka provides parallel processing while preserving order inside each partition. Different consumer groups can read the same topic independently.

### 4. Message Accumulated in Kafka, Consumer Cannot Consume All Data on Time

This is consumer lag. I would first check metrics: consumer lag, producer rate, consumer processing rate, partition count, consumer group size, broker CPU, disk I/O, network, and error rate.

If processing is slow, I would scale consumers, but only up to the number of partitions. If the topic has 3 partitions, only 3 consumers in the same group can actively process in parallel. If we need more parallelism, we may increase partitions, but we must understand that ordering is guaranteed only inside one partition.

I would also optimize consumer logic: batch database writes, reduce slow downstream calls, tune `max.poll.records`, improve SQL, and move poison messages to a retry topic or dead-letter topic. If messages may expire before being consumed, I would temporarily increase retention or reduce producer rate.

### 5. How Does Kafka Deal with Expired Data?

Kafka deletes old messages based on retention configuration, not based on whether messages were consumed. Common settings are `retention.ms` for time-based retention and `retention.bytes` for size-based retention.

Kafka stores messages in log segments. When segments become older than the retention time or the partition exceeds the size limit, Kafka deletes eligible old segments. If a consumer is too slow and the data expires, the consumer may hit an offset-out-of-range problem and has to reset based on its `auto.offset.reset` policy.

Kafka can also use log compaction with `cleanup.policy=compact`, which keeps the latest value for each key instead of simply deleting by age.

### 6. Data Volume in Kafka

Kafka data volume means how much data is produced, stored, and consumed over time. I calculate it with message size, messages per second, retention time, replication factor, compression ratio, topic count, and partition count.

Example:

```text
Average message size = 1 KB
Messages per second = 1,000
Retention = 7 days
Replication factor = 3

Raw daily data = 1 KB * 1,000 * 86,400 = about 86.4 GB/day
7-day data = 604.8 GB
With replication factor 3 = about 1.8 TB before compression
```

This estimate helps plan broker disk, network throughput, and retention policy.

### 7. How to Calculate Partition Number?

I calculate partitions based on throughput and parallelism.

If one partition can handle 10 MB/s and the target write throughput is 100 MB/s, I need at least 10 partitions for producer throughput. If one consumer can process 500 messages per second and the topic receives 3,000 messages per second, I need at least 6 partitions so 6 consumers can process in parallel.

I would also consider ordering requirements. If strict order is needed by customer ID or order ID, I choose the message key carefully so related messages go to the same partition. I would not create too many partitions blindly because too many partitions increase metadata, file handles, recovery time, and operational overhead.

### 8. AWS SQS vs RabbitMQ vs Kafka

SQS is a fully managed AWS queue. It is simple, reliable, and good for decoupling cloud services. It is best when I need a managed point-to-point queue without managing servers. Standard SQS gives high throughput with at-least-once delivery, while FIFO SQS gives ordering with lower throughput.

RabbitMQ is a traditional message broker. It supports queues, exchanges, routing keys, acknowledgments, and flexible routing patterns. It is good for task queues and complex routing, but we usually manage the broker ourselves unless using a managed service.

Kafka is an event streaming platform. It is best for high-throughput event pipelines, replayable events, analytics, and multiple consumer groups reading the same event stream. Kafka keeps messages for a retention window, so consumers can replay data by resetting offsets.

### 9. AWS SNS vs SQS

SNS is publish-subscribe. It pushes messages to multiple subscribers, such as SQS queues, Lambda functions, HTTP endpoints, or email. It is good when one event should notify many systems.

SQS is a queue. Consumers pull messages from it, and each message is normally processed by one consumer. It is good for background jobs and workload buffering.

In real AWS systems, SNS and SQS are often used together. For example, an order service publishes `OrderCreated` to SNS, and SNS fan-outs the event to multiple SQS queues. Payment, inventory, email, and analytics services each consume from their own queue.

## My Implementation Explanation

In my project, when a student is created through `POST /api/v1/students`, the service saves the student first and then calls `StudentEventPublisher.publishStudentCreated`. When Kafka is enabled, the implementation is `KafkaStudentEventPublisher`, which sends a `StudentEvent` to the `student-events` topic.

The consumer is `StudentEventConsumer`. It uses `@KafkaListener` with topic `student-events`, group ID `student-management-events`, and concurrency `3`. Since the topic has 3 partitions, the consumer group can process the partitions in parallel.

The topic is created by `KafkaTopicConfig` with 3 partitions and 3 replicas. The Docker compose file starts 3 brokers, so replication factor 3 is valid in the local demo cluster.
