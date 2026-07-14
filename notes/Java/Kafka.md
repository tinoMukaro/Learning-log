# Apache Kafka Basics

## What is Kafka?

Apache Kafka is a distributed event-streaming platform. Applications use it to
send and receive messages without calling each other directly.

Example:

```text
Order Service -> Kafka topic -> Email Service
                           -> Inventory Service
```

The Order Service publishes an event and continues working. Other services read
the event when they are ready.

## Core Concepts

- **Producer** - sends messages to Kafka.
- **Consumer** - reads messages from Kafka.
- **Topic** - a named channel that stores related messages, such as `orders`.
- **Broker** - a Kafka server that stores messages.
- **Partition** - a topic is divided into partitions for ordering and scaling.
- **Offset** - the position of a message inside a partition.
- **Consumer group** - consumers with the same group ID share the work.
- **KRaft** - Kafka's built-in system for managing cluster metadata.

Messages are ordered only within the same partition. Messages with the same key
normally go to the same partition.

## Why Use Kafka?

- Asynchronous communication
- Loose coupling between services
- High message throughput
- Messages are stored and can be read again
- Easy to add more consumers without changing the producer

## Basic Local Setup with Docker

Docker is the easiest way to run Kafka locally.

Create a `compose.yaml` file:

```yaml
services:
  kafka:
    image: apache/kafka:latest
    container_name: kafka
    ports:
      - "9092:9092"
```

Start Kafka:

```bash
docker compose up -d
```

Check that it is running:

```bash
docker ps
docker logs kafka
```

Stop it:

```bash
docker compose down
```

## Basic Kafka Commands

Run these commands from inside the Kafka container.

### Create a topic

```bash
docker exec kafka /opt/kafka/bin/kafka-topics.sh \
  --bootstrap-server localhost:9092 \
  --create \
  --topic orders \
  --partitions 1 \
  --replication-factor 1
```

### List topics

```bash
docker exec kafka /opt/kafka/bin/kafka-topics.sh \
  --bootstrap-server localhost:9092 \
  --list
```

### Send messages

```bash
docker exec -it kafka /opt/kafka/bin/kafka-console-producer.sh \
  --bootstrap-server localhost:9092 \
  --topic orders
```

Type a message and press Enter:

```text
Order 1001 created
```

Press `Ctrl+C` to exit.

### Read messages

```bash
docker exec -it kafka /opt/kafka/bin/kafka-console-consumer.sh \
  --bootstrap-server localhost:9092 \
  --topic orders \
  --from-beginning
```

## Kafka with Spring Boot

Add Spring Kafka to `pom.xml`:

```xml
<dependency>
    <groupId>org.springframework.kafka</groupId>
    <artifactId>spring-kafka</artifactId>
</dependency>
```

Configure Kafka in `application.properties`:

```properties
spring.kafka.bootstrap-servers=localhost:9092
spring.kafka.consumer.group-id=order-service
spring.kafka.consumer.auto-offset-reset=earliest
```

### Producer example

```java
@Service
public class OrderProducer {

    private final KafkaTemplate<String, String> kafkaTemplate;

    public OrderProducer(KafkaTemplate<String, String> kafkaTemplate) {
        this.kafkaTemplate = kafkaTemplate;
    }

    public void sendOrder(String order) {
        kafkaTemplate.send("orders", order);
    }
}
```

### Consumer example

```java
@Service
public class OrderConsumer {

    @KafkaListener(topics = "orders", groupId = "order-service")
    public void receiveOrder(String order) {
        System.out.println("Received: " + order);
    }
}
```

## Simple Flow

1. A producer sends an event to a topic.
2. Kafka stores it in a partition.
3. A consumer reads the event.
4. Kafka tracks the consumer's position using an offset.

## Important Notes

- Kafka must be running before the Spring Boot application connects to it.
- A single-broker setup is suitable for learning, not production.
- Use JSON for objects such as order events.
- Do not send passwords or other sensitive data in events.
- Consumers should safely handle receiving the same message more than once.
