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
root@9ce074455855:~/kafka_2.12-2.2.0# ping kafka-1
PING kafka-1 (172.27.0.5) 56(84) bytes of data.
64 bytes from apache-kafka-cli-tools_kafka-1_1.apache-kafka-cli-tools_default (172.27.0.5): icmp_seq=1 ttl=64 time=0.093 ms
64 bytes from apache-kafka-cli-tools_kafka-1_1.apache-kafka-cli-tools_default (172.27.0.5): icmp_seq=2 ttl=64 time=0.112 ms
```

## Cleanup

```
docker ps
docker-compose rm
```
