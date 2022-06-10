# Kafka Connectors API 

#### Get Connectors

```bash
curl 'localhost:8083/connectors'
```

#### Get Conector Configuration

- CMD

```bash
curl 'localhost:8083/connectors/<CONNECTOR_NAME>/config'
```

- Examples

```bash
curl 'localhost:8083/connectors/s3-json-to-kafka/config'
curl 'localhost:8083/connectors/s3-json-to-kafka-index/config'
```

#### Connector Status - Check if any errors

- CMD

```bash
curl 'localhost:8083/connectors/<CONNECTOR_NAME>/status'
```

- Examples

```bash
curl 'localhost:8083/connectors/s3-json-to-kafka/status'
curl 'localhost:8083/connectors/s3-json-to-kafka-index/status'
```
#### Get Connector's tasks 

- CMD

```bash
curl 'localhost:8083/connectors/<CONNECTOR_NAME>/tasks'
```

- Examples

```bash
curl 'localhost:8083/connectors/s3-json-to-kafka/tasks'
```
#### Pause Connector

- CMD

```bash
curl 'localhost:8083/connectors/<CONNECTOR_NAME>/pause'
```

- Examples

```bash
curl -XPUT 'localhost:8083/connectors/s3-json-to-kafka/pause'
```
#### Resume Connector

- CMD

```bash
curl 'localhost:8083/connectors/<CONNECTOR_NAME>/resume'
```

- Examples

```bash
curl -XPUT 'localhost:8083/connectors/s3-json-to-kafka/resume'
```

#### Restart Connector

- CMD

```bash
curl 'localhost:8083/connectors/<CONNECTOR_NAME>/restart'
```

- Examples

```bash
curl -XPOST 'localhost:8083/connectors/s3-json-to-kafka/restart'
```

#### Delete the connector

```bash
curl -X DELETE  "http://localhost:8083/connectors/<CONNECTOR_NAME>"
```
