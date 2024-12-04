# Apache Kafka Fundamentals

## Introduction

Apache Kafka is a distributed event streaming platform capable of handling trillions of events a day. 
It provides high-throughput, low-latency, and fault-tolerant capabilities for real-time data pipelines and stream 
processing applications.

## Motivation & Customer Use Cases

### Motivation
- Single Platform to connect everyone to every event
- Real-time stream of events
- All events stored in a historical view
- Apache Kafka: the official standard for real-time event streaming
  - Global-scale
  - Real-time
  - Persistent Storage
  - Stream Processing
- The event streaming paradigm enables enterprises to transform into data-driven businesses.
- Over 70% of Fortune 500's Already Trust Kafka for Mission Critical Apps
- Real-time fraud detection:
  - Act in real-time
  - Detect fraud
  - Minimise risk
  - Improve customer experience
  
### Customer Use Cases

- How Kafka helped Audi:
  - Worldwide tracking and data gathering of Audi cars to improve user experience
  - Providing sales, engineering and post-sales departments with the data required to provide additional or better services to customers
  - Increased availability and scalability 
- Real-time e-Commerce:
  - Rewards Program:
    - Onboarding new merchants faster
    - Increased speed at which mobile applications are delivered to customers
    - Enabled a full 360 view of customers
    - Enhanced performance and monitoring
    - Projected savings of millions of dollars by reducing reliance on IBM MQ, Informatica and their preexisting mainframe.
- Customer 360:
  - Improved data integration
  - Increased up-sell and cross-sell opportunities
  - Increased scalability and flexibility
  - Saved costs by reducing the reliance on costly IBM Unica and IBM MQ solutions
- Core Banking:
  - Empowered successful core banking platform relaunch
  - Met high availability and SLA needs
  - Improved scalability, as the bank anticipates that transactions will triple or quadruple in the incoming years
  - Power AI for Chat Bots
- Healthcare:
  - Improved ability to more accurately customise medications and dosages of each unique patient
  - Achieved stability and agility via microservices event-driven architecture
- Online Gaming:
  - Increased reliability
  - Accurate, real-time data
  - Ability to process data at scale
  - Faster ramp time with access to Confluent team
- Government:
  - Near real-time events and better data quality 
  - Increased efficiency 
  - Ability to change their organisation
  - Produce and store population data from several sources
  - Reduce welfare crime through strengthened identity management
  - Provide better privacy and meet GDPR requirements
- Financial Services:
  - Customer communications:
    - Enhanced the customer experience
    - Enabled the "One Bank" strategy 
  - Payments engine:
    - Improved fraud detection, saving millions of euros
    - Grew topics in production by 600 percent


### Why is it relevant?

Kafka enables organisations to move away from batch processing to real-time event streaming, unlocking the ability to 
make immediate decisions based on live data. Its scalability, durability, and flexibility make it a critical 
infrastructure component for modern data-driven businesses.

### Some interesting using cases customers solve with Kafka

- Real-time fraud detection for financial services.
- Powering customer-centric e-commerce platforms with context-aware recommendations.
- Monitoring and alerting for IT operations in real-time.
- Enabling smart manufacturing with IoT sensor integration.

## Apache Kafka Fundamentals

Motivation for stream processing and high-level overview of the following:
The World produces data, lots of it. We want to collect and store all that data in Kafka.
- connected cars whose many detectors emit streams of readings
- hospitals producing a wealth of health related data
- insurances collecting a wealth of customers and environmental data
- banks and other financial institutions producing streams of transactions
- logistics tracking all goods shipped worldwide
- manufacturing automating their whole assembly lines

### Broker

- Kafka consists of a bunch of **brokers**. More formally Kafka is a **cluster of brokers**
- Brokers receive the data sent from producers and store it temporarily in the page cache, or permanently on disk after the OS flushes the page
- Brokers keep the data ready for downstream customers
- How long the data is kept around is determined by **retention time** (1 week default)

### Producer

The applications that forward all this data to Kafka are called **producers**.
- The producer (optionally) receives an **ACK** or **NACK** from Kafka
- If an **ACK** (acknowledge) is received then all is good
- If a **NACK** (not acknowledge) is received then the producer knows that Kafka was not able to accept the data for whatever reason. In this case the producer automatically retries to send the data.
- Many producers can send data to Kafka concurrently

