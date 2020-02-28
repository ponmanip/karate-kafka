(work in progress)

## Introduction

TBD

## Managing the local Kafka broker

The configuration for Kafka and Zookeeper is specified in `kafka-single-broker.yml`. See
[Wurstmeister's Github wiki](https://github.com/wurstmeister/kafka-docker) on how to configure this.

### Setting up the Kafka Cluster

From the command line, run 

```
$ ./bin/setup.sh
```

Start off a consumer ...

```
kafka-console-consumer.sh --bootstrap-server localhost:9092 \
    --topic test-topic \
    --from-beginning \
    --formatter kafka.tools.DefaultMessageFormatter \
    --property print.key=true \
    --property print.value=true \
    --property print.timestamp=true \
    --property key.deserializer=org.apache.kafka.common.serialization.StringDeserializer \
    --property value.deserializer=org.apache.kafka.common.serialization.LongDeserializer
```

In another terminal start off a producer, and enter some data for the producer. Ideally the consumer
will print them out on the console ... 
```
$ kafka-console-producer.sh --broker-list 127.0.0.1:9092 --topic test-input
>
> (ctrl C to quit)

```

Type something into the producer. If all goes well, you should see the consumer echo it back.

### Tearing down the setup

From the command-line, run

```
$ ./teardown.sh
```

## Interop between Karate and Java

- hello-feature
- parameter passing to constructor and methods

## Producing Data to Kafka
### Setup
We will write something to the `test-topic` in Kafka and consume it through the console consumer

Start the Kafka cluster and a consumer
```
$ ./setup.sh
Starting kafka-karate_zookeeper_1 ... done
Starting kafka-karate_kafka_1     ... done
CONTAINER ID        IMAGE                    NAMES
ce9b01556d15        wurstmeister/zookeeper   kafka-karate_zookeeper_1
33685067cb82        wurstmeister/kafka       kafka-karate_kafka_1
*** sleeping for 10 seconds (give time for containers to spin up)
*** the following topic were created ....
test-input
test-output
test-topic
$ kafka-console-consumer.sh --bootstrap-server localhost:9092 \
      --topic test-topic \
      --formatter kafka.tools.DefaultMessageFormatter \
      --property print.key=true \
      --property print.value=true \
      --property print.timestamp=true \
      --property key.deserializer=org.apache.kafka.common.serialization.StringDeserializer \
      --property value.deserializer=org.apache.kafka.common.serialization.StringDeserializer
```

From the IDE, run `kafka-producer.feature` in the folder `src/test/java/karate/kafka/`. If all goes well, the 
Kafka consumer should output all the data written by the producer.

### Kafka Producer

```
# Create Kafka Producer with the default properties
* def kp = new KafkaProducer()

# Create Kafka Producer with the specified properties
* def kp = new KafkaProducer( { ... } )

# Get the default Properties
* def props = KafkaProducer.getDefaultProperties()

# Write a message with a null key
* kp.send(topic, "hello world")

# Write a message with a key
* kp.send(topic, "the key", "hello again")

# Writing a JSON object to Kafka
# def data = { ... }
* string str = data
* kp.send(topic, str)

# Close the Kafka producer
* kp.close()
```


### References

* [Running Kafka inside Docker](https://github.com/wurstmeister/kafka-docker)
* [Markdown syntax](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)
* [Word count usin Kafka Streams](https://github.com/gwenshap/kafka-streams-wordcount) by Gwen Shapiro


