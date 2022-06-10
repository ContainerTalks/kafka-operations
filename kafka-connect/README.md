# Kafka Connect

## Running Kafka with Connect 

To run the kafka connect we need to create the `.env` file in the server with 
```env
AWS_ACCESS_KEY=dummy
AWS_SECRET_KEY=dummy
```
This keys are having the access to required bucket.

- Run the container with the following

```bash
cd kafka-connect/
docker-compose up -d
```
