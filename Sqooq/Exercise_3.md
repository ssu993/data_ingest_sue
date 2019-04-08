# Exercise 3
Finally save in /loudacre/accounts/CA only clients whose state is from California. Save the file in avro format and compressed using snappy. From the terminal, display some of the records
that you just imported. Take a screenshot and save it as CA_only.

## 1. State 칼럼의 Value 형식 확인
- Command
<pre><code>sqoop eval \
--connect jdbc:mysql://localhost/loudacre \
--username training --password training \
--query "select distinct(state) from accounts"</pre></code>

- Result
<pre><code>19/04/07 23:01:30 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.7.0
19/04/07 23:01:30 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
19/04/07 23:01:30 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
------------------------
| state                | 
------------------------
| CA                   | 
| OR                   | 
| NV                   | 
| AZ                   | 
------------------------</pre></code>

## 2. Import Data to HDFS
- Command
<pre><code>sqoop import \
--connect jdbc:mysql://localhost/loudacre \
--username training --password training \
--table accounts \
--where "state='CA'" \
--columns "acct_num,first_name,last_name" \
--target-dir /loudacre/accounts/CA \
--fields-terminated-by "\t" \
--compression-codec \
org.apache.hadoop.io.compress.SnappyCodec \
--as-avrodatafile</pre></code>

<pre><code>19/04/07 23:12:25 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.7.0
19/04/07 23:12:25 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
19/04/07 23:12:25 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
19/04/07 23:12:25 INFO tool.CodeGenTool: Beginning code generation
19/04/07 23:12:25 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `accounts` AS t LIMIT 1
19/04/07 23:12:25 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `accounts` AS t LIMIT 1
19/04/07 23:12:25 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-training/compile/88630b528fadc0dd0b87550f4bc72a49/accounts.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
19/04/07 23:12:28 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-training/compile/88630b528fadc0dd0b87550f4bc72a49/accounts.jar
19/04/07 23:12:28 WARN manager.MySQLManager: It looks like you are importing from mysql.
19/04/07 23:12:28 WARN manager.MySQLManager: This transfer can be faster! Use the --direct
19/04/07 23:12:28 WARN manager.MySQLManager: option to exercise a MySQL-specific fast path.
19/04/07 23:12:28 INFO manager.MySQLManager: Setting zero DATETIME behavior to convertToNull (mysql)
19/04/07 23:12:28 INFO mapreduce.ImportJobBase: Beginning import of accounts
19/04/07 23:12:28 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
19/04/07 23:12:28 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
19/04/07 23:12:29 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `accounts` AS t LIMIT 1
19/04/07 23:12:29 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `accounts` AS t LIMIT 1
19/04/07 23:12:29 INFO mapreduce.DataDrivenImportJob: Writing Avro schema file: /tmp/sqoop-training/compile/88630b528fadc0dd0b87550f4bc72a49/accounts.avsc
19/04/07 23:12:29 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
19/04/07 23:12:29 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
19/04/07 23:12:31 INFO db.DBInputFormat: Using read commited transaction isolation
19/04/07 23:12:31 INFO db.DataDrivenDBInputFormat: BoundingValsQuery: SELECT MIN(`acct_num`), MAX(`acct_num`) FROM `accounts` WHERE ( state='CA' )
19/04/07 23:12:31 INFO db.IntegerSplitter: Split size: 32439; Num splits: 4 from: 1 to: 129760
19/04/07 23:12:31 INFO mapreduce.JobSubmitter: number of splits:4
19/04/07 23:12:31 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1554697675014_0006
19/04/07 23:12:32 INFO impl.YarnClientImpl: Submitted application application_1554697675014_0006
19/04/07 23:12:32 INFO mapreduce.Job: The url to track the job: http://localhost:8088/proxy/application_1554697675014_0006/
19/04/07 23:12:32 INFO mapreduce.Job: Running job: job_1554697675014_0006
19/04/07 23:12:40 INFO mapreduce.Job: Job job_1554697675014_0006 running in uber mode : false
19/04/07 23:12:40 INFO mapreduce.Job:  map 0% reduce 0%
19/04/07 23:12:47 INFO mapreduce.Job:  map 25% reduce 0%
19/04/07 23:12:54 INFO mapreduce.Job:  map 50% reduce 0%
19/04/07 23:13:01 INFO mapreduce.Job:  map 75% reduce 0%
19/04/07 23:13:07 INFO mapreduce.Job:  map 100% reduce 0%
19/04/07 23:13:07 INFO mapreduce.Job: Job job_1554697675014_0006 completed successfully
19/04/07 23:13:08 INFO mapreduce.Job: Counters: 30
	File System Counters
		FILE: Number of bytes read=0
		FILE: Number of bytes written=563552
		FILE: Number of read operations=0
		FILE: Number of large read operations=0
		FILE: Number of write operations=0
		HDFS: Number of bytes read=470
		HDFS: Number of bytes written=1350990
		HDFS: Number of read operations=16
		HDFS: Number of large read operations=0
		HDFS: Number of write operations=8
	Job Counters 
		Launched map tasks=4
		Other local map tasks=4
		Total time spent by all maps in occupied slots (ms)=0
		Total time spent by all reduces in occupied slots (ms)=0
		Total time spent by all map tasks (ms)=20123
		Total vcore-seconds taken by all map tasks=20123
		Total megabyte-seconds taken by all map tasks=5151488
	Map-Reduce Framework
		Map input records=92416
		Map output records=92416
		Input split bytes=470
		Spilled Records=0
		Failed Shuffles=0
		Merged Map outputs=0
		GC time elapsed (ms)=328
		CPU time spent (ms)=6810
		Physical memory (bytes) snapshot=613507072
		Virtual memory (bytes) snapshot=8286130176
		Total committed heap usage (bytes)=251920384
	File Input Format Counters 
		Bytes Read=0
	File Output Format Counters 
		Bytes Written=1350990
