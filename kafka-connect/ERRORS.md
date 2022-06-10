# 1

#### ERROR
```error
Command [/usr/local/bin/dub path /var/lib/kafka/data writable] FAILED ! #131
```

#### Sol

Update your docker-compose with `user: root` this will resolve the issues. 

```yml
kafka-connect:
    image: confluentinc/cp-kafka-connect-base:latest
    container_name: kafka-connect
    user: root
   ......
```

