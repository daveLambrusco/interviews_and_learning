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
    - [Important Consumer Considerations](#important-consumer-considerations)
  - [How They Work Together](#how-they-work-together)
  - [Brokers](#brokers)
    - [Brokers and Partitions](#brokers-and-partitions)
    - [Consumer Replica Fetching (Kafka 2.4+)](#consumer-replica-fetching-kafka-24)
      - [Traditional Model (Before 2.4)](#traditional-model-before-24)
      - [New Model (Kafka 2.4+)](#new-model-kafka-24)
        - [Example Scenario](#example-scenario)
      - [Benefits](#benefits)
    - [Horizontal Scaling with Brokers](#horizontal-scaling-with-brokers)
    - [Bootstrap Servers and Discovery](#bootstrap-servers-and-discovery)
    - [ZooKeeper and Controller (Cluster Coordination)](#zookeeper-and-controller-cluster-coordination)

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

Starting with **Kafka 2.4**, a performance optimization called **consumer replica fetching** (also known as **follower fetching**) was introduced.

#### Traditional Model (Before 2.4)

Consumers could only read from the **partition leader**:

```md
Consumer → Leader Broker (reads from leader only)
           Follower Broker 1 (idle for consumers)
           Follower Broker 2 (idle for consumers)
```

**Problem:**

- All consumer traffic goes to leader brokers
- Followers only replicate data, not used for reads
- Can cause network bottlenecks, especially in cross-datacenter scenarios
- Consumers in different regions/racks had high latency

#### New Model (Kafka 2.4+)

Consumers can read from the **nearest replica** (leader or follower):

```md
Consumer in Rack A → Follower Broker (Rack A) - low latency
Consumer in Rack B → Leader Broker (Rack B) - low latency
Consumer in Rack C → Follower Broker (Rack C) - low latency
```

**How It Works:**

1. **Rack Awareness**: Brokers are configured with rack IDs (can represent datacenters, availability zones, or physical racks)
2. **Replica Selection**: Consumers automatically fetch from the closest replica based on rack/datacenter
3. **Consistency**: Followers only serve data that's been committed (part of ISR), so consumers see consistent data
4. **Automatic Fallback**: If the nearest replica fails, consumers automatically switch to another replica

**Broker Configuration:**

```properties
# Assign each broker to a rack/zone
broker.rack=us-east-1a
replica.selector.class=org.apache.kafka.common.replica.RackAwareReplicaSelector
```

**Consumer Configuration:**

```java
Properties props = new Properties();
props.put("client.rack", "us-east-1a");  // Tell consumer which rack it's in
```

##### Example Scenario

```md
Topic: "events" with 1 partition, replication factor 3

Broker 1 (Leader) - us-east-1a
Broker 2 (Follower) - us-east-1b  
Broker 3 (Follower) - us-west-2a

Consumer A (rack: us-east-1a) → Reads from Broker 1 (leader, same rack)
Consumer B (rack: us-east-1b) → Reads from Broker 2 (follower, same rack)
Consumer C (rack: us-west-2a) → Reads from Broker 3 (follower, same rack)
```

#### Benefits

✅ **Reduced latency**: Consumers read from nearby brokers  
✅ **Lower network costs**: Especially in cloud environments (cross-AZ/region charges)  
✅ **Better load distribution**: Followers help with read traffic  
✅ **Improved scalability**: More brokers can serve reads  
✅ **Cross-datacenter efficiency**: Crucial for multi-region deployments

**Important Notes:**

- Only works with **committed messages** (part of ISR)
- Requires **rack awareness** configuration on brokers
- Consumer must specify its `client.rack` to benefit
- Followers must be in-sync to serve reads
- No impact on write operations (still go to leader only)
- Compatible with all consumer delivery guarantees (at-least-once, exactly-once)

**Use Cases:**

1. **Multi-datacenter deployments**: Consumers in different regions read from local replicas
2. **Cloud environments**: Reduce cross-availability-zone data transfer costs
3. **High-throughput topics**: Distribute read load across multiple brokers
4. **Disaster recovery**: Consumers in backup datacenter read from local replicas

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
