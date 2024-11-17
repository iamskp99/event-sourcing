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


## Do we store consumer group's consumer offset in __consumer_offset topic ?

Yes, Kafka stores the **consumer group's offsets** in a special internal topic called **`__consumer_offsets`**. This topic keeps track of the last committed offset for each partition that a consumer group is consuming.

### How It Works:
1. **Offset Tracking**:
   - When a consumer reads messages from a topic's partition, it periodically commits the offset of the last processed message to the **`__consumer_offsets`** topic.
   - This ensures that if the consumer restarts or fails, it can resume processing from the last committed offset.

2. **Commit Types**:
   - **Automatic Commit**:
     - Kafka automatically commits offsets at intervals specified by the `enable.auto.commit` (default is `true`) and `auto.commit.interval.ms` properties.
   - **Manual Commit**:
     - The application explicitly commits offsets using `commitSync()` or `commitAsync()` methods. This gives more control over when offsets are committed.

3. **Content of `__consumer_offsets`**:
   - The topic stores offsets as key-value pairs, where:
     - **Key**: Encodes the consumer group, topic, and partition.
     - **Value**: Encodes the committed offset and metadata (e.g., the timestamp of the commit).

4. **Replication and Durability**:
   - The **`__consumer_offsets`** topic is replicated like any other Kafka topic to ensure high availability and durability.

### Viewing Offsets in `__consumer_offsets`:
You can query the **`__consumer_offsets`** topic to inspect its content, but this is typically not needed for normal operations. Use the `kafka-consumer-groups` tool instead for a higher-level view:

```bash
docker exec -ti kafka /bin/kafka-consumer-groups --bootstrap-server localhost:9092 --group <group_name> --describe
```

This command will show:
- Consumer group members.
- Current offset, lag, and partition assignments.

### Key Notes:
- **`__consumer_offsets`** is an internal topic, so it is not displayed in regular topic listings unless you specify `--include-internal` with `kafka-topics`.
- By default, it is compacted, meaning only the latest offset for each consumer group-topic-partition is retained. This helps optimize storage and retrieval.







