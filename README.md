### To go into the docker container : 
```
docker exec -it kafka
```

### To make a producer with partition

```
docker exec -ti kafka /bin/kafka-console-producer --topic my_topic --bootstrap-server localhost:9092 --property "partition=0"

docker exec -ti kafka /bin/kafka-console-producer --topic my_topic --bootstrap-server localhost:9092 --property "partition=1"
```

### To make a consumer with consumer group

```
docker exec -ti kafka /bin/kafka-console-consumer --topic my_topic --bootstrap-server localhost:9092 --group terminal_consumers
```

Note : Multiple consumer cannot listen to same partition of a topic.

## Can one consumer be part of multiple consumer group ?

No, a single Kafka consumer instance cannot belong to multiple consumer groups simultaneously. Each consumer instance can only join one consumer group at a time, which is specified using the --group option when starting the consumer.

Why?
Kafka's consumer group mechanism assigns topic partitions to consumers within a group. The group acts as a logical unit for load balancing and offset tracking.
Allowing a consumer to join multiple groups would create conflicts in offset tracking and partition assignment, as each group maintains its own state and offsets.


## Can consumers in one consumer group listen to different topics ?

Yes, consumers within a single consumer group can listen to different topics, but this must be configured programmatically or through custom logic. 
The Kafka consumer group protocol does not enforce that all consumers in a group must subscribe to the same topics. 
However, the typical use case is for all consumers in a group to subscribe to the same topics and balance partitions among themselves.










