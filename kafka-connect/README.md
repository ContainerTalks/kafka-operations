# Kafka Connect

## Running Kafka with Connect 

```bash
cd kafka-connect/
docker-compose up -d
```
# Kafka Connect - Prod

..... yet to write

# Kafka Connect - Staging 

## Run Kafka Connect 

To run the kafka connect we need to create the `.env` file in the server with 
```env
AWS_ACCESS_KEY=dummy
AWS_SECRET_KEY=dummy
```
This keys are having the access to required bucket.

- Run the container with the following

```bash
cd wsrepo/docker/kafka-connect/
docker-compose up -d
```

- Plugins API

Install plugins 

```bash
confluent-hub install --no-prompt confluentinc/kafka-connect-s3-source:latest
confluent-hub install --no-prompt confluentinc/kafka-connect-elasticsearch:5.3.0
```
```bash
# To know the specific plugins
curl -s localhost:8083/connector-plugins|jq '.[].class'| egrep 'ElasticsearchSinkConnector|S3SourceConnector'

# List all the plugins
curl 'localhost:8083/connector-plugins'
```


## Create the connectors

- Create s3 to kafka connector  for the given bucket json files path `wisestepstaging/candidate-data-migration/candidatesearchindex/`

```bash
# Post / Create the connector
curl -i -X PUT -H  "Content-Type:application/json" http://localhost:8083/connectors/candidatesearch-data/config -d '{
        "name": "candidatesearch-data",
        "connector.class": "io.confluent.connect.s3.source.S3SourceConnector",
        "tasks.max": "1",
        "value.converter": "org.apache.kafka.connect.json.JsonConverter",
        "mode": "GENERIC",
        "topics.dir": "candidate-data-migration",
        "confluent.topic.bootstrap.servers": "192.31.2.108:19092",
        "confluent.topic.replication.factor": "1",
        "format.class": "io.confluent.connect.s3.format.json.JsonFormat",
        "topic.regex.list": "candidatesearchindex:.*",
        "s3.bucket.name": "wisestepstaging",
        "s3.region": "us-east-1",
        "s3.credentials.provider.class": "com.amazonaws.auth.EnvironmentVariableCredentialsProvider",
        "value.converter.schemas.enable": "false"
    }'
```

> Check the topic name `candidatesearchindex` with `--list` API in broker and run console consumer to know the messages. If no momenet is 

- Create the kafka to es connector

```bash
# Post / Create the connector
curl -i -X PUT -H  "Content-Type:application/json" \
    http://localhost:8083/connectors/candidatesearch-data-index/config \
    -d '{
    "connector.class": "io.confluent.connect.elasticsearch.ElasticsearchSinkConnector",
    "tasks.max": "1",
    "topics" : "candidatesearchindex", 
    "topic.index.map" : "candidatesearchindex:candidatesearch-index",
    "connection.url": "http://192.31.2.108:9200",
    "type.name": "log",
    "key.ignore": "true",
    "schema.ignore" : "true"
}'
```



## Ref:

- https://sematext.com/blog/kafka-connect-elasticsearch-how-to/ 