### Consumers, Consumer offsets

Simply storing data in Kafka makes no sense. The real value is when the data is used by downstream consumers to create business value
- A consumer **polls** data from Kafka
- Each consumer **periodically** asks brokers of the Kafka cluster: "Do you have more data for me?". Normally the consumer does this on an endless loop
- Many consumers can poll data from Kafka at the same time
- Many different consumers can poll the same data, each **at their own pace**
- To allow for **parallelism**, consumers are organised in consumer groups that **split up the work**

### Decoupling Producers and Consumers

- A key feature of Kafka is that Producers and Consumers are **decoupled**
  - they do not need to know about each other's existence
  - the internal logic of the Producers does not depend on any of the downstream consumers
  - the internals of the Consumer do not depend on the upstream Producer
- Slow Consumers do not affect Producers
- Add Consumers without affecting Producers
- Failure of Consumer does not affect System
- Producers and Consumers simply need to agree on the data format of the records produced and consumed

### Topics, Partitions and Replication

#### Topics
- **Topics**: Streams of "related" Messages in Kafka
  - Is a **Logical Representation**
  - **Categorises Messages** into Groups
- Developers define Topics
- Producer <--> Topic: N to N Relation
- Unlimited Number of Topics
- The Topic is a logical representation that spans across Producers, Brokers, and Consumers
  - Developers decide which Topic exist
    - By default, a Topic is auto-created when it first used
  - One or more Producers can write to one or more Topics
  - There is no limit to the number of Topics that can be used
  
**Topic**: A topic comprises all messages of a given category. We could e.g. have the topic "temperature_readings" which would contain all messages that contain a temperature reading from one of many measurement stations the company has around the globe

**Partition**: To parallelise work thus increase the throughput Kafka can split a single topic into many partitions. The messages of the topic will be split between the partitions. The default algorithm used to decide to which partition a message goes uses the hash code of the message key. A partition is handled in its entirety by a single Kafka broker. A partition can be viewed as a "log"

**Segment**: The broker stores the messages as they come in memory (page cache), then periodically flushes them to a physical file. Since the data can potentially be endless the broker is using a "rolling-file" strategy. It creates/allocates a new file and fills it with messages. When the segment is full (or given max. time per segment expires), the next one is allocated by the broker. Kafka, rolls over files in segments to make it easier to manage **data retention** and **remove old data**

**Log**: A data structure that is like a queue of elements. New elements are always appended at the end of the log and once written they are never changed. In this regard on talks of an append only, write once data structure. Elements that are added to the log are strictly ordered in time. The first element added to the log is older than the second one which in turn is older than the third one. The offset of the elements in that sense can be viewed as a timescale.

**Stream**: A stream is a sequence of events. The sequence has abeginning somewhere inthe past. The first element has an offset "0". "Past" in this context can mean anything from seconds to hours to weeks or even longer periods. Events with offset "n" to be "now". Everything that comes after "n" is considered future. A stream is open-ended we cannot know how much data is expected in the future, and we certainly cannot wait until all data has arrived before we start doing something with the stream. A stream is "immutable", one can never modify an existing stream but always generates a new output stream. 


Normally one uses several Kafka Brokers that form a cluster to scale out. If we have now 3 topics (that is 3 categories of messages) then they maybe organised physically.
- Partitions of each Topic are distributed among the brokers
- Each partition on a given broker results in one to many physical files called segments
- Segments are treated in a rolling file strategy. Kafka opens/allocates a file on disk and then fills that file sequentially and in append only mode until it is full, where "full" depends on the defined max size of the file, (or defined max time per segment is expired)

### The (commit) log and streams

Kafka uses a commit log as its core data structure. Messages are appended sequentially to the log, and consumers 
independently track their position (offset) in the log. Streams represent continuous data flows that can be processed in
real-time to derive insights and transform data.

### High level architecture

Kafka operates as a cluster of brokers, organised to provide scalability, high availability, and durability. Producers 
send messages to topics, which are divided into partitions. Consumers read messages from these partitions, which are 
replicated for fault tolerance. Kafka's architecture decouples data producers and consumers, enabling independent 
scaling.

