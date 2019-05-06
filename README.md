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
ping kafka-1
ping kafka-2
ping kafka-3
```

## Cleanup

```
docker ps
docker-compose rm
```
