# Produce Message from Console

#### Create the topic

```bash
docker exec -it broker kafka-topics --create --bootstrap-server broker:9092 --replication-factor 1 --partitions 1 --topic test
```

OUTPUT: 
```bash
Created topic test.
```
#### List the Topics
```bash
docker exec -it broker kafka-topics kafka-topics --list --bootstrap-server broker:9092
```

OUTPUT : Returns the list of topics created
```bash
test
```
#### Console json producer 
- Run the console producer message using the json object

```bash
docker exec -it broker kafka-console-producer --broker-list broker:9092 --topic test 
```
- It prompts for message, enter the message without \n or space

```json
{"title":"The Matrix","year":1999,"cast":["Keanu Reeves","Laurence Fishburne","Carrie-Anne Moss","Hugo Weaving","Joe Pantoliano"],"genres":["Science Fiction"]}

{"title":"The Matrix Return","year":1999,"cast":["Keanu Reeves","Laurence Fishburne","Carrie-Anne Moss","Hugo Weaving","Joe Pantoliano"],"genres":["Science Fiction"]}
```
#### Console message producer

```bash
docker exec -it broker kafka-console-producer --broker-list broker:9092 --topic test
```

#### Console Consumer

docker exec -it broker  kafka-console-consumer --bootstrap-server broker:9092 --topic test --from-beginning