### ZooKeeper

A cluster of ZooKeeper instances form an **ensemble**
Kafka Brokers use ZooKeeper for a number of important internal features such as:
- Cluster management
- Failure detection & recovery (e.g. when a broker does down)
- Store Access Control Lists (ACLs) used for authorisation in the Kafka cluster & secrets

ZooKeeper Basics
- Open Source Apache project
- Distributed **Key Value Store**
- Maintains **configuration information**
- Store **ACLs** and **Secrets**
- Enables highly reliable **distributed coordination**
- Provides **distributed synchronisation**
- Run from 3 or 5 servers (called an **ensemble**) for **Resiliency** 
- The number needs to be odd due to the **ZAB** consensus algorithm used to achieve a quorum
- It is not recommended to run more than 5 instances in an ensemble for performance reasons (ZK instances need to communicate synchronously with each other to achieve a quorum)

## How Kafka works

### Introduction to development

### Partition Leadership & Replication

Each partition in Kafka has a leader broker, which handles all read and write requests for that partition.
Other brokers maintain replicas of the partition and act as followers.
Kafka uses replication to ensure high availability and fault tolerance. In the event of a broker failure, a follower is 
elected as the new leader.

### Data Retention Policy

Kafka retains messages based on time-based policies (e.g., 7 days by default) or size-based policies (e.g., retain until
a topic reaches a configured size). Retention policies ensure scalability by deleting older or less relevant data.

### Producer Design & Guarantees

Kafka producers are designed to handle retries, acknowledgments, and fault tolerance. Producers can choose between 
at-most-once, at-least-once, or exactly-once delivery guarantees, depending on the application's requirements.

### Delivery Guarantees, Idempotent Producers, EOS

- At-least-once: Ensures every message is delivered but may result in duplicates.
- At-most-once: Ensures no duplicates but may lose messages.
- Exactly-once (EOS): Guarantees each message is processed exactly once using idempotent producers and transactional 
semantics.

### Partitions Strategies

Kafka partitions data based on a key provided by the producer or through round-robin partitioning if no key is 
specified. This partitioning ensures efficient load distribution and parallelism in data processing.

### Consumer Groups and Consumer Rebalances

- Consumer groups allow for parallel consumption by dividing partition ownership among consumers in the group.
- Rebalancing occurs when consumers join or leave the group, or when partitions are added to the topic. 
This redistributes partition ownership dynamically.

### Topic Compaction

Topic compaction ensures that only the most recent value for a given key is retained. This is useful for scenarios like 
maintaining the latest state or configuration, where old data can be discarded.

### Troubleshooting

Common Kafka troubleshooting steps include:
- Checking consumer lag to ensure consumers are keeping up. 
- Reviewing broker logs for errors or performance bottlenecks. 
- Ensuring ZooKeeper is operating correctly for metadata management.
- Monitoring cluster metrics like disk usage, replication lag, and partition leadership.

### Security Overview

Kafka provides robust security features:
- Authentication via SSL and SASL mechanisms.
- Authorisation using Access Control Lists (ACLs).
- Data encryption both in transit (TLS) and at rest (via external tools).


## Integrating Kafka into your Environment

Kafka integrates seamlessly into modern data architectures, providing flexibility for real-time event streaming and 
processing. Various tools and frameworks enhance its capabilities for connecting to external systems, managing schemas, and building real-time applications.

### REST-proxy

The Kafka REST Proxy enables interaction with Kafka through a RESTful API, making it easier for applications that do 
not use Kafka clients directly.
Key features:
- Publish and consume messages via HTTP.
- Manage topics, partitions, and consumer groups.
- Simplify integration with non-Java applications or systems.

### Schema-Registry

The Confluent Schema Registry manages and enforces schemas for Kafka messages, ensuring data consistency across producers and consumers.
Key features:
- Supports Avro, Protobuf, and JSON schemas.
- Provides schema validation and versioning.
- Ensures compatibility during schema evolution.
- Reduces message overhead by storing schemas centrally and including only schema IDs in messages.

### Kafka Connect

Kafka Connect simplifies data integration by providing connectors to ingest and export data from external systems.
Key features:
- Source connectors import data into Kafka (e.g., from databases, file systems).
- Sink connectors export data from Kafka (e.g., to data warehouses, object storage).
- Supports distributed and standalone modes.
- Built-in fault tolerance and scalability.

