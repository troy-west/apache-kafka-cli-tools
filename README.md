# Apache Kafka Three Ways: CLI Tools

## Initialize

Start a 3-node Kafka Cluster and enter a shell with all kafka-tools scripts:
```sh
docker-compose up -d
docker-compose -f docker-compose.tools.yml run kafka-tools
```

## Monitor

In a new terminal, view the running kafka logs:
```sh
docker-compose logs -f
```

## Play 

From within the tools shell.

Confirm the kafka nodes are visible:

```sh
# ping kafka-1

PING kafka-1 (172.27.0.5) 56(84) bytes of data.
64 bytes from apache-kafka-cli-tools_kafka-1_1.apache-kafka-cli-tools_default (172.27.0.5): icmp_seq=1 ttl=64 time=0.093 ms
64 bytes from apache-kafka-cli-tools_kafka-1_1.apache-kafka-cli-tools_default (172.27.0.5): icmp_seq=2 ttl=64 time=0.112 ms
```

Create a new Topic 'x-topic' with 12 partitions and RF=3:

```sh
# ./bin/kafka-topics.sh --bootstrap-server kafka-1:19092 --create --topic x-topic --partitions 12 --replication-factor 3
```

Confirm the new topic has been created:

```sh
# ./bin/kafka-topics.sh --bootstrap-server kafka-1:19092 --list

x-topic
```

Describe the new topic:

```sh
# ./bin/kafka-topics.sh --bootstrap-server kafka-1:19092 --describe --topic x-topic

Topic:x-topic	PartitionCount:12	ReplicationFactor:3	Configs:
	Topic: x-topic	Partition: 0	Leader: 1	Replicas: 1,2,3	Isr: 1,2,3
	Topic: x-topic	Partition: 1	Leader: 2	Replicas: 2,3,1	Isr: 2,3,1
	Topic: x-topic	Partition: 2	Leader: 3	Replicas: 3,1,2	Isr: 3,1,2
	Topic: x-topic	Partition: 3	Leader: 1	Replicas: 1,3,2	Isr: 1,3,2
	Topic: x-topic	Partition: 4	Leader: 2	Replicas: 2,1,3	Isr: 2,1,3
	Topic: x-topic	Partition: 5	Leader: 3	Replicas: 3,2,1	Isr: 3,2,1
	Topic: x-topic	Partition: 6	Leader: 1	Replicas: 1,2,3	Isr: 1,2,3
	Topic: x-topic	Partition: 7	Leader: 2	Replicas: 2,3,1	Isr: 2,3,1
	Topic: x-topic	Partition: 8	Leader: 3	Replicas: 3,1,2	Isr: 3,1,2
	Topic: x-topic	Partition: 9	Leader: 1	Replicas: 1,3,2	Isr: 1,3,2
	Topic: x-topic	Partition: 10	Leader: 2	Replicas: 2,1,3	Isr: 2,1,3
	Topic: x-topic	Partition: 11	Leader: 3	Replicas: 3,2,1	Isr: 3,2,1
```

Show the offsets of each partition in the new topic:

```sh
# ./bin/kafka-run-class.sh kafka.tools.GetOffsetShell --broker-list kafka-1:19092 --topic x-topic --time -1

x-topic:0:0
x-topic:1:0
x-topic:2:0
x-topic:3:0
x-topic:4:0
x-topic:5:0
x-topic:6:0
x-topic:7:0
x-topic:8:0
x-topic:9:0
x-topic:10:0
x-topic:11:0
```

Send messages with the same key to the new topic: (ctrl-c to exit the producer when done)

```sh
# ./bin/kafka-console-producer.sh --broker-list kafka-1:19092 --topic x-topic --property "parse.key=true" --property "key.separator=:"

a-key:some-value
a-key:another-value
a-key:third-value
```

Inspect the offsets again, showing that each message has been sent to the same partition

