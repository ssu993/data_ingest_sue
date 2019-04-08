# Exercise 2
This time save the same in parquet format with snappy compression. Save it in /loudacre/accounts/user_compressed. 
Provide.a screenshot of HUE with the new directory created.

## 1. Import Data to HDFS
- Command
<pre><code>sqoop import \
--connect jdbc:mysql://localhost/loudacre \
--username training --password training \
--table accounts \
--target-dir /loudacre/accounts/user_compressed \
--columns "acct_num,first_name,last_name" \
--fields-terminated-by "\t" \
--compression-codec \
org.apache.hadoop.io.compress.SnappyCodec \
--as-parquetfile</pre></code>

<pre><code>19/04/07 22:42:44 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.7.0
19/04/07 22:42:44 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
19/04/07 22:42:44 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
19/04/07 22:42:44 INFO tool.CodeGenTool: Beginning code generation
19/04/07 22:42:44 INFO tool.CodeGenTool: Will generate java class as codegen_accounts
19/04/07 22:42:44 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `accounts` AS t LIMIT 1
19/04/07 22:42:44 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `accounts` AS t LIMIT 1
19/04/07 22:42:45 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-training/compile/c1a634bc50a0e6418e7fdfab5580a2fd/codegen_accounts.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
19/04/07 22:42:47 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-training/compile/c1a634bc50a0e6418e7fdfab5580a2fd/codegen_accounts.jar
19/04/07 22:42:47 WARN manager.MySQLManager: It looks like you are importing from mysql.
19/04/07 22:42:47 WARN manager.MySQLManager: This transfer can be faster! Use the --direct
19/04/07 22:42:47 WARN manager.MySQLManager: option to exercise a MySQL-specific fast path.
19/04/07 22:42:47 INFO manager.MySQLManager: Setting zero DATETIME behavior to convertToNull (mysql)
19/04/07 22:42:47 INFO mapreduce.ImportJobBase: Beginning import of accounts
19/04/07 22:42:47 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
19/04/07 22:42:47 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
19/04/07 22:42:48 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `accounts` AS t LIMIT 1
19/04/07 22:42:48 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `accounts` AS t LIMIT 1
19/04/07 22:42:49 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
19/04/07 22:42:50 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
19/04/07 22:42:52 INFO db.DBInputFormat: Using read commited transaction isolation
19/04/07 22:42:52 INFO db.DataDrivenDBInputFormat: BoundingValsQuery: SELECT MIN(`acct_num`), MAX(`acct_num`) FROM `accounts`
19/04/07 22:42:52 INFO db.IntegerSplitter: Split size: 32440; Num splits: 4 from: 1 to: 129761
19/04/07 22:42:52 INFO mapreduce.JobSubmitter: number of splits:4
19/04/07 22:42:52 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1554697675014_0004
19/04/07 22:42:52 INFO impl.YarnClientImpl: Submitted application application_1554697675014_0004
19/04/07 22:42:52 INFO mapreduce.Job: The url to track the job: http://localhost:8088/proxy/application_1554697675014_0004/
19/04/07 22:42:52 INFO mapreduce.Job: Running job: job_1554697675014_0004
19/04/07 22:43:01 INFO mapreduce.Job: Job job_1554697675014_0004 running in uber mode : false
19/04/07 22:43:01 INFO mapreduce.Job:  map 0% reduce 0%
19/04/07 22:43:09 INFO mapreduce.Job:  map 25% reduce 0%
19/04/07 22:43:16 INFO mapreduce.Job:  map 50% reduce 0%
19/04/07 22:43:24 INFO mapreduce.Job:  map 75% reduce 0%
19/04/07 22:43:31 INFO mapreduce.Job:  map 100% reduce 0%
19/04/07 22:43:32 INFO mapreduce.Job: Job job_1554697675014_0004 completed successfully
19/04/07 22:43:32 INFO mapreduce.Job: Counters: 30
	File System Counters
		FILE: Number of bytes read=0
		FILE: Number of bytes written=565908
		FILE: Number of read operations=0
		FILE: Number of large read operations=0
		FILE: Number of write operations=0
		HDFS: Number of bytes read=25234
		HDFS: Number of bytes written=1305047
		HDFS: Number of read operations=272
		HDFS: Number of large read operations=0
		HDFS: Number of write operations=40
	Job Counters 
		Launched map tasks=4
		Other local map tasks=4
		Total time spent by all maps in occupied slots (ms)=0
		Total time spent by all reduces in occupied slots (ms)=0
		Total time spent by all map tasks (ms)=25445
		Total vcore-seconds taken by all map tasks=25445
		Total megabyte-seconds taken by all map tasks=6513920
	Map-Reduce Framework
		Map input records=129761
		Map output records=129761
		Input split bytes=470
		Spilled Records=0
		Failed Shuffles=0
		Merged Map outputs=0
		GC time elapsed (ms)=522
		CPU time spent (ms)=8720
		Physical memory (bytes) snapshot=672428032
		Virtual memory (bytes) snapshot=8296480768
		Total committed heap usage (bytes)=251920384
	File Input Format Counters 
		Bytes Read=0
	File Output Format Counters 
		Bytes Written=0
19/04/07 22:43:32 INFO mapreduce.ImportJobBase: Transferred 1.2446 MB in 42.9149 seconds (29.6974 KB/sec)
19/04/07 22:43:32 INFO mapreduce.ImportJobBase: Retrieved 129761 records.</pre></code>

## 2. Check Data in HDFS
- HDFS에 생성된 user_compressed 폴더 리스트 확인 
<pre><code>[training@localhost ~]$ hdfs dfs -ls /loudacre/accounts/user_compressed
Found 6 items
drwxrwxrwx   - training supergroup          0 2019-04-07 22:42 /loudacre/accounts/user_compressed/.metadata
drwxrwxrwx   - training supergroup          0 2019-04-07 22:43 /loudacre/accounts/user_compressed/.signals
-rw-rw-rw-   1 training supergroup     324989 2019-04-07 22:43 /loudacre/accounts/user_compressed/280e8c03-f46d-4361-b4bb-8773ab47c5ce.parquet
-rw-rw-rw-   1 training supergroup     324753 2019-04-07 22:43 /loudacre/accounts/user_compressed/4bfa7272-ad6f-4011-a1d5-47e7936ddfb5.parquet
-rw-rw-rw-   1 training supergroup     325200 2019-04-07 22:43 /loudacre/accounts/user_compressed/ce3f2a38-9f70-4aee-be43-11411834d5e8.parquet
-rw-rw-rw-   1 training supergroup     324481 2019-04-07 22:43 /loudacre/accounts/user_compressed/e2a193b3-c760-45c7-835a-01adea536011.parquet</pre></code>

- 데이터 확인
<pre><code>[training@localhost ~]$ parquet-tools head \
> hdfs://localhost/loudacre/accounts/user_compressed/
acct_num = 1
first_name = Donald
last_name = Becton

acct_num = 2
first_name = Donna
last_name = Jones

acct_num = 3
first_name = Dorthy
last_name = Chalmers

acct_num = 4
first_name = Leila
last_name = Spencer

acct_num = 5
first_name = Anita
last_name = Laughlin</pre></code>

![screenshot_20171221-151714](https://github.com/ssu993/data_ingest_sue/blob/master/Sqooq/Exercise2.PNG?raw=true)
