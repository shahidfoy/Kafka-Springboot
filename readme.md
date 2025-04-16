Setting up and running Kafka
=====================================

# setup kafka
Setup Kafka using Windows wsl with Ubuntu.

### Windows Powershell
```shell
wsl --status
wsl --list

wsl --update
wsl --install -d Ubuntu
```


### Ubuntu
```bash
sudo apt update

sudo apt install openjdk-17-jdk
java -version

# get kafka tgz
cd tools
mkdir kafka
cd kafka

tar -xvf ../kafka_2.13-3.9.0.tgz

cd kafka_2.13-3.9.0
```

# start kafka
Set up and configure kafka cluster id then start up kafka.

### Ubuntu
```bash
cd tools/kafka/kafka_2.13-3.9.0/

KAFKA_CLUSTER_ID="$(bin/kafka-storage.sh random-uuid)"

echo $KAFKA_CLUSTER_ID

bin/kafka-storage.sh format -t $KAFKA_CLUSTER_ID -c config/kraft/server.properties

bin/kafka-server-start.sh config/kraft/server.properties
```


# kafka sending and receiving
Send and receive messages in kafka by using the following.

Open three terminals 
- one for the process to run (crated from running the `start kafka` steps)
- one as the consumer
- one as the producer


### Ubuntu
consumer
```bash
cd tools/kafka/kafka_2.13-3.9.0/
bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic my.first.topic
```

producer
```bash
cd tools/kafka/kafka_2.13-3.9.0/
bin/kafka-console-producer.sh --bootstrap-server localhost:9092 --topic my.first.topic

# send message
>my first message
```

you should see the message show up in the consumer


# kafka tools

example of using helpon the bin shell files
```bash
bin/kafka-console-consumer.sh --help

# start kafka
bin/kafka-server-start.sh config/kraft/server.properties

# stop kafka
bin/kafka-server-stop.sh
```

# kafka topic tools

topic cli tool

```bash
# list all topics
bin/kafka-topics.sh --bootstrap-server localhost:9092 --list

# create new topic
bin/kafka-topics.sh --bootstrap-server localhost:9092 --create --topic my.new.topic

# get info on a topic
bin/kafka-topics.sh --bootstrap-server localhost:9092 --describe --topic my.new.topic

# change topic
bin/kafka-topics.sh --bootstrap-server localhost:9092 --alter --topic my.new.topic --partitions 3

# remove topic
bin/kafka-topics.sh --bootstrap-server localhost:9092 --delete --topic my.new.topic
```


# kafka consumer group tools

```bash
# set up topic
bin/kafka-topics.sh --bootstrap-server localhost:9092 --create --topic cg.demo.topic --partitions 5

bin/kafka-topics.sh --bootstrap-server localhost:9092 --describe cg.demo.topic

# list consumer groups
bin/kafka-consumer-groups.sh --bootstrap-server localhost:9092 --list

# create consumer
bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic cg.demo.topic --group my.new.group

# get consumer group details
bin/kafka-consumer-groups.sh --bootstrap-server localhost:9092 --describe --group my.new.group

# get state of consumer group
bin/kafka-consumer-groups.sh --bootstrap-server localhost:9092 --describe --group my.new.group --state

# get consumer group members
bin/kafka-consumer-groups.sh --bootstrap-server localhost:9092 --describe --group my.new.group --members
```
















