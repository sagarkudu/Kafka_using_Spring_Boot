# Kafka_using_Spring_Boot
Learn kafka

zookeeper comes with the kafka distribution that we have downlaodeded already.

After that we are going to spin up kafka broker, once the broker is up it register with zookeeper.

After that zookeeper manages and monitor the health of kafka broker.

Zookeeper plays a vital role when you have a multiple brokers.



# Setting Up Kafka


<p>

- Make sure you are navigated inside the **kafka/config** root directory.

## Edit server.properties file

```

log.dirs=c:/kafka/kafka-logs
```

## Edit zookeeper.properties file

```
dataDir=c:/kafka/zookeeper-data
```

<p>

- Make sure you are navigated inside the **kafka** root directory.


## Start Zookeeper and Kafka Broker

-   Start up the Zookeeper. (always start zookeeper first)
->  Open new cmd > go to kafka folder, then start zookeeper server and pass config file to it.

```
.\bin\windows\zookeeper-server-start.bat .\config\zookeeper.properties
```
-> you can see zookeeper has been started on port #2181


-   Start up the Kafka Broker
->  Open new cmd > go to kafka folder, then start kafka broker and pass config file to it.

```
.\bin\windows\kafka-server-start.bat .\config\server.properties
```
-> you can see kafka server is running with brokerId=0

## How to create a topic ? 
- open new cmd > come in /kafka directory. (make sure above 2 instances are running).
-> we are going to create a topic "**TestTopic**"

```
.\bin\windows\kafka-topics.bat --create --zookeeper localhost:2181 --replication-factor 1 --partitions 4 --topic TestTopic
```
-> you can see Topic is created with name "**TestTopic**"

## Checking list of Topics available in kafka server.

```
.\bin\windows\kafka-topics.bat --list --zookeeper localhost:2181 
```
-> you can see Topic is listed with name "**TestTopic**"

## Sending and Receiving messages

### lets start our kafka producer

```
.\bin\windows\kafka-console-producer.bat --broker-list localhost:9092 --topic TestTopic
```

--> Type your message in single line 
e.g 
- Hi Sagar
- How are you?
- I am good. Thanks
	
--> Now we have sent message till now, now kafka consumer is waiting to consume this messages.


### lets start our kafka consumer
--> open new cmd to recieve messages

```
.\bin\windows\kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic TestTopic --from-beginning
```

--> List of message received which is sent by Producer. 
**  Hi Sagar
	How are you?
	I am good. Thanks**
	
--> Now we have received message till now, which is sent by Kafka Producer and it will continuously wait in queue for the new message from producer.

Note: The **--from-beginning** is important, if not used order will not be maintained and messages will go into another partitions.

**Note**: you can also check for the server console for  logs or what is activities going here, so you see what is clean-up policy, what are the segments available, how many partitions are there,data retension time period, etc.


## Additional things to learn

## How to create a topic ?

```
./kafka-topics.sh --create --topic test-topic -zookeeper localhost:2181 --replication-factor 1 --partitions 4
```

## How to instantiate a Console Producer?

### Without Key

```
./kafka-console-producer.sh --broker-list localhost:9092 --topic test-topic
```

### With Key

```
./kafka-console-producer.sh --broker-list localhost:9092 --topic test-topic --property "key.separator=-" --property "parse.key=true"
```

## How to instantiate a Console Consumer?

### Without Key

```
./kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test-topic --from-beginning
```

### With Key

```
./kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test-topic --from-beginning -property "key.separator= - " --property "print.key=true"
```

### With Consumer Group

```
./kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test-topic --group <group-name>
```
</p>

</details>

<details><summary>Windows</summary>
<p>

- Make sure you are inside the **bin/windows** directory.

## Start Zookeeper and Kafka Broker

-   Start up the Zookeeper.

```
zookeeper-server-start.bat ..\..\config\zookeeper.properties
```

-   Start up the Kafka Broker.

```
kafka-server-start.bat ..\..\config\server.properties
```

## How to create a topic ?

