# Kafka

## Running Kafka with Connect 

```bash
cd kafka-connect/
docker-compose up -d
```

## Wait for Connect 

```bash
bash -c ' \
echo -e "\n\n=============\nWaiting for Kafka Connect to start listening on localhost ⏳\n=============\n"
while [ $(curl -s -o /dev/null -w %{http_code} http://localhost:8083/connectors) -ne 200 ] ; do
  echo -e "\t" $(date) " Kafka Connect listener HTTP state: " $(curl -s -o /dev/null -w %{http_code} http://localhost:8083/connectors) " (waiting for 200)"
  sleep 5
done
echo -e $(date) "\n\n--------------\n\o/ Kafka Connect is ready! Listener HTTP state: " $(curl -s -o /dev/null -w %{http_code} http://localhost:8083/connectors) "\n--------------\n"
'
```

# Verify S3 and Elasticsearch Connectors are available


- FIND THE SPECIFIC 

```bash
curl -s localhost:8083/connector-plugins|jq '.[].class'| egrep 'ElasticsearchSinkConnector'
```

- ALL THE PLUGINS INSTALLED

```bash
curl -s localhost:8083/connector-plugins|jq '.[].class'
```

## Stream Data from S3 to Kafka

```bash
curl -i -X PUT -H  "Content-Type:application/json" \
    http://localhost:8083/connectors/s3-candidate-json-file-list-00/config \
    -d '{
        name=candidates-data-pipeline
        connector.class=io.confluent.connect.s3.source.S3SourceConnector
        tasks.max=1
        value.converter=org.apache.kafka.connect.json.JsonConverter
        mode=GENERIC
        topics.dir=candidates-data
        format.class=io.confluent.connect.s3.format.json.JsonFormat
        topic.regex.list=candidates-data:.*
        s3.bucket.name=sthreetoes
        value.converter.schemas.enable=false
    }'
```

## Stream Data from Kafka to Elasticsearch

```bash
curl -i -X PUT -H  "Content-Type:application/json" \
    http://localhost:8083/connectors/sink-elastic-orders-00/config \
    -d '{
        "connector.class": "io.confluent.connect.elasticsearch.ElasticsearchSinkConnector",
        "topics": "candidates-data",
        "connection.url": "http://elasticsearch:9200",
        "type.name": "type.name=kafkaconnect",
        "key.ignore": "true",
        "schema.ignore": "true"
    }'
```




## Elasticsearch 

```bash
curl -X GET "localhost:9200/_cat/indice?v" 
```