### Kafka Streams

Kafka Streams is a Java-based library for building real-time stream processing applications.
Key features:
- Processes data directly from Kafka topics.
- Supports stateless and stateful operations like filtering, aggregations, and joins.
- Provides exactly-once processing semantics for reliable transformations.
- Fully integrated with Kafka security features.
- Example use cases include microservices, continuous transformations, and analytics pipelines.

#### Apache Kafka Stream
Transform data with real-time applications
- Write standard Java applications
- No separate processing cluster required
- Exactly-once processing semantics
- Elastic, highly scalable, fault-tolerant
- Fully integrated with Kafka security
- Example use cases:
  - Microservices
  - Continuous queries
  - Continuous transformations
### KSQL
KSQL (now ksqlDB) is a SQL-based platform for stream processing in Kafka. It allows users to write real-time queries and 
transformations on Kafka topics without requiring complex coding.
Key features:
- Simplifies stream processing with SQL-like syntax.
- Supports joins, aggregations, and filtering in real-time.
- Enables building real-time applications like analytics dashboards, fraud detection, and monitoring systems.
- Fully integrated with the Confluent Platform and Kafka Streams.

## The Confluent Platform

The Confluent Platform extends Apache Kafka by adding enterprise-grade tools and features, making it easier to build and
manage event-driven architectures. It includes tools for monitoring, security, schema management, and data integration, 
providing a comprehensive solution for modern data pipelines.

### Central Nervous System

Confluent positions Kafka as the central nervous system of an organisation’s data architecture. It acts as the backbone 
for connecting various data sources and sinks, enabling real-time communication and decision-making across the 
enterprise.

### The maturity Model

The Confluent Maturity Model describes the evolution of event streaming adoption:
1. Initial Adoption: Basic use cases like log aggregation or data pipeline integration.
2. Scaling Up: Expanding Kafka usage to additional teams and use cases.
3. Enterprise Integration: Kafka becomes central to the organisation’s data strategy.
4. Real-Time Decision-Making: Kafka powers real-time analytics, AI/ML pipelines, and event-driven microservices.

### Confluent Platform

The Confluent Platform is a fully-featured distribution of Apache Kafka with added tools for enterprise needs:

- Schema Registry: Ensures consistent data formats.
- Connectors: Simplifies integration with external systems.
- Confluent Control Center: Provides a GUI for monitoring and managing Kafka clusters.
- RBAC: Implements secure role-based access control.
- Cluster Linking: Enables multi-data-center setups with active-active replication.

### Confluent Cloud

Confluent Cloud is a managed Kafka service that eliminates the operational complexity of running Kafka clusters.
Key features:
- Fully managed by Confluent with 99.99% SLA.
- Scalability and flexibility to handle enterprise-grade workloads.
- Integrates with cloud-native tools and ecosystems like AWS, Azure, and GCP.

### Confluent Control Center

The Confluent Control Center is a web-based GUI for managing and monitoring Kafka clusters.
Key features:
- Visualize consumer lag, broker health, and topic performance.
- Configure alerts for potential issues.
- Easily manage schemas, connectors, and security settings.

### Confluent CLI

The Confluent CLI is a command-line tool for managing Kafka and the Confluent Platform.
Key features:
- Automates cluster setup and monitoring tasks.
- Supports topic creation, connector configuration, and security settings.
- Integrates with Confluent Cloud for hybrid setups.

### RBAC
Role Based Access Control
- Utilises a predefined set of roles
- User who is assigned a role receives all privileges of that role
- Each user could belong to multiple roles
- Privileged user within a given scope can create/update other user's roles
- Privileged user can update roles for existing users
- Across the platform (KSQL, Connect, Schema Registry, Connectors, etc.) 

### Confluent Operator

#### Deploy
- Single-Command deployment of Confluent Platform on Kubernetes
- Storage uses persistent Kubernetes Volumes (Storage Class based)
- Includes automated authentication and security configuration
  - SASL Plain, TLS certificate based 2-way authentication
  - TLS in transit encryption for all deployed components

