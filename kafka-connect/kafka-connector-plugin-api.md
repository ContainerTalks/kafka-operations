# Kafka Connector Plugin API 

#### List all the plugins

```bash
curl 'localhost:8083/connector-plugins'
```
#### # To know the specific plugins

```bash
curl -s localhost:8083/connector-plugins|jq '.[].class'| egrep 'ElasticsearchSinkConnector|S3SourceConnector'
```