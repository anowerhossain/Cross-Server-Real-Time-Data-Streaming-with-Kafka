# Cross Server Real Time Data Streaming with Kafka 🚀
Cross-server real-time data streaming architecture using Apache Kafka, where two separate servers are configured to run Kafka producer and consumer components.

### Download and Extract Kafka 📥

- To begin, you'll need to download the Kafka archive from the official Apache site. This command fetches Kafka version 3.8.0

```bash
wget https://downloads.apache.org/kafka/3.8.0/kafka_2.13-3.8.0.tgz
```
📂 This will download the Kafka archive as a .tgz file to your current directory.

- Next, extract the downloaded Kafka tarball to prepare it for use. 

```bash
tar -xzf kafka_2.13-3.8.0.tgz
```
📂 This will extract the Kafka files into a folder named kafka_2.13-3.8.0.

### Configure KRaft and Start Server ⚙️

- Change into the Kafka directory where the Kafka files are located.

```bash
cd kafka_2.13-3.8.0
```
🏠 This takes you into the extracted Kafka directory to perform configurations.

- KRaft mode requires a unique cluster identifier (UUID) to manage the metadata and leader election process.

```bash
KAFKA_CLUSTER_ID="$(bin/kafka-storage.sh random-uuid)"
```
🔑 This generates a random UUID for your Kafka cluster.

- Format the Kafka storage to initialize it with the KRaft configuration, using the generated UUID

```bash
bin/kafka-storage.sh format -t $KAFKA_CLUSTER_ID -c config/kraft/server.properties
```
💾 This will set up the Kafka storage directories and prepare it for KRaft mode.

- Open `config/kraft/server.properties` and configure Kafka to listen on all network interfaces and be accessible from external machines.

```text
advertised.listeners=PLAINTEXT://161.ANO.WER.145:9092
```
🗣️ Tells Kafka clients (like your consumer) to connect to the server using the external IP 161.ANO.WER.145:9092

- Open Kafka Port (9092) in the Firewall

```bash
sudo firewall-cmd --zone=public --add-port=9092/tcp --permanent
sudo firewall-cmd --reload
```
✅ This allows external servers (like your consumer) to connect to the Kafka broker on 161.ANO.WER.145:9092


- Start the Kafka server in KRaft mode using the server.properties file located in the config/kraft folder:

```bash
bin/kafka-server-start.sh config/kraft/server.properties
```
🚀 This starts Kafka in KRaft mode, and you'll see logs indicating that the server has started successfully.


### Create a Topic and Start Producer 📝

- You need to create a topic where producers will send messages. For this example, we'll create a topic called `news`

```bash
bin/kafka-topics.sh --create --topic news --bootstrap-server 161.ANO.WER.145:9092
```
🗣️ This command creates a Kafka topic called `news` that will be used by the producer to send messages.

- With the topic created, start the Kafka producer to send messages to the `news` topic:

```bash
bin/kafka-console-producer.sh --bootstrap-server 161.ANO.WER.145:9092 --topic news
```
🖋️ This starts the Kafka producer, and you'll see a prompt (`>`) to type your messages.

- Once the producer is running, type your messages one by one, pressing Enter after each.

```text
Hello, Kafka!
This is a test message.
Kafka in KRaft mode is working!
```
💬 Each message will be sent to the `news` topic on the Kafka server.

### Run the consumer code from another server

- To run, the consumer code you'll need to download the Kafka archive from the official Apache site. This command fetches Kafka version 3.8.0

```bash
wget https://downloads.apache.org/kafka/3.8.0/kafka_2.13-3.8.0.tgz
```
📂 This will download the Kafka archive as a .tgz file to your current directory.

- Next, extract the downloaded Kafka tarball to prepare it for use. 

```bash
tar -xzf kafka_2.13-3.8.0.tgz
```
📂 This will extract the Kafka files into a folder named kafka_2.13-3.8.0.

- Change into the Kafka directory where the Kafka files are located.

```bash
cd kafka_2.13-3.8.0
```
🏠 This takes you into the extracted Kafka directory to perform configurations.

- Run the consumer code from another server with this command to consume the data which are comming in the `news` topic.

```bash
bin/kafka-console-consumer.sh --bootstrap-server 161.97.ANO.WER:9092 --topic news --from-beginning
```
💬 Now you will see data pushing in server 161.97.ANO.WER:9092 in `news` topic are consuming in 75.ANO.WER.143


