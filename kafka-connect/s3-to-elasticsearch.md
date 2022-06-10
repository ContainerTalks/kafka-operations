# Connector S3 Source

## Pre-requisites
- Plugins : [kafka-connect-s3-source](https://docs.confluent.io/kafka-connect-s3-source/current/index.html), [kafka-connect-elasticsearch](https://docs.confluent.io/kafka-connect-elasticsearch/current/overview.html), installed while running the container. check with the `docker-compose` file `command`.

- **Source** : Create the S3 Bucket, push the `.json` files to bucket, here is the bucket general bucket path looks like `test-kafka-connect-bucket-balu-test/jsonTest/test_topic/*.json`, we will use this folder paths in `kafka-connect-s3-source` connector while creation. 

```json
{
    .....
    "s3.bucket.name": "test-kafka-connect-bucket-balu-test",
    "topics.dir": "jsonTest",
    "topic.regex.list": "test_topic:.*"
    .....
}
```
- **Destination** : Elasticsearch, it is running as a container configured with docker compose.

Verify the connector plugins [Connector Plugins API](https://github.com/JinnaBalu/docker-kafka/blob/main/kafka-connect/kafka-connector-plugin-api.md)

## Stream Data from S3 to Kafka

- Create the connector s3 to kafka

```bash
curl -i -X PUT -H  "Content-Type:application/json" http://localhost:8083/connectors/s3-json-to-kafka/config -d '{
        "name": "s3-json-to-kafka",
        "connector.class": "io.confluent.connect.s3.source.S3SourceConnector",
        "tasks.max": "1",
        "value.converter": "org.apache.kafka.connect.json.JsonConverter",
        "mode": "GENERIC",
        "topics.dir": "jsonTest",
        "confluent.topic.bootstrap.servers": "broker:29092",
        "confluent.topic.replication.factor": "1",
        "format.class": "io.confluent.connect.s3.format.json.JsonFormat",
        "topic.regex.list": "test_topic:.*",
        "s3.bucket.name": "test-kafka-connect-bucket-balu-test",
        "s3.region": "ap-south-1",
        "s3.credentials.provider.class": "com.amazonaws.auth.EnvironmentVariableCredentialsProvider",
        "value.converter.schemas.enable": "false"
    }'
```

Check the status of the connectors using [Connectos API](https://github.com/JinnaBalu/docker-kafka/blob/main/kafka-connect/kafka-connector-api.md)


### Verify the topics

- List the topics

```bash
docker exec -it broker kafka-topics --list --bootstrap-server broker:90
```

- Get the consumer messages
```
docker exec -it broker-1  kafka-console-consumer --bootstrap-server 192.31.2.108:19092 --topic test_topic
```


## Stream Data from Kafka to Elasticsearch

- Create the connector to sync data between kafka to elasticsearch 

```bash
curl -i -X PUT -H  "Content-Type:application/json" \
    http://localhost:8083/connectors/elasticsearch-sink/config \
    -d '{
        "name": "elasticsearch-sink",
        "connector.class": "io.confluent.connect.elasticsearch.ElasticsearchSinkConnector",
        "tasks.max": "1",
        "topics" : "test_topic", 
        "topic.index.map" : "test_topic:test_topic_index",
        "connection.url": "http://elasticsearch:9200",
        "type.name": "log",
        "key.ignore": "true",
        "schema.ignore" : "true"
  }'
```

## Elasticsearch 

- List the indices in elasticsearch 

```bash
curl -X GET "localhost:9200/_cat/indices?v" 
```

- Search messages in index

```bash
curl -XGET 'localhost:9200/test_topic_index/_search?pretty'
```
