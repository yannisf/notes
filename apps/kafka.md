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

_Produce some messages_
```
$ ./kafka-console-producer.sh --broker-list localhost:9092 --topic my_topic
```

_Consume the messages_
```
$ ./kafka-console-consumer.sh --zookeeper localhost:2181 --topic my_topic --from-beginning
```

## Empty a Kafka topic

```sh
$ kafka-topics.sh --zookeeper localhost:13003 --alter --topic MyTopic --config retention.ms=1000
```
wait a bit and reset it back to the appropriate value
```
$ kafka-topics.sh --zookeeper localhost:13003 --alter --topic MyTopic --config retention.ms=3600000
```