```sh
# ./bin/kafka-run-class.sh kafka.tools.GetOffsetShell --broker-list kafka-1:19092 --topic x-topic --time -1

x-topic:0:0
x-topic:1:0
x-topic:2:0
x-topic:3:0
x-topic:4:0
x-topic:5:0
x-topic:6:0
x-topic:7:0
x-topic:8:0
x-topic:9:0
x-topic:10:0
x-topic:11:3   <-- all three messages on partition 11
```

Increase the number of partitions to 15

```sh
# ./bin/kafka-topics.sh --bootstrap-server kafka-1:19092 --topic x-topic --alter --partitions 15
```

And confirm the updated partition count

```sh
# ./bin/kafka-topics.sh --bootstrap-server kafka-1:19092 --describe --topic x-topic
Topic:x-topic	PartitionCount:15	ReplicationFactor:3	Configs:
	Topic: x-topic	Partition: 0	Leader: 1	Replicas: 1,2,3	Isr: 1,2,3
	Topic: x-topic	Partition: 1	Leader: 2	Replicas: 2,3,1	Isr: 2,3,1
	Topic: x-topic	Partition: 2	Leader: 3	Replicas: 3,1,2	Isr: 3,1,2
	Topic: x-topic	Partition: 3	Leader: 1	Replicas: 1,3,2	Isr: 1,3,2
	Topic: x-topic	Partition: 4	Leader: 2	Replicas: 2,1,3	Isr: 2,1,3
	Topic: x-topic	Partition: 5	Leader: 3	Replicas: 3,2,1	Isr: 3,2,1
	Topic: x-topic	Partition: 6	Leader: 1	Replicas: 1,2,3	Isr: 1,2,3
	Topic: x-topic	Partition: 7	Leader: 2	Replicas: 2,3,1	Isr: 2,3,1
	Topic: x-topic	Partition: 8	Leader: 3	Replicas: 3,1,2	Isr: 3,1,2
	Topic: x-topic	Partition: 9	Leader: 1	Replicas: 1,3,2	Isr: 1,3,2
	Topic: x-topic	Partition: 10	Leader: 2	Replicas: 2,1,3	Isr: 2,1,3
	Topic: x-topic	Partition: 11	Leader: 3	Replicas: 3,2,1	Isr: 3,2,1
	Topic: x-topic	Partition: 12	Leader: 1	Replicas: 1,3,2	Isr: 1,3,2
	Topic: x-topic	Partition: 13	Leader: 2	Replicas: 2,1,3	Isr: 2,1,3
	Topic: x-topic	Partition: 14	Leader: 3	Replicas: 3,2,1	Isr: 3,2,1
```

Send another three messages with the same key as previous:

```sh
# ./bin/kafka-console-producer.sh --broker-list kafka-1:19092 --topic x-topic --property "parse.key=true" --property "key.separator=:"
>a-key:post-repartition-value
>a-key:another-post-value
>a-key:one-more-value
```

And inspect the offsets again, expecting to see the same key written to a new partition

```
# ./bin/kafka-run-class.sh kafka.tools.GetOffsetShell --broker-list kafka-1:19092 --topic x-topic --time -1
x-topic:0:0
x-topic:1:0
x-topic:2:0
x-topic:3:0
x-topic:4:0
x-topic:5:3 <---- a-key messages after partition count updated
x-topic:6:0
x-topic:7:0
x-topic:8:0
x-topic:9:0
x-topic:10:0
x-topic:11:3 <---- original a-key messages
x-topic:12:0
x-topic:13:0
x-topic:14:0
```

Consume those messages, noting:

 * individual partitions may be returned out of order (as below)
 * within a partition messages are ordered
 
```sh
# ./bin/kafka-console-consumer.sh --bootstrap-server kafka-1:19092 --topic x-topic --from-beginning --group x-consumer
post-repartition-value
another-post-value
one-more-value
some-value
another-value
third-value
```

## Cleanup
```
docker ps
docker-compose rm
```
