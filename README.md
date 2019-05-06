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

Create a new Topic 'x-topic' with 12 partitions and RF=3
```sh
# ./bin/kafka-topics.sh --bootstrap-server kafka-1:19092 --create --topic x-topic --partitions 12 --replication-factor 3
## Cleanup
```

docker ps
docker-compose rm
```
