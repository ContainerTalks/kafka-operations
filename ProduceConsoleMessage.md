# Produce Message from Console

## Create the topic

## List the Topics

## Run the Broker and then continue with the following

- Create the json file 
```
cat <<EOF>> sample.json
{"f1": "value1"}
{"f1": "value2"}
{"f1": "value3"}
EOF
```

- Run the console producer message using the json file created above

docker exec -it broker kafka-console-producer --broker-list broker:9092 --topic test-topic < sample.json


