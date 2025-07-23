# Kafka-Theory
Contains theory part of Kafka Programming

### What is Apache Kafka ?

- Apache Kafka is an open-source distributed event streaming platform
- Distributed: Multiple Regions to perform the operations to balance the load of data streaming.
- Event Streaming:
    - Create Real-Time Stream - Triggering a Transaction or Real-time stream of data.
    - Process Real-Time Stream - Processing a Transaction. Ex. if there’s limit for an user with transaction count, the processing needs to be done and validated.

### Where does Kafka come from ?

- Kafka was originally developed at LinkedIn and was subsequently open sourced in early 2011.
- Now it comes under Apache Software Foundation.

### Why do we need Kafka ?

- It’s a Messaging System acts as middle layer between the applications in-order to transfer / sync the transactions or data manipulations.
- If the App 1 sends a message to App 2 and when the App 2 isn’t available, the message stays in Kafka until the App 2 is back online to process the messages.
- Data loss can be prevented and business wouldn’t be impacted.
- Data Format - Data format or payload differs from each connections.
- Connection Type - The Type of Connections are different when connecting from different sources.
- Number of Connections - Each connection and its subsequent destinations will increase in connection count.

### How does Kafka Works ?

- It works in Publisher/Subscriber Model.
- Publisher pushes the message / event to Kafka (Message Broker)
- Subscriber reads/listens the message / event from Message Broker and process them.

### Kafka Components

- Producer
    - Its the source of the data which publishes the message to Kafka Message Broker.
- Consumer
    - Its the receiver of the data or reads the message which is published from the Message Broker.
- Broker
    - Its nothing but just a server. in simple words, a broker is just an intermediate entity that helps in message exchanges b/w producer and consumer.
- Cluster
    - As Kafka is distributed systems, cluster is a replica or a region where a Message brokers are hosted. An application can have any number of clusters as replicas based on data flow and clusters has many Kafka Brokers.
- Topic
    - its a primary component in a broker.
    - Topic categorizes the different type of messages.
    - Broker can have any number of topics, based on the topic the producer pushes the messages and the consumer reads the messages.
    - Acts as a Database table, stores specific data in relevant topic.
    - Ex. A Global enterprise can have country based topics.
    - It specifies the category of the message or the classification of the message. Listeners can then just respond to the messages that belong to the topics they are listening on.
- Partitions
    - Segregated sections of a topic to store the messages for processing.
    - Partitions are internally load balanced and no. of partitions are to be defined at the time of topic creation.
- Offset
    - In Kafka, a sequence number is assigned to each message in each partition of a Kafka topic. This sequence number is called offset.
    - When a partition has 10 messages and a consumer reads 5 messages and goes down, the offset value is set to 6 for that consumer in that partition. So when the consumer is back online he reads from 6th message and not from 1st.
- Consumer Groups
    - A group of consumer instance combined to share the workload.
    - It is just like dividing a piece of large task among multiple individuals.
    - No guarantee on the partitions and consumer instance mapping.
- Zookeeper
    - Zookeeper is a prerequisite for Kafka and its a key component of Kafka which contains the metadata of the Kakfa cluster, producer and consumer.
    - Kafka uses Zookeeper for coordination and to track the status of Kafka Cluster nodes. It also keeps track of Kafka Topics, Partitions, offsets etc.
    - It’s possible to start the Kafka server without zookeeper using kraft mode.
- Benefits of Kafka without Zookeeper
    - Eliminating system complexities.
    - Data redundancy while running Kafka without Zookeeper.
    - Simplified Kafka architecture without any third-party service dependencies.

### Installations: 
- Apache Kafka - Open Source : https://kafka.apache.org/downloads
- Commercial Distribution - Confluent Kafka Community Edition : https://www.confluent.io/get-started/?product=self-managed
- Kafka Offset explorer - GUI interface to manage Kafka : https://www.kafkatool.com/download.html

### Commands & Steps to Perform Locally

- ZooKeeper
    - run the below script
        - `bin/zookeeper-server-start.sh config/zookeeper.properties`
    - Default port 2181
- kafka Server
    - run the below script
        - `bin/kafka-server-start.sh config/server.properties`
    - Default port is 9092
- Topic → Partition count, Replication Factor
- run the below script
    - `bin/kafka-topics.sh --bootstrap-server localhost:9092 --create --topic javatraining-topic --partitions 3 --replication-factor 1`
    --bootstrap-server localhost:9092 —> bootstrap server
    - --create --topic javatraining-topic —> creation of topic with the name javatraining-topic
    - --partitions 3 —> 3 partitions defined for the topic
    - --replication-factor 1 —> 1 replica defined for the topic
- `bin/kafka-topics.sh --bootstrap-server localhost:9092 --list` —> to show the list of topics available.
- `bin/kafka-topics.sh --bootstrap-server localhost:9092 --describe` --topic javatraining-topic —> to describe the specific topic in detail.
- `bin/kafka-console-producer.sh --broker-list localhost:9092 --topic javatraning-topic` —> to start the producer to push messages.
    - --broker-list localhost:9092 —> list of brokers which needs to be sent.
    - --topic javatraning-topic —> topic name which needs to be sent
