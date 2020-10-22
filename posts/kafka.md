---
date: "2020-05-15"
title: Kafka (notes)
---

_Research into Kafka_  

**Kafka basics**  
- An instance of Kafka acts as a message broker.
- Kafka clients are producers and consumers.
- The producer sends messages to the broker, and the consumer reads messages from the broker.
- Producers send messages categorized by `topic`
- The broker stores those messages under that topic in `partitions` - partitions are how Kafka provides redundancy
    - Say the topic has 4 partitions. When messages are sent to the broker, the broker appends the messages to the end of the messages already in the partition(s)

**Kafka parts**  
<ins>Messages and Batches:</ins>

- A message is an array of bytes to Kafka, so its data does not have a specific meaning or format to Kafka
- Messages can have keys (optional bits of metadata); keys are used when messages need to be written to partitions in a more controlled manner
- Messages are written into Kafka in batches
    - a batch is a collection of messages, all of which are being produced to the same topic and partition

<ins>Schemas:</ins>

- It's recommended that additional structure be imposed on the message content so it can be easily understood. JSON and XML are more simplistic systems, but many devs favor Apache Avro (serialization framework)
- Consistent data format is important for true decoupling of reading and writing messages

<ins>Topics and Partitions:</ins>

- Messages are categorized into topics
    - closest analogy for a topic is a directory or table
- Topics are broken down into partitions
- Messages are written to a partition in an append-only manner, are read in order from beginning to end
- If a topic has multiple partitions, there is no guarantee of message time-ordering across the topic; only within a single partition
- Partitions are how Kafka provides redundancy and scalability
    - each partition can be hosted on a different server, which means that a single topic can be scaled horizontally across multiple servers
- a stream is considered to be a single topic of data, regardless of the number of partitions

<ins>Producers and Consumers:</ins>

- Two basic types of Kafka clients (clients are users of the system)
- Producers create new messages. May also be referred to as publishers or writers.
- Consumers read messages. May also be referred to as subscribers or readers.
- How they work together:
    - By default, producer does not care what partition a specific message is written to and will balance messages over all partitions of a topic evenly.
    - In some cases, the producer will direct messages to specific partitions, assuring that all messages produced with a given key will get written to the same partition
    - Consumer subscribes to one or more topics and reads the messages in the order in which they were produced.
    - Consumer keeps track of which messages it has already consumed by keeping track of the offset of messages
        - the offset is another bit of metadata that Kafka adds to each message as it's produced. It's an integer value that continually increases.
        - By storing the offset of the last consumed message for each partition, a consumer can stop and restart without losing its place
    - Consumers work as part of a consumer group to consume a topic. The group assures that each partition is only consumed by one member.
        - The mapping of a consumer to a partition is often called ownership of the partition by the consumer
    - Consumers can thus horizontally scale to consume topics with a large number of messages. 
    - If a single consumer fails, remaining members of the group will rebalance the partitions to take over for the missing member

<ins>Brokers and Clusters:</ins>

- A single Kafka server is called a broker.
- The broker receives messages from producers, assigns offsets to them, and commits the messages to storage.
- The broker also services consumers, responding to fetch requests for partitions and responding with the messages that have been stored
- A single broker can easily handle thousands of partitions and millions of messages per second.
- Brokers are designed to operate as part of a cluster.
    - Within a cluser of brokers, one broker will also function as the cluster controller (elected automatically from other cluster members).
        - The controller is responsible for administrative operations, including assigning partitions to brokers and monitoring for broker failures.
    - A partition is owned by a singler broker in the cluster, and that broker is called the leader of the partition. 
        - A partition may be assigned to multiple brokers, which will result in the partition being replicated, and another broker can take over leadership if there is a broker failure. However, all consumers and producers operating on that partition must connect to the leader
    - Replication mechanisms are designed only to work within a single cluster, not between multiple clusters

<ins>Retention:</ins>

- Rentention is the durable storage of messages for some period of time. 
- Brokers are configured with default retention settings for topics, and individual topics can be configured with their own retention settings

**How to**  
<ins>Writing producers:</ins>

- Start producing messages Kafka by creating a `ProducerRecord`, which must include the topic we want to send the record to and a value.
    - Can also optionally specify a key and/or partition
- Once we send the `ProducerRecord`, the first thing the producer does is serialize the key and value objects so they can be sent over the network
- Next, the data is sent to a partitioner.
    - If we specified a partition in the `ProducerRecord`, the partitioner doesn't do anything except returns the partition we specified
    - If we didn't specify, the partitioner will choose a partition for us, then return that partition to us
    - Once a partition is selected, the producer knows which topic and partition the record will go to
    - Then the producer adds that record to the batch of records also going to that same topic and partition.
    - Batches are sent to Kafka
- Next, the broker receives the messages and sends back a response.
    - If the messages were succesfully written to Kafka, broker returns a `RecordMetadata` object with the topic, partition, and offset of the record within the partition.
    - If the broker failed to write the messages, it will return an error.
        - When the producer receives the error, it may retry sending the message a few more times before giving up and returning its own error



[See a sketch of how publisher/subscriber systems work](https://excalidraw.com/#json=5152003063808000,TkiXjCU4Ng3qRkOmOa0o7Q)

[Quick fun tutorial here](https://kafka.apache.org/quickstart)

[Resource: Kafka, The Definitive Guide](https://www.confluent.io/resources/kafka-the-definitive-guide/)

[Resource: Confluent podcast w/ Kelsey Hightower](https://confluent.buzzsprout.com/186154/3545173-kubernetes-meets-apache-kafka-ft-kelsey-hightower)
