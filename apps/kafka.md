# Apache Kafka

## Run Apache Kafka in a container

```sh
$ docker run -p 2181:2181 -p 9092:9092 --env ADVERTISED_HOST=localhost --env ADVERTISED_PORT=9092 --name kafka -d spotify/kafka
```

## Create a topic

```sh
$ ./kafka-topics.sh --create --topic my_topic --zookeeper localhost:2181 --replication-factor 1 --partitions 1
```

## Ensure the topic is created

```sh
$ ./kafka-topics.sh --list --zookeeper localhost:2181
```

_Produce some messages_
```
$ ./kafka-console-producer.sh --broker-list localhost:9092 --topic my_topic
```

_Consume the messages_
```
$ ./kafka-console-consumer.sh --zookeeper localhost:2181 --topic my_topic --from-beginning
```

## Empty a topic

```sh
$ kafka-topics.sh --zookeeper  localhost:2181 --alter --topic MyTopic --config retention.ms=1000
```
wait a bit and reset it back to the appropriate value
```
$ kafka-topics.sh --zookeeper  localhost:2181 --alter --topic MyTopic --config retention.ms=3600000
```

## Delete a topic

```sh
$ kafka-topics.sh --zookeeper localhost:2181--delete --topic myTopic
```

---

- Messaging: transfering data
- RDBMS replication only between RDBMS and is vendor specific, while schema specific
- ETL is complex and not scallable

- The master node, named _Controller_, decides who will own the task. The owner of the task is called the _Leader_. The leader then selects workers, called _Peers_ to handle the task, taking into account the replication factor. The leader and the peers form a _Quorum_ and the peers are now called _Followers_.
- Zookeeper serves as a centralized service for maintaining cluster configuration, status and membership.
- Kafka topic is the main Kafka abstraction. It is a logical entity that spans across all cluster. Publishing to a topic appends an immutable fact onto an ordered sequence.
- A Kafka message has a timestamp (when the message was received by the broker), an identifier and a payload.
- The _offset_ is the position of the last read message. It is maintained by the consumer and persisted in a topic in the cluster.
- Messages in Kafka are retained regardless if they were consumed by one or several consumers. This is done according to the retention policy. By default retention is 7 days. It is set on a per topic basis.
- Once zookeeper has started, one can connect to port 2181 using telnet and send commands to it (e.g. stat)
- To create a topic the zookeeper instance, the replication factor and the number of partitioned needs to be provied
- Kafka is a disributed commit log
- Each topic is consisted by one or more partitions. Each partition is maintained in one or more brokers. Each partition **must fit entirely on one machine**. When there are multiple partitions, each partition gets an offset. A consumer that consumes a multipartitioned topic must pay attention on the order of the messages.
- ISR is in sync replicas. This should be equal to the quorum number that should be equal to the replication factor.

### Kafka producer

- Configuration properties (e.g. bootstrap servers, serializers)
- Producer sends messages, that is instances of the ProducerRecord class. These objects need a topic and a value, and optionally a partition, a timestamp and a key. It is a good practice to define a key (processing information, partition route), nevertheless, it creates an overhead.
- Sending a message concerns:
  - Fetching metadata
  - Serializing value
  - Run partitioner
  - Dispatch messages to RecordAccumulator (microbatching)
  - Send the messages to the network and receive a record metadata receipt

- _batch.size_ is not based on the number of records, but on the number of bytes in the batch. _buffer.memory_ is the size of all the buffers together. When the limit is reached, _max.block.ms_ comes into play. _Linger_ is the time unfull buffers are kept before send over the network.
- Broker acknowledgement (acks) can be:
  - 0: fire and forget
  - 1: leader acknowledgement
  - 2: quorum acknowledgement
- When an error is received the producer has the option to _retry_. This can happen for a certain number of times, with configurable backoff (retry wait period).

### Kafka consumer

- Configuration properties (e.g. bootstrap servers, serializers)
- Subscribe to a topic or more (can use a pattern to match topics). Subscribing to additional topics overrides the previous ones. Subscribe to specific partitions is also possible via assign(), regardless of the topic.
- The _SubscriptionState_ object maintains all state related to partitions and topics this subscriber has subscribed on.
- The _ConsumerCoordinator_ manages the offsets.
- The _Fetcher_ is responsible for the communication between the consumer and the cluster. Nevertheless, the actual communication with the cluster is done by the _Consumer Network Client_.
- The _poll loop_ kicks off a consumer. Its parameters is its polling cycle in ms. When this expires, a batch of messages is returned and are processed. This is a single-threaded operation.
- The consumer sends a hearbeat to the coordinator at predefined intervals.
- The _last committed offset_ is the last record the consumer has confirmed to have processed.
- Committed offsets are stored in a topic on the cluster.
- When a consumer group is deployed, a partition is assigned to a consumer. If the consumers are more than the partitions it is an overprovision scenario. If there are less consumers than partitions, some consumers might be assigned more than one partitions.

## Extending Kafka

- Apache Avro: Extensible serialization format and framework
- Kafka Connect: Connectors (produces, consumers) for various datasources
- Kafka Streams: Low latency messaging
