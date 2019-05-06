# Apache Kafka Three Ways: CLI Tools

## Initialize

Start a 3-node Kafka Cluster and enter a shell with all kafka-tools scripts:
```
docker-compose up -d
docker-compose -f docker-compose.tools.yml run kafka-tools
```

## Play

In a new terminal, view the running kafka logs:
```
docker-compose logs -f
```

## Cleanup

```
docker ps
docker-compose rm
```
