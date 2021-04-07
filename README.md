# Kafka_using_Spring_Boot
Learn kafka

zookeeper comes with the kafka distribution that we have downlaodeded already.

After that we are going to spin up kafka broker, once the broker is up it register with zookeeper.

After that zookeeper manages and monitor the health of kafka broker.

Zookeeper plays a vital role when you have a multiple brokers.

# Setting Up Kafka

<p>

- Make sure you are navigated inside the bin directory.

## Start Zookeeper and Kafka Broker

-   Start up the Zookeeper.
->   go to kafka folder, then start zookeeper server and pass config file to it.

```
c:\kafka > .\bin\windows\zookeeper-server-start.bat .\config\zookeeper.properties
```
-> you can see zookeeper has been started on port #2181


-   Start up the Kafka Broker

```
c:\kafka > .\bin\windows\kafka-server-start.bat .\config\server.properties
```
-> you can see kafka server is running with brokerId=0

## How to create a topic ? 
- open new cmd > come in /kafka directory. (make sure above 2 instances are running).
-> we are going to create a topic "**TestTopic**"

```
c:\kafka > .\bin\windows\kafka-topics.bat --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic TestTopic
```
-> you can see Topic is created with name "**TestTopic**"

## Checking list of Topics available in kafka server.

```
c:\kafka > .\bin\windows\kafka-topics.bat --list --zookeeper localhost:2181 
```
-> you can see Topic is listed with name "**TestTopic**"

## Sending and Receiving messages

### lets start our kafka producer

```
c:\kafka > .\bin\windows\kafka-console-producer.bat --broker-list localhost:9092 --topic TestTopic
```

--> Type your message in single line 
e.g > Hi Sagar
	> How are you?
	> I am good. Thanks
	
--> Now we have sent message till now, now kafka consumer is waiting to consume this messages.


### lets start our kafka consumer
--> open new cmd to recieve messages

```
c:\kafka > .\bin\windows\kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic TestTopic --from-beginning
```

--> List of message received which is sent by Producer. 
**  Hi Sagar
	How are you?
	I am good. Thanks**
	
--> Now we have sent message till now, now kafka consumer is waiting to consume this messages.

Note: The **--from-beginning** is important, if not used order will not be maintained and messages will go into another partitions.

**Note**: you can also check for the server console for  logs or what is activities going here, so you see what is clean-up policy, what are the segments available, how many partitions are there,data retension time period, etc.