- `bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic javatraining-topic --from-beginning` —> to start the consumer to read the messages
    - --bootstrap-server localhost:9092 —> bootstrap server which needs to be listened.
    - --topic javatraining-topic —> topic which needs to be listened.
    - --from-beginning —> read message from offset or beginning
- bin/kafka-console-producer.sh --broker-list localhost:9092 --topic javatraining-topic </Users/vijayganesh/Downloads/Users.csv

### Install Kafka via Docker Image:

- Navigate to the folder of docker-compose.yml file and execute the below command.
- `docker compose -f docker-compose.yml up -d`
- Create a topic
    - `kafka-topics.sh --create --zookeeper zookeeper:2181  --topic docket-kafka-topic --partitions 1 --replication-factor 1`

## Producer

- Sending message to Kafka Topic can be achieved by KafkaTemplate class by defining the Topic Name and the message to be sent.
- Example Project : https://github.com/Vijay-Ganesh-08/kafka-producer

### SpringBoot Topic Creation

- Allowing SpringBoot to create the topic by itself with Default Configuration
- We just need to pass the topic name while pushing messages to topic via Kafka Template
- SpringBoot default configuration is Partition : 1 , ReplicaSet : 1

### Manual Topic Creation

- Creation of Topic with the below command
- bin/kafka-topics --bootstrap-server localhost:9092 --create --topic --partitions 3 --replication-factor 1
- Partition and the ReplicaSet is being defined by the user manually

### Topic Creation via Code

- NewTopic class can be used in configuration class to create the topic manually
- Partition and the ReplicaSet is being defined by the user in the code

### Portal Creation

- Some companies have a UI portal to create topic and managing the same
- That can be used to create topic with required configurations.

### Sending JSON Object

- Below properties to be added in the application.properties file in-order to send the JSON Object to Kafka Topic
- `spring.kafka.producer.key-serializer=org.apache.kafka.common.serialization.StringSerializer`
- `spring.kafka.producer.value-serializer=org.springframework.kafka.support.serializer.JsonSerializer`
- The configuration can also be done via Java Class files. it's given in config\KafkaProducerConfig class file.
- KafkaTemplate.send is a overloaded method, were the producer can send message to specific Partition.

## Consumer

- When there’s multiple partitions of the producer, we can have multiple instance of the consumer to share the load.
- When each consumer is assigned to a partition and if there’s an extra consumer instance available with work as DR and its called Consumer Rebalancing.
- @KafkaListener is used to listen to specific topic
- Group-Id is mandatory to consume from a Topic
- Example Project : https://github.com/Vijay-Ganesh-08/kafka-consumer

### Consuming JSON Object

- Below properties to be added in the application.properties file in-order to read the JSON Object from Kafka Topic
- `spring.kafka.consumer.key-deserializer=org.apache.kafka.common.serialization.StringDeserializer`
- `spring.kafka.consumer.value-deserializer=org.springframework.kafka.support.serializer.JsonDeserializer`
- `spring.kafka.consumer.properties.spring.json.trusted.packages=com.training.model`
- The configuration can also be done via Java Class files. its given in config\EventConsumerConfig class file.
- With the below command and TopicPartition annotation, consumer can be routed to single partition to listen the messages.
- `topicPartitions = {@TopicPartition(topic="<TopicName>",partitions = {"2"})}`


# Commands
## Open Source Kafka Startup in local

- Start Zookeeper Server
`sh bin/zookeeper-server-start.sh config/zookeeper.properties`
- Start Kafka Server / Broker
`sh bin/kafka-server-start.sh config/server.properties`
- Create topic
`sh bin/kafka-topics.sh --bootstrap-server localhost:9092 --create --topic NewTopic --partitions 3 --replication-factor 1`
- list out all topic names
`sh bin/kafka-topics.sh --bootstrap-server localhost:9092 --list`
- Describe topics
`sh bin/kafka-topics.sh --bootstrap-server localhost:9092 --describe --topic NewTopic`
- Produce message
`sh bin/kafka-console-producer.sh --broker-list localhost:9092 --topic NewTopic`
- consume message
`sh bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic NewTopic --from-beginning`

## Confluent Kafka Community Edition in local

- Start Zookeeper Server
`bin/zookeeper-server-start etc/kafka/zookeeper.properties`
- Start Kafka Server / Broker
`bin/kafka-server-start etc/kafka/server.properties`
- Create topic
`bin/kafka-topics --bootstrap-server localhost:9092 --create --topic NewTopic1 --partitions 3 --replication-factor 1`
- list out all topic names
`bin/kafka-topics --bootstrap-server localhost:9092 --list`
- Describe topics
`bin/kafka-topics --bootstrap-server localhost:9092 --describe --topic NewTopic1`
- Produce message
`bin/kafka-console-producer --broker-list localhost:9092 --topic NewTopic1`
- consume message
`bin/kafka-console-consumer --bootstrap-server localhost:9092 --topic NewTopic1 --from-beginning `
- Send CSV File data to kafka
`bin/kafka-console-producer --broker-list localhost:9092 --topic NewTopic1 <bin/customers.csv`