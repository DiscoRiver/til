# Create topic
/usr/local/kafka/bin/kafka-topics.sh --bootstrap-server localhost:9092 --create --replication-factor 1 --partitions 1 --topic test
Created topic "test".

# Describe Topic
/usr/local/kafka/bin/kafka-topics.sh --bootstrap-server localhost:9092 --describe --topic test

# Produce message to a topic
/usr/local/kafka/bin/kafka-console-producer.sh --bootstrap-server localhost:9092 --topic test

# Consume messages from a topic
/usr/local/kafka/bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test --from-beginning