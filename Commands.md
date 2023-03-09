
## Create Topic 

docker run --net=host --rm confluentinc/cp-kafka:7.0.3 kafka-topics --create --topic test_topic_name --partitions 3 --replication-factor 2 --if-not-exists --bootstrap-server localhost:29092

## List Topics

docker run --net=host --rm confluentinc/cp-kafka:7.0.3 kafka-topics --list --bootstrap-server localhost:29092

## Delete Topic 

docker run --net=host --rm confluentinc/cp-kafka:7.0.3 kafka-topics --bootstrap-server localhost:29092 --delete --topic test_topic_name
