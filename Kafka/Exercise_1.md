# Exercise 1

## 1. Create a kafka topic
- weblogs topic 생성
<pre><code>[training@localhost test.har]$ kafka-topics --create \
>--zookeeper localhost:2181 \
>--replication-factor 1 \
>--partitions 1 \
>--topic weblogs
Created topic "weblogs".</pre></code>

- topic list 확인
<pre><code>[training@localhost test.har]$ kafka-topics --list \
>--zookeeper localhost:2181
weblogs</pre></code>

- weblogs topic 정보 확인
<pre><code>[training@localhost test.har]$ kafka-topics --describe weblogs \
> --zookeeper localhost:2181
Topic:weblogs	PartitionCount:1	ReplicationFactor:1	Configs:
	Topic: weblogs	Partition: 0	Leader: 0	Replicas: 0	Isr: 0</pre></code>

![screenshot_20171221-151714](https://github.com/ssu993/data_ingest_sue/blob/master/Kafka/kafka1.PNG?raw=true)

## 2. Producing and Consuming Messages
- Kafka producer 시작
<pre><code>[training@localhost test.har]$ kafka-console-producer \
> --broker-list localhost:9092 \
> --topic weblogs</pre></code>

- producer에 'test weblog entry 1' 입력

- Kafka consumer 시작
(from-beginning 옵션을 추가했으므로 producer에 consumer를 시작하기 전에 입력했더라도 전부 출력됨)
<pre><code>[training@localhost ~]$ kafka-console-consumer \
> --zookeeper localhost:2181 \
> --topic weblogs \
> --from-beginning
test weblog entry 1
</pre></code>

![screenshot_20171221-151714](https://github.com/ssu993/data_ingest_sue/blob/master/Kafka/kafka2.PNG?raw=true)
