
# Apache Kafka - Introduction

## What is Apache Kafka?

Apache Kafka is an **open-source distributed event streaming platform**. It is designed to handle **high-throughput, fault-tolerant, scalable real-time data streams**.

Kafka is used by thousands of companies to **build real-time data pipelines, streaming applications, event-driven architectures**, and to process and analyze large volumes of data in real time.

Kafka was originally developed at **LinkedIn** and later open-sourced under the **Apache Software Foundation**.

---

## Key Concepts

### Topics
A **topic** is a category to which records (messages) are sent by producers and from which consumers retrieve records.

### Producers
**Producers** publish (send) data to topics.

### Consumers
**Consumers** subscribe to topics and process the data.

### Brokers
**Brokers** are Kafka servers that store and serve data.

### Partitions
Topics are split into **partitions**, which enable **parallelism** and scalability.

### Offsets
Each message within a partition has an **offset** — a unique identifier that allows consumers to track their progress.

---

## Why Use Kafka?

✅ **High Throughput**  
✅ **Scalable** (horizontal scalability across many servers)  
✅ **Fault-Tolerant** (handles server failures gracefully)  
✅ **Real-time Processing**  
✅ **Durable** (data is written to disk)  
✅ **Distributed Architecture**

---

## Common Use Cases

- Real-time analytics (clickstreams, IoT, sensor data)
- Log aggregation
- Data pipeline backbone
- Event sourcing
- Microservices communication via events
- Machine learning pipelines
- Data lake ingestion

---

## Basic Architecture

\`\`\`
+------------+         +---------+         +----------+
|  Producer  |  --->   |  Kafka  |  --->   | Consumer |
+------------+         |  Broker |         +----------+
                            |
                         +------+  
                         | Zookeeper | (Kafka < v3.x only)
                         +------+
\`\`\`

*Modern Kafka (v3.x and above) can run **without Zookeeper** using KRaft (Kafka Raft Metadata mode).*

---

## Getting Started

### Install Kafka (local machine)

1. Download Kafka from [Kafka Downloads](https://kafka.apache.org/downloads)
2. Extract the package.
3. Start Kafka:

\`\`\`bash
# Start Zookeeper (if using classic mode)
bin/zookeeper-server-start.sh config/zookeeper.properties

# Start Kafka broker
bin/kafka-server-start.sh config/server.properties
\`\`\`

---

### Create a Topic

\`\`\`bash
bin/kafka-topics.sh --create --topic test-topic --bootstrap-server localhost:9092 --partitions 3 --replication-factor 1
\`\`\`

### Send a message (Producer)

\`\`\`bash
bin/kafka-console-producer.sh --topic test-topic --bootstrap-server localhost:9092
\`\`\`

### Read messages (Consumer)

\`\`\`bash
bin/kafka-console-consumer.sh --topic test-topic --bootstrap-server localhost:9092 --from-beginning
\`\`\`

---

## Ecosystem

- **Kafka Connect** → for data integration with external systems  
- **Kafka Streams** → stream processing library  
- **ksqlDB** → SQL-like queries on Kafka topics  
- **Kafka REST Proxy** → REST interface for Kafka  
- **Confluent Platform** → Enterprise Kafka platform with extended tooling

---

## Key Features

- **At least once / Exactly once** delivery semantics
- **Distributed and highly available**
- **Built for throughput and low latency**
- **Replayable event log**
- **Backpressure handling**

---

## Advanced Concepts

- **Replication** → ensures high availability  
- **Compaction** → enables log compaction of topics  
- **Consumer Groups** → scales processing horizontally  
- **Retention policies** → define how long data stays in Kafka  

---

## Reference Material and Further Reading

### Official Documentation
- [Apache Kafka Documentation](https://kafka.apache.org/documentation/)  
- [Kafka Quickstart](https://kafka.apache.org/quickstart)  
- [Kafka Operations Guide](https://kafka.apache.org/documentation/#operations)

### Books
- [Kafka: The Definitive Guide (O’Reilly)](https://www.oreilly.com/library/view/kafka-the-definitive/9781491936153/)  

### Tutorials and Articles
- [Confluent Kafka Tutorials](https://developer.confluent.io/learn-kafka/)  
- [Kafka Streams Documentation](https://kafka.apache.org/documentation/streams/)  
- [Kafka Connect Documentation](https://kafka.apache.org/documentation/#connect)

### Online Courses
- [Udemy - Apache Kafka Series](https://www.udemy.com/course/apache-kafka/)  

---

## Community and Support

- [Kafka Users Mailing List](https://kafka.apache.org/contact)
- [Kafka GitHub Repository](https://github.com/apache/kafka)
- [StackOverflow - Apache Kafka Questions](https://stackoverflow.com/questions/tagged/apache-kafka)
- [Confluent Community Forum](https://forum.confluent.io/)

---

## Final Notes

Apache Kafka is a **powerful, battle-tested platform** for real-time event streaming.  
Learning Kafka is an excellent step if you're interested in **data engineering**, **streaming architectures**, or **real-time analytics**.