#### Update/Upgrade
- Automated rolling upgrades (no impact to availability)
- Automated rolling updates to make config changes

#### Scale Up and Down
- Elastic scaling of new Kafka/ CP components (run data balancer manually)

#### Monitor
- JMX Kafka metrics aggregation - exportable to Prometheus 

## Conclusion

## Kafka Labs

### Lab 01 Exploring Apache Kafka

MQTT stands for Message Queuing Telemetry Transport. It is a lightweight publish and subscribe system where you can publish and receive messages as a client. MQTT is a simple messaging protocol, designed for constrained devices with low-bandwidth. So, it’s the perfect solution for Internet of Things (IoT) applications.
In this exercise we created a simple real-time data pipeline powered by Kafka. We have written data that originates from a public IoT data source into a simple Kafka cluster. This data we then have consumed with a simple Kafka tool called kafka-console-consumer.

### Lab 02 Fundamentals of Apache Kafka

Creating a Kafka Topic:
```
kafka-topics --bootstrap-server kafka:9092 \
    --create \
    --topic vehicle-positions \
    --partitions 6 \
    --replication-factor 1
```

Listing Kafka Topics:
```
kafka-topics --bootstrap-server kafka:9092 --list
```

Deleting a Kafka Topic:
```
kafka-topics --bootstrap-server kafka:9092 \
    --delete \
    --topic test-topic
```

Adding data to a Kafka topic:

```
kafka-console-producer --broker-list kafka:9092 \
    --topic sample-topic
```

```
>hello
>world
>Kafka
>is
>cool!
```

`CTRL-d` to exit.

Consuming the data from the beginning:

```
kafka-console-consumer --bootstrap-server kafka:9092 \
    --topic sample-topic \
    --from-beginning
```

Adding data with keys:
``` 
kafka-console-producer --broker-list kafka:9092 \
    --topic sample-topic \
    --property parse.key=true \
    --property key.separator=,
```
```
>1,apples
>2,pears
>3,walnuts
>4,peanuts
>5,oranges
```

`CTRL-d` to exit.

Using the console consumer tool to configure it such as that it outputs keys and values:
```
kafka-console-consumer --bootstrap-server kafka:9092 \
    --topic sample-topic \
    --from-beginning \
    --property print.key=true
```
Output:

```
null	hello
null	Kafka
1       apples
5       oranges
null	is
4       peanuts
null	world
null	cool!
2       pears
3       walnuts
```

`CTRL-c` to exit the consumer.

#### Using ZooKeeper Shell
```shell
$ zookeeper-shell zookeeper

Connecting to zookeeper
Welcome to ZooKeeper!
JLine support is disabled

WATCHER::

WatchedEvent state:SyncConnected type:None path:null

> ls /

[admin, brokers, cluster, config, consumers, controller, controller_epoch, feature,
isr_change_notification, latest_producer_id_block, leadership_priority, 
log_dir_event_notification, zookeeper]

> ls /brokers

[ids, seqid, topics]

> ls /brokers/ids

[101]

> get /cluster/id

{"version":"1","id":"Rslk7ZJnRsGFfeHwfwhzmw"}
```

#### Conclusion

In this exercise we created a simple Kafka cluster. We then used the tool kafka-topics to list, create, describe and delete topics. Next we used the tools kafka-console-producer and kafka-console-consumer to produce data to and consume data from Kafka. Finally, we used the ZooKeeper shell to analyse some of the data stored by Kafka in ZooKeeper.

### Lab 03 How Kafka Works

To prepare ourselves to scale the consumer up and down we want to run each consumer instance as a Docker container.
Consumer lag is an indication of whether or not the consumer group is falling behind.

In Kafka, if you scale the consumer group to more than 6 instances while consuming from a topic with 6 partitions, 
the additional instances will remain idle because Kafka's partition assignment mechanism ensures that each partition is 
consumed by only one consumer within the same consumer group.

#### Conclusion

Scaling the consumer group beyond the number of partitions leads to diminishing returns, as excess consumers remain 
idle. If you want to improve throughput or reduce lag, focus on increasing the number of partitions or optimising the 
consumer application.

### Lab 04 Integrating Kafka into your Environment

MQTT (Message Queuing Telemetry Transport) is the de facto data exchange protocol for IoT messaging.