# Exercise 1

## 1. Configure a Flume Agent with a Kafka Sink
- Kafka sink를 활용하는 Flume agent의 configuration 파일을 아래와 같이 설정.
<pre><code># spooldir_kafka.conf: A Spooling Directory Source with a Kafka Sink

# Name the components on this agent
agent1.sources = webserver-log-source
agent1.sinks = kafka-sink
agent1.channels = memory-channel

# Configure the source
agent1.sources.webserver-log-source.type = spooldir
agent1.sources.webserver-log-source.spoolDir = /flume/weblogs_spooldir
agent1.sources.webserver-log-source.channels = memory-channel

# Configure the sink
agent1.sinks.kafka-sink.type = org.apache.flume.sink.kafka.KafkaSink
agent1.sinks.kafka-sink.topic = weblogs
agent1.sinks.kafka-sink.brokerList = localhost:9092
agent1.sinks.kafka-sink.batchSize = 20
agent1.sinks.kafka-sink.channel = memory-channel


# Use a channel which buffers events in memory
agent1.channels.memory-channel.type = memory
agent1.channels.memory-channel.capacity = 100000
agent1.channels.memory-channel.transactionCapacity = 1000</pre></code>

## 2. Run the Flume Agent
- flume agent 실행
<pre><code>flume-ng agent --conf /etc/flume-ng/conf \
--conf-file $DEVSH/exercises/flafka/spooldir_kafka.conf \
--name agent1 -Dflume.root.logger=INFO,console</pre></code>

## 3. Test the Flume Agent Kafka Sink
- 새로운 터미널에서 kafka consumer 실행
<pre><code>kafka-console-consumer \
--zookeeper localhost:2181 \
--topic weblogs</pre></code>

- weblogs copy 하여 결과 확인
<pre><code>$DEVSH/flume/copy-move-weblogs.sh \
/flume/weblogs_spooldir</pre></code>

![screenshot_20171221-151714](https://github.com/ssu993/data_ingest_sue/blob/master/Flafka/flafka_result.PNG?raw=true)

