# Message Encryption

Although Kafka provides multiple authentication methods and encrypted communication over [TLS](https://en.wikipedia.org/wiki/Transport\_Layer\_Security), it does not encrypt data at rest. Yet Kafka is increasingly used as a store of data, not just as a means of transferring data from one location to another. This means that Kafka must conform to the same security and compliance requirements as conventional data storage systems such as relational databases. To compensate, Kafka service providers often use disk or file system encryption. This approach provides a degree of risk mitigation but also has well-known shortcomings, most notably the ability of anyone with appropriate file system permissions, such as system administrators, to read data. Security best practices and compliance standards therefore explicitly call for application- or database-level encryption to protect sensitive information against such exposures. Further, there should be a separation of duty between key management and system administration.

BigConnect Core aims to deliver topic encryption by intercepting the message stream between client and broker in a proxy.

The architecture has the advantage that neither broker nor client need to be modified in order to provide encryption at rest. Incoming data messages ([Produce requests](https://cwiki.apache.org/confluence/display/KAFKA/A%2BGuide%2BTo%2BThe%2BKafka%2BProtocol#AGuideToTheKafkaProtocol-ProduceRequest)) are inspected to determine whether their payload should be encrypted according to a policy. If so, the data portions are encrypted and the modified message is forwarded to the broker. The topic data is then stored by the broker in encrypted form. In the reverse direction, encrypted responses (i.e., [responses to Fetch requests](https://cwiki.apache.org/confluence/display/KAFKA/A%2BGuide%2BTo%2BThe%2BKafka%2BProtocol#AGuideToTheKafkaProtocol-FetchResponse)) are decrypted prior to being sent to clients.

The diagram below depicts the main components, illustrating clients sending and receiving plaintext messages while the proxy exchanges ciphertext messages with the broker:

<figure><img src="../../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

One core component, the _Encryption Module_, provides the encryption functionality. A second core component, the _Proxy_, intercepts Kafka connections and delegates message processing to the Encryption Module.

Topics can be encrypted by different keys, allowing brokers to store a mix of encrypted and unencrypted data, with data owners managing the keys to their topics. Keys will be stored in an external key management system with access policies and logging.

### 1. Configure the Kafka broker's listeners

The address advertised by Kafka must be that of the proxy, not the broker itself.

Modify the `advertised.listeners` property in `$KAFKA_HOME/config/server.properties` to point to the proxy host and port, as shown in the snippet below:

```
# The address the socket server listens on. It will get the value returned from 
# java.net.InetAddress.getCanonicalHostName() if not configured.
#   FORMAT:
#     listeners = listener_name://host_name:port
#   EXAMPLE:
#     listeners = PLAINTEXT://your.host.name:9092
listeners=PLAINTEXT://:9092

# Hostname and port the broker will advertise to producers and consumers. If not set, 
# it uses the value for "listeners" if configured.  Otherwise, it will use the value
# returned from java.net.InetAddress.getCanonicalHostName().
advertised.listeners=PLAINTEXT://127.0.0.1:1234
```

Stop the Kafka broker and start it after the proxy is running.

### 2. Configure the proxy

Create a configuration file, `config.json` and add the following JSON contents:

```
{
  "listening_port" : 1234,
  "kafka_broker"   : "localhost:9092",
  "policy_repo"    : "test"
}
```

### 3. Start Kafka broker

Start the Kafka broker, for example:

```
$KAFKA_HOME/bin/kafka-server-start.sh config/server.properties 
```

### 4. Run Kafka clients

Start the Kafka console producer (note the proxy address in the broker list):

```
$KAFKA_HOME/bin/kafka-console-producer.sh  --broker-list localhost:1234 --topic enctest --producer.config config/producer.properties 
```

Start the Kafka console consumer, like the producer, specifying the proxy host and port:

```
$KAFKA_HOME/bin/kafka-console-consumer.sh --bootstrap-server localhost:1234 --consumer.config config/consumer.properties --topic enctest  --from-beginning
```

Enter arbitry data in the producer and verify that it appears in consumer.

Inspect the topic segment files and verify they indeed are encrypted.

```
$KAFKA_HOME/kafka-dump-log.sh --files /tmp/kafka-logs/enctest-0/00000000000000000000.log --value-decoder-class kafka.serializer.StringDecoder
```