```
kafka-topics.bat --create --topic test-topic -zookeeper localhost:2181 --replication-factor 1 --partitions 4
```

## How to instantiate a Console Producer?

### Without Key

```
kafka-console-producer.bat --broker-list localhost:9092 --topic test-topic
```

### With Key

```
kafka-console-producer.bat --broker-list localhost:9092 --topic test-topic --property "key.separator=-" --property "parse.key=true"
```

## How to instantiate a Console Consumer?

### Without Key

```
kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic test-topic --from-beginning
```

### With Key

```
kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic test-topic --from-beginning -property "key.separator= - " --property "print.key=true"
```

### With Consumer Group

```
kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic test-topic --group <group-name>
```
</p>

</details>

## Setting Up Multiple Kafka Brokers

- The first step is to add a new **server.properties**.

- We need to modify three properties to start up a multi broker set up.

```
broker.id=<unique-broker-d>
listeners=PLAINTEXT://localhost:<unique-port>
log.dirs=/tmp/<unique-kafka-folder>
auto.create.topics.enable=false
```

- Example config will be like below.

```
broker.id=1
listeners=PLAINTEXT://localhost:9093
log.dirs=/tmp/kafka-logs-1
auto.create.topics.enable=false
```

### Starting up the new Broker

- Provide the new **server.properties** thats added.

```
./kafka-server-start.sh ../config/server-1.properties
```

```
./kafka-server-start.sh ../config/server-2.properties
```

# Advanced Kafka CLI operations:

<details><summary>Mac</summary>
<p>

## List the topics in a cluster

```
./kafka-topics.sh --zookeeper localhost:2181 --list
```

## Describe topic

- The below command can be used to describe all the topics.

```
./kafka-topics.sh --zookeeper localhost:2181 --describe
```

- The below command can be used to describe a specific topic.

```
./kafka-topics.sh --zookeeper localhost:2181 --describe --topic <topic-name>
```

## Alter the min insync replica
```
./kafka-topics.sh --alter --zookeeper localhost:2181 --topic library-events --config min.insync.replicas=2
```

## Delete a topic

```
./kafka-topics.sh --zookeeper localhost:2181 --delete --topic test-topic
```
## How to view consumer groups

```
./kafka-consumer-groups.sh --bootstrap-server localhost:9092 --list
```

### Consumer Groups and their Offset

```
./kafka-consumer-groups.sh --bootstrap-server localhost:9092 --describe --group console-consumer-27773
```

## Viewing the Commit Log

```
./kafka-run-class.sh kafka.tools.DumpLogSegments --deep-iteration --files /tmp/kafka-logs/test-topic-0/00000000000000000000.log
```

## Setting the Minimum Insync Replica

```
./kafka-configs.sh --alter --zookeeper localhost:2181 --entity-type topics --entity-name test-topic --add-config min.insync.replicas=2
```
</p>
</details>


<details><summary>Windows</summary>
<p>

- Make sure you are inside the **bin/windows** directory.

## List the topics in a cluster

```
kafka-topics.bat --zookeeper localhost:2181 --list
```

## Describe topic

- The below command can be used to describe all the topics.

```
kafka-topics.bat --zookeeper localhost:2181 --describe
```

- The below command can be used to describe a specific topic.

```
kafka-topics.bat --zookeeper localhost:2181 --describe --topic <topic-name>
```

## Alter the min insync replica
```
kafka-topics.bat --alter --zookeeper localhost:2181 --topic library-events --config min.insync.replicas=2
```


## Delete a topic

```
kafka-topics.bat --zookeeper localhost:2181 --delete --topic <topic-name>
```


## How to view consumer groups

```
kafka-consumer-groups.bat --bootstrap-server localhost:9092 --list
```

### Consumer Groups and their Offset

```
kafka-consumer-groups.bat --bootstrap-server localhost:9092 --describe --group console-consumer-27773
```

## Viewing the Commit Log

```
kafka-run-class.bat kafka.tools.DumpLogSegments --deep-iteration --files /tmp/kafka-logs/test-topic-0/00000000000000000000.log
```
</p>
</details>