19/04/07 23:13:08 INFO mapreduce.ImportJobBase: Transferred 1.2884 MB in 38.4308 seconds (34.3299 KB/sec)
19/04/07 23:13:08 INFO mapreduce.ImportJobBase: Retrieved 92416 records.</pre></code>

## 3. Check Data in HDFS
- HDFS에 생성된 CA 폴더 리스트 확인 
<pre><code>[training@localhost ~]$ hdfs dfs -ls /loudacre/accounts/CA
Found 5 items
-rw-rw-rw-   1 training supergroup          0 2019-04-07 23:13 /loudacre/accounts/CA/_SUCCESS
-rw-rw-rw-   1 training supergroup     382757 2019-04-07 23:12 /loudacre/accounts/CA/part-m-00000.avro
-rw-rw-rw-   1 training supergroup     322873 2019-04-07 23:12 /loudacre/accounts/CA/part-m-00001.avro
-rw-rw-rw-   1 training supergroup     323254 2019-04-07 23:12 /loudacre/accounts/CA/part-m-00002.avro
-rw-rw-rw-   1 training supergroup     322106 2019-04-07 23:13 /loudacre/accounts/CA/part-m-00003.avro</pre></code>

- 데이터 확인
<pre><code>[training@localhost ~]$ avro-tools tojson hdfs://localhost/loudacre/accounts/CA/part-m-00000.avro | tail
log4j:WARN No appenders could be found for logger (org.apache.hadoop.metrics2.lib.MutableMetricsFactory).
log4j:WARN Please initialize the log4j system properly.
log4j:WARN See http://logging.apache.org/log4j/1.2/faq.html#noconfig for more info.
{"acct_num":{"int":32426},"first_name":{"string":"Dorothy"},"last_name":{"string":"Thompson"}}
{"acct_num":{"int":32428},"first_name":{"string":"Philip"},"last_name":{"string":"Gamble"}}
{"acct_num":{"int":32429},"first_name":{"string":"Marvin"},"last_name":{"string":"Potts"}}
{"acct_num":{"int":32431},"first_name":{"string":"Judith"},"last_name":{"string":"Lindgren"}}
{"acct_num":{"int":32432},"first_name":{"string":"Darlene"},"last_name":{"string":"Gonzales"}}
{"acct_num":{"int":32435},"first_name":{"string":"Marjorie"},"last_name":{"string":"Morrison"}}
{"acct_num":{"int":32436},"first_name":{"string":"Helen"},"last_name":{"string":"Perez"}}
{"acct_num":{"int":32438},"first_name":{"string":"Violet"},"last_name":{"string":"Searcy"}}</pre></code>

![screenshot_20171221-151714](https://github.com/ssu993/data_ingest_sue/blob/master/Sqooq/Exercise3.PNG?raw=true)
