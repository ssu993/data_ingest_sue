# Exercise 1

## 1. Create a new flume configuration file with the following
- Source
Type  | Netcat
Bind  | localhost
Port  | 44444
Channel
- Type : Memory
- Capacity : 1000
- transactionCapacity : 100
Sink
- Type : logger

- Flume Configuration File
<pre><code>[training@localhost ~]$ cat $DEVSH/exercises/flume/netcat.conf# netcat.conf

# Name the components on this agent
agent1.sources = Netcat
agent1.sinks = LoggerSink
agent1.channels = MemChannel

# Describe/configure the source
agent1.sources.Netcat.spoolDir = /flume/weblogs_spooldir
agent1.sources.Netcat.type = Netcat
agent1.sources.Netcat.bind = localhost
agent1.sources.Netcat.port = 44444
agent1.sources.Netcat.channels = MemChannel

# Describe the sink
agent1.sinks.LoggerSink.type = logger
agent1.sinks.LoggerSink.hdfs.path = /loudacre/weblogs_flume/
agent1.sinks.LoggerSink.channel = MemChannel
agent1.sinks.LoggerSink.hdfs.rollInterval = 0
agent1.sinks.LoggerSink.hdfs.rollSize = 524288
agent1.sinks.LoggerSink.hdfs.rollCount = 0
agent1.sinks.LoggerSink.hdfs.fileType = DataStream

# Use a channel which buffers events in memory
agent1.channels.MemChannel.type = memory
agent1.channels.MemChannel.capacity = 1000
agent1.channels.MemChannel.transactionCapacity = 100
</code></pre>

## 2. Start the agent
<pre><code>flume-ng agent \
--conf /etc/flume-ng/conf \
--conf-file $DEVSH/exercises/flume/netcat.conf \
--name agent1 -Dflume.root.logger=INFO,console</pre></code>

## 3. Test
From another terminal start telnet and connect to port 4444. Start typing and you should see the
results from the other terminal. Provide a screenshot of your results.

![screenshot_20171221-151714](https://github.com/ssu993/data_ingest_sue/blob/master/Flume/flume_result.PNG?raw=true)
