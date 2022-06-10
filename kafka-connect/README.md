# Kafka Connect

## Running Kafka with Connect 

```bash
cd kafka-connect/
docker-compose up -d
```

# Verify the Connector Plugins

- FIND THE SPECIFIC PLUGINS INSTALLED
```bash
curl -s localhost:8083/connector-plugins|jq '.[].class'| egrep 'ElasticsearchSinkConnector'
```

- FIND ALL THE PLUGINS INSTALLED
```bash
curl -s localhost:8083/connector-plugins|jq '.[].class'
```

## Stream Data from S3 to Kafka

```bash
curl -i -X PUT -H  "Content-Type:application/json" http://localhost:8083/connectors/test-s3-json-to-kafka/config -d '{
        "name": "test-s3-json-to-kafka",
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

### Check the status of the connectors

- List the container 

`curl -X GET "localhost:8083/connectors"`

- Delete the connector

`curl -X DELETE  "http://localhost:8083/connectors/<CONNECTOR_NAME>"`


curl 'localhost:8083/connector-plugins'


## Stream Data from Kafka to Elasticsearch

docker exec -it broker kafka-console-producer --broker-list broker:9092 --topic test-elasticsearch-sink-2 < sample.json

cat <<EOF>> sample.json
{"f1": "value1"}
{"f1": "value2"}
{"f1": "value3"}
EOF


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


 "type.name" : "event",
    "key.ignore" : "false",
    "schema.ignore" : "true",
    "schemas.enable" : "false",
    "auto.create.indices.at.start": "true",
    "key.ignore": "true",
    "name": "test-2",
    "transforms": "addSuffix",  
    "transforms.addSuffix.type": "org.apache.kafka.connect.transforms.RegexRouter",
    "transforms.addSuffix.regex": "test_topic.*",
    "transforms.addSuffix.replacement": "test_topic"
 
### Produce the Message to topic

docker exec -it broker kafka-console-producer --topic test_topic --broker-list broker:9092


curl -XGET 'localhost:9200/test_topic_index/_search?pretty'

   


## View the topic in the CLI:

docker exec -it broker kafka-topics --list --bootstrap-server broker:9092


docker exec -it broker  kafka-console-consumer --bootstrap-server broker:9092 --topic test_topic --from-beginning



```bash
    docker exec kafkacat kafkacat \
            -b broker:29092 \
            -r http://schema-registry:8081 \
            -s json \
            -t test_topic
```


## Elasticsearch 

```bash
curl -X GET "localhost:9200/_cat/indices?v" 
```
