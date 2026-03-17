# Apache Kafka

- [Apache Kafka](#apache-kafka)
  - [Topics](#topics)
  - [Partitions](#partitions)
    - [Key Characteristics of Partitions](#key-characteristics-of-partitions)
  - [Producers](#producers)
    - [Partition selection](#partition-selection)
    - [Producer Acknowledgment Modes](#producer-acknowledgment-modes)
    - [Kafka Message Anatomy](#kafka-message-anatomy)
      - [Message Structure](#message-structure)
        - [Example Message](#example-message)
      - [Message Flow](#message-flow)
    - [Serialization in Kafka](#serialization-in-kafka)
      - [How Serialization Works](#how-serialization-works)
    - [Built-in Serializers](#built-in-serializers)
      - [Common Serialization Formats](#common-serialization-formats)
      - [Configuration Example](#configuration-example)
      - [Important Notes](#important-notes)
    - [Hashing in Kafka](#hashing-in-kafka)
      - [Default Hashing Algorithm](#default-hashing-algorithm)
      - [How It Works](#how-it-works)
        - [Example](#example)
      - [Default Partitioner Behavior](#default-partitioner-behavior)
      - [Hash Collisions](#hash-collisions)
      - [Custom Hashing](#custom-hashing)
      - [Important Considerations](#important-considerations)
    - [Producer Timeouts and Retries](#producer-timeouts-and-retries)
      - [Producer Timeout Properties](#producer-timeout-properties)
      - [delivery.timeout.ms](#deliverytimeoutms)
      - [Producer Batching: batch.size and linger.ms](#producer-batching-batchsize-and-lingerms)
      - [Producer Buffer Management: buffer.memory and max.block.ms](#producer-buffer-management-buffermemory-and-maxblockms)
      - [Producer Retries](#producer-retries)
      - [Retry Behavior](#retry-behavior)
      - [Timeout and Retry Relationship](#timeout-and-retry-relationship)
      - [Best Practices](#best-practices)
    - [Idempotent Producers](#idempotent-producers)
      - [The Problem: Duplicate Messages](#the-problem-duplicate-messages)
      - [How Idempotence Works](#how-idempotence-works)
      - [Enabling Idempotence](#enabling-idempotence)
      - [enable.idempotence Configuration](#enableidempotence-configuration)
      - [Required Settings for Idempotence](#required-settings-for-idempotence)
      - [Trade-offs](#trade-offs)
      - [When to Use Idempotent Producers](#when-to-use-idempotent-producers)
  - [Consumers](#consumers)
    - [Consumer Responsibilities](#consumer-responsibilities)
    - [Consumer Groups](#consumer-groups)
    - [Partition Rebalancing](#partition-rebalancing)
      - [What Happens When a Consumer Joins](#what-happens-when-a-consumer-joins)
      - [Rebalancing Protocols](#rebalancing-protocols)
        - [Eager Rebalance (Stop-the-World)](#eager-rebalance-stop-the-world)
        - [Incremental Cooperative Rebalance](#incremental-cooperative-rebalance)
    - [Partition Assignment Strategies](#partition-assignment-strategies)
      - [RangeAssignor](#rangeassignor)
      - [RoundRobinAssignor](#roundrobinassignor)
      - [StickyAssignor](#stickyassignor)
      - [CooperativeStickyAssignor](#cooperativestickyassignor)
      - [Assignment Strategy Comparison](#assignment-strategy-comparison)
    - [Static Group Membership](#static-group-membership)
      - [Problem with Dynamic Membership](#problem-with-dynamic-membership)
      - [How Static Membership Works](#how-static-membership-works)
      - [Static vs Dynamic Membership](#static-vs-dynamic-membership)
      - [Use Cases for Static Membership](#use-cases-for-static-membership)
    - [Multiple Consumer Groups](#multiple-consumer-groups)
    - [Deserialization](#deserialization)
    - [How Consumers Know What to Read](#how-consumers-know-what-to-read)
      - [1. Topic Subscription](#1-topic-subscription)
      - [2. Partition Assignment (Automatic)](#2-partition-assignment-automatic)
      - [3. Offset Management (Commit Strategies)](#3-offset-management-commit-strategies)
        - [Automatic Commit (default)](#automatic-commit-default)
        - [Manual Commit (more control)](#manual-commit-more-control)
        - [Message Delivery Semantics](#message-delivery-semantics)
          - [At-Most-Once Delivery](#at-most-once-delivery)
          - [At-Least-Once Delivery (Default)](#at-least-once-delivery-default)
          - [Exactly-Once Semantics (EOS)](#exactly-once-semantics-eos)
          - [Comparison Table](#comparison-table)
          - [Choosing the Right Strategy](#choosing-the-right-strategy)
      - [4. Offset Storage](#4-offset-storage)
    - [Consumer Lifecycle](#consumer-lifecycle)
    - [Consumer Example](#consumer-example)
    - [Consumer Liveliness](#consumer-liveliness)
      - [Consumer Coordinator](#consumer-coordinator)
      - [Heartbeat Thread](#heartbeat-thread)
        - [heartbeat.interval.ms](#heartbeatintervalms)
        - [session.timeout.ms](#sessiontimeoutms)
        - [Heartbeat Thread Workflow](#heartbeat-thread-workflow)
      - [Poll Thread](#poll-thread)
        - [max.poll.interval.ms](#maxpollintervalms)
        - [max.poll.records](#maxpollrecords)
        - [fetch.min.bytes](#fetchminbytes)
        - [fetch.max.wait.ms](#fetchmaxwaitms)
        - [max.partition.fetch.bytes](#maxpartitionfetchbytes)
        - [fetch.max.bytes](#fetchmaxbytes)
        - [Poll Thread Workflow](#poll-thread-workflow)
      - [How Heartbeat and Poll Threads Work Together](#how-heartbeat-and-poll-threads-work-together)
      - [Consumer Failure Detection Scenarios](#consumer-failure-detection-scenarios)
      - [Configuration Best Practices](#configuration-best-practices)
      - [Troubleshooting Common Issues](#troubleshooting-common-issues)
    - [Important Consumer Considerations](#important-consumer-considerations)
  - [How They Work Together](#how-they-work-together)
  - [Brokers](#brokers)
    - [Brokers and Partitions](#brokers-and-partitions)
    - [Consumer Replica Fetching (Kafka 2.4+)](#consumer-replica-fetching-kafka-24)
      - [Traditional Model (Before 2.4)](#traditional-model-before-24)
      - [New Model (Kafka 2.4+)](#new-model-kafka-24)
      - [Understanding ISR (In-Sync Replicas)](#understanding-isr-in-sync-replicas)
      - [Rack Awareness](#rack-awareness)
      - [Replica Selector](#replica-selector)
      - [Consumer Configuration: client.rack](#consumer-configuration-clientrack)
      - [Producers and Consumers in Same vs. Different Datacenters](#producers-and-consumers-in-same-vs-different-datacenters)
        - [Scenario 1: Everything in Same Datacenter (Simplest)](#scenario-1-everything-in-same-datacenter-simplest)
        - [Scenario 2: Producers and Consumers in Same DC, Brokers Distributed](#scenario-2-producers-and-consumers-in-same-dc-brokers-distributed)
        - [Scenario 3: Brokers in One DC, Consumers in Another (Anti-pattern Before 2.4)](#scenario-3-brokers-in-one-dc-consumers-in-another-anti-pattern-before-24)
        - [Scenario 4: Multi-Region Active-Active (Optimal with 2.4+)](#scenario-4-multi-region-active-active-optimal-with-24)
        - [Scenario 5: Disaster Recovery (DR) Setup](#scenario-5-disaster-recovery-dr-setup)
      - [Complete Configuration Example](#complete-configuration-example)
      - [Benefits Summary](#benefits-summary)
      - [Limitations and Considerations](#limitations-and-considerations)
      - [Troubleshooting](#troubleshooting)
      - [Best Practices](#best-practices-1)
    - [Horizontal Scaling with Brokers](#horizontal-scaling-with-brokers)
    - [Bootstrap Servers and Discovery](#bootstrap-servers-and-discovery)
    - [ZooKeeper and Controller (Cluster Coordination)](#zookeeper-and-controller-cluster-coordination)
  - [Banking Use Case: Async Transfers with Kafka](#banking-use-case-async-transfers-with-kafka)
    - [The Problem: Consistency vs. Latency](#the-problem-consistency-vs-latency)
    - [The Transactional Outbox Pattern](#the-transactional-outbox-pattern)
      - [Outbox Table Schema](#outbox-table-schema)
      - [Application Code (Spring Boot)](#application-code-spring-boot)
    - [Single-Writer Principle](#single-writer-principle)
    - [Full Bank Transfer Flow](#full-bank-transfer-flow)
      - [HTTP Status Codes for Async Operations](#http-status-codes-for-async-operations)
    - [Idempotency on the Consumer Side](#idempotency-on-the-consumer-side)
      - [Strategy: Deduplication Table](#strategy-deduplication-table)
      - [Partition Key for Ordering](#partition-key-for-ordering)
    - [How to Relay the Outbox Event to Kafka (Without External Tools)](#how-to-relay-the-outbox-event-to-kafka-without-external-tools)
      - [The `@TransactionalEventListener` + Outbox Pattern](#the-transactionaleventlistener--outbox-pattern)
      - [Why this is the right answer](#why-this-is-the-right-answer)
    - [Interview Summary](#interview-summary)

Apache Kafka is a distributed event streaming platform designed for high-throughput, fault-tolerant, real-time data processing. Originally developed by LinkedIn and now maintained by the Apache Software Foundation, it's widely used for building real-time data pipelines and streaming applications.

## Topics

A **topic** is a category or feed name to which records are published. Topics in Kafka are:

- **Logical channels** for organizing data streams
- **Multi-subscriber**: Multiple consumers can read from the same topic independently
- **Append-only logs**: Messages are immutable once written
- **Persistent**: Data is stored on disk and retained based on configured retention policies
- **Named entities**: Each topic has a unique name within a Kafka cluster

A topic is like a table in a database or a folder in a filesystem: it's a way to organize and categorize your data streams.

## Partitions

**Partitions** are the fundamental unit of parallelism and scalability in Kafka:

- **Physical division**: Each topic is split into one or more partitions
- **Ordered sequence**: Within a partition, messages maintain strict ordering
- **Distributed storage**: Partitions are distributed across different brokers (servers) in the cluster
- **Independent logs**: Each partition is an ordered, immutable sequence of records
- **Scalability mechanism**: More partitions = more parallelism for both reading and writing

### Key Characteristics of Partitions

1. **Ordering guarantees**: Messages within a single partition are strictly ordered, but ordering across partitions is not guaranteed

2. **Partition key**: When producing messages, you can specify a key. Messages with the same key always go to the same partition, ensuring ordering for related events

3. **Offset**: Each message within a partition has a unique sequential ID called an offset, which acts as its position in the partition

4. **Replication**: Each partition can be replicated across multiple brokers for fault tolerance. One broker acts as the "leader" and others as "followers"

5. **Consumer parallelism**: Each partition can be consumed by only one consumer within a consumer group, so the number of partitions determines the maximum parallelism for consumption

## Producers

**Producers** are client applications that publish (write) records to Kafka topics. They are responsible for:

1. **Sending messages**: Publishing data to one or more Kafka topics
2. **Partition selection**: Determining which partition a message should go to
3. **Serialization**: Converting objects into byte arrays for transmission
4. **Batching**: Grouping messages together for efficient network transmission
5. **Error handling**: Retrying failed sends and handling acknowledgments

### Partition selection

When a producer sends a message, it must decide which partition to write to:

**With a key (key != null)**:

- Kafka uses a hash function on the key to determine the partition
- Formula: `partition = hash(key) % number_of_partitions`
- Same key always goes to the same partition, guaranteeing ordering for related messages
- Example: All messages with `user_id=123` go to the same partition

**Without a key (key == null)**:

Kafka uses a **round-robin** or **sticky partitioning** strategy:

- **Round-robin** (older behavior): Distributes messages evenly across all partitions in rotation
- **Sticky partitioning** (default since Kafka 2.4): Fills a batch for one partition before switching to another, improving batching efficiency
  - No ordering guarantees across messages
  - Better for load balancing when order doesn't matter

**Custom partitioner**:

- You can implement custom logic to control partition assignment
- Useful for specific business requirements (e.g., geographic distribution)

### Producer Acknowledgment Modes

Producers can configure how many acknowledgments they require:

- **acks=0**: Fire and forget (no acknowledgment, fastest but least safe)
- **acks=1**: Leader acknowledgment only (balanced)
- **acks=all**: All in-sync replicas must acknowledge (slowest but safest)

### Kafka Message Anatomy

Each Kafka message (also called a **record**) consists of several components:

#### Message Structure

1. **Key** (optional, byte array)
   - Used for partition assignment and message ordering
   - Can be null (no key) or any serialized data (string, integer, JSON, etc.)
   - Messages with the same key go to the same partition
   - Enables logical grouping of related events

2. **Value** (required, byte array)
   - The actual payload/content of the message
   - Can be any format: JSON, Avro, Protobuf, plain text, binary data
   - This is the primary data you want to transmit

3. **Timestamp** (long)
   - When the message was created
   - Can be set by the producer (CreateTime) or when the broker receives it (LogAppendTime)
   - Used for time-based processing and retention policies

4. **Headers** (optional, key-value pairs)
   - Metadata about the message
   - Array of key-value pairs (both strings or byte arrays)
   - Used for tracing, routing, or carrying additional context
   - Example: `{"source": "mobile-app", "version": "2.1", "correlation-id": "abc-123"}`

5. **Partition** (integer)
   - Which partition within the topic this message belongs to
   - Assigned by the producer based on key or partitioning strategy
   - Determines physical storage location

6. **Offset** (long)
   - Unique sequential ID within the partition
   - Assigned by Kafka broker when the message is written
   - Acts as the message's permanent position in the partition log
   - Used by consumers to track their progress

7. **Compression** (codec type)
   - Messages can be compressed to save space and bandwidth
   - Supported codecs: none, gzip, snappy, lz4, zstd
   - Typically applied to batches of messages, not individual ones
   - Configured at producer level

##### Example Message

```sh
Topic: "user-events"
Partition: 2
Offset: 1543
Timestamp: 1701964800000
Key: "user-123"
Value: {"action": "login", "timestamp": "2025-12-07T10:00:00Z"}
Headers: {"source": "web-app", "ip": "192.168.1.1"}
Compression: lz4
```

#### Message Flow

```md
1. Producer creates a message with key and value
2. Producer serializes the data
3. Producer determines target partition (based on key or strategy)
4. Message is compressed (if enabled)
5. Broker receives the message and assigns an offset
6. Broker appends timestamp (if not provided)
7. Message is written to the partition log on disk
8. Message is replicated to follower brokers
```

### Serialization in Kafka

**Serialization** is the process of converting objects/data structures into byte arrays for transmission over the network. Kafka only works with byte arrays, so everything must be serialized.

#### How Serialization Works

**Producer Side:**

1. You have data in your application (String, Integer, JSON object, custom Java object, etc.)
2. The producer uses a **Serializer** to convert it to bytes
3. Serialized bytes are sent over the network to Kafka brokers

**Consumer Side:**

1. Consumer receives byte arrays from Kafka
2. Uses a **Deserializer** to convert bytes back to the original data type
3. Application processes the reconstructed data

### Built-in Serializers

Kafka provides several serializers out of the box:

- **StringSerializer**: Converts strings to UTF-8 byte arrays
- **IntegerSerializer**: Converts integers to 4-byte arrays
- **LongSerializer**: Converts longs to 8-byte arrays
- **ByteArraySerializer**: No-op, already bytes
- **DoubleSerializer**: Converts doubles to byte arrays
- **ShortSerializer**: Converts shorts to byte arrays

#### Common Serialization Formats

**JSON Serialization:**

```java
// Using a JSON library like Jackson or Gson
ObjectMapper mapper = new ObjectMapper();
String jsonString = mapper.writeValueAsString(userObject);
byte[] bytes = jsonString.getBytes("UTF-8");
```

**Avro Serialization:**

- Binary format with schema evolution support
- More compact than JSON
- Schema registry keeps track of schemas
- Popular choice for production systems

**Protobuf (Protocol Buffers):**

- Google's binary serialization format
- Very efficient and compact
- Requires schema definition (.proto files)

**Custom Serializers:**

```java
public class CustomSerializer implements Serializer<MyObject> {
    @Override
    public byte[] serialize(String topic, MyObject data) {
        // Your custom serialization logic
        return data.toByteArray();
    }
}
```

#### Configuration Example

```java
Properties props = new Properties();
props.put("key.serializer", "org.apache.kafka.common.serialization.StringSerializer");
props.put("value.serializer", "org.apache.kafka.common.serialization.StringSerializer");
```

#### Important Notes

- **Both key and value** must be serialized separately
- Producers and consumers must use **compatible** serializers/deserializers
- Schema changes can break compatibility (schema registry helps manage this)
- Serialization adds overhead - choose efficient formats for high-throughput scenarios

### Hashing in Kafka

**Hashing** is used to deterministically assign messages to partitions based on the message key.

#### Default Hashing Algorithm

Kafka uses the **MurmurHash2** algorithm (non-cryptographic hash function):

```java
partition = murmur2(keyBytes) % number_of_partitions
```

Why MurmurHash2?

- **Fast**: Optimized for speed, not cryptographic security
- **Good distribution**: Spreads keys evenly across partitions
- **Deterministic**: Same input always produces same output
- **Non-cryptographic**: No need for security in partition assignment

#### How It Works

1. **Key is serialized** to byte array (e.g., "user-123" → `[117, 115, 101, 114, 45, 49, 50, 51]`)
2. **MurmurHash2** is applied to the byte array, producing a 32-bit integer
3. **Modulo operation** maps the hash to a partition number
4. Same key always produces the same hash, ensuring consistent partition assignment

##### Example

```sh
Key: "user-123"
Serialized bytes: [117, 115, 101, 114, 45, 49, 50, 51]
Hash (murmur2): 2147483647
Number of partitions: 3
Partition: 2147483647 % 3 = 2

Result: Message goes to partition 2
```

#### Default Partitioner Behavior

**With key:**

```java
// DefaultPartitioner source code (simplified)
public int partition(String topic, Object key, byte[] keyBytes,
                     Object value, byte[] valueBytes, Cluster cluster) {
    if (keyBytes == null) {
        // Sticky partitioning for null keys
        return stickyPartitionCache.partition(topic, cluster);
    }
    
    // Hash-based partitioning for non-null keys
    int numPartitions = cluster.partitionCountForTopic(topic);
    return Utils.toPositive(Utils.murmur2(keyBytes)) % numPartitions;
}
```

#### Hash Collisions

- Different keys can hash to the same partition (this is normal and expected)
- Within a partition, messages are still ordered by arrival
- The goal is even distribution, not uniqueness

#### Custom Hashing

You can implement a custom partitioner with your own hashing logic:

```java
public class CustomPartitioner implements Partitioner {
    @Override
    public int partition(String topic, Object key, byte[] keyBytes,
                        Object value, byte[] valueBytes, Cluster cluster) {
        // Custom logic, e.g., geographic partitioning
        if (key.toString().startsWith("EU-")) {
            return 0;  // European users to partition 0
        } else if (key.toString().startsWith("US-")) {
            return 1;  // US users to partition 1
        }
        // Default hashing for others
        return Utils.toPositive(Utils.murmur2(keyBytes)) % cluster.partitionCountForTopic(topic);
    }
}
```

#### Important Considerations

- **Don't change the number of partitions** if using key-based partitioning - it will change the hash distribution and break ordering guarantees
- **Null keys** are not hashed - they use sticky partitioning instead
- The hash is computed on the **serialized byte array**, not the original object
- Different serializers for the same logical key may produce different hashes

### Producer Timeouts and Retries

Kafka producers have built-in timeout and retry mechanisms to handle transient failures and ensure reliable message delivery.

#### Producer Timeout Properties

Kafka producers have several timeout-related configurations:

1. **delivery.timeout.ms** (default: 120,000 ms = 2 minutes)
   - Upper bound on the time to report success or failure after `send()` is called
   - Includes time for retries, serialization, partitioning, compression, and waiting for acknowledgments
   - Should be >= `linger.ms + request.timeout.ms`

2. **request.timeout.ms** (default: 30,000 ms = 30 seconds)
   - Maximum time the client waits for a response from the broker for a single request
   - Does not include retries
   - If no response within this time, the request fails or is retried

3. **linger.ms** (default: 0 ms)
   - How long to wait before sending a batch to allow more messages to accumulate
   - Increases throughput by batching but adds latency

4. **max.block.ms** (default: 60,000 ms = 1 minute)
   - How long `send()` and `partitionsFor()` will block when the buffer is full
   - Blocks the calling thread, not the network thread

#### delivery.timeout.ms

**delivery.timeout.ms** is the most important timeout configuration for producers. It controls the total time from when `send()` is called until the producer either receives an acknowledgment or gives up.

**What it includes:**

```md
delivery.timeout.ms =
  Time waiting in buffer (linger.ms)
  + Time for serialization/compression
  + Time for partitioner to assign partition
  + Time sending to broker (request.timeout.ms)
  + Time for retries (retries × retry.backoff.ms)
  + Time waiting for acknowledgment
```

**Configuration Example:**

```java
Properties props = new Properties();
props.put("bootstrap.servers", "localhost:9092");
props.put("key.serializer", "org.apache.kafka.common.serialization.StringSerializer");
props.put("value.serializer", "org.apache.kafka.common.serialization.StringSerializer");

// Set delivery timeout to 2 minutes
props.put("delivery.timeout.ms", 120000);

// Set request timeout to 30 seconds
props.put("request.timeout.ms", 30000);

// Set linger time to 10ms for better batching
props.put("linger.ms", 10);

KafkaProducer<String, String> producer = new KafkaProducer<>(props);
```

**Behavior:**

- If the message cannot be delivered within `delivery.timeout.ms`, the producer returns a **TimeoutException**
- The producer will automatically retry failed requests within this timeout window
- Once the timeout expires, no more retries will be attempted

#### Producer Batching: batch.size and linger.ms

Kafka producers batch multiple messages together to improve throughput and efficiency. Two key properties control batching behavior:

**batch.size** (default: 16,384 bytes = 16 KB)

- Maximum size in bytes of a batch of messages sent to a single partition
- The producer groups messages destined for the same partition into batches
- Once a batch reaches this size, it's sent immediately (regardless of `linger.ms`)
- Does not limit individual message size - messages larger than `batch.size` are sent individually
- Larger batch sizes improve throughput but use more memory

**linger.ms** (default: 0 ms)

- Time to wait for additional messages before sending a batch
- If set to 0 (default), batches are sent as soon as possible
- Higher values allow more messages to accumulate, improving batching efficiency
- Adds artificial latency but increases throughput

**How Batching Works:**

```md
Producer receives messages → Accumulates in batch buffer
                                      |
                                      v
                    Batch is sent when EITHER condition is met:
                    1. batch.size limit reached (e.g., 16 KB)
                    2. linger.ms timeout expires (e.g., 10 ms)
                                      |
                                      v
                    Batch sent to broker → Acknowledgment received
```

**Example Scenarios:**

**Scenario 1: batch.size reached first**

```md
Settings: batch.size=16KB, linger.ms=100ms

Time 0ms:   Message 1 (5 KB) arrives → Added to batch
Time 20ms:  Message 2 (6 KB) arrives → Added to batch (total: 11 KB)
Time 40ms:  Message 3 (7 KB) arrives → Added to batch (total: 18 KB)
            ↳ Batch size exceeds 16 KB → Batch sent immediately!
            (linger.ms timer not used)
```

**Scenario 2: linger.ms expires first**

```md
Settings: batch.size=16KB, linger.ms=10ms

Time 0ms:   Message 1 (2 KB) arrives → Added to batch, timer starts
Time 5ms:   Message 2 (3 KB) arrives → Added to batch (total: 5 KB)
Time 10ms:  No more messages, linger.ms expires
            ↳ Batch sent with 5 KB (below batch.size limit)
```

**Configuration Example:**

```java
Properties props = new Properties();
props.put("bootstrap.servers", "localhost:9092");
props.put("key.serializer", "org.apache.kafka.common.serialization.StringSerializer");
props.put("value.serializer", "org.apache.kafka.common.serialization.StringSerializer");

// Configure batching for high throughput
props.put("batch.size", 32768);  // 32 KB batches
props.put("linger.ms", 10);       // Wait up to 10ms for more messages

// Buffer memory for all batches (across all partitions)
props.put("buffer.memory", 33554432);  // 32 MB total buffer

KafkaProducer<String, String> producer = new KafkaProducer<>(props);
```

**Relationship Between batch.size and linger.ms:**

| batch.size | linger.ms | Behavior | Use Case |
|------------|-----------|----------|----------|
| 16 KB (default) | 0 (default) | Send immediately when possible | Low latency, default behavior |
| 32 KB | 0 | Send when batch fills | Balance latency and throughput |
| 16 KB | 10-100 ms | Wait for more messages | Higher throughput, acceptable latency |
| 64 KB | 10-20 ms | Maximum batching | Bulk data ingestion, high throughput |
| 16 KB | 1000 ms | Aggressive batching | Batch processing, latency not critical |

**Memory Considerations:**

Batches consume memory from the producer's buffer:

```md
Total memory usage ≈ batch.size × number_of_active_partitions

Example:
- batch.size = 32 KB
- Writing to 100 partitions
- Memory needed ≈ 32 KB × 100 = 3.2 MB (minimum)

Actual memory controlled by buffer.memory (default: 32 MB)
```

**buffer.memory** (default: 33,554,432 bytes = 32 MB)

- Total memory available for buffering unsent messages
- Shared across all partitions and batches
- When full, `send()` will block up to `max.block.ms` before throwing an exception
- Should be large enough to handle: `batch.size × active_partitions + overhead`

**Tuning Guidelines:**

**For Low Latency (Real-time applications):**

```java
props.put("batch.size", 16384);    // 16 KB (default)
props.put("linger.ms", 0);         // No waiting (default)
props.put("compression.type", "none");  // No compression overhead

// Messages sent immediately, minimal batching
// Latency: < 5 ms
// Throughput: Moderate
```

**For High Throughput (Bulk data, analytics):**

```java
props.put("batch.size", 65536);    // 64 KB
props.put("linger.ms", 10);        // Wait 10ms for more messages
props.put("compression.type", "lz4");  // Fast compression
props.put("buffer.memory", 67108864);  // 64 MB buffer

// More efficient batching, better compression
// Latency: 10-20 ms
// Throughput: High (3-5x improvement)
```

**For Balanced Performance (Most applications):**

```java
props.put("batch.size", 32768);    // 32 KB
props.put("linger.ms", 5);         // Wait 5ms
props.put("compression.type", "snappy");  // Good compression/speed ratio

// Good balance of latency and throughput
// Latency: 5-10 ms
// Throughput: Good (2-3x improvement)
```

**Monitoring Batching Performance:**

Key metrics to track:

1. **batch-size-avg**: Average batch size in bytes
   - Should be close to `batch.size` for high throughput scenarios
   - Low values indicate poor batching efficiency

2. **record-queue-time-avg**: Average time messages wait in buffer
   - Should be close to `linger.ms` when batches fill regularly
   - High values may indicate backpressure or slow brokers

3. **records-per-request-avg**: Average number of records per request
   - Higher is better for throughput
   - Low values (< 10) suggest poor batching

4. **compression-rate-avg**: Compression ratio achieved
   - Only relevant if compression is enabled
   - Higher values indicate better compression

**Common Mistakes:**

❌ **Setting batch.size too small:**

```java
props.put("batch.size", 1024);  // Only 1 KB - too small!
// Result: Many small requests, poor throughput, high CPU usage
```

❌ **Setting linger.ms too high for latency-sensitive apps:**

```java
props.put("linger.ms", 1000);  // 1 second delay!
// Result: Unacceptable latency for real-time applications
```

❌ **Not considering buffer.memory with many partitions:**

```java
props.put("batch.size", 65536);  // 64 KB
props.put("buffer.memory", 32768);  // Only 32 KB total!
// Result: Buffer fills immediately, frequent blocking
```

✅ **Correct Configuration:**

```java
props.put("batch.size", 32768);         // 32 KB batches
props.put("linger.ms", 10);             // 10ms batching window
props.put("buffer.memory", 67108864);   // 64 MB (ample space)
props.put("compression.type", "lz4");   // Fast compression
```

**Best Practices:**

1. **Start with defaults** (batch.size=16KB, linger.ms=0) and tune based on metrics

2. **Increase batch.size for high-throughput scenarios**: 32-64 KB works well for most cases

3. **Add small linger.ms for throughput**: 5-20ms typically provides good batching with acceptable latency

4. **Enable compression**: Use `lz4` or `snappy` for better network utilization with batching

5. **Scale buffer.memory with partitions**: Ensure `buffer.memory >= batch.size × active_partitions × 2`

6. **Test under load**: Measure `batch-size-avg` and `records-per-request-avg` to verify batching is effective

7. **Consider producer lifecycle**: Long-lived producers benefit more from batching than short-lived ones

#### Producer Buffer Management: buffer.memory and max.block.ms

Kafka producers use an internal memory buffer to queue messages before sending them to brokers. Two critical properties control this buffer behavior: `buffer.memory` and `max.block.ms`.

**buffer.memory** (default: 33,554,432 bytes = 32 MB)

- Total amount of memory the producer can use for buffering messages waiting to be sent
- Shared across **all partitions and batches** the producer is writing to
- Acts as a pool that holds unsent messages until they can be transmitted
- Independent of heap memory settings

**max.block.ms** (default: 60,000 ms = 1 minute)

- Maximum time `send()` and `partitionsFor()` methods will **block** when the buffer is full
- Applies to the calling thread (your application thread), not the I/O thread
- After this timeout, a `TimeoutException` is thrown
- Provides backpressure mechanism to prevent overwhelming the producer

**How They Work Together:**

```md
Application Thread calls producer.send(record)
          ↓
    Is buffer full?
          ↓
    ┌─────┴─────┐
    │           │
   NO          YES
    │           │
    │      Block thread up to
    │      max.block.ms waiting
    │      for buffer space
    │           │
    │      ┌────┴────┐
    │      │         │
    │   Space      Timeout
    │   freed       expires
    │      │         │
    └──────┴─────────┘
           │
    Add to buffer
           │
    Return Future
```

**Buffer Memory Allocation:**

```md
buffer.memory (32 MB total)
├── Batch for Topic A, Partition 0: 16 KB
├── Batch for Topic A, Partition 1: 16 KB
├── Batch for Topic A, Partition 2: 16 KB
├── Batch for Topic B, Partition 0: 16 KB
├── Batch for Topic B, Partition 1: 16 KB
├── Compression buffer: ~2 MB
├── Metadata overhead: ~1 MB
└── Free space: ~29 MB (available for more batches)

When buffer fills:
- send() blocks for up to max.block.ms
- Waiting for I/O thread to send batches and free space
```

**What Causes Buffer to Fill:**

1. **Slow network/brokers**: Messages produced faster than they can be sent
2. **High message volume**: Burst of messages exceeds network capacity
3. **Too many partitions**: Writing to hundreds of partitions simultaneously
4. **Large messages**: Big messages consume more buffer space
5. **Broker unavailability**: Network issues prevent sending
6. **Insufficient buffer.memory**: Buffer too small for workload

**Blocking Behavior Example:**

```java
Properties props = new Properties();
props.put("buffer.memory", 1048576);  // Only 1 MB (very small!)
props.put("max.block.ms", 5000);      // Block max 5 seconds
props.put("batch.size", 16384);       // 16 KB batches

KafkaProducer<String, String> producer = new KafkaProducer<>(props);

try {
    for (int i = 0; i < 10000; i++) {
        String largeMessage = generateLargeMessage(10000); // 10 KB each
        
        // This will block when buffer fills!
        Future<RecordMetadata> future = producer.send(
            new ProducerRecord<>("my-topic", "key-" + i, largeMessage)
        );
        
        // After 5 seconds of blocking, throws TimeoutException
    }
} catch (BufferExhaustedException e) {
    System.err.println("Buffer full and max.block.ms exceeded!");
    // Handle backpressure: slow down, retry, alert, etc.
}
```

**Scenario Analysis:**

**Scenario 1: Buffer sufficient**

```md
Settings:
- buffer.memory = 64 MB
- batch.size = 32 KB
- Producing to 10 partitions
- Message rate: 1000 msg/sec
- Network can keep up

Behavior:
✅ Messages queue in buffer
✅ Batches sent as they fill
✅ Buffer never fills
✅ send() returns immediately
✅ No blocking occurs

Memory usage: ~10 partitions × 32 KB = ~320 KB (plenty of headroom)
```

**Scenario 2: Buffer fills temporarily**

```md
Settings:
- buffer.memory = 8 MB
- batch.size = 64 KB  
- Producing to 50 partitions
- Burst: 10,000 msg/sec for 5 seconds
- Network temporarily slow

Behavior:
⚠️ Buffer fills during burst
⚠️ send() blocks for ~2 seconds
⚠️ Network catches up
⚠️ Buffer drains
✅ send() resumes normal operation

Memory usage: Peaks at 8 MB, then drains as network sends batches
```

**Scenario 3: Buffer exhausted**

```md
Settings:
- buffer.memory = 4 MB
- batch.size = 16 KB
- Producing to 100 partitions
- Sustained rate: 5000 msg/sec
- Broker down or network failure

Behavior:
❌ Buffer fills completely
❌ send() blocks for max.block.ms (60s)
❌ Broker still unavailable
❌ TimeoutException thrown
❌ Messages not sent

Result: Application must handle exception (retry logic, alerting, etc.)
```

**Calculating Required buffer.memory:**

```md
Formula:
buffer.memory >= (batch.size × active_partitions) + overhead

Example 1 - Small scale:
- Writing to 10 partitions
- batch.size = 16 KB
- Minimum: 10 × 16 KB = 160 KB
- Recommended: 160 KB × 4 = 640 KB (4× safety margin)
- Use: 1 MB or default 32 MB

Example 2 - Medium scale:
- Writing to 100 partitions
- batch.size = 32 KB
- Minimum: 100 × 32 KB = 3.2 MB
- Recommended: 3.2 MB × 4 = 12.8 MB
- Use: 16 MB or 32 MB

Example 3 - Large scale:
- Writing to 1000 partitions
- batch.size = 64 KB
- Minimum: 1000 × 64 KB = 64 MB
- Recommended: 64 MB × 4 = 256 MB
- Use: 256 MB or 512 MB

Overhead typically includes:
- Compression buffers (~2-5 MB)
- Metadata structures (~1-2 MB)
- In-flight requests (~2-5 MB)
```

**Configuration Examples:**

**For Low Partition Count (< 20 partitions):**

```java
props.put("buffer.memory", 33554432);    // 32 MB (default is fine)
props.put("max.block.ms", 60000);        // 1 minute (default)
props.put("batch.size", 16384);          // 16 KB

// Total buffer needed: ~20 × 16 KB = 320 KB
// 32 MB provides ample headroom
```

**For High Partition Count (100-500 partitions):**

```java
props.put("buffer.memory", 67108864);    // 64 MB
props.put("max.block.ms", 30000);        // 30 seconds (faster failure)
props.put("batch.size", 32768);          // 32 KB

// Total buffer needed: ~500 × 32 KB = 16 MB
// 64 MB provides 4× safety margin
```

**For Very High Partition Count (1000+ partitions):**

```java
props.put("buffer.memory", 268435456);   // 256 MB
props.put("max.block.ms", 10000);        // 10 seconds (fail fast)
props.put("batch.size", 65536);          // 64 KB

// Total buffer needed: ~1000 × 64 KB = 64 MB
// 256 MB provides 4× safety margin for bursts
```

**For Bursty Workloads:**

```java
props.put("buffer.memory", 134217728);   // 128 MB (handle bursts)
props.put("max.block.ms", 5000);         // 5 seconds (fail quickly)
props.put("linger.ms", 0);               // Send immediately
props.put("batch.size", 16384);          // Smaller batches

// Large buffer absorbs bursts
// Short max.block.ms detects problems quickly
```

**Monitoring Buffer Health:**

Key metrics to track:

1. **buffer-available-bytes**: Current free space in buffer
   - Should generally be > 50% of buffer.memory
   - Low values indicate buffer pressure

2. **buffer-total-bytes**: Total buffer memory configured
   - Should match your buffer.memory setting

3. **buffer-exhausted-rate**: How often buffer fills completely
   - Should be 0 in healthy system
   - Non-zero indicates undersized buffer or network issues

4. **bufferpool-wait-time-total**: Time threads spent blocked
   - Should be minimal
   - High values indicate frequent blocking

5. **produce-throttle-time-avg**: Time broker throttled requests
   - Can cause buffer to fill
   - Indicates quota limits or broker load

**Exception Handling:**

```java
KafkaProducer<String, String> producer = new KafkaProducer<>(props);

try {
    // Attempt to send
    Future<RecordMetadata> future = producer.send(record);
    
    // Wait for result
    RecordMetadata metadata = future.get();
    
} catch (BufferExhaustedException e) {
    // Buffer full and max.block.ms exceeded during send()
    logger.error("Producer buffer exhausted", e);
    
    // Handle backpressure:
    // Option 1: Slow down production
    Thread.sleep(1000);
    
    // Option 2: Drop message (if acceptable)
    logger.warn("Message dropped due to buffer pressure");
    
    // Option 3: Store to local queue for retry
    retryQueue.add(record);
    
    // Option 4: Alert operations
    alerting.sendAlert("Kafka producer buffer exhausted");
    
} catch (TimeoutException e) {
    // Couldn't get metadata or partitionsFor() timed out
    logger.error("Timeout while blocking on buffer", e);
    
} catch (ExecutionException e) {
    // Other errors during send
    logger.error("Failed to send message", e.getCause());
}
```

**Best Practices:**

1. **Size buffer appropriately**: Use formula `buffer.memory >= 4 × (batch.size × partitions)`

2. **Monitor buffer metrics**: Track `buffer-available-bytes` to detect pressure early

3. **Set reasonable max.block.ms**: 
   - Long timeout (60s+): For batch jobs, can wait for network recovery
   - Short timeout (5-10s): For real-time apps, fail fast and handle

4. **Handle TimeoutException**: Always implement retry logic or degradation strategy

5. **Don't set buffer too large**: Wastes memory that could be used elsewhere

6. **Consider partition count**: More partitions = more memory needed

7. **Account for bursts**: If workload is bursty, size buffer for peak load

8. **Test under load**: Simulate production traffic to verify buffer sizing

**Common Mistakes:**

❌ **Buffer too small for partition count:**

```java
props.put("buffer.memory", 1048576);  // 1 MB
// But writing to 100 partitions with 32 KB batches
// Needs: 100 × 32 KB = 3.2 MB minimum
// Result: Constant blocking and timeouts
```

❌ **max.block.ms too long:**

```java
props.put("max.block.ms", 300000);  // 5 minutes!
// Result: Application freezes for 5 minutes on network issues
// Better: Fail fast (10-30s) and handle gracefully
```

❌ **Not handling exceptions:**

```java
// Dangerous - ignores buffer exhaustion!
producer.send(record);  // Fire and forget
// Result: Silent message loss when buffer fills
```

❌ **Ignoring buffer metrics:**

```java
// No monitoring of buffer-available-bytes
// Result: Buffer fills gradually, then sudden failures
// Better: Alert when available bytes < 20%
```

✅ **Correct Configuration:**

```java
// Writing to 50 partitions
int partitionCount = 50;
int batchSize = 32768;  // 32 KB
int safetyMargin = 4;

// Calculate buffer size
long minBuffer = partitionCount * batchSize;
long recommendedBuffer = minBuffer * safetyMargin;

props.put("buffer.memory", recommendedBuffer);  // ~6.4 MB
props.put("max.block.ms", 10000);  // Fail fast at 10 seconds
props.put("batch.size", batchSize);

// Always handle exceptions
try {
    producer.send(record).get();
} catch (Exception e) {
    handleFailure(record, e);
}

// Monitor buffer health
logger.info("Buffer available: " + producer.metrics()
    .get("buffer-available-bytes"));
```

**Relationship with Other Properties:**

```md
buffer.memory works with:

1. batch.size:
   - Each batch consumes buffer space
   - More partitions × larger batches = more memory needed

2. linger.ms:
   - Longer linger = messages wait in buffer longer
   - Can cause buffer to fill if linger.ms is high

3. compression.type:
   - Compression needs temporary buffer space
   - Add 2-5 MB overhead for compression buffers

4. max.in.flight.requests.per.connection:
   - More in-flight requests = more buffer usage
   - Each request holds batches in memory until acked

5. delivery.timeout.ms:
   - Messages stay in buffer until delivered or timeout
   - Longer timeout = potentially more buffer usage
```

**When to Increase buffer.memory:**

- ✅ Writing to many partitions (100+)
- ✅ Large batch sizes (64 KB+)
- ✅ Bursty traffic patterns
- ✅ High message rate (10,000+ msg/sec)
- ✅ Seeing frequent `buffer-exhausted-rate` > 0
- ✅ Logs show TimeoutException from send()
- ✅ Application threads blocked frequently

**When to Decrease max.block.ms:**

- ✅ Real-time applications requiring fast failure
- ✅ Better user experience with graceful degradation
- ✅ Want to detect broker issues quickly
- ✅ Implementing circuit breaker pattern
- ✅ Can handle failures at application level

#### Producer Retries

Kafka producers automatically retry failed requests to handle transient network issues and temporary broker failures.

**Key Retry Properties:**

1. **retries** (default: Integer.MAX_VALUE in Kafka 2.1+)
   - Maximum number of retry attempts for a failed request
   - In older versions (< 2.1), default was 0 (no retries)
   - Set to a high value or Integer.MAX_VALUE to rely on `delivery.timeout.ms` instead

2. **retry.backoff.ms** (default: 100 ms)
   - Time to wait between retry attempts
   - Prevents overwhelming the broker with rapid retries
   - Exponential backoff is not used by default (constant wait time)

3. **max.in.flight.requests.per.connection** (default: 5)
   - Maximum number of unacknowledged requests the client will send on a single connection
   - Setting this to 1 ensures strict ordering (but reduces throughput)
   - With idempotence enabled, can be up to 5 while maintaining ordering

**Configuration Example:**

```java
Properties props = new Properties();

// Set maximum retries (default is already very high)
props.put("retries", Integer.MAX_VALUE);

// Set retry backoff to 200ms
props.put("retry.backoff.ms", 200);

// Maximum 5 in-flight requests (default)
props.put("max.in.flight.requests.per.connection", 5);
```

#### Retry Behavior

**Retriable Errors:**

The producer automatically retries for these transient errors:

- Network timeouts
- Broker not available
- Leader not available (during leader election)
- Not enough replicas
- Request timed out on broker

**Non-Retriable Errors:**

These errors cause immediate failure without retries:

- Message too large
- Invalid topic or partition
- Authorization failures
- Serialization errors
- Message exceeds max size

**Example with Callbacks:**

```java
ProducerRecord<String, String> record = 
    new ProducerRecord<>("my-topic", "key", "value");

producer.send(record, new Callback() {
    @Override
    public void onCompletion(RecordMetadata metadata, Exception exception) {
        if (exception != null) {
            if (exception instanceof RetriableException) {
                // This was retried but ultimately failed
                System.err.println("Failed after retries: " + exception.getMessage());
            } else {
                // This failed immediately (non-retriable)
                System.err.println("Non-retriable failure: " + exception.getMessage());
            }
        } else {
            System.out.println("Message sent successfully to partition " + 
                             metadata.partition() + " at offset " + metadata.offset());
        }
    }
});
```

#### Timeout and Retry Relationship

```md
delivery.timeout.ms controls the OVERALL time limit
└── Multiple retries can happen within this window
    ├── Retry 1: Send → Wait (retry.backoff.ms) → Retry if failed
    ├── Retry 2: Send → Wait (retry.backoff.ms) → Retry if failed
    ├── Retry N: Send → Wait (retry.backoff.ms) → Retry if failed
    └── Stop when either:
        • Success (acknowledgment received)
        • delivery.timeout.ms expires
        • Non-retriable error occurs
```

**Important Notes:**

- Setting `retries=0` disables retries completely (not recommended)
- Setting `retries` to a high value is safe because `delivery.timeout.ms` acts as the upper bound
- Each retry consumes time from the `delivery.timeout.ms` budget

#### Best Practices

1. **Use defaults for most cases**: The default settings (high retries, 2-minute delivery timeout) work well for most applications

2. **Adjust delivery.timeout.ms based on your SLA**: If you need faster failure detection, reduce it; if you want more resilience, increase it

3. **Don't set retries to 0**: Always allow retries unless you have a specific reason not to

4. **Monitor retry metrics**: Track `record-retry-rate` and `record-error-rate` metrics to detect issues

5. **Use callbacks or Futures**: Always handle the result of `send()` to detect failures:

   ```java
   // Using Future
   Future<RecordMetadata> future = producer.send(record);
   try {
       RecordMetadata metadata = future.get(); // Block until complete
   } catch (ExecutionException e) {
       // Handle failure
   }
   
   // Using Callback (better for async)
   producer.send(record, (metadata, exception) -> {
       if (exception != null) {
           // Handle failure
       }
   });
   ```

6. **Balance throughput and ordering**: If strict ordering is critical, set `max.in.flight.requests.per.connection=1` or enable idempotence

### Idempotent Producers

**Idempotent producers** ensure that messages are delivered exactly once to a particular partition, even in the presence of retries. This prevents duplicate messages caused by network failures or broker crashes.

#### The Problem: Duplicate Messages

Without idempotence, retries can cause duplicates:

```md
Scenario: Network timeout during acknowledgment

1. Producer sends message "A" to broker
2. Broker successfully writes message "A" to partition
3. Broker sends acknowledgment back to producer
4. Network failure: acknowledgment is lost
5. Producer times out and retries
6. Producer sends message "A" again
7. Broker writes duplicate message "A" to partition

Result: Message "A" is written twice with different offsets
```

**Impact:**

- Consumers may process the same message multiple times
- Analytics and aggregations may be incorrect
- Idempotency violations in downstream systems

#### How Idempotence Works

Kafka uses a combination of **Producer ID** and **sequence numbers** to detect and reject duplicates:

1. **Producer ID (PID)**: Each producer is assigned a unique ID by the broker
2. **Sequence Number**: Each message from a producer to a partition gets an incremental sequence number
3. **Broker tracking**: Brokers track the last sequence number received from each producer for each partition
4. **Duplicate detection**: If a broker receives a message with a sequence number it has already seen, it acknowledges it but doesn't write it again

**Message Flow with Idempotence:**

```md
1. Producer requests a Producer ID (PID) from broker
   → Broker assigns PID: 12345

2. Producer sends message with metadata:
   {
     key: "user-1",
     value: "login",
     PID: 12345,
     epoch: 0,
     sequence: 0
   }

3. Broker writes message and remembers: PID 12345, Partition 0, Sequence 0

4. Acknowledgment is lost due to network issue

5. Producer retries with same metadata:
   {
     key: "user-1",
     value: "login",
     PID: 12345,
     epoch: 0,
     sequence: 0  ← Same sequence number!
   }

6. Broker detects duplicate (sequence 0 already processed)
   → Sends acknowledgment without writing duplicate
   → Producer receives success response

Result: Message written exactly once, even though it was sent twice
```

#### Enabling Idempotence

Idempotence is enabled with a single configuration:

```java
Properties props = new Properties();
props.put("bootstrap.servers", "localhost:9092");
props.put("key.serializer", "org.apache.kafka.common.serialization.StringSerializer");
props.put("value.serializer", "org.apache.kafka.common.serialization.StringSerializer");

// Enable idempotence
props.put("enable.idempotence", "true");

KafkaProducer<String, String> producer = new KafkaProducer<>(props);
```

**Note:** In Kafka 3.0+, idempotence is **enabled by default** (`enable.idempotence=true`).

#### enable.idempotence Configuration

When you enable idempotence, Kafka automatically adjusts related settings to ensure safety:

**Automatic Configuration Changes:**

| Property | Default Value | With Idempotence | Reason |
|----------|--------------|------------------|--------|
| `acks` | 1 | **all** | Ensures all in-sync replicas confirm write |
| `retries` | 0 (old) / MAX_INT (new) | **MAX_INT** | Allows unlimited retries within timeout |
| `max.in.flight.requests.per.connection` | 5 | **≤ 5** | Maintains ordering with sequence numbers |

**Why these changes?**

1. **acks=all**: Ensures durability by requiring all ISR replicas to acknowledge
2. **retries=MAX_INT**: Relies on `delivery.timeout.ms` for retry limits instead of retry count
3. **max.in.flight.requests ≤ 5**: Allows pipelining while maintaining order using sequence numbers

#### Required Settings for Idempotence

If you manually configure producer settings, idempotence requires:

```java
Properties props = new Properties();
props.put("enable.idempotence", "true");

// These are required for idempotence
props.put("acks", "all");  // Must be "all" or "-1"
props.put("retries", Integer.MAX_VALUE);  // Must be > 0
props.put("max.in.flight.requests.per.connection", 5);  // Must be ≤ 5
```

**If you set incompatible values**, Kafka will throw a `ConfigException`:

```java
// This will fail!
props.put("enable.idempotence", "true");
props.put("acks", "1");  // ERROR: acks must be 'all' with idempotence
```

**Valid Configurations:**

✅ **Option 1: Simple (recommended)**

```java
props.put("enable.idempotence", "true");
// That's it! Other settings are auto-configured
```

✅ **Option 2: Explicit**

```java
props.put("enable.idempotence", "true");
props.put("acks", "all");
props.put("retries", Integer.MAX_VALUE);
props.put("max.in.flight.requests.per.connection", 5);
```

❌ **Invalid Configuration:**

```java
props.put("enable.idempotence", "true");
props.put("acks", "1");  // ERROR
props.put("max.in.flight.requests.per.connection", 10);  // ERROR
```

#### Trade-offs

**Benefits:**

- ✅ **Exactly-once delivery** to each partition (no duplicates)
- ✅ **Message ordering** preserved within a partition
- ✅ **Safe retries** without duplication risk
- ✅ **Minimal overhead** (just Producer ID and sequence numbers)

**Costs:**

- ⚠️ **Slightly lower throughput** due to `acks=all` requirement
- ⚠️ **Higher latency** due to waiting for all ISR acknowledgments
- ⚠️ **Broker state** required to track sequence numbers per producer
- ⚠️ **Producer ID lifecycle** management (though transparent to application)

**Performance Impact:**

```md
Typical latency increase: 5-20ms per message
Throughput reduction: 10-30% compared to acks=1

This is often acceptable for the guarantee of no duplicates.
```

#### When to Use Idempotent Producers

**Use Idempotence When:**

- ✅ Duplicate messages would cause incorrect results (financial transactions, billing, inventory)
- ✅ You need exactly-once semantics within Kafka
- ✅ Message ordering per key is critical
- ✅ You want safe retries without duplication risk
- ✅ You're building event sourcing or CQRS systems
- ✅ Downstream consumers are not idempotent

**Skip Idempotence When:**

- ❌ Duplicates are acceptable (logging, monitoring, best-effort scenarios)
- ❌ You need absolute maximum throughput at any cost
- ❌ You're using Kafka < 0.11 (idempotence not available)
- ❌ Downstream systems handle deduplication themselves

**Best Practice:**

For most production applications, **enable idempotence**. The overhead is minimal, and the safety guarantees are valuable. Since Kafka 3.0+, it's enabled by default, reflecting the community's recommendation.

```java
// Modern best practice (Kafka 3.0+)
Properties props = new Properties();
props.put("bootstrap.servers", "localhost:9092");
props.put("key.serializer", StringSerializer.class.getName());
props.put("value.serializer", StringSerializer.class.getName());
// enable.idempotence defaults to true (no need to set explicitly)

KafkaProducer<String, String> producer = new KafkaProducer<>(props);
```

## Consumers

**Consumers** are client applications that read (consume) records from Kafka topics. They subscribe to topics and process the messages in real-time or batch mode.

### Consumer Responsibilities

1. **Subscribing to topics**: Declaring which topics to read from
2. **Fetching messages**: Pulling data from Kafka brokers
3. **Deserialization**: Converting byte arrays back to usable objects
4. **Offset management**: Tracking which messages have been processed
5. **Partition assignment**: Determining which partitions to read from
6. **Error handling**: Managing failures and retries

### Consumer Groups

A **consumer group** is a set of consumers working together to consume a topic. This is the key mechanism for scalability and fault tolerance:

**Key Concepts:**

- Each consumer in a group has a unique `group.id`
- **Partitions are assigned to consumers** within the group
- Each partition is consumed by **exactly one consumer** in the group
- If there are more consumers than partitions, some consumers will be idle
- If a consumer fails, its partitions are reassigned to other consumers (rebalancing)

**Example:**

```md
Topic: "orders" with 4 partitions
Consumer Group: "order-processors" with 2 consumers

Assignment:
- Consumer 1: reads from partitions 0, 1
- Consumer 2: reads from partitions 2, 3
```

**Scaling Scenarios:**

| Consumers | Partitions | Result |
|-----------|------------|--------|
| 1 | 4 | Consumer reads all 4 partitions |
| 2 | 4 | Each consumer reads 2 partitions |
| 4 | 4 | Each consumer reads 1 partition (ideal) |
| 6 | 4 | 4 consumers active, 2 idle |

### Partition Rebalancing

**Partition rebalancing** is the process of redistributing partition ownership among consumers in a consumer group when the group membership changes.

#### What Happens When a Consumer Joins

When a new consumer joins a consumer group, Kafka triggers a rebalancing process:

**Basic Flow:**

```md
Initial State (2 consumers, 4 partitions):
Consumer A: partitions 0, 1
Consumer B: partitions 2, 3

Consumer C joins the group:
1. Group coordinator detects new member
2. Rebalancing is triggered
3. All consumers STOP consuming (in eager rebalance)
4. Partitions are reassigned using the assignment strategy
5. Consumers resume with new assignments

New State (3 consumers, 4 partitions):
Consumer A: partition 0
Consumer B: partition 1
Consumer C: partitions 2, 3
```

**Rebalancing Triggers:**

- Consumer joins the group
- Consumer leaves gracefully (via `close()`)
- Consumer is considered dead (missed heartbeats)
- Consumer exceeds `max.poll.interval.ms`
- Partitions added to subscribed topics
- Topic subscription changes

#### Rebalancing Protocols

Kafka supports two rebalancing protocols with different characteristics:

##### Eager Rebalance (Stop-the-World)

The traditional rebalancing protocol used by older assignment strategies.

**How It Works:**

```md
1. Coordinator signals: "Rebalance starting"
2. ALL consumers revoke ALL their partitions immediately
3. Consumers rejoin the group
4. New partition assignment is computed
5. Partitions are reassigned to consumers
6. Consumers start consuming from new assignments
```

**Timeline Example:**

```md
t=0s: Consumer C joins
t=1s: Rebalance triggered, all consumers stop
t=2s: All partitions revoked
t=3s: New assignment calculated
t=4s: Partitions reassigned
t=5s: All consumers resume

Total downtime: ~5 seconds for ALL consumers
```

**Characteristics:**

- ❌ **Complete downtime**: All consumers stop processing during rebalance
- ❌ **Slow**: Must wait for all consumers to stop and rejoin
- ❌ **Inefficient**: Even consumers keeping their partitions must stop
- ❌ **Stop-the-world**: No consumption happens during rebalance
- ✅ **Simple**: Easier to implement and reason about
- ✅ **Proven**: Well-tested, stable protocol

**Used By:**

- `RangeAssignor`
- `RoundRobinAssignor`
- `StickyAssignor`

##### Incremental Cooperative Rebalance

Modern protocol introduced in Kafka 2.4+ that minimizes downtime.

**How It Works:**

```md
1. Coordinator signals: "Rebalance starting"
2. Only partitions being MOVED are revoked
3. Consumers keep consuming from stable partitions
4. Revoked partitions are reassigned
5. Second short rebalance to finalize (if needed)
```

**Two-Phase Process:**

**Phase 1 - Identify and revoke partitions to be moved:**

```md
Consumer A: keeps partitions 0, 1 (continues consuming)
Consumer B: revokes partition 3 (stops only this partition)
Consumer C: ready to receive partitions
```

**Phase 2 - Assign revoked partitions:**

```md
Consumer A: keeps partitions 0, 1 (never stopped)
Consumer B: keeps partition 2 (never stopped)
Consumer C: receives partition 3
```

**Timeline Example:**

```md
t=0.0s: Consumer C joins
t=0.5s: Phase 1 - identify partitions to move (partition 3)
t=1.0s: Only partition 3 stops consuming
t=1.5s: Phase 2 - partition 3 assigned to Consumer C
t=2.0s: All consumers active

Downtime: ~0.5s for partition 3 only, others never stopped
```

**Characteristics:**

- ✅ **Minimal downtime**: Only moving partitions stop briefly
- ✅ **Fast**: Most consumers continue working throughout
- ✅ **Efficient**: Preserves existing assignments where possible
- ✅ **Graceful**: No stop-the-world pause
- ⚠️ **Two rebalances**: But each is very short
- ⚠️ **Requires support**: All consumers must use compatible assignor

**Used By:**

- `CooperativeStickyAssignor`

**Visual Comparison:**

```md
EAGER REBALANCE:
[All consumers working] → [ALL STOP - nobody consuming] → [All restart]
                          ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
                          Complete downtime period

COOPERATIVE REBALANCE:
[All consumers working] → [Most keep working continuously] → [All working]
                          [Only partition 3 pauses briefly]
                          ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
                          Minimal targeted downtime
```

### Partition Assignment Strategies

The `partition.assignment.strategy` configuration determines how partitions are distributed among consumers in a group.

#### RangeAssignor

**Default strategy** - assigns partitions in contiguous ranges.

**Algorithm:**

```java
// For each topic independently:
// 1. Sort partitions numerically (0, 1, 2, ...)
// 2. Sort consumers alphabetically
// 3. Divide partitions into ranges
// 4. Assign each range to a consumer
```

**Example:**

```md
Topic "orders" with 10 partitions (0-9), 3 consumers:

Consumer-1: partitions 0, 1, 2, 3    (4 partitions)
Consumer-2: partitions 4, 5, 6       (3 partitions)
Consumer-3: partitions 7, 8, 9       (3 partitions)
```

**Multi-Topic Problem:**

```md
Consumer A and B both subscribe to: topic1, topic2, topic3
Each topic has 3 partitions

RangeAssignor assigns PER TOPIC:

Consumer A:
- topic1: partitions 0, 1 (2/3)
- topic2: partitions 0, 1 (2/3)
- topic3: partitions 0, 1 (2/3)
Total: 6 partitions

Consumer B:
- topic1: partition 2 (1/3)
- topic2: partition 2 (1/3)
- topic3: partition 2 (1/3)
Total: 3 partitions

Result: Imbalanced! Consumer A has 2× the load
```

**Configuration:**

```java
props.put("partition.assignment.strategy", 
    "org.apache.kafka.clients.consumer.RangeAssignor");
```

**Characteristics:**

- Uses **eager rebalance** (stop-the-world)
- Works per-topic independently
- Can cause imbalance with multiple topics
- Predictable partition assignment
- Default strategy (for backward compatibility)

#### RoundRobinAssignor

**Distributes partitions evenly** across all consumers in round-robin fashion.

**Algorithm:**

```java
// 1. Create sorted list of ALL topic-partitions across ALL topics
// 2. Sort consumers alphabetically
// 3. Assign partitions one-by-one to consumers in rotation
```

**Example:**

```md
Topic "orders" (5 partitions) + "payments" (3 partitions)
2 consumers

Sorted partitions: orders-0, orders-1, orders-2, orders-3, orders-4,
                   payments-0, payments-1, payments-2

Consumer-1: orders-0, orders-2, orders-4, payments-1
Consumer-2: orders-1, orders-3, payments-0, payments-2

Result: Balanced! Each consumer has 4 partitions
```

**Configuration:**

```java
props.put("partition.assignment.strategy", 
    "org.apache.kafka.clients.consumer.RoundRobinAssignor");
```

**Characteristics:**

- Uses **eager rebalance** (stop-the-world)
- Considers all topics together
- Better balance than RangeAssignor
- Requires all consumers subscribe to the same topics
- Even distribution of load

#### StickyAssignor

**Minimizes partition movement** during rebalancing by preserving existing assignments.

**Goals:**

1. Balance partitions across consumers as evenly as possible
2. Preserve as many existing assignments as possible during rebalance

**Example - Consumer Failure:**

```md
Initial State (9 partitions, 3 consumers):
Consumer A: 0, 1, 2
Consumer B: 3, 4, 5
Consumer C: 6, 7, 8

Consumer B fails:

Eager RangeAssignor would reassign everything:
Consumer A: 0, 1, 2, 3, 4    (all reassigned)
Consumer C: 5, 6, 7, 8       (all reassigned)
Movement: ALL 9 partitions

StickyAssignor preserves existing:
Consumer A: 0, 1, 2, 3, 4    (keeps 0,1,2 + adds 3,4)
Consumer C: 6, 7, 8, 5       (keeps 6,7,8 + adds 5)
Movement: Only 3 partitions (3, 4, 5)
```

**Benefits:**

- Reduces state transfer overhead
- Faster recovery from rebalances
- Preserves consumer-local caches/state
- More balanced than RoundRobinAssignor

**Configuration:**

```java
props.put("partition.assignment.strategy", 
    "org.apache.kafka.clients.consumer.StickyAssignor");
```

**Characteristics:**

- Uses **eager rebalance** (stop-the-world)
- Minimizes partition movement
- More complex computation
- Better for stateful consumers
- Reduces rebalance impact

#### CooperativeStickyAssignor

**Recommended for production** - combines sticky assignment with incremental cooperative rebalancing.

**Algorithm:** Same as StickyAssignor but uses cooperative rebalancing protocol.

**Example - Consumer Joins:**

```md
Initial State (6 partitions, 2 consumers):
Consumer A: 0, 1, 2
Consumer B: 3, 4, 5

Consumer C joins:

Phase 1 (identify what to move):
Consumer A: keeps 0, 1 (consuming), revokes 2
Consumer B: keeps 3, 4 (consuming), revokes 5
Consumer C: ready to receive

Phase 2 (reassign):
Consumer A: 0, 1           (never stopped consuming these!)
Consumer B: 3, 4           (never stopped consuming these!)
Consumer C: 2, 5           (receives revoked partitions)

Result: 4 partitions never stopped, 2 partitions briefly paused
```

**Configuration:**

```java
props.put("partition.assignment.strategy", 
    "org.apache.kafka.clients.consumer.CooperativeStickyAssignor");
```

**Characteristics:**

- ✅ Uses **incremental cooperative rebalance** (minimal downtime)
- ✅ Sticky assignment (preserves existing assignments)
- ✅ **Best for production** - minimal disruption
- ✅ Perfect for rolling restarts
- ✅ Reduces consumer lag spikes
- ⚠️ Requires all consumers in group to support it
- ⚠️ Available since Kafka 2.4+

#### Assignment Strategy Comparison

| Strategy | Rebalance | Balance | Stability | Downtime | Use Case |
| -------- | --------- | ------- | --------- | -------- | -------- |
| **RangeAssignor** | Eager | Fair | Low | High | Simple, single-topic |
| **RoundRobinAssignor** | Eager | Good | Low | High | Multi-topic balance |
| **StickyAssignor** | Eager | Good | High | High | Minimize movement |
| **CooperativeStickyAssignor** | Cooperative | Good | High | **Minimal** | **Production** |

**Multiple Strategies (Fallback):**

You can configure multiple strategies for compatibility:

```java
props.put("partition.assignment.strategy", 
    "org.apache.kafka.clients.consumer.CooperativeStickyAssignor," +
    "org.apache.kafka.clients.consumer.RangeAssignor");
```

- Consumers try strategies in order
- All consumers must share at least one common strategy
- Used during rolling upgrades to new assignors

### Static Group Membership

**Static group membership** (introduced in Kafka 2.3) prevents unnecessary rebalancing when consumers temporarily disconnect.

#### Problem with Dynamic Membership

**Default behavior** (dynamic membership) treats every consumer restart as a new member:

```md
1. Consumer A crashes
2. Coordinator waits for session.timeout.ms (default: 10s)
3. Timeout expires → Rebalance #1 triggered
4. Partitions redistributed to remaining consumers
5. Consumer A restarts after 8 seconds
6. Treated as NEW member → Rebalance #2 triggered
7. Partitions redistributed again

Result: TWO rebalances for a single restart!
Total disruption: ~25 seconds
```

**Why This Happens:**

- Each consumer gets a random `member.id` on startup
- Kafka can't tell if restarted consumer is the "same" consumer
- Every restart = new member = new rebalance

#### How Static Membership Works

**Configuration:**

```java
Properties props = new Properties();
props.put("group.id", "my-consumer-group");
props.put("group.instance.id", "consumer-1");  // Static identity
props.put("session.timeout.ms", "300000");     // 5 minutes
```

**Key Concept:** `group.instance.id` provides a **persistent identity** that survives restarts.

**Behavior:**

```md
1. Consumer A (instance ID: "consumer-1") crashes
2. Coordinator marks partitions as temporarily unavailable
3. NO rebalance triggered yet
4. Consumer A restarts within session.timeout.ms
5. Rejoins with SAME group.instance.id
6. Kafka recognizes it as the same consumer
7. Partitions automatically reassigned to Consumer A
8. NO rebalance needed!

Result: ZERO rebalances
Downtime: Only for Consumer A's partitions during restart
```

#### Static vs Dynamic Membership

**Dynamic (Default):**

```java
// No group.instance.id configured
// Consumer gets random member.id each time

Startup #1: member.id = "consumer-c6a2f4" (random)
Restart:    member.id = "consumer-b9d81e" (different!)

→ Kafka treats restart as new member
→ Rebalance triggered
```

**Static:**

```java
props.put("group.instance.id", "consumer-1");

Startup #1: group.instance.id = "consumer-1"
Restart:    group.instance.id = "consumer-1" (same!)

→ Kafka recognizes as same consumer
→ No rebalance if within timeout
```

**Comparison Table:**

| Scenario | Dynamic Membership | Static Membership |
| -------- | ----------------- | ---------------- |
| Consumer crashes | Wait timeout → Rebalance | Partitions frozen |
| Consumer restarts quickly | New member → Another rebalance | Resumes with same partitions |
| Total rebalances | 2 rebalances | 0 rebalances |
| Downtime | All consumers pause 2× | Only crashed consumer's partitions |
| Failover speed | Fast (after timeout) | Slow (wait for timeout) |
| Good for | Auto-scaling, fast failover | Planned restarts, rolling deploys |

#### Use Cases for Static Membership

**1. Rolling Restarts:**

```md
3 consumers with static IDs:
- consumer-1: partitions 0, 1
- consumer-2: partitions 2, 3
- consumer-3: partitions 4, 5

Rolling restart process:
1. Stop consumer-1 → partitions 0,1 paused (not reassigned)
2. Restart consumer-1 → immediately resumes 0,1
3. Stop consumer-2 → partitions 2,3 paused
4. Restart consumer-2 → immediately resumes 2,3
5. Stop consumer-3 → partitions 4,5 paused
6. Restart consumer-3 → immediately resumes 4,5

Result: Zero rebalances, smooth rolling deployment!
```

**2. Kubernetes StatefulSets:**

```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: kafka-consumer
spec:
  serviceName: "kafka-consumer"
  replicas: 3
  template:
    spec:
      containers:
      - name: consumer
        env:
        - name: GROUP_INSTANCE_ID
          valueFrom:
            fieldRef:
              fieldPath: metadata.name  # kafka-consumer-0, kafka-consumer-1, kafka-consumer-2
        - name: SESSION_TIMEOUT_MS
          value: "300000"  # 5 minutes for pod restarts
```

**3. Scheduled Maintenance:**

```md
1. Set high session.timeout.ms (e.g., 30 minutes)
2. Take consumer offline for maintenance
3. Perform updates, configuration changes, etc.
4. Restart consumer within timeout window
5. Consumer resumes with same partitions automatically
```

**Configuration Best Practices:**

```java
Properties props = new Properties();
props.put("group.id", "order-processing");

// Must be UNIQUE per consumer in the group
props.put("group.instance.id", getHostname() + "-" + getProcessId());
// Example: "prod-server-01-12345"

// Set timeout appropriate for restart duration
props.put("session.timeout.ms", "300000");  // 5 minutes for deployment

// Recommended: Use cooperative rebalancing
props.put("partition.assignment.strategy",
    "org.apache.kafka.clients.consumer.CooperativeStickyAssignor");
```

**Important Rules:**

1. **Uniqueness**: Each consumer needs a unique `group.instance.id` within the group
2. **Persistence**: The ID should survive restarts (hostname, pod name, etc.)
3. **Timeout**: Set `session.timeout.ms` longer than expected restart time
4. **Cannot change**: Cannot modify `group.instance.id` while consumer is active
5. **Manual cleanup**: If consumer is permanently removed, must wait for timeout

**Advantages:**

- ✅ Eliminates rebalance storms during rolling deployments
- ✅ Reduces consumer group instability
- ✅ Perfect for container orchestration (Kubernetes, ECS)
- ✅ Preserves consumer-local state during restarts
- ✅ Predictable partition assignment

**Disadvantages:**

- ❌ **Slower failover**: Must wait for full timeout before reassignment
- ❌ **Partitions unassigned during downtime**: No consumption until restart or timeout
- ❌ **Increased lag**: Partitions don't get reassigned immediately
- ❌ **Manual management**: Need to track instance IDs
- ❌ **Not for auto-scaling**: Adding/removing consumers still triggers rebalance

**When to Use Static Membership:**

- ✅ Short, planned restarts (deployments, upgrades)
- ✅ Rolling updates in production
- ✅ Stable, long-running consumers
- ✅ Container environments with predictable identities
- ✅ When consumer restart is faster than rebalancing

**When NOT to Use:**

- ❌ Auto-scaling scenarios (dynamic membership better)
- ❌ When fast failover is critical
- ❌ Short-lived, ephemeral consumers
- ❌ High consumer churn environments
- ❌ When immediate reassignment is required

### Multiple Consumer Groups

Different consumer groups can read the same topic independently:

```md
Topic: "user-events"

Consumer Group "analytics" → All partitions (for analytics)
Consumer Group "notifications" → All partitions (for sending notifications)
Consumer Group "archive" → All partitions (for archiving)
```

Each group maintains its own offset, so they can process at different speeds.

### Deserialization

**Deserialization** is the reverse of serialization - converting byte arrays back to objects.

**Process:**

```md
1. Consumer fetches a record (byte array) from Kafka
2. Applies the configured **Deserializer** to the key and value
3. Converts bytes back to the original data type
4. Returns the reconstructed object to your application
```

**Built-in Deserializers:**

- **StringDeserializer**: UTF-8 bytes → String
- **IntegerDeserializer**: 4 bytes → Integer
- **LongDeserializer**: 8 bytes → Long
- **ByteArrayDeserializer**: No conversion needed
- **DoubleDeserializer**: Bytes → Double

**Configuration Example:**

```java
Properties props = new Properties();
props.put("bootstrap.servers", "localhost:9092");
props.put("group.id", "my-consumer-group");
props.put("key.deserializer", "org.apache.kafka.common.serialization.StringDeserializer");
props.put("value.deserializer", "org.apache.kafka.common.serialization.StringDeserializer");

KafkaConsumer<String, String> consumer = new KafkaConsumer<>(props);
```

**JSON Deserialization:**

```java
ConsumerRecord<String, byte[]> record = consumer.poll(...);
ObjectMapper mapper = new ObjectMapper();
MyObject obj = mapper.readValue(record.value(), MyObject.class);
```

**Avro Deserialization:**

```java
props.put("value.deserializer", "io.confluent.kafka.serializers.KafkaAvroDeserializer");
props.put("schema.registry.url", "http://localhost:8081");
```

**Critical Rule:** The deserializer must match the serializer used by the producer, or you'll get deserialization errors.

### How Consumers Know What to Read

Consumers use several mechanisms to determine what to read and from where:

#### 1. Topic Subscription

**Manual Subscription:**

```java
consumer.subscribe(Arrays.asList("orders", "payments"));
```

**Pattern Subscription:**

```java
// Subscribe to all topics starting with "logs-"
consumer.subscribe(Pattern.compile("logs-.*"));
```

**Manual Partition Assignment:**

```java
// Bypass consumer group, directly assign partitions
TopicPartition partition0 = new TopicPartition("orders", 0);
TopicPartition partition1 = new TopicPartition("orders", 1);
consumer.assign(Arrays.asList(partition0, partition1));
```

#### 2. Partition Assignment (Automatic)

When using consumer groups, Kafka automatically assigns partitions using a **partition assignment strategy**:

**RangeAssignor (default):**

- Assigns partitions in ranges to consumers
- Can cause imbalance if consumers subscribe to different topics

**RoundRobinAssignor:**

- Distributes partitions evenly in round-robin fashion
- Better balance across consumers

**StickyAssignor:**

- Tries to keep existing assignments during rebalancing
- Minimizes partition movement

**CooperativeStickyAssignor:**

- Like StickyAssignor but allows incremental rebalancing
- Reduces downtime during rebalancing

Configuration:

```java
props.put("partition.assignment.strategy", "org.apache.kafka.clients.consumer.RoundRobinAssignor");
```

#### 3. Offset Management (Commit Strategies)

**Offsets** tell consumers their position in each partition. Kafka tracks this automatically.

**Offset Commit Strategies** determine when and how consumer offsets are saved, which directly impacts **message delivery guarantees**.

##### Automatic Commit (default)

```java
props.put("enable.auto.commit", "true");
props.put("auto.commit.interval.ms", "5000");  // Commit every 5 seconds
```

- Convenient but can lead to duplicate processing on failures
- Offsets are committed periodically in the background
- If consumer crashes after processing but before auto-commit, messages will be reprocessed

##### Manual Commit (more control)

```java
props.put("enable.auto.commit", "false");

while (true) {
    ConsumerRecords<String, String> records = consumer.poll(Duration.ofMillis(100));
    for (ConsumerRecord<String, String> record : records) {
        processRecord(record);
    }
    consumer.commitSync();  // or commitAsync()
}
```

- Full control over when offsets are committed
- Can commit after each message, after each batch, or after successful processing
- `commitSync()`: Blocks until commit succeeds (slower but safer)
- `commitAsync()`: Non-blocking, continues immediately (faster but may fail silently)

##### Message Delivery Semantics

The timing of offset commits relative to message processing determines your **delivery guarantee**

###### At-Most-Once Delivery

Pattern: **Commit BEFORE processing**

```java
while (true) {
    ConsumerRecords<String, String> records = consumer.poll(Duration.ofMillis(100));
    consumer.commitSync();  // Commit immediately after polling
    
    for (ConsumerRecord<String, String> record : records) {
        processRecord(record);  // Process after committing
    }
}
```

**Characteristics:**

- ✅ **No duplicates**: Each message delivered at most once
- ❌ **Data loss possible**: If consumer crashes during processing, messages are lost
- **Use case**: Where losing some data is acceptable (e.g., metrics, non-critical logs)

**What happens on crash:**

```md
1. Poll messages (offsets 100-109)
2. Commit offset 110
3. Start processing offset 100
4. CRASH at offset 102
5. Restart → Start from offset 110
6. Messages 102-109 are LOST
```

###### At-Least-Once Delivery (Default)

Pattern: **Commit AFTER processing**

```java
while (true) {
    ConsumerRecords<String, String> records = consumer.poll(Duration.ofMillis(100));
    
    for (ConsumerRecord<String, String> record : records) {
        processRecord(record);  // Process first
    }
    
    consumer.commitSync();  // Commit after all processed
}
```

**Characteristics:**

- ✅ **No data loss**: Messages guaranteed to be delivered
- ❌ **Duplicates possible**: Same message may be processed multiple times
- **Use case**: Most common pattern - when you can handle duplicates (idempotent processing)

**What happens on crash:**

1. Poll messages (offsets 100-109)
2. Process offsets 100-105
3. CRASH before committing
4. Restart → Start from offset 100 (last committed)
5. Messages 100-105 are REPROCESSED (duplicates)

**Making it idempotent:**

```java
for (ConsumerRecord<String, String> record : records) {
    // Use a unique ID to check if already processed
    String messageId = record.headers().lastHeader("message-id").value();
    
    if (!database.exists(messageId)) {
        processRecord(record);
        database.save(messageId, record.value());
    }
}
consumer.commitSync();
```

###### Exactly-Once Semantics (EOS)

Pattern: **Transactional processing**

Kafka provides exactly-once semantics through **transactions** that coordinate offset commits with message production.

**Configuration:**

```java
// Producer configuration
Properties producerProps = new Properties();
producerProps.put("enable.idempotence", "true");
producerProps.put("transactional.id", "my-transactional-id");

// Consumer configuration
Properties consumerProps = new Properties();
consumerProps.put("isolation.level", "read_committed");
consumerProps.put("enable.auto.commit", "false");
```

**Implementation:**

```java
KafkaProducer<String, String> producer = new KafkaProducer<>(producerProps);
KafkaConsumer<String, String> consumer = new KafkaConsumer<>(consumerProps);

producer.initTransactions();

try {
    while (true) {
        ConsumerRecords<String, String> records = consumer.poll(Duration.ofMillis(100));
        
        producer.beginTransaction();
        
        for (ConsumerRecord<String, String> record : records) {
            // Process and produce transformed records
            ProducerRecord<String, String> outputRecord = transform(record);
            producer.send(outputRecord);
        }
        
        // Commit consumer offsets as part of the transaction
        producer.sendOffsetsToTransaction(
            getOffsets(records), 
            consumer.groupMetadata()
        );
        
        producer.commitTransaction();
    }
} catch (Exception e) {
    producer.abortTransaction();
}
```

**Characteristics:**

- ✅ **No duplicates**: Each message processed exactly once
- ✅ **No data loss**: Atomic commit of processing and offsets
- ❌ **Slower**: More coordination overhead
- ❌ **Complex**: Requires transactional producer/consumer setup
- **Use case**: Financial transactions, critical data pipelines where accuracy is paramount

**How it works:**

1. Begin transaction
2. Process messages
3. Produce output messages (staged, not visible yet)
4. Commit offsets (staged, not visible yet)
5. Commit transaction (atomically makes everything visible)
6. If crash occurs, entire transaction is rolled back

###### Comparison Table

| Delivery Semantic | Commit Timing | Duplicates? | Data Loss? | Performance | Complexity |
|-------------------|---------------|-------------|------------|-------------|------------|
| **At-Most-Once** | Before processing | No | Yes | Fastest | Simple |
| **At-Least-Once** | After processing | Yes | No | Fast | Simple |
| **Exactly-Once** | Transactional | No | No | Slower | Complex |

###### Choosing the Right Strategy

**Use At-Most-Once when:**

- Losing occasional messages is acceptable
- Duplicates would cause serious problems
- Performance is critical
- Example: Real-time dashboards, approximate metrics

**Use At-Least-Once when:**

- You can handle duplicates (via idempotency)
- Data loss is unacceptable
- Good balance of reliability and performance
- Example: Event processing with deduplication, log aggregation

**Use Exactly-Once when:**

- Both duplicates and data loss are unacceptable
- Worth the performance trade-off
- Building stream processing pipelines (Kafka Streams uses this)
- Example: Financial transactions, billing systems, inventory management

**Starting Position:**

When a consumer group starts for the first time (no committed offset):

```java
// Start from the earliest message
props.put("auto.offset.reset", "earliest");

// Start from the latest message (default)
props.put("auto.offset.reset", "latest");

// Throw exception if no offset found
props.put("auto.offset.reset", "none");
```

**Seeking to Specific Offset:**

```java
// Seek to beginning
consumer.seekToBeginning(consumer.assignment());

// Seek to end
consumer.seekToEnd(consumer.assignment());

// Seek to specific offset
TopicPartition partition = new TopicPartition("orders", 0);
consumer.seek(partition, 1000);  // Start at offset 1000

// Seek to timestamp
long timestamp = Instant.now().minus(1, ChronoUnit.HOURS).toEpochMilli();
Map<TopicPartition, Long> timestampsToSearch = new HashMap<>();
timestampsToSearch.put(partition, timestamp);
consumer.offsetsForTimes(timestampsToSearch);
```

#### 4. Offset Storage

Offsets are stored in a special Kafka topic called `__consumer_offsets`:

- Each consumer group has its own offsets
- Kafka manages this topic automatically
- Replicated for fault tolerance
- Allows consumers to resume from where they left off after restart

**Offset Metadata:**

```sh
Group: "order-processors"
Topic: "orders"
Partition: 2
Offset: 15234
Timestamp: 2025-12-07T10:30:00Z
```

### Consumer Lifecycle

```md
1. Consumer starts and joins consumer group
2. Group coordinator triggers rebalancing
3. Partitions are assigned to consumer
4. Consumer seeks to last committed offset (or uses auto.offset.reset)
5. Consumer polls for records in a loop
6. Deserializes and processes messages
7. Commits offsets (auto or manual)
8. Repeat steps 5-7
9. Consumer leaves group (graceful shutdown or failure)
10. Rebalancing reassigns partitions to remaining consumers
```

### Consumer Example

```java
Properties props = new Properties();
props.put("bootstrap.servers", "localhost:9092");
props.put("group.id", "my-app");
props.put("key.deserializer", "org.apache.kafka.common.serialization.StringDeserializer");
props.put("value.deserializer", "org.apache.kafka.common.serialization.StringDeserializer");
props.put("auto.offset.reset", "earliest");
props.put("enable.auto.commit", "false");

KafkaConsumer<String, String> consumer = new KafkaConsumer<>(props);
consumer.subscribe(Arrays.asList("user-events"));

try {
    while (true) {
        ConsumerRecords<String, String> records = consumer.poll(Duration.ofMillis(1000));
        
        for (ConsumerRecord<String, String> record : records) {
            System.out.printf("Consumed: topic=%s, partition=%d, offset=%d, key=%s, value=%s%n",
                            record.topic(), record.partition(), record.offset(), 
                            record.key(), record.value());
            
            // Process the record
            processRecord(record);
        }
        
        // Commit offsets after processing
        consumer.commitSync();
    }
} finally {
    consumer.close();
}
```

### Consumer Liveliness

Consumer liveliness is the mechanism Kafka uses to determine if a consumer is healthy and actively processing messages.  
This system prevents "zombie" consumers from holding onto partition assignments and ensures the consumer group remains responsive.  
Kafka uses two independent threads to monitor consumer health: the **heartbeat thread** and the **poll thread**.

#### Consumer Coordinator

The **Consumer Coordinator** is a component running on one of the Kafka brokers that manages a consumer group. Each consumer group has one designated coordinator.

**Responsibilities:**

- **Group membership management**: Tracks which consumers belong to the group
- **Partition assignment**: Coordinates rebalancing and partition distribution
- **Heartbeat monitoring**: Receives heartbeats from consumers to determine liveness
- **Offset management**: Handles offset commits (when using `__consumer_offsets` topic)
- **Rebalance orchestration**: Initiates and manages the rebalancing process

**How Consumers Find Their Coordinator:**

1. Consumer sends a `FindCoordinator` request to any broker
2. Broker responds with the coordinator location (broker ID) based on the group ID
3. Consumer establishes a connection with the coordinator
4. Coordinator is determined by: `hash(group.id) % number_of_partitions(__consumer_offsets)`

**Example:**

```md
Consumer Group: "order-processors"
Coordinator: Broker 2 (determined by hashing the group ID)

All consumers in "order-processors" communicate with Broker 2 for:
- Joining the group
- Sending heartbeats
- Rebalancing
- Committing offsets
```

#### Heartbeat Thread

The **heartbeat thread** is a background thread that runs independently of the main poll loop. Its sole purpose is to send periodic heartbeats to the coordinator to signal that the consumer process is alive.

**Key Purpose:**

- Signals that the consumer **process** is running
- Maintains group membership
- Triggers rebalancing when consumers join/leave
- Operates **independently** of message processing

##### heartbeat.interval.ms

**What it is:** The frequency at which the consumer sends heartbeats to the coordinator.

**Default value:** `3000` (3 seconds)

**How it works:**

```md
Every 3 seconds (by default):
Consumer → Heartbeat → Coordinator: "I'm alive!"
```

**Configuration:**

```properties
heartbeat.interval.ms=3000
```

**Guidelines:**

- Should be **significantly lower** than `session.timeout.ms`
- Recommended: Set to 1/3 of `session.timeout.ms`
- Lower values = faster failure detection but more network traffic
- Higher values = less overhead but slower failure detection

**Example Timeline:**

```md
t=0s:  Heartbeat sent ✓
t=3s:  Heartbeat sent ✓
t=6s:  Heartbeat sent ✓
t=9s:  Heartbeat sent ✓
t=12s: Consumer crashes
t=15s: Heartbeat MISSED (should have been sent)
t=18s: Heartbeat MISSED
t=21s: Heartbeat MISSED (3 heartbeats missed)
t=22s: Session timeout (10s configured) → Rebalance triggered
```

##### session.timeout.ms

**What it is:** The maximum time the coordinator will wait without receiving a heartbeat before considering the consumer dead and triggering a rebalance.

**Default value:** `45000` (45 seconds) in Kafka 3.0+, `10000` (10 seconds) in older versions

**How it works:**

```md
If no heartbeat received within this time → Consumer is dead → Rebalance
```

**Configuration:**

```properties
session.timeout.ms=45000
```

**Guidelines:**

- Must be in the broker's allowed range: `group.min.session.timeout.ms` to `group.max.session.timeout.ms`
- Default broker range: 6 seconds to 5 minutes
- Lower values = faster failure detection but more false positives (network hiccups)
- Higher values = more tolerance for network issues but slower failure detection

**What triggers session timeout:**

- Consumer process crashes
- Consumer gets stuck in long GC pause
- Network partition between consumer and coordinator
- Consumer machine becomes unresponsive

**Example:**

```properties
session.timeout.ms=45000      # 45 seconds
heartbeat.interval.ms=3000    # 3 seconds

# Consumer can miss up to ~15 heartbeats before being kicked out
# 45000ms / 3000ms = 15 heartbeats
```

##### Heartbeat Thread Workflow

```md
┌────────────────────────────────────────────────────────────┐
│                    Consumer Process                        │
│                                                            │
│  ┌────────────────────────────────────────────────────┐    │
│  │          Heartbeat Thread (Background)             │    │
│  │                                                    │    │
│  │   while (consumer is active):                      │    │
│  │       sleep(heartbeat.interval.ms)                 │    │
│  │       send_heartbeat_to_coordinator()              │    │
│  │       if (response == REBALANCE_NEEDED):           │    │
│  │           trigger_rebalance()                      │    │
│  │                                                    │    │
│  └────────────────────────────────────────────────────┘    │
│                                                            │
└────────────────────────────────────────────────────────────┘
                           │
                           │ Heartbeat every 3s
                           ▼
                 ┌──────────────────────┐
                 │   Consumer           │
                 │   Coordinator        │
                 │   (Broker 2)         │
                 │                      │
                 │ Tracks last heartbeat│
                 │ Waits up to          │
                 │ session.timeout.ms   │
                 └──────────────────────┘
```

#### Poll Thread

The **poll thread** is the main application thread that calls `consumer.poll()` to fetch and process records. It monitors whether the consumer is making progress in consuming messages.

**Key Purpose:**

- Signals that the consumer is **actively processing messages**
- Prevents slow/stuck consumers from holding partitions
- Operates on the **main application thread**

##### max.poll.interval.ms

**What it is:** The maximum time between two consecutive `poll()` calls before the consumer is considered dead and removed from the group.

**Default value:** `300000` (5 minutes)

**How it works:**

```md
If time between poll() calls exceeds this → Consumer is stuck → Rebalance
```

**Configuration:**

```properties
max.poll.interval.ms=300000
```

**Why it exists:**

To detect consumers that are stuck processing messages and can't make progress. The heartbeat thread might be sending heartbeats (process is alive), but if the poll thread is blocked, the consumer isn't actually consuming.

**Common scenarios that trigger this:**

```java
// BAD: Processing takes too long
ConsumerRecords<String, String> records = consumer.poll(Duration.ofMillis(100));
for (ConsumerRecord<String, String> record : records) {
    processRecord(record);  // Takes 10 minutes per record!
    // Next poll() won't happen for 10 minutes
    // If max.poll.interval.ms = 5 minutes → Consumer kicked out
}
```

**Guidelines:**

- Set based on your **maximum expected processing time** for a batch of records
- Must be higher than the time to process `max.poll.records` messages
- Formula: `max.poll.interval.ms > (max.poll.records × time_per_record) + buffer`
- If processing is slow, either:
  - Increase `max.poll.interval.ms`, OR
  - Decrease `max.poll.records`, OR
  - Optimize processing logic

**Example Timeline:**

```md
t=0s:   consumer.poll() called - fetches 500 records ✓
t=0s:   Start processing 500 records
t=4m:   Still processing... (400 records done)
t=5m:   Still processing... (450 records done)
t=6m:   Processing complete (500 records done)
t=6m:   consumer.poll() called again ✓

Time between polls: 6 minutes
max.poll.interval.ms: 5 minutes
Result: Consumer exceeded max.poll.interval.ms → Rebalance triggered!
```

**Fix:**

```properties
# Option 1: Increase interval
max.poll.interval.ms=420000  # 7 minutes

# Option 2: Process fewer records per poll
max.poll.records=250  # Process half as many
```

##### max.poll.records

**What it is:** The maximum number of records returned in a single `poll()` call.

**Default value:** `500`

**How it works:**

```java
ConsumerRecords<String, String> records = consumer.poll(Duration.ofMillis(100));
// records.count() will be AT MOST max.poll.records (500 by default)
```

**Configuration:**

```properties
max.poll.records=500
```

**Why it matters for liveliness:**

- Directly impacts how long before the next `poll()` call
- More records = longer processing time = risk of exceeding `max.poll.interval.ms`
- Fewer records = more frequent polling = better for slow processing

**Trade-offs:**

| Value        | Throughput | Liveliness Risk | Use Case                           |
|--------------|------------|-----------------|------------------------------------|
| High (1000+) | ✅ Better   | ⚠️ Higher       | Fast processing, high throughput   |
| Medium (500) | ✅ Good     | ✅ Balanced      | Default, general use               |
| Low (10-100) | ⚠️ Lower   | ✅ Lower         | Slow processing, long transactions |

**Example:**

```java
// Fast processing: Can handle more records
props.put("max.poll.records", 1000);
props.put("max.poll.interval.ms", 300000);  // 5 minutes
// 1000 records × 0.1s per record = 100s total (well under 5 minutes)

// Slow processing: Reduce records per poll
props.put("max.poll.records", 50);
props.put("max.poll.interval.ms", 300000);  // 5 minutes
// 50 records × 5s per record = 250s total (under 5 minutes)
```

##### fetch.min.bytes

**What it is:** The minimum amount of data the broker should return for a fetch request. If not enough data is available, the broker will wait.

**Default value:** `1` (1 byte)

**How it works:**

```md
Consumer: "Give me at least 1MB of data"
Broker: "I only have 500KB right now, I'll wait for more..."
Broker: [waits up to fetch.max.wait.ms]
Broker: "Here's 1MB of data" OR "Timeout, here's what I have"
```

**Configuration:**

```properties
fetch.min.bytes=1
```

**Impact on liveliness:**

- Higher values reduce polling frequency (waiting for more data)
- Can improve throughput by batching
- May increase latency in low-volume topics
- Generally doesn't cause liveliness issues unless set extremely high

**Use cases:**

```properties
# High throughput scenario: Wait for bigger batches
fetch.min.bytes=1048576  # 1 MB

# Low latency scenario: Return data immediately
fetch.min.bytes=1  # Default
```

##### fetch.max.wait.ms

**What it is:** The maximum time the broker will wait before answering a fetch request if there isn't enough data to satisfy `fetch.min.bytes`.

**Default value:** `500` (500 milliseconds)

**How it works:**

```md
Consumer → Broker: "Give me at least 1MB (fetch.min.bytes)"
Broker: "I only have 100KB now"
Broker: [waits up to 500ms for more data]

Option 1: More data arrives → Returns 1MB+
Option 2: 500ms timeout → Returns whatever is available (100KB)
```

**Configuration:**

```properties
fetch.max.wait.ms=500
```

**Impact on liveliness:**

- Adds latency to `poll()` calls but doesn't affect liveliness directly
- `poll()` will return at least every `fetch.max.wait.ms` if no data
- Helps ensure frequent polling even with low message volume

**Trade-offs:**

```properties
# Lower latency: Return faster with less data
fetch.max.wait.ms=100

# Higher throughput: Wait longer for bigger batches
fetch.max.wait.ms=2000
```

**Example scenario:**

```md
fetch.min.bytes=1048576    # Want 1MB
fetch.max.wait.ms=2000     # Wait up to 2s

Timeline:
t=0s:   poll() called
t=0s:   Only 200KB available
t=1s:   500KB available (still less than 1MB)
t=2s:   Timeout! Return 500KB even though less than 1MB
t=2s:   poll() returns to application
```

##### max.partition.fetch.bytes

**What it is:** The maximum amount of data per partition the broker will return in a single fetch request.

**Default value:** `1048576` (1 MB)

**How it works:**

```md
Consumer assigned to 3 partitions:
- Partition 0: Can fetch up to 1MB
- Partition 1: Can fetch up to 1MB  
- Partition 2: Can fetch up to 1MB

Total maximum fetch: 3MB (in this example)
```

**Configuration:**

```properties
max.partition.fetch.bytes=1048576
```

**Impact on liveliness:**

- Must be larger than your largest message
- If a message is larger than this value, consumer will fail
- Doesn't directly impact liveliness but can cause processing issues

**Important relationship:**

```properties
max.partition.fetch.bytes=1048576     # 1MB per partition
# If assigned 10 partitions, could fetch up to 10MB in one poll

# Ensure broker can handle the size:
# message.max.bytes (broker) >= max.partition.fetch.bytes (consumer)
```

**Example:**

```properties
# Large messages scenario
max.partition.fetch.bytes=10485760  # 10 MB
max.poll.records=10                 # Fetch fewer records
# Allows large messages but limits batch size
```

##### fetch.max.bytes

**What it is:** The maximum amount of data the broker will return for a fetch request **across all partitions**.

**Default value:** `52428800` (50 MB)

**How it works:**

```md
Consumer assigned to 3 partitions:
fetch.max.bytes=5MB (total limit)
max.partition.fetch.bytes=2MB (per partition limit)

Actual fetch:
- Partition 0: 2MB (hit partition limit)
- Partition 1: 2MB (hit partition limit)  
- Partition 2: 1MB (hit total limit of 5MB)

Total: 5MB (respects fetch.max.bytes)
```

**Configuration:**

```properties
fetch.max.bytes=52428800
```

**Impact on liveliness:**

- Controls total memory used per fetch
- Prevents overwhelming the consumer with too much data
- Should be > `max.partition.fetch.bytes` for multi-partition assignments

**Relationship:**

```properties
fetch.max.bytes >= max.partition.fetch.bytes × (typical partition count)

# Example for 10 partitions:
max.partition.fetch.bytes=1048576   # 1MB per partition
fetch.max.bytes=10485760            # 10MB total (10 × 1MB)
```

##### Poll Thread Workflow

```md
┌────────────────────────────────────────────────────────────┐
│                    Consumer Application                    │
│                                                            │
│  ┌───────────────────────────────────────────────────┐     │
│  │            Main Poll Loop (Poll Thread)           │     │
│  │                                                   │     │
│  │   while (running):                                │     │
│  │       // Fetch records                            │     │
│  │       records = consumer.poll(100ms)              │     │
│  │       ↓                                           │     │
│  │       Check: Time since last poll <               │     │
│  │              max.poll.interval.ms?                │     │
│  │       ↓                                           │     │
│  │       // Process records                          │     │
│  │       for (record in records):                    │     │
│  │           processRecord(record)                   │     │
│  │       ↓                                           │     │
│  │       // Commit offsets                           │     │
│  │       consumer.commitSync()                       │     │
│  │       ↓                                           │     │
│  │       // Loop back to poll()                      │     │
│  │                                                   │     │
│  └───────────────────────────────────────────────────┘     │
│                                                            │
└────────────────────────────────────────────────────────────┘
                           │
                           │ Fetch request every poll()
                           ▼
                 ┌──────────────────────┐
                 │   Kafka Broker       │
                 │                      │
                 │ Checks:              │
                 │ - fetch.min.bytes    │
                 │ - fetch.max.wait.ms  │
                 │ - max.partition.fetch│
                 │ - fetch.max.bytes    │
                 │                      │
                 │ Returns records      │
                 └──────────────────────┘
```

#### How Heartbeat and Poll Threads Work Together

Both threads monitor **different aspects** of consumer health:

```md
┌────────────────────────────────────────────────────────────┐
│                       Consumer Process                     │
│                                                            │
│  ┌────────────────────────┐      ┌──────────────────────┐  │
│  │   Heartbeat Thread     │      │    Poll Thread       │  │
│  │                        │      │                      │  │
│  │  Monitors:             │      │  Monitors:           │  │
│  │  - Process alive       │      │  - Processing alive  │  │
│  │  - Network connected   │      │  - Not stuck         │  │
│  │                        │      │  - Making progress   │  │
│  │  Timeout:              │      │  Timeout:            │  │
│  │  session.timeout.ms    │      │  max.poll.interval   │  │
│  │                        │      │                      │  │
│  └────────────────────────┘      └──────────────────────┘  │
│             │                               │              │
│             │ Heartbeat                     │ Fetch        │
│             ▼                               ▼              │
│    ┌─────────────────────────────────────────────────┐     │
│    │        Consumer Coordinator (Broker)            │     │
│    │                                                 │     │
│    │  Both must be healthy for consumer to remain    │     │
│    │  active in the group                            │     │
│    └─────────────────────────────────────────────────┘     │
│                                                            │
└────────────────────────────────────────────────────────────┘
```

**Scenario Examples:**

**Scenario 1: Process crashes**

```md
Result: Heartbeat thread stops → session.timeout.ms exceeded → Rebalance
Detection time: ~45 seconds (session.timeout.ms)
```

**Scenario 2: Processing stuck (infinite loop, deadlock)**

```md
Heartbeat thread: ✓ Still sending (process alive)
Poll thread: ✗ Not calling poll() (stuck in processing)
Result: max.poll.interval.ms exceeded → Rebalance
Detection time: ~5 minutes (max.poll.interval.ms)
```

**Scenario 3: Long GC pause**

```md
Scenario A: GC pause < session.timeout.ms
  Heartbeat thread: Paused
  Poll thread: Paused
  Result: Both resume, no rebalance

Scenario B: GC pause > session.timeout.ms
  Heartbeat thread: Timeout
  Result: Rebalance triggered
```

**Scenario 4: Network partition**

```md
Heartbeat thread: Can't reach coordinator
Result: session.timeout.ms exceeded → Rebalance
Detection time: ~45 seconds
```

#### Consumer Failure Detection Scenarios

**Comparison:**

| Scenario | Heartbeat Thread | Poll Thread | Trigger | Detection Time |
|----------|-----------------|-------------|---------|----------------|
| Process crash | ✗ Stopped | ✗ Stopped | session.timeout.ms | ~45s |
| Stuck processing | ✓ Sending | ✗ Not polling | max.poll.interval.ms | ~5min |
| Network issue | ✗ Can't send | ⚠️ May work | session.timeout.ms | ~45s |
| Long GC pause (>45s) | ✗ Paused | ✗ Paused | session.timeout.ms | ~45s |
| Long GC pause (<45s) | ⚠️ Delayed | ⚠️ Delayed | None | 0s (recovers) |
| Slow processing | ✓ Sending | ⚠️ Infrequent | max.poll.interval.ms | ~5min |

#### Configuration Best Practices

**1. Start with proven defaults (Kafka 3.0+):**

```properties
# Heartbeat thread
heartbeat.interval.ms=3000         # 3 seconds
session.timeout.ms=45000           # 45 seconds

# Poll thread
max.poll.interval.ms=300000        # 5 minutes
max.poll.records=500               # 500 records

# Fetch settings
fetch.min.bytes=1                  # 1 byte (immediate)
fetch.max.wait.ms=500              # 500ms
max.partition.fetch.bytes=1048576  # 1 MB
fetch.max.bytes=52428800           # 50 MB
```

**2. Relationship rules:**

```properties
# Rule 1: Heartbeat interval should be 1/3 of session timeout
session.timeout.ms=45000
heartbeat.interval.ms=15000        # 45000 / 3

# Rule 2: Max poll interval > time to process max.poll.records
max.poll.records=500
# If each record takes 100ms to process:
# 500 × 0.1s = 50s processing time
max.poll.interval.ms=60000         # 60s > 50s (with buffer)

# Rule 3: Session timeout within broker limits
group.min.session.timeout.ms=6000  # Broker setting
group.max.session.timeout.ms=300000  # Broker setting
session.timeout.ms=45000           # Within range
```

**3. Fast failure detection (critical systems):**

```properties
heartbeat.interval.ms=1000         # 1 second
session.timeout.ms=6000            # 6 seconds
# Detects failures in ~6 seconds, but more sensitive to network hiccups
```

**4. Slow/heavy processing:**

```properties
max.poll.records=50                # Fewer records per batch
max.poll.interval.ms=600000        # 10 minutes
# Allows more time for processing without being kicked out
```

**5. High throughput (fast processing):**

```properties
max.poll.records=2000              # More records per batch
fetch.min.bytes=1048576            # Wait for 1MB
fetch.max.wait.ms=1000             # Wait up to 1s for batch
# Optimizes for throughput over latency
```

**6. Low latency (real-time processing):**

```properties
max.poll.records=100               # Smaller batches
fetch.min.bytes=1                  # Return immediately
fetch.max.wait.ms=100              # Short wait time
# Optimizes for latency over throughput
```

#### Troubleshooting Common Issues

**Problem 1: "Consumer keeps getting kicked out / constant rebalancing"**

**Symptoms:**

```log
[Consumer clientId=consumer-1, groupId=my-group] Offset commit failed on partition my-topic-0 
at offset 12345: The coordinator is not aware of this member
```

**Possible causes:**

A. **Processing takes too long**

```properties
# Check: Is processing time > max.poll.interval.ms?
# Symptom: Heartbeats are fine, but poll() not called often enough

# Solution 1: Increase max.poll.interval.ms
max.poll.interval.ms=600000  # 10 minutes

# Solution 2: Reduce max.poll.records
max.poll.records=100

# Solution 3: Optimize processing logic
# - Process in parallel
# - Move slow operations outside poll loop
# - Use async processing
```

B. **Network issues or high latency**

```properties
# Check: Are heartbeats reaching coordinator?
# Symptom: Frequent session timeouts

# Solution: Increase session timeout
session.timeout.ms=60000
heartbeat.interval.ms=3000
```

C. **Long GC pauses**

```bash
# Check GC logs
java -XX:+PrintGCDetails -XX:+PrintGCDateStamps

# Solution: Tune JVM or increase session timeout
session.timeout.ms=60000
```

**Problem 2: "High lag / consumer not keeping up"**

**Symptoms:**

```md
Consumer lag keeps increasing
Messages are piling up in the topic
```

**Solutions:**

```properties
# Option 1: Increase batch size (if processing is fast)
max.poll.records=2000
fetch.min.bytes=1048576

# Option 2: Add more consumers to the group
# Scale horizontally (up to number of partitions)

# Option 3: Optimize processing
# - Profile and optimize slow code
# - Use parallel processing within consumer
```

**Problem 3: "Consumer stuck, not processing messages"**

**Symptoms:**

```md
Consumer appears alive (heartbeats) but no messages processed
No errors in logs
```

**Debug checklist:**

```java
// Add logging to detect the issue
ConsumerRecords<String, String> records = consumer.poll(Duration.ofMillis(100));
logger.info("Polled {} records", records.count());

if (records.isEmpty()) {
    logger.warn("No records fetched - check topic has data");
}

for (ConsumerRecord<String, String> record : records) {
    logger.info("Processing record: offset={}", record.offset());
    long startTime = System.currentTimeMillis();
    processRecord(record);
    long duration = System.currentTimeMillis() - startTime;
    
    if (duration > 1000) {
        logger.warn("Slow processing: {}ms for offset {}", duration, record.offset());
    }
}
```

**Problem 4: "Messages larger than max.partition.fetch.bytes"**

**Symptoms:**

```log
RecordTooLargeException: The message is too large
```

**Solution:**

```properties
# Increase consumer fetch size
max.partition.fetch.bytes=10485760  # 10 MB

# Ensure broker allows it
message.max.bytes=10485760          # Broker config

# Consider message design
# - Compress messages
# - Store large payloads externally (S3, database)
# - Reference by ID instead of embedding
```

**Problem 5: "Memory issues / OutOfMemoryError"**

**Symptoms:**

```log
java.lang.OutOfMemoryError: Java heap space
```

**Solutions:**

```properties
# Reduce memory usage per fetch
fetch.max.bytes=10485760           # 10 MB instead of 50 MB
max.partition.fetch.bytes=1048576  # 1 MB per partition
max.poll.records=100               # Fewer records per poll

# Increase JVM heap
java -Xmx2g -Xms2g
```

### Important Consumer Considerations

- **Poll regularly**: Consumers must call `poll()` frequently or they'll be considered dead and rebalanced out
- **Rebalancing**: Adding/removing consumers triggers rebalancing, temporarily pausing consumption
- **Processing time**: Long processing can cause rebalancing - consider increasing `max.poll.interval.ms`
- **At-least-once vs exactly-once**: Default is at-least-once (may see duplicates); exactly-once requires additional configuration
- **Lag monitoring**: Track consumer lag (difference between latest offset and committed offset) to detect slow consumers

## How They Work Together

- A topic with 3 partitions distributes messages across 3 independent logs
- Producers write to specific partitions (either by key, round-robin, or custom logic)
- Consumers read from assigned partitions
- More partitions enable higher throughput but add overhead
- The partition count affects how you scale consumers and overall system performance

## Brokers

A **broker** is a Kafka server that stores data and serves client requests. Brokers are the core infrastructure of Kafka's distributed architecture.

**What is a Broker:**

- A single Kafka server/instance running the Kafka process
- Stores partition data on disk
- Handles read and write requests from producers and consumers
- Manages replication of partitions
- Each broker is identified by a unique integer ID

**Kafka Cluster:**

Multiple brokers work together to form a **Kafka cluster**:

```sh
Kafka Cluster
├── Broker 1 (ID: 1)
├── Broker 2 (ID: 2)
├── Broker 3 (ID: 3)
└── Broker 4 (ID: 4)
```

**Position in Architecture:**

Brokers sit **between producers and consumers** - they are the central storage layer:

```md
Producers → [Brokers (Kafka Cluster)] → Consumers
               ↓
          Persistent Storage
             (Disk)
```

More detailed flow:

```sh
Producer App 1 ─┐
Producer App 2 ─┤
Producer App 3 ─┤→ Broker 1 ──┐
                │  Broker 2 ──┤→ (Cluster) → Consumer Group A
                │  Broker 3 ──┤            → Consumer Group B
                └→ Broker 4 ──┘            → Consumer Group C
```

Both producers and consumers connect to brokers to write and read data. Brokers don't "push" to consumers; consumers "pull" from brokers.

### Brokers and Partitions

**Relationship:**

- Each **partition** is stored on one or more brokers
- Partitions are **distributed** across brokers for load balancing
- Each partition has a **leader broker** and zero or more **follower brokers** (replicas)

**Example Distribution:**

```md
Topic: "orders" with 4 partitions, replication factor 3

Broker 1: Partition 0 (Leader), Partition 1 (Follower), Partition 3 (Follower)
Broker 2: Partition 1 (Leader), Partition 2 (Follower), Partition 0 (Follower)
Broker 3: Partition 2 (Leader), Partition 3 (Leader), Partition 0 (Follower)
```

**Leader-Follower Model:**

- **Leader**: Handles all reads and writes for that partition
- **Followers**: Replicate data from the leader (backup copies)
- If leader fails, a follower is automatically promoted to leader

**In-Sync Replicas (ISR):**

- Replicas that are fully caught up with the leader
- Only ISR replicas can become leaders
- Ensures data consistency and availability

### Consumer Replica Fetching (Kafka 2.4+)

Starting with **Kafka 2.4**, a groundbreaking performance optimization called **consumer replica fetching** (also known as **follower fetching** or **read from closest replica**) was introduced. This feature allows consumers to read from any **in-sync replica (ISR)** rather than being restricted to the partition leader only.

#### Traditional Model (Before 2.4)

Consumers could only read from the **partition leader**:

```md
┌─────────────────────────────────────────────────────────┐
│                   Datacenter 1 (US-East)                │
│                                                         │
│  Broker 1 (Leader) ◄─── Producer (writes)               │
│       │                                                 │
│       │ Replicates ────────────────────┐                │
│       │                                │                │
│  Broker 2 (Follower) ◄─────────────────┘                │
│                                                         │
│  Consumer A ──────┐                                     │
│                   │ Both read from leader               │
│  Consumer B ──────┴──► Broker 1 (Leader)                │
│                                                         │
└─────────────────────────────────────────────────────────┘
┌─────────────────────────────────────────────────────────┐
│                   Datacenter 2 (US-West)                │
│                                                         │
│  Broker 3 (Follower) ◄─── Replicates from Broker 1      │
│       ▲                                                 │
│       │                                                 │
│       │ High latency!                                   │
│       │                                                 │
│  Consumer C ──────┘ Must read from leader in DC1        │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

**Problems with Traditional Model:**

- ❌ All consumer traffic goes to leader brokers (single point of bottleneck)
- ❌ Followers only replicate data, not used for reads (wasted resources)
- ❌ Network bottlenecks on leader brokers
- ❌ High latency for consumers in different regions/racks (cross-datacenter reads)
- ❌ Expensive cross-region/cross-AZ data transfer costs in cloud
- ❌ Poor load distribution (leaders overworked, followers underutilized)

#### New Model (Kafka 2.4+)

Consumers can read from the **nearest in-sync replica** (leader or follower):

```md
┌─────────────────────────────────────────────────────────┐
│                   Datacenter 1 (US-East)                │
│                                                         │
│  Broker 1 (Leader) ◄─── Producer (writes)               │
│       │                     ▲                           │
│       │                     │                           │
│       │ Replicates          │ Reads locally             │
│       │                     │                           │
│       ▼                     │                           │
│  Broker 2 (Follower) ───────┘                           │
│       ▲                                                 │
│       │ Reads locally                                   │
│       │                                                 │
│  Consumer A ──────┘ (reads from Broker 1 - same rack)   │
│  Consumer B ──────┐ (reads from Broker 2 - same rack)   │
│                                                         │
└─────────────────────────────────────────────────────────┘
┌─────────────────────────────────────────────────────────┐
│                   Datacenter 2 (US-West)                │
│                                                         │
│  Broker 3 (Follower) ◄─── Replicates from Broker 1      │
│       ▲                                                 │
│       │ Reads locally (low latency!)                    │
│       │                                                 │
│  Consumer C ──────┘ (reads from Broker 3 - same DC)     │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

**Advantages of New Model:**

- ✅ Consumers read from nearby replicas (reduced latency)
- ✅ Followers actively serve read traffic (better resource utilization)
- ✅ Load distributed across all ISR replicas (scalability)
- ✅ Lower network costs (stay within same rack/AZ/region)
- ✅ Better cross-datacenter performance

#### Understanding ISR (In-Sync Replicas)

**ISR** is the set of replicas that are fully caught up with the leader. Only replicas in the ISR can serve consumer reads to ensure data consistency.

**ISR Criteria:**

A follower is considered in-sync if:

1. It has caught up with the leader's log end offset
2. It hasn't fallen behind by more than `replica.lag.time.max.ms` (default: 10 seconds)
3. It is actively fetching data from the leader

**ISR Example:**

```md
Topic: "orders" - Partition 0
Replication Factor: 3

Broker 1 (Leader):    [msg1][msg2][msg3][msg4][msg5] ← Latest
Broker 2 (Follower):  [msg1][msg2][msg3][msg4][msg5] ← In-Sync (ISR)
Broker 3 (Follower):  [msg1][msg2][msg3][msg4][msg5] ← In-Sync (ISR)
Broker 4 (Follower):  [msg1][msg2] ← Out-of-Sync (NOT in ISR)

ISR = {Broker 1, Broker 2, Broker 3}

Consumer Replica Fetching:
- Can read from: Broker 1, 2, or 3 (ISR members only)
- Cannot read from: Broker 4 (not in ISR - stale data)
```

**Why ISR Matters for Consumer Reads:**

```md
Scenario: Follower falls out of ISR

t=0s:   ISR = {Leader, Follower1, Follower2}
        Consumer can read from any of the 3

t=30s:  Follower2 network issue, falls behind
        ISR = {Leader, Follower1}
        Consumer automatically stops reading from Follower2
        Consumer switches to Leader or Follower1

t=60s:  Follower2 catches up
        ISR = {Leader, Follower1, Follower2}
        Consumer can read from Follower2 again
```

**Consistency Guarantee:**

- Consumers only read **committed messages** (messages replicated to all ISR members)
- High Water Mark (HWM) determines which messages are committed
- Followers serving reads only expose messages up to HWM
- Result: **Strong consistency** - all consumers see the same data regardless of which replica they read from

**Monitoring ISR:**

```bash
# Check ISR status
kafka-topics.sh --describe --topic orders --bootstrap-server localhost:9092

# Output shows:
# Partition: 0  Leader: 1  Replicas: 1,2,3  Isr: 1,2,3
# If Isr is smaller than Replicas, some replicas are out of sync
```

#### Rack Awareness

**Rack awareness** is the foundation that enables consumer replica fetching. It allows Kafka to understand the physical or logical topology of your infrastructure.

**What is a "Rack"?**

In Kafka terminology, a "rack" is a logical grouping that can represent:

- **Physical racks** in an on-premise datacenter
- **Availability Zones (AZs)** in AWS, GCP, Azure
- **Datacenters** in multi-region deployments
- **Geographic regions** for global deployments
- **Any logical grouping** that represents network proximity

**Broker Configuration: broker.rack**

Each broker must be assigned to a rack:

```properties
# Broker 1 - server.properties
broker.id=1
broker.rack=us-east-1a

# Broker 2 - server.properties
broker.id=2
broker.rack=us-east-1b

# Broker 3 - server.properties
broker.id=3
broker.rack=us-west-2a
```

**AWS Example (Availability Zones):**

```properties
# Broker in us-east-1a
broker.rack=us-east-1a

# Broker in us-east-1b
broker.rack=us-east-1b

# Broker in us-east-1c
broker.rack=us-east-1c
```

**Multi-Datacenter Example:**

```properties
# Broker in primary datacenter
broker.rack=dc-primary

# Broker in secondary datacenter
broker.rack=dc-secondary

# Broker in DR datacenter
broker.rack=dc-disaster-recovery
```

**Benefits of Rack Awareness:**

1. **Replica Placement**: Kafka distributes replicas across different racks for fault tolerance
2. **Consumer Optimization**: Consumers can read from replicas in their own rack
3. **Producer Optimization**: Producers can be configured to prefer local acks
4. **Failure Isolation**: Rack failure doesn't cause data loss (replicas in other racks)

**Replica Distribution with Rack Awareness:**

```md
Without rack awareness:
Topic: "events" - Replication Factor: 3
Partition 0: [Broker 1, Broker 2, Broker 3] (all might be in same rack - risky!)

With rack awareness:
Topic: "events" - Replication Factor: 3
Partition 0: [Broker 1 (rack-a), Broker 4 (rack-b), Broker 7 (rack-c)]
Ensures replicas are spread across different racks for fault tolerance
```

#### Replica Selector

The **replica selector** is the component that determines which replica a consumer should read from.

**Default: LeaderSelector (Pre-2.4 behavior)**

```properties
# Default behavior - always read from leader
replica.selector.class=org.apache.kafka.common.replica.LeaderSelector
```

**RackAwareReplicaSelector (Recommended for 2.4+)**

```properties
# Enable rack-aware replica selection
replica.selector.class=org.apache.kafka.common.replica.RackAwareReplicaSelector
```

**How RackAwareReplicaSelector Works:**

```md
1. Consumer sends fetch request with client.rack information
2. Broker's RackAwareReplicaSelector evaluates:
   a. Which replicas are in ISR (eligible to serve reads)
   b. Which ISR replica is in the same rack as consumer
3. Selector returns the preferred replica:
   - Same rack replica (if available and in ISR) → Best choice
   - Leader replica (if no same-rack replica in ISR) → Fallback
4. Consumer reads from selected replica
```

**Selection Algorithm:**

```java
// Simplified logic
Replica selectReplica(FetchRequest request) {
    String clientRack = request.getClientRack();
    List<Replica> isrReplicas = partition.getInSyncReplicas();
    
    // Prefer replica in same rack
    for (Replica replica : isrReplicas) {
        if (replica.getRack().equals(clientRack)) {
            return replica;  // Found same-rack ISR replica
        }
    }
    
    // Fallback to leader
    return partition.getLeader();
}
```

**Custom Replica Selector:**

You can implement your own replica selection logic:

```java
public class CustomReplicaSelector implements ReplicaSelector {
    @Override
    public Optional<ReplicaView> select(TopicPartition topicPartition,
                                        ClientMetadata clientMetadata,
                                        PartitionView partitionView) {
        // Custom logic: prefer replicas in specific datacenter,
        // or load-balance across multiple replicas, etc.
        return Optional.of(chosenReplica);
    }
}
```

```properties
# Use custom selector
replica.selector.class=com.mycompany.CustomReplicaSelector
```

#### Consumer Configuration: client.rack

Consumers must specify which rack they're in to benefit from replica fetching:

**Java Consumer:**

```java
Properties props = new Properties();
props.put("bootstrap.servers", "localhost:9092");
props.put("group.id", "my-consumer-group");
props.put("key.deserializer", "org.apache.kafka.common.serialization.StringDeserializer");
props.put("value.deserializer", "org.apache.kafka.common.serialization.StringDeserializer");

// Critical: Specify consumer's rack
props.put("client.rack", "us-east-1a");

KafkaConsumer<String, String> consumer = new KafkaConsumer<>(props);
```

**Spring Kafka:**

```yaml
spring:
  kafka:
    consumer:
      properties:
        client.rack: us-east-1a
```

**What Happens Without client.rack?**

```md
If consumer doesn't specify client.rack:
- Consumer always reads from partition leader
- Loses benefits of follower fetching
- Higher latency for cross-rack consumers
- Misses cost savings opportunities

With client.rack specified:
- Consumer reads from nearest ISR replica
- Reduced latency
- Lower data transfer costs
- Better load distribution
```

**Dynamic client.rack Assignment:**

```java
// Get rack from environment or cloud metadata
String rack = System.getenv("AWS_AVAILABILITY_ZONE");  // e.g., "us-east-1a"
// OR
String rack = getEC2InstanceMetadata("/placement/availability-zone");

props.put("client.rack", rack);
```

**Best Practices:**

```properties
# ✅ Be specific with rack IDs
client.rack=us-east-1a  # Good

# ✅ Match broker.rack format exactly
# If brokers use "us-east-1a", consumers should too

# ❌ Don't use generic values
client.rack=east  # Too broad, won't match specific broker racks
```

#### Producers and Consumers in Same vs. Different Datacenters

The location of producers and consumers relative to brokers significantly impacts performance and architecture decisions.

##### Scenario 1: Everything in Same Datacenter (Simplest)

```md
┌─────────────────────────────────────────────────────────┐
│                   Datacenter: US-East-1                 │
│                                                         │
│  Producer ──────► Broker 1 (Leader)                     │
│                        │                                │
│                        │ Replicates                     │
│                        ▼                                │
│                   Broker 2 (Follower)                   │
│                        │                                │
│                        │ Replicates                     │
│                        ▼                                │
│                   Broker 3 (Follower)                   │
│                        ▲                                │
│                        │ Reads locally                  │
│                        │                                │
│  Consumer ─────────────┘                                │
│                                                         │
└─────────────────────────────────────────────────────────┘

Characteristics:
✅ Lowest latency (everything local)
✅ No cross-datacenter costs
✅ Simplest configuration
❌ Single point of failure (datacenter outage)
❌ Not suitable for global applications
```

##### Scenario 2: Producers and Consumers in Same DC, Brokers Distributed

```md
┌─────────────────────────────────────────────────────────┐
│                   Datacenter 1: US-East                 │
│                                                         │
│  Producer ──────► Broker 1 (Leader) ◄────── Consumer A  │
│                        │                                │
│                        │ Replicates                     │
│                        ▼                                │
│                   Broker 2 (Follower) ◄───── Consumer B │
│                                                         │
└─────────────────────────────────────────────────────────┘
                          │
                          │ Cross-DC Replication
                          ▼
┌─────────────────────────────────────────────────────────┐
│                   Datacenter 2: US-West                 │
│                                                         │
│                   Broker 3 (Follower)                   │
│                                                         │
└─────────────────────────────────────────────────────────┘

Producer Configuration:
props.put("acks", "all");  // Wait for all ISR replicas (including US-West)
// Write latency increases due to cross-DC replication

Consumer Configuration:
props.put("client.rack", "us-east-1a");  // Reads locally from Broker 1 or 2

Characteristics:
✅ High availability (replicas in multiple DCs)
✅ Consumers read locally (low latency)
⚠️ Producer latency increased (waits for cross-DC replication)
💰 Cross-DC replication costs (producer → broker traffic only)
```

##### Scenario 3: Brokers in One DC, Consumers in Another (Anti-pattern Before 2.4)

```md
┌─────────────────────────────────────────────────────────┐
│                   Datacenter 1: US-East                 │
│                                                         │
│  Producer ──────► Broker 1 (Leader)                     │
│                        │                                │
│                        │ Replicates                     │
│                        ▼                                │
│                   Broker 2 (Follower)                   │
│                        │                                │
│                        │ Replicates                     │
│                        ▼                                │
│                   Broker 3 (Follower)                   │
│                                                         │
└─────────────────────────────────────────────────────────┘
                          ▲
                          │ Cross-DC reads (high latency!)
                          │
┌─────────────────────────────────────────────────────────┐
│                   Datacenter 2: US-West                 │
│                        │                                │
│  Consumer ─────────────┘                                │
│                                                         │
└─────────────────────────────────────────────────────────┘

Before Kafka 2.4:
❌ Consumer reads from leader in US-East (high latency)
❌ High cross-DC data transfer costs
❌ Poor performance

After Kafka 2.4 (with replica fetching):
✅ Add Broker 4 (Follower) in US-West datacenter
✅ Consumer reads from local Broker 4
✅ Low latency, reduced costs
```

##### Scenario 4: Multi-Region Active-Active (Optimal with 2.4+)

```md
┌─────────────────────────────────────────────────────────┐
│                   Region: US-East                       │
│                                                         │
│  Producer A ────► Broker 1 (Leader for Part 0,1)        │
│                        │                                │
│                        │ Replicates                     │
│                        ▼                                │
│                   Broker 2 (Follower for Part 2,3)      │
│                        ▲                                │
│                        │ Reads locally                  │
│  Consumer A ───────────┘                                │
│                                                         │
└─────────────────────────────────────────────────────────┘
                          │
                          │ Cross-Region Replication
                          ▼
┌─────────────────────────────────────────────────────────┐
│                   Region: US-West                       │
│                                                         │
│  Producer B ────► Broker 3 (Leader for Part 2,3)        │
│                        │                                │
│                        │ Replicates                     │
│                        ▼                                │
│                   Broker 4 (Follower for Part 0,1)      │
│                        ▲                                │
│                        │ Reads locally                  │
│  Consumer B ───────────┘                                │
│                                                         │
└─────────────────────────────────────────────────────────┘

Configuration:
# US-East Brokers
broker.rack=us-east
# US-West Brokers
broker.rack=us-west

# US-East Consumer
client.rack=us-east  # Reads from Broker 1 or 2

# US-West Consumer
client.rack=us-west  # Reads from Broker 3 or 4

Characteristics:
✅ Both regions serve producers and consumers
✅ Consumers read locally (low latency)
✅ Leadership distributed across regions
✅ Fault tolerant (either region can fail)
✅ Optimal for global applications
💰 Cross-region replication costs (unavoidable but minimized)
```

##### Scenario 5: Disaster Recovery (DR) Setup

```md
┌─────────────────────────────────────────────────────────┐
│                Primary Datacenter: US-East              │
│                                                         │
│  Producers ─────► Broker 1 (Leader)                     │
│                        │                                │
│                        │ Replicates                     │
│                        ▼                                │
│                   Broker 2 (Follower)                   │
│                        ▲                                │
│                        │                                │
│  Consumers ────────────┘ (reads locally)                │
│                                                         │
└─────────────────────────────────────────────────────────┘
                          │
                          │ Async Replication
                          ▼
┌─────────────────────────────────────────────────────────┐
│                DR Datacenter: US-West                   │
│                                                         │
│                   Broker 3 (Follower)                   │
│                        ▲                                │
│                        │                                │
│  Standby Consumers ────┘ (reads from local replica)     │
│  (monitoring, testing)                                  │
│                                                         │
└─────────────────────────────────────────────────────────┘

Normal Operation:
- Primary producers/consumers in US-East
- DR consumers in US-West read from local Broker 3 (for monitoring/testing)
- Low latency for DR consumers

During Failover:
- DR broker becomes leader
- DR consumers already reading from local broker (no change needed)
- Seamless transition
```

#### Complete Configuration Example

**Multi-Datacenter Setup with Rack Awareness:**

**Broker Configuration (3 brokers across 3 AZs):**

```properties
# Broker 1 - us-east-1a/server.properties
broker.id=1
broker.rack=us-east-1a
listeners=PLAINTEXT://broker1:9092
replica.selector.class=org.apache.kafka.common.replica.RackAwareReplicaSelector

# Broker 2 - us-east-1b/server.properties
broker.id=2
broker.rack=us-east-1b
listeners=PLAINTEXT://broker2:9092
replica.selector.class=org.apache.kafka.common.replica.RackAwareReplicaSelector

# Broker 3 - us-west-2a/server.properties
broker.id=3
broker.rack=us-west-2a
listeners=PLAINTEXT://broker3:9092
replica.selector.class=org.apache.kafka.common.replica.RackAwareReplicaSelector
```

**Producer Configuration (in us-east-1a):**

```java
Properties producerProps = new Properties();
producerProps.put("bootstrap.servers", "broker1:9092,broker2:9092,broker3:9092");
producerProps.put("key.serializer", "org.apache.kafka.common.serialization.StringSerializer");
producerProps.put("value.serializer", "org.apache.kafka.common.serialization.StringSerializer");

// Producer writes go to partition leaders (rack doesn't matter for writes)
// But you can optionally set for monitoring/logging
producerProps.put("client.rack", "us-east-1a");

// Wait for all ISR replicas (including cross-region)
producerProps.put("acks", "all");

// Enable idempotence for exactly-once
producerProps.put("enable.idempotence", "true");

KafkaProducer<String, String> producer = new KafkaProducer<>(producerProps);
```

**Consumer Configuration (in us-east-1a):**

```java
Properties consumerProps = new Properties();
consumerProps.put("bootstrap.servers", "broker1:9092,broker2:9092,broker3:9092");
consumerProps.put("group.id", "my-consumer-group");
consumerProps.put("key.deserializer", "org.apache.kafka.common.serialization.StringDeserializer");
consumerProps.put("value.deserializer", "org.apache.kafka.common.serialization.StringDeserializer");

// CRITICAL: Tell consumer which rack it's in
consumerProps.put("client.rack", "us-east-1a");

// Consumer will automatically read from Broker 1 (same rack) if it's in ISR
// Falls back to Broker 2 or leader if Broker 1 is unavailable

KafkaConsumer<String, String> consumer = new KafkaConsumer<>(consumerProps);
```

**Consumer Configuration (in us-west-2a):**

```java
Properties consumerProps = new Properties();
consumerProps.put("bootstrap.servers", "broker1:9092,broker2:9092,broker3:9092");
consumerProps.put("group.id", "my-consumer-group");
consumerProps.put("key.deserializer", "org.apache.kafka.common.serialization.StringDeserializer");
consumerProps.put("value.deserializer", "org.apache.kafka.common.serialization.StringDeserializer");

// Tell consumer it's in us-west
consumerProps.put("client.rack", "us-west-2a");

// Consumer will automatically read from Broker 3 (same rack) if it's in ISR
// This avoids cross-region data transfer!

KafkaConsumer<String, String> consumer = new KafkaConsumer<>(consumerProps);
```

#### Benefits Summary

**Performance Benefits:**

| Metric                      | Before 2.4  | After 2.4 (with Replica Fetching) | Improvement      |
|-----------------------------|-------------|-----------------------------------|------------------|
| Read Latency (same rack)    | 50-100ms    | 1-5ms                             | 10-50x faster    |
| Read Latency (cross-region) | 200-500ms   | 1-5ms                             | 40-100x faster   |
| Leader CPU                  | 80-90%      | 30-50%                            | 40% reduction    |
| Network Cross-AZ            | High        | Low                               | 70-90% reduction |
| Read Throughput             | Leader only | All ISR replicas                  | 2-3x increase    |

**Cost Benefits (AWS Example):**

```md
Topic: 1TB/day consumption
Consumers: 10 in different AZs

Before Kafka 2.4:
- All reads from leader: 10TB/day cross-AZ traffic
- AWS cross-AZ cost: $0.01/GB
- Monthly cost: 10TB × 30 days × $0.01 = $3,000

After Kafka 2.4 (with replica fetching):
- Each consumer reads from local replica: 0TB cross-AZ
- Monthly cost: $0
- Savings: $3,000/month or $36,000/year
```

#### Limitations and Considerations

**1. ISR Requirement:**

```md
✅ Follower in ISR → Can serve consumer reads
❌ Follower not in ISR → Cannot serve reads (consumers fallback to leader)

If follower falls out of ISR:
- Consumers automatically redirect to another ISR replica
- May cause temporary latency spike
- Monitor ISR health!
```

**2. Writes Still Go to Leader:**

```md
Producer writes:
Always → Partition Leader → Replicate to followers

Consumer reads:
Can come from → Any ISR replica (leader or follower)

Benefit: Reads are optimized, writes remain consistent
```

**3. Metadata Requests:**

```md
Consumer metadata requests (topic info, partition assignments, etc.):
- Still go to any broker
- Not affected by client.rack

Only fetch requests (actual data reads) benefit from replica selection
```

**4. Rebalancing Considerations:**

```md
During rebalancing:
- Consumer may switch to different replica
- client.rack preference is re-evaluated
- Usually seamless, but brief latency spike possible
```

**5. Monitoring Requirements:**

```bash
# Monitor ISR status
kafka-topics.sh --describe --topic my-topic --bootstrap-server localhost:9092

# Monitor consumer lag per replica
kafka-consumer-groups.sh --describe --group my-group --bootstrap-server localhost:9092

# Check which replica consumer is reading from
# (Check consumer metrics: fetch-manager-metrics)
```

**6. Configuration Mismatch Issues:**

```md
❌ Broker rack: "us-east-1a"
❌ Consumer rack: "east-1a"
Result: No match, consumer reads from leader

✅ Broker rack: "us-east-1a"
✅ Consumer rack: "us-east-1a"
Result: Perfect match, consumer reads from local follower
```

#### Troubleshooting

**Problem: Consumer not reading from local replica**

```bash
# Check broker rack configuration
kafka-configs.sh --describe --broker 1 --bootstrap-server localhost:9092

# Verify consumer is setting client.rack
# Check consumer logs for:
# "Discovered group coordinator" - should show local broker

# Verify replica selector is configured on brokers
# In server.properties:
replica.selector.class=org.apache.kafka.common.replica.RackAwareReplicaSelector
```

**Problem: High cross-AZ costs still occurring**

```bash
# Monitor fetch metrics
# Consumer should show fetch-manager-metrics with replica details

# Check ISR status - followers might be out of sync
kafka-topics.sh --describe --topic my-topic --bootstrap-server localhost:9092

# Look for: Isr: [1,2,3] - all replicas should be in ISR
# If Isr: [1] - only leader is in sync, followers not serving reads
```

**Problem: Increased latency after enabling replica fetching**

```md
Possible causes:
1. Follower lag: Follower is behind leader (check ISR)
2. Follower load: Follower overloaded with read requests
3. Misconfiguration: Wrong rack IDs causing suboptimal routing
4. Network issues: Local replica connection problems

Solution:
- Monitor follower lag (replica.lag.time.max.ms)
- Add more replicas to distribute load
- Verify rack configuration matches exactly
- Check network connectivity between consumer and local brokers
```

#### Best Practices

**1. Always Configure Rack Awareness:**

```properties
# Even if single datacenter, use rack IDs for physical racks
broker.rack=rack-1  # Broker in rack 1
broker.rack=rack-2  # Broker in rack 2
```

**2. Match Rack Granularity to Your Needs:**

```properties
# Fine-grained (availability zone level)
broker.rack=us-east-1a
client.rack=us-east-1a

# Coarse-grained (datacenter level)
broker.rack=dc-east
client.rack=dc-east

# Choose based on your fault tolerance and cost optimization goals
```

**3. Monitor ISR Health:**

```bash
# Set up alerts for ISR shrinkage
# Alert if: Isr.size < Replicas.size

# Monitor replica lag
# Alert if: replica.lag.time.max.ms threshold exceeded
```

**4. Test Failover Scenarios:**

```md
Test plan:
1. Stop broker in consumer's rack
2. Verify consumer switches to another ISR replica
3. Measure latency impact
4. Restart broker and verify consumer switches back
```

**5. Use Appropriate Replication Factor:**

```properties
# Multi-datacenter: Ensure at least one replica per datacenter
# 3 datacenters → replication.factor=3 (minimum)
# More replicas = more read capacity but more storage
```

**6. Document Rack Topology:**

```md
# Maintain documentation of rack assignments
# Example:
Broker 1: rack=us-east-1a  (AZ-1a, 10.0.1.50)
Broker 2: rack=us-east-1b  (AZ-1b, 10.0.2.50)
Broker 3: rack=us-west-2a  (AZ-2a, 10.1.1.50)
```

### Horizontal Scaling with Brokers

Brokers enable **horizontal scalability** - you can add more brokers to handle more load.

**Scaling Storage:**

```md
Initial Setup (1 TB storage):
Broker 1: 1 TB

After adding brokers (3 TB total):
Broker 1: 1 TB
Broker 2: 1 TB
Broker 3: 1 TB
```

**Scaling Throughput:**

More brokers = more partition leaders = more parallel read/write operations

```md
3 Brokers with 6 partitions:
- Each broker leads 2 partitions
- Throughput: 300 MB/s per broker × 3 = 900 MB/s total

6 Brokers with 12 partitions:
- Each broker leads 2 partitions
- Throughput: 300 MB/s per broker × 6 = 1800 MB/s total
```

**Partition Rebalancing:**

When you add new brokers, you can rebalance partitions:

```md
Before (2 brokers):
Broker 1: Partitions 0, 1, 2, 3
Broker 2: Partitions 4, 5, 6, 7

After adding Broker 3:
Broker 1: Partitions 0, 1, 2
Broker 2: Partitions 3, 4, 5
Broker 3: Partitions 6, 7
```

**Advantages:**

- ✅ Linear scalability - add more brokers for more capacity
- ✅ No single point of failure - replicas distributed across brokers
- ✅ Load distribution - partitions spread evenly
- ✅ Hot partition mitigation - high-traffic partitions on different brokers

### Bootstrap Servers and Discovery

**Bootstrap Servers:**

When clients (producers/consumers) connect to Kafka, they only need to know about one or a few brokers initially. This is called the **bootstrap process**.

**Configuration:**

```java
Properties props = new Properties();
props.put("bootstrap.servers", "broker1:9092,broker2:9092,broker3:9092");
```

You don't need to list all brokers - just provide 2-3 for redundancy.

**Discovery Mechanism (Metadata Request):**

Once connected, clients automatically discover the entire cluster:

```md
1. Client connects to any bootstrap server
2. Client sends metadata request
3. Broker returns cluster metadata:
    - List of all brokers (ID, host, port)
    - List of topics and partitions
    - Partition leaders and replicas
    - ISR (In-Sync Replicas) information
4. Client caches this metadata
5. Client connects directly to leader brokers for each partition
```

**Example Metadata Response:**

```json
{
  "brokers": [
    {"id": 1, "host": "kafka1.example.com", "port": 9092},
    {"id": 2, "host": "kafka2.example.com", "port": 9092},
    {"id": 3, "host": "kafka3.example.com", "port": 9092}
  ],
  "topics": {
    "orders": {
      "partitions": [
        {"id": 0, "leader": 1, "replicas": [1, 2, 3], "isr": [1, 2, 3]},
        {"id": 1, "leader": 2, "replicas": [2, 3, 1], "isr": [2, 3, 1]},
        {"id": 2, "leader": 3, "replicas": [3, 1, 2], "isr": [3, 1, 2]}
      ]
    }
  }
}
```

**Metadata Refresh:**

- Clients periodically refresh metadata (default: every 5 minutes)
- Clients also refresh metadata when they detect failures
- This allows clients to adapt to cluster changes (new brokers, leader changes, etc.)

**Why Bootstrap Discovery is Powerful:**

1. **Resilience**: If one bootstrap server is down, clients try others
2. **Dynamic topology**: Clients learn about all brokers automatically
3. **Leader routing**: Clients know which broker to contact for each partition
4. **Automatic failover**: When leaders change, clients update their routing
5. **No load balancer needed**: Clients connect directly to the right brokers

**Connection Flow Example:**

```md
Producer wants to write to topic "orders", partition 2:

1. Producer connects to broker2:9092 (bootstrap server)
2. Producer requests metadata for "orders"
3. Broker2 responds: "Partition 2 leader is Broker 3"
4. Producer establishes connection to Broker 3
5. Producer writes directly to Broker 3 for partition 2
```

### ZooKeeper and Controller (Cluster Coordination)

**ZooKeeper (older versions, being phased out):**

- External service for cluster coordination
- Manages broker membership and leader election
- Stores cluster metadata
- Required in Kafka < 3.0

**KRaft Mode (Kafka 3.0+, production-ready as of 3.3):**

- Built-in consensus protocol (no ZooKeeper needed)
- One or more brokers act as **controllers**
- Controllers manage cluster metadata and leader election
- Simpler architecture, better scalability

**Controller Responsibilities:**

- Elect partition leaders when brokers fail
- Manage partition reassignments
- Handle topic creation/deletion
- Maintain cluster metadata

---

## Banking Use Case: Async Transfers with Kafka

This section answers a classic senior-level interview question:

> *"How would you design a bank transfer system that feels fast to the user but guarantees correctness — using Kafka, without cron jobs, and without losing money?"*

---

### The Problem: Consistency vs. Latency

A naive synchronous transfer looks like this:

```sh
User clicks "Transfer" →
  1. DB: debit account A
  2. DB: credit account B
  3. Publish Kafka event "TransferCompleted"
  4. Return 200 OK
```

**What goes wrong?**

- Step 3 can fail after steps 1+2 committed → Kafka never knows → downstream services (notifications, ledger, fraud, audit) never triggered.
- If step 1 or 2 fails mid-way, partial updates happen unless wrapped in a transaction.
- The DB and Kafka are **two different systems** — you cannot have a distributed transaction across both with standard JDBC + Kafka.

This is the **dual-write problem**: writing to two systems atomically is impossible without a coordination protocol.

---

### The Transactional Outbox Pattern

The solution is to **never write directly to Kafka from your service**. Instead:

1. Write to the DB and an **outbox table** in the **same local DB transaction**.
2. A separate relay process reads the outbox and publishes to Kafka.

```sh
┌─────────────────────────────────┐
│        Transfer Service         │
│                                 │
│  BEGIN TRANSACTION              │
│    UPDATE accounts (debit A)    │
│    UPDATE accounts (credit B)   │
│    INSERT INTO outbox (event)   │ ← same transaction!
│    publishEvent(trigger)        │ ← Spring in-memory event
│  COMMIT                         │
└─────────────────────────────────┘
              │
              ▼  @TransactionalEventListener(AFTER_COMMIT) fires
┌──────────────────────────────────┐
│  OutboxRelayPublisher            │──► Kafka topic: bank.transfers
│  (Spring @Component, built-in)   │
└──────────────────────────────────┘
              │
              ▼
┌──────────────────────────────────────────────────────┐
│  Consumers: Notification, Fraud, Ledger, Audit...    │
└──────────────────────────────────────────────────────┘
```

**Key guarantee**: The debit + credit + outbox INSERT are atomic. Either all succeed or none do. The outbox row is the proof that the event must be published.

#### Outbox Table Schema

```sql
CREATE TABLE outbox (
    id          UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    event_type  VARCHAR(100) NOT NULL,         -- e.g. 'TRANSFER_INITIATED'
    payload     JSONB NOT NULL,                -- full event data
    topic       VARCHAR(200) NOT NULL,         -- target Kafka topic
    status      VARCHAR(20) DEFAULT 'PENDING', -- PENDING | SENT
    created_at  TIMESTAMP NOT NULL DEFAULT NOW(),
    sent_at     TIMESTAMP
);

CREATE INDEX idx_outbox_status ON outbox(status) WHERE status = 'PENDING';
```

#### Application Code (Spring Boot)

```java
@Service
@RequiredArgsConstructor
public class TransferService {

    private final AccountRepository accountRepo;
    private final OutboxRepository outboxRepo;

    @Transactional  // single DB transaction — both writes are atomic
    public TransferResult transfer(TransferRequest req) {
        Account from = accountRepo.findByIdForUpdate(req.fromAccountId()); // SELECT FOR UPDATE
        Account to   = accountRepo.findByIdForUpdate(req.toAccountId());

        if (from.getBalance().compareTo(req.amount()) < 0) {
            throw new InsufficientFundsException();
        }

        from.debit(req.amount());
        to.credit(req.amount());

        // Write domain event to outbox — same transaction
        OutboxEvent event = OutboxEvent.builder()
            .eventType("TRANSFER_INITIATED")
            .topic("bank.transfers")
            .payload(buildPayload(req, from, to))
            .build();

        outboxRepo.save(event);

        // Return immediately — Kafka publish happens asynchronously
        return TransferResult.accepted(req.transactionId());
    }
}
```

> ⚡ The user gets **200 Accepted** (or **202 Accepted**) in **< 50ms** — only local DB writes, no Kafka round-trip in the critical path.

---

### Single-Writer Principle

> **Only one service owns a piece of data and is allowed to write to it.**

In the banking context:

- The **Account Service** is the **single writer** for account balances.
- No other service (Notification, Fraud, Audit) ever directly writes to the `accounts` table.
- All downstream services consume Kafka events and maintain their own **read models**.

**Why this matters:**

| Without Single-Writer                              | With Single-Writer                                        |
|----------------------------------------------------|-----------------------------------------------------------|
| Multiple services update balance → race conditions | Only Account Service modifies balance → no conflicts      |
| Hard to reason about who changed what              | Clear audit trail                                         |
| Need distributed locking across services           | Local DB locks are sufficient                             |
| Inconsistent reads across services                 | Each service has its own eventually-consistent read model |

**Practical enforcement:**

```java
// ✅ Account Service owns writes
@RestController
@RequestMapping("/api/accounts")
public class AccountController {
    @PostMapping("/{id}/transfer")
    public ResponseEntity<TransferResult> transfer(...) { ... }
}

// ❌ Fraud Service should NOT do this:
accountRepository.save(updatedAccount); // Never — it's not the owner!
// ✅ Fraud Service should do this:
kafkaTemplate.send("fraud.alerts", fraudEvent); // Publish its own events
```

---

### Full Bank Transfer Flow

This is the complete end-to-end flow combining low latency + correctness + async processing:

```
1. User submits transfer via REST API
         │
         ▼
2. Transfer Service
   ├── Validate (balance check, limits)
   ├── BEGIN TRANSACTION
   │    ├── Debit account A (SELECT FOR UPDATE → UPDATE)
   │    ├── Credit account B (SELECT FOR UPDATE → UPDATE)
   │    └── INSERT INTO outbox (TRANSFER_INITIATED event)
   └── COMMIT
   └── Return 202 Accepted { transactionId: "abc-123" } ← fast!
         │
         ▼
3. @TransactionalEventListener(AFTER_COMMIT) fires automatically
   └── OutboxRelayPublisher reads outbox row → publishes to Kafka: topic=bank.transfers, key=accountId
         │
         ▼ (async, milliseconds later)
4. Kafka Consumers (each in its own consumer group):
   ├── Ledger Service      → records the double-entry in ledger
   ├── Notification Service → sends push/email to user
   ├── Fraud Service       → scores the transaction
   └── Audit Service       → writes to immutable audit log
         │
         ▼
5. User polls GET /api/transfers/abc-123
   └── Returns status: COMPLETED / PENDING / FAILED
```

#### HTTP Status Codes for Async Operations

| Status | Meaning |
|--------|---------|
| `202 Accepted` | Request received, processing async. Returns `{ transactionId }`. |
| `200 OK` | Synchronous — result is in the response body. Don't use for async. |
| `409 Conflict` | Duplicate request detected (same idempotency key). |

---

### Idempotency on the Consumer Side

Kafka delivers **at-least-once** by default. The `@TransactionalEventListener` could fire more than once on retry, and Kafka itself may redeliver messages on consumer restart. Every consumer **must** be idempotent.

#### Strategy: Deduplication Table

```sql
CREATE TABLE processed_events (
    event_id   UUID PRIMARY KEY,
    processed_at TIMESTAMP NOT NULL DEFAULT NOW()
);
```

```java
@KafkaListener(topics = "bank.transfers", groupId = "ledger-service")
@Transactional
public void onTransfer(TransferEvent event) {
    // Idempotency check + business logic in one transaction
    if (processedEventRepo.existsById(event.getEventId())) {
        log.info("Duplicate event {}, skipping", event.getEventId());
        return;
    }

    ledgerService.recordEntry(event);                          // business logic
    processedEventRepo.save(new ProcessedEvent(event.getEventId())); // mark done
}
// If the transaction rolls back, the event is not marked as processed → safe retry
```

#### Partition Key for Ordering

Use the **account ID as the Kafka partition key**. This guarantees:
- All events for the same account are in the **same partition** → strictly ordered.
- Concurrent events for different accounts are processed in parallel → high throughput.

```java
kafkaTemplate.send("bank.transfers", transfer.getFromAccountId().toString(), event);
//                  topic              key (partition selector)              value
```

---

### How to Relay the Outbox Event to Kafka (Without External Tools)

The answer is **Spring's `@TransactionalEventListener`** — built into Spring, no extra libraries.

**The key idea:**
- Inside the `@Transactional` method, call `eventPublisher.publishEvent(...)` — this queues an **in-memory** Spring event (nothing happens yet)
- Spring **holds** it until the DB transaction commits
- If the transaction **rolls back**, the event is silently **discarded** — Kafka is never touched
- After a successful commit, Spring fires the listener immediately → Kafka publish happens

No polling. No cron. No external tools.

---

#### The `@TransactionalEventListener` + Outbox Pattern

Write to the outbox in the same transaction, trigger the relay via `AFTER_COMMIT`:

```java
// 1. In-memory trigger event (plain Java record)
public record OutboxRelayTrigger(UUID outboxId) {}
```

```java
// 2. Service: everything in one DB transaction
@Service
@RequiredArgsConstructor
public class TransferService {

    private final AccountRepository accountRepo;
    private final OutboxRepository outboxRepo;
    private final ApplicationEventPublisher eventPublisher; // Spring built-in

    @Transactional
    public TransferResult transfer(TransferRequest req) {
        Account from = accountRepo.findByIdForUpdate(req.fromAccountId()); // SELECT FOR UPDATE
        Account to   = accountRepo.findByIdForUpdate(req.toAccountId());

        from.debit(req.amount());
        to.credit(req.amount());

        // Write outbox row atomically with the balance changes
        OutboxEvent saved = outboxRepo.save(OutboxEvent.builder()
            .eventId(req.transactionId())
            .eventType("TRANSFER_COMPLETED")
            .payload(serialize(req))
            .build());

        // Queue the in-memory trigger — Spring holds it until AFTER commit
        eventPublisher.publishEvent(new OutboxRelayTrigger(saved.getId()));

        return TransferResult.accepted(req.transactionId()); // 202 returned immediately
    }
}
```

```java
// 3. Relay: fires automatically after the DB transaction commits
@Component
@RequiredArgsConstructor
public class OutboxRelayPublisher {

    private final OutboxRepository outboxRepo;
    private final KafkaTemplate<String, String> kafkaTemplate;

    @TransactionalEventListener(phase = TransactionPhase.AFTER_COMMIT)
    public void relay(OutboxRelayTrigger trigger) {
        OutboxEvent event = outboxRepo.findById(trigger.outboxId()).orElseThrow();

        kafkaTemplate.send("bank.transfers", event.getEventId(), event.getPayload())
            .whenComplete((result, ex) -> {
                if (ex == null) {
                    outboxRepo.markAsSent(event.getId()); // Clean up outbox row
                } else {
                    // Row stays PENDING — can be recovered on restart or via retry endpoint
                    log.error("Kafka publish failed, outbox row retained for recovery", ex);
                }
            });
    }
}
```

#### Why this is the right answer

| Property                    | Guarantee                                         |
|-----------------------------|---------------------------------------------------|
| DB rollback → no Kafka call | ✅ `AFTER_COMMIT` only fires on success            |
| App crash after commit      | ✅ Outbox row stays `PENDING` — durable safety net |
| No cron job / polling       | ✅ Purely event-driven                             |
| No external tools           | ✅ Pure Spring                                     |
| Low latency                 | ✅ Fires immediately after commit                  |

**The one remaining edge case:** app crashes in the window between DB commit and Kafka publish. The outbox row survives (it's in the DB), so on restart you can replay any `PENDING` rows. This is the recovery path — not the happy path.

---

### Interview Summary

If asked *"How do you handle a bank transfer with low latency but guaranteed consistency?"*, answer with these key points:

1. **ACID local transaction**: Debit + credit happen in one DB transaction. No partial state possible.

2. **Dual-write problem**: You can't atomically write to both a DB and Kafka. The DB is always the source of truth.

3. **`@TransactionalEventListener(AFTER_COMMIT)`**: Spring built-in. Publishes to Kafka only after the DB commits. No polling, no external tools. Simple and clean for an interview.

4. **Outbox for production**: For zero tolerance of lost events, add an outbox table written atomically with the business data. The `@TransactionalEventListener` relays it — no external tools needed.

5. **Single-Writer**: Only the Account Service writes balances. Other services consume events.

6. **Low latency to user**: Return `202 Accepted` after DB commit (~10–50ms). Kafka publish is async.

7. **Idempotent consumers**: Every downstream consumer deduplicates by `eventId`.

8. **Partition key = accountId**: Guarantees ordering of events per account.

```sh
┌─────────────────────────────────────────────────────────────────────┐
│                    BANK TRANSFER GUARANTEE STACK                    │
├──────────────────────┬──────────────────────────────────────────────┤
│ Correctness          │ ACID + single DB transaction                 │
│ Durability           │ DB WAL + Kafka replication (acks=all)        │
│ No dual-write        │ Outbox written in same transaction           │
│ No lost events       │ @TransactionalEventListener(AFTER_COMMIT)    │
│ No duplicates        │ Idempotent consumers + dedup table           │
│ Ordering             │ Partition key = accountId                    │
│ Low latency to user  │ 202 Accepted before Kafka publish            │
│ Single authority     │ Single-Writer per aggregate                  │
│ No external tools    │ Pure Spring — no external tools, no cron     │
└──────────────────────┴──────────────────────────────────────────────┘
```
