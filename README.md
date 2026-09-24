# Apache Kafka — Two-Broker Cluster with a Java Producer & Consumer

A Kafka exercise with a **two-broker cluster** (Confluent Platform images via Docker Compose) and a Java producer / consumer pair that exchange JSON "operation" messages.

## 📦 What's inside

| File | Description |
| --- | --- |
| `KAFKA/KAFKAEX/docker-compose.yml` | ZooKeeper + two Kafka brokers (`kafka1` → host port 9091, `kafka2` → host port 9092) |
| `KAFKA/KAFKAEX/src/main/java/org/example/ProducerExample.java` | Sends `{"operand": 13, "operation": "fibPrime"}` to the `operations` topic and prints the partition it landed on |
| `KAFKA/KAFKAEX/src/main/java/org/example/ConsumerExample.java` | Consumer group `my-group`; reads `operations` from the earliest offset and processes each message |
| `KAFKA/KAFKA_EX_01.docx` | Exercise notes (Turkish) |

## 🧰 Tech stack

Java · Maven · kafka-clients 3.0.0 · Gson · Docker Compose · Confluent Platform (ZooKeeper, Kafka)

## 🚀 Running it

1. **Start the cluster**

   ```bash
   cd KAFKA/KAFKAEX
   docker compose up -d
   ```

2. **Start the consumer** (it keeps polling):

   ```bash
   mvn compile exec:java -Dexec.mainClass=org.example.ConsumerExample
   ```

3. **Send a message** from another terminal:

   ```bash
   mvn compile exec:java -Dexec.mainClass=org.example.ProducerExample
   ```

> ℹ️ The brokers advertise `kafka1:9092` / `kafka2:9092`. When the Java clients run on the host instead of inside the Docker network, adjust `KAFKA_ADVERTISED_LISTENERS` (for example `PLAINTEXT://localhost:9091` for `kafka1`) so the brokers are reachable.
