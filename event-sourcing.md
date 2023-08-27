# Event Sourcing

### Kafka and its components

<img width="1512" alt="Screenshot 2023-08-26 at 1 15 30 PM" src="https://github.com/iamskp99/event-sourcing/assets/42648568/32ad19a1-2f0d-46c8-9eb2-08931fe4fe45">


1. Producer
2. Kafka Broker
3. Consumer

Kafka brokers have logical seperate spaces that are called topics and consumer groups have consumers that consume from it.

1. Producers push messages to a specific user topic and the messages get appended at the end of the topic.

<img width="1512" alt="Screenshot 2023-08-26 at 1 23 29 PM" src="https://github.com/iamskp99/event-sourcing/assets/42648568/c4a29808-7700-49c4-be66-1b5e8dac40e8">

2. Like databases , kafka also have partitions. But Why ?
   Suppose I have a topic name Users having two partitions, the first one stores name with range of A-I and the other one J-Z.
   Let's consider the first scenario where I have only one partition and one consumer. In this case I can't utilise parallelism
   but if I have two partitions and two consumers then I can utilise parallelism by making two threads and consuming the messages
   at a faster rate.

   <img width="1512" alt="Screenshot 2023-08-26 at 1 35 26 PM" src="https://github.com/iamskp99/event-sourcing/assets/42648568/3407557f-db52-4d01-8a04-ee026b7dc110">


4. So, how can we utilise kafka as a queue and pub-sub system when we want ?

   If you want to treat it as a queue, then put all your consumers in one consumer group. If you want to utilise it as a kafka pub-sub system
   then put all your consumers in different consumer groups.

   
