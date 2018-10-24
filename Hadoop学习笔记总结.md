
# 引言
Hadoop分布式文件系统(HDFS)被设计成适合运行在通用硬件(commodity hardware)上的分布式文件系统。
它和现有的分布式文件系统有很多共同点。但同时，它和其他的分布式文件系统的区别也是很明显的。
HDFS是一个高度容错性的系统，适合部署在廉价的机器上。HDFS能提供高吞吐量的数据访问，非常适合大规模数据集上的应用。
HDFS放宽了一部分POSIX约束，来实现流式读取文件系统数据的目的。HDFS在最开始是作为Apache Nutch搜索引擎项目的基础架构而开发的。
HDFS是Apache Hadoop Core项目的一部分。这个项目的地址是http://hadoop.apache.org/core/。




# Namenode 和 Datanode
HDFS采用master/slave架构。一个HDFS集群是由一个Namenode和一定数目的Datanodes组成。
Namenode是一个中心服务器，负责管理文件系统的名字空间(namespace)以及客户端对文件的访问。
集群中的Datanode一般是一个节点一个，负责管理它所在节点上的存储。HDFS暴露了文件系统的名字空间，
用户能够以文件的形式在上面存储数据。从内部看，一个文件其实被分成一个或多个数据块，这些块存储在
一组Datanode上。Namenode执行文件系统的名字空间操作，比如打开、关闭、重命名文件或目录。它也负责
确定数据块到具体Datanode节点的映射。Datanode负责处理文件系统客户端的读写请求。在Namenode的统一
调度下进行数据块的创建、删除和复制。

Namenode和Datanode被设计成可以在普通的商用机器上运行。这些机器一般运行着GNU/Linux操作系统(OS)。
HDFS采用Java语言开发，因此任何支持Java的机器都可以部署Namenode或Datanode。由于采用了可移植性极
强的Java语言，使得HDFS可以部署到多种类型的机器上。一个典型的部署场景是一台机器上只运行一个Namenode实例，
而集群中的其它机器分别运行一个Datanode实例。这种架构并不排斥在一台机器上运行多个Datanode，
只不过这样的情况比较少见。

集群中单一Namenode的结构大大简化了系统的架构。Namenode是所有HDFS元数据的仲裁者和管理者，
这样，用户数据永远不会流过Namenode。





Hadoop部署模式有：本地模式、伪分布模式、完全分布式模式、HA完全分布式模式。

区分的依据是NameNode、DataNode、ResourceManager、NodeManager等模块运行在几个JVM进程、几个机器。

# 本地模式部署

本地模式是最简单的模式，所有模块都运行与一个JVM进程中，使用的本地文件系统，而不是HDFS，本地模式主要是用于本地开发过程中的运行调试用。下载hadoop安装包后不用任何设置，默认的就是本地模式。

运行MapReduce程序，验证

1、 准备mapreduce输入文件wc.input
```bash

$ cat $HOME/wc.input
hadoop mapreduce hive
hbase spark storm
sqoop hadoop hive
spark hadoop
```

2、 运行hadoop自带的mapreduce Demo
```bash
[hadoop@localhost ~]$ hadoop jar $HADOOP_HOME/share/hadoop/mapreduce/hadoop-mapreduce-examples-2.9.1.jar wordcount wc.input output2
18/10/22 12:39:26 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
18/10/22 12:39:26 INFO Configuration.deprecation: session.id is deprecated. Instead, use dfs.metrics.session-id
18/10/22 12:39:26 INFO jvm.JvmMetrics: Initializing JVM Metrics with processName=JobTracker, sessionId=
18/10/22 12:39:26 INFO input.FileInputFormat: Total input files to process : 1
18/10/22 12:39:26 INFO mapreduce.JobSubmitter: number of splits:1
18/10/22 12:39:27 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_local841462251_0001
18/10/22 12:39:27 INFO mapreduce.Job: The url to track the job: http://localhost:8080/
18/10/22 12:39:27 INFO mapreduce.Job: Running job: job_local841462251_0001
18/10/22 12:39:27 INFO mapred.LocalJobRunner: OutputCommitter set in config null
18/10/22 12:39:27 INFO output.FileOutputCommitter: File Output Committer Algorithm version is 1
18/10/22 12:39:27 INFO output.FileOutputCommitter: FileOutputCommitter skip cleanup _temporary folders under output directory:false, ignore cleanup failures: false
18/10/22 12:39:27 INFO mapred.LocalJobRunner: OutputCommitter is org.apache.hadoop.mapreduce.lib.output.FileOutputCommitter
18/10/22 12:39:27 INFO mapred.LocalJobRunner: Waiting for map tasks
18/10/22 12:39:27 INFO mapred.LocalJobRunner: Starting task: attempt_local841462251_0001_m_000000_0
18/10/22 12:39:27 INFO output.FileOutputCommitter: File Output Committer Algorithm version is 1
18/10/22 12:39:27 INFO output.FileOutputCommitter: FileOutputCommitter skip cleanup _temporary folders under output directory:false, ignore cleanup failures: false
18/10/22 12:39:27 INFO mapred.Task:  Using ResourceCalculatorProcessTree : [ ]
18/10/22 12:39:27 INFO mapred.MapTask: Processing split: file:/home/hadoop/wc.input:0+71
18/10/22 12:39:27 INFO mapred.MapTask: (EQUATOR) 0 kvi 26214396(104857584)
18/10/22 12:39:27 INFO mapred.MapTask: mapreduce.task.io.sort.mb: 100
18/10/22 12:39:27 INFO mapred.MapTask: soft limit at 83886080
18/10/22 12:39:27 INFO mapred.MapTask: bufstart = 0; bufvoid = 104857600
18/10/22 12:39:27 INFO mapred.MapTask: kvstart = 26214396; length = 6553600
18/10/22 12:39:27 INFO mapred.MapTask: Map output collector class = org.apache.hadoop.mapred.MapTask$MapOutputBuffer
18/10/22 12:39:27 INFO mapred.LocalJobRunner: 
18/10/22 12:39:27 INFO mapred.MapTask: Starting flush of map output
18/10/22 12:39:27 INFO mapred.MapTask: Spilling map output
18/10/22 12:39:27 INFO mapred.MapTask: bufstart = 0; bufend = 115; bufvoid = 104857600
18/10/22 12:39:27 INFO mapred.MapTask: kvstart = 26214396(104857584); kvend = 26214356(104857424); length = 41/6553600
18/10/22 12:39:27 INFO mapred.MapTask: Finished spill 0
18/10/22 12:39:27 INFO mapred.Task: Task:attempt_local841462251_0001_m_000000_0 is done. And is in the process of committing
18/10/22 12:39:27 INFO mapred.LocalJobRunner: map
18/10/22 12:39:27 INFO mapred.Task: Task 'attempt_local841462251_0001_m_000000_0' done.
18/10/22 12:39:27 INFO mapred.LocalJobRunner: Finishing task: attempt_local841462251_0001_m_000000_0
18/10/22 12:39:27 INFO mapred.LocalJobRunner: map task executor complete.
18/10/22 12:39:27 INFO mapred.LocalJobRunner: Waiting for reduce tasks
18/10/22 12:39:27 INFO mapred.LocalJobRunner: Starting task: attempt_local841462251_0001_r_000000_0
18/10/22 12:39:27 INFO output.FileOutputCommitter: File Output Committer Algorithm version is 1
18/10/22 12:39:27 INFO output.FileOutputCommitter: FileOutputCommitter skip cleanup _temporary folders under output directory:false, ignore cleanup failures: false
18/10/22 12:39:27 INFO mapred.Task:  Using ResourceCalculatorProcessTree : [ ]
18/10/22 12:39:27 INFO mapred.ReduceTask: Using ShuffleConsumerPlugin: org.apache.hadoop.mapreduce.task.reduce.Shuffle@65deffe9
18/10/22 12:39:27 INFO reduce.MergeManagerImpl: MergerManager: memoryLimit=334338464, maxSingleShuffleLimit=83584616, mergeThreshold=220663392, ioSortFactor=10, memToMemMergeOutputsThreshold=10
18/10/22 12:39:27 INFO reduce.EventFetcher: attempt_local841462251_0001_r_000000_0 Thread started: EventFetcher for fetching Map Completion Events
18/10/22 12:39:27 INFO reduce.LocalFetcher: localfetcher#1 about to shuffle output of map attempt_local841462251_0001_m_000000_0 decomp: 90 len: 94 to MEMORY
18/10/22 12:39:27 INFO reduce.InMemoryMapOutput: Read 90 bytes from map-output for attempt_local841462251_0001_m_000000_0
18/10/22 12:39:27 INFO reduce.MergeManagerImpl: closeInMemoryFile -> map-output of size: 90, inMemoryMapOutputs.size() -> 1, commitMemory -> 0, usedMemory ->90
18/10/22 12:39:27 INFO reduce.EventFetcher: EventFetcher is interrupted.. Returning
18/10/22 12:39:27 INFO mapred.LocalJobRunner: 1 / 1 copied.
18/10/22 12:39:27 INFO reduce.MergeManagerImpl: finalMerge called with 1 in-memory map-outputs and 0 on-disk map-outputs
18/10/22 12:39:27 INFO mapred.Merger: Merging 1 sorted segments
18/10/22 12:39:27 INFO mapred.Merger: Down to the last merge-pass, with 1 segments left of total size: 81 bytes
18/10/22 12:39:27 INFO reduce.MergeManagerImpl: Merged 1 segments, 90 bytes to disk to satisfy reduce memory limit
18/10/22 12:39:27 INFO reduce.MergeManagerImpl: Merging 1 files, 94 bytes from disk
18/10/22 12:39:27 INFO reduce.MergeManagerImpl: Merging 0 segments, 0 bytes from memory into reduce
18/10/22 12:39:27 INFO mapred.Merger: Merging 1 sorted segments
18/10/22 12:39:27 INFO mapred.Merger: Down to the last merge-pass, with 1 segments left of total size: 81 bytes
18/10/22 12:39:27 INFO mapred.LocalJobRunner: 1 / 1 copied.
18/10/22 12:39:27 INFO Configuration.deprecation: mapred.skip.on is deprecated. Instead, use mapreduce.job.skiprecords
18/10/22 12:39:27 INFO mapred.Task: Task:attempt_local841462251_0001_r_000000_0 is done. And is in the process of committing
18/10/22 12:39:27 INFO mapred.LocalJobRunner: 1 / 1 copied.
18/10/22 12:39:27 INFO mapred.Task: Task attempt_local841462251_0001_r_000000_0 is allowed to commit now
18/10/22 12:39:27 INFO output.FileOutputCommitter: Saved output of task 'attempt_local841462251_0001_r_000000_0' to file:/home/hadoop/output2/_temporary/0/task_local841462251_0001_r_000000
18/10/22 12:39:27 INFO mapred.LocalJobRunner: reduce > reduce
18/10/22 12:39:27 INFO mapred.Task: Task 'attempt_local841462251_0001_r_000000_0' done.
18/10/22 12:39:27 INFO mapred.LocalJobRunner: Finishing task: attempt_local841462251_0001_r_000000_0
18/10/22 12:39:27 INFO mapred.LocalJobRunner: reduce task executor complete.
18/10/22 12:39:28 INFO mapreduce.Job: Job job_local841462251_0001 running in uber mode : false
18/10/22 12:39:28 INFO mapreduce.Job:  map 100% reduce 100%
18/10/22 12:39:28 INFO mapreduce.Job: Job job_local841462251_0001 completed successfully
18/10/22 12:39:28 INFO mapreduce.Job: Counters: 30
	File System Counters
		FILE: Number of bytes read=607284
		FILE: Number of bytes written=1537636
		FILE: Number of read operations=0
		FILE: Number of large read operations=0
		FILE: Number of write operations=0
	Map-Reduce Framework
		Map input records=4
		Map output records=11
		Map output bytes=115
		Map output materialized bytes=94
		Input split bytes=91
		Combine input records=11
		Combine output records=7
		Reduce input groups=7
		Reduce shuffle bytes=94
		Reduce input records=7
		Reduce output records=7
		Spilled Records=14
		Shuffled Maps =1
		Failed Shuffles=0
		Merged Map outputs=1
		GC time elapsed (ms)=0
		Total committed heap usage (bytes)=525336576
	Shuffle Errors
		BAD_ID=0
		CONNECTION=0
		IO_ERROR=0
		WRONG_LENGTH=0
		WRONG_MAP=0
		WRONG_REDUCE=0
	File Input Format Counters 
		Bytes Read=71
	File Output Format Counters 
		Bytes Written=72
[hadoop@localhost ~]$ 

这里可以看到job ID中有local字样，说明是运行在本地模式下的。
```

3、 查看输出文件
```bash

[hadoop@localhost ~]$ ls -alh output2/
总用量 20K
drwxrwxr-x. 2 hadoop hadoop 4.0K 10月 22 12:39 .
drwx------. 8 hadoop hadoop 4.0K 10月 22 12:39 ..
-rw-r--r--. 1 hadoop hadoop   60 10月 22 12:39 part-r-00000
-rw-r--r--. 1 hadoop hadoop   12 10月 22 12:39 .part-r-00000.crc
-rw-r--r--. 1 hadoop hadoop    0 10月 22 12:39 _SUCCESS
-rw-r--r--. 1 hadoop hadoop    8 10月 22 12:39 ._SUCCESS.crc

# 输出目录中有_SUCCESS文件说明JOB运行成功，part-r-00000是输出结果文件。 

```

# Hadoop伪分布式模式安装

1、 配置Hadoop环境变量
```bash

export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.91-2.b14.fc22.x86_64/jre

export HADOOP_HOME=/home/hadoop/hadoop-2.9.1

export PATH=$PATH:$HADOOP_HOME/bin
export PATH=$PATH:$HADOOP_HOME/sbin
```

2、 配置 hadoop-env.sh、mapred-env.sh、yarn-env.sh文件的JAVA_HOME参数
```bash


```

3、 配置core-site.xml
```bash
${HADOOP_HOME}/etc/hadoop/core-site.xml
（1） fs.defaultFS参数配置的是HDFS的地址。
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://10.0.63.48:8020</value>
    </property>
    
（2） hadoop.tmp.dir配置的是Hadoop临时目录
    <property>
        <name>hadoop.tmp.dir</name>
        <value>/home/hadoop/hadoop-2.9.1/tmp</value>
    </property>
默认的hadoop.tmp.dir是/tmp/hadoop-${user.name},此时有个问题就是NameNode会将HDFS的元数据存储在这个/tmp目录下，
如果操作系统重启了，系统会清空/tmp目录下的东西，导致NameNode元数据丢失，是个非常严重的问题，所有我们应该修改这个路径。
```

4、 配置hdfs-site.xml
```bash

dfs.replication配置的是HDFS存储时的备份数量，因为这里是伪分布式环境只有一个节点，所以这里设置为1。
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>

```
5、 格式化HDFS 
```bash
[hadoop@localhost hadoop-2.9.1]$ hdfs
Usage: hdfs [--config confdir] [--loglevel loglevel] COMMAND
       where COMMAND is one of:
  dfs                  run a filesystem command on the file systems supported in Hadoop.
  classpath            prints the classpath
  namenode -format     format the DFS filesystem
  secondarynamenode    run the DFS secondary namenode
  namenode             run the DFS namenode
  journalnode          run the DFS journalnode
  zkfc                 run the ZK Failover Controller daemon
  datanode             run a DFS datanode
  debug                run a Debug Admin to execute HDFS debug commands
  dfsadmin             run a DFS admin client
  dfsrouter            run the DFS router
  dfsrouteradmin       manage Router-based federation
  haadmin              run a DFS HA admin client
  fsck                 run a DFS filesystem checking utility
  balancer             run a cluster balancing utility
  jmxget               get JMX exported values from NameNode or DataNode.
  mover                run a utility to move block replicas across
                       storage types
  oiv                  apply the offline fsimage viewer to an fsimage
  oiv_legacy           apply the offline fsimage viewer to an legacy fsimage
  oev                  apply the offline edits viewer to an edits file
  fetchdt              fetch a delegation token from the NameNode
  getconf              get config values from configuration
  groups               get the groups which users belong to
  snapshotDiff         diff two snapshots of a directory or diff the
                       current directory contents with a snapshot
  lsSnapshottableDir   list all snapshottable dirs owned by the current user
						Use -help to see options
  portmap              run a portmap service
  nfs3                 run an NFS version 3 gateway
  cacheadmin           configure the HDFS cache
  crypto               configure HDFS encryption zones
  storagepolicies      list/get/set block storage policies
  version              print the version

Most commands print help when invoked w/o parameters.
```
```bash

[hadoop@localhost hadoop-2.9.1]$ hdfs namenode -format
18/10/22 13:40:46 INFO namenode.NameNode: STARTUP_MSG: 
/************************************************************
STARTUP_MSG: Starting NameNode
STARTUP_MSG:   host = localhost.localdomain/127.0.0.1
STARTUP_MSG:   args = [-format]
STARTUP_MSG:   version = 2.9.1
STARTUP_MSG:   classpath = /home/hadoop/hadoop-2.9.1/etc/hadoop:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/xz-1.0.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/avro-1.7.7.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/hadoop-annotations-2.9.1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/guava-11.0.2.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/commons-math3-3.1.1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/commons-collections-3.2.2.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/hadoop-auth-2.9.1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/mockito-all-1.8.5.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/commons-digester-1.8.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/java-xmlbuilder-0.4.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/jackson-core-asl-1.9.13.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/zookeeper-3.4.6.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/jackson-xc-1.9.13.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/slf4j-api-1.7.25.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/jets3t-0.9.0.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/commons-lang-2.6.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/httpclient-4.5.2.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/snappy-java-1.0.5.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/jersey-core-1.9.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/jaxb-api-2.2.2.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/commons-logging-1.1.3.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/xmlenc-0.52.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/api-util-1.0.0-M20.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/log4j-1.2.17.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/apacheds-i18n-2.0.0-M15.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/asm-3.2.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/commons-cli-1.2.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/jetty-util-6.1.26.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/httpcore-4.4.4.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/jetty-6.1.26.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/commons-beanutils-core-1.8.0.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/commons-io-2.4.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/woodstox-core-5.0.3.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/junit-4.11.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/commons-net-3.1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/protobuf-java-2.5.0.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/jettison-1.1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/curator-framework-2.7.1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/activation-1.1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/apacheds-kerberos-codec-2.0.0-M15.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/nimbus-jose-jwt-4.41.1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/paranamer-2.3.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/jetty-sslengine-6.1.26.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/commons-lang3-3.4.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/jsr305-3.0.0.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/jsp-api-2.1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/curator-recipes-2.7.1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/curator-client-2.7.1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/commons-compress-1.4.1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/jsch-0.1.54.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/jaxb-impl-2.2.3-1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/servlet-api-2.5.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/api-asn1-api-1.0.0-M20.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/commons-beanutils-1.7.0.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/commons-codec-1.4.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/jackson-jaxrs-1.9.13.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/stax2-api-3.1.4.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/json-smart-1.3.1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/hamcrest-core-1.3.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/gson-2.2.4.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/jersey-server-1.9.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/stax-api-1.0-2.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/jcip-annotations-1.0-1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/htrace-core4-4.1.0-incubating.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/jackson-mapper-asl-1.9.13.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/jersey-json-1.9.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/commons-configuration-1.6.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/netty-3.6.2.Final.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/slf4j-log4j12-1.7.25.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/hadoop-nfs-2.9.1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/hadoop-common-2.9.1-tests.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/common/hadoop-common-2.9.1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/hdfs:/home/hadoop/hadoop-2.9.1/share/hadoop/hdfs/lib/guava-11.0.2.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/hdfs/lib/okhttp-2.7.5.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/hdfs/lib/leveldbjni-all-1.8.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/hdfs/lib/jackson-core-asl-1.9.13.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/hdfs/lib/jackson-core-2.7.8.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/hdfs/lib/commons-lang-2.6.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/hdfs/lib/commons-daemon-1.0.13.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/hdfs/lib/jersey-core-1.9.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/hdfs/lib/netty-all-4.0.23.Final.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/hdfs/lib/commons-logging-1.1.3.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/hdfs/lib/okio-1.6.0.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/hdfs/lib/xmlenc-0.52.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/hdfs/lib/log4j-1.2.17.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/hdfs/lib/asm-3.2.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/hdfs/lib/commons-cli-1.2.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/hdfs/lib/jetty-util-6.1.26.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/hdfs/lib/jetty-6.1.26.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/hdfs/lib/commons-io-2.4.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/hdfs/lib/xml-apis-1.3.04.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/hdfs/lib/protobuf-java-2.5.0.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/hdfs/lib/jsr305-3.0.0.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/hdfs/lib/servlet-api-2.5.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/hdfs/lib/jackson-databind-2.7.8.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/hdfs/lib/xercesImpl-2.9.1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/hdfs/lib/commons-codec-1.4.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/hdfs/lib/jackson-annotations-2.7.8.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/hdfs/lib/hadoop-hdfs-client-2.9.1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/hdfs/lib/jersey-server-1.9.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/hdfs/lib/htrace-core4-4.1.0-incubating.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/hdfs/lib/jackson-mapper-asl-1.9.13.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/hdfs/lib/netty-3.6.2.Final.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/hdfs/hadoop-hdfs-native-client-2.9.1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/hdfs/hadoop-hdfs-2.9.1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/hdfs/hadoop-hdfs-native-client-2.9.1-tests.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/hdfs/hadoop-hdfs-rbf-2.9.1-tests.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/hdfs/hadoop-hdfs-rbf-2.9.1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/hdfs/hadoop-hdfs-2.9.1-tests.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/hdfs/hadoop-hdfs-client-2.9.1-tests.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/hdfs/hadoop-hdfs-client-2.9.1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/hdfs/hadoop-hdfs-nfs-2.9.1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/xz-1.0.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/avro-1.7.7.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/guava-11.0.2.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/commons-math3-3.1.1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/commons-collections-3.2.2.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/fst-2.50.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/guice-3.0.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/leveldbjni-all-1.8.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/commons-digester-1.8.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/java-xmlbuilder-0.4.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/mssql-jdbc-6.2.1.jre7.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/jackson-core-asl-1.9.13.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/zookeeper-3.4.6.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/jackson-xc-1.9.13.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/jersey-guice-1.9.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/jets3t-0.9.0.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/commons-lang-2.6.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/httpclient-4.5.2.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/snappy-java-1.0.5.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/jersey-core-1.9.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/jaxb-api-2.2.2.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/commons-logging-1.1.3.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/xmlenc-0.52.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/metrics-core-3.0.1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/guice-servlet-3.0.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/api-util-1.0.0-M20.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/geronimo-jcache_1.0_spec-1.0-alpha-1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/log4j-1.2.17.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/apacheds-i18n-2.0.0-M15.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/asm-3.2.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/commons-cli-1.2.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/jetty-util-6.1.26.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/httpcore-4.4.4.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/jetty-6.1.26.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/java-util-1.9.0.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/commons-beanutils-core-1.8.0.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/commons-io-2.4.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/woodstox-core-5.0.3.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/commons-net-3.1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/protobuf-java-2.5.0.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/jettison-1.1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/curator-framework-2.7.1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/HikariCP-java7-2.4.12.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/activation-1.1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/apacheds-kerberos-codec-2.0.0-M15.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/nimbus-jose-jwt-4.41.1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/paranamer-2.3.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/jetty-sslengine-6.1.26.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/commons-lang3-3.4.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/jsr305-3.0.0.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/jsp-api-2.1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/json-io-2.5.1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/curator-recipes-2.7.1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/curator-client-2.7.1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/commons-compress-1.4.1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/jsch-0.1.54.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/jaxb-impl-2.2.3-1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/servlet-api-2.5.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/api-asn1-api-1.0.0-M20.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/commons-beanutils-1.7.0.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/commons-codec-1.4.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/jackson-jaxrs-1.9.13.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/javax.inject-1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/stax2-api-3.1.4.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/json-smart-1.3.1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/gson-2.2.4.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/jersey-server-1.9.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/aopalliance-1.0.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/stax-api-1.0-2.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/ehcache-3.3.1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/jcip-annotations-1.0-1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/htrace-core4-4.1.0-incubating.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/jackson-mapper-asl-1.9.13.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/jersey-json-1.9.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/commons-configuration-1.6.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/netty-3.6.2.Final.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/jersey-client-1.9.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/hadoop-yarn-server-router-2.9.1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/hadoop-yarn-api-2.9.1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/hadoop-yarn-server-web-proxy-2.9.1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/hadoop-yarn-client-2.9.1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/hadoop-yarn-server-tests-2.9.1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/hadoop-yarn-server-sharedcachemanager-2.9.1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/hadoop-yarn-registry-2.9.1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/hadoop-yarn-applications-unmanaged-am-launcher-2.9.1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/hadoop-yarn-applications-distributedshell-2.9.1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/hadoop-yarn-server-common-2.9.1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/hadoop-yarn-common-2.9.1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/hadoop-yarn-server-nodemanager-2.9.1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/hadoop-yarn-server-resourcemanager-2.9.1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/hadoop-yarn-server-timeline-pluginstorage-2.9.1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/hadoop-yarn-server-applicationhistoryservice-2.9.1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/mapreduce/lib/xz-1.0.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/mapreduce/lib/avro-1.7.7.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/mapreduce/lib/hadoop-annotations-2.9.1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/mapreduce/lib/guice-3.0.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/mapreduce/lib/leveldbjni-all-1.8.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/mapreduce/lib/jackson-core-asl-1.9.13.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/mapreduce/lib/jersey-guice-1.9.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/mapreduce/lib/snappy-java-1.0.5.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/mapreduce/lib/jersey-core-1.9.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/mapreduce/lib/guice-servlet-3.0.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/mapreduce/lib/log4j-1.2.17.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/mapreduce/lib/asm-3.2.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/mapreduce/lib/commons-io-2.4.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/mapreduce/lib/junit-4.11.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/mapreduce/lib/protobuf-java-2.5.0.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/mapreduce/lib/paranamer-2.3.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/mapreduce/lib/commons-compress-1.4.1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/mapreduce/lib/javax.inject-1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/mapreduce/lib/hamcrest-core-1.3.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/mapreduce/lib/jersey-server-1.9.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/mapreduce/lib/aopalliance-1.0.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/mapreduce/lib/jackson-mapper-asl-1.9.13.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/mapreduce/lib/netty-3.6.2.Final.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/mapreduce/hadoop-mapreduce-client-core-2.9.1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/mapreduce/hadoop-mapreduce-client-jobclient-2.9.1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/mapreduce/hadoop-mapreduce-client-jobclient-2.9.1-tests.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/mapreduce/hadoop-mapreduce-examples-2.9.1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/mapreduce/hadoop-mapreduce-client-hs-2.9.1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/mapreduce/hadoop-mapreduce-client-hs-plugins-2.9.1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/mapreduce/hadoop-mapreduce-client-app-2.9.1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/mapreduce/hadoop-mapreduce-client-common-2.9.1.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/mapreduce/hadoop-mapreduce-client-shuffle-2.9.1.jar:/home/hadoop/hadoop-2.9.1/contrib/capacity-scheduler/*.jar
STARTUP_MSG:   build = https://github.com/apache/hadoop.git -r e30710aea4e6e55e69372929106cf119af06fd0e; compiled by 'root' on 2018-04-16T09:33Z
STARTUP_MSG:   java = 1.8.0_91
************************************************************/
18/10/22 13:40:46 INFO namenode.NameNode: registered UNIX signal handlers for [TERM, HUP, INT]
18/10/22 13:40:46 INFO namenode.NameNode: createNameNode [-format]
18/10/22 13:40:46 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
Formatting using clusterid: CID-85451f7e-c811-4028-8eee-62d3202c00bc
18/10/22 13:40:47 INFO namenode.FSEditLog: Edit logging is async:true
18/10/22 13:40:47 INFO namenode.FSNamesystem: KeyProvider: null
18/10/22 13:40:47 INFO namenode.FSNamesystem: fsLock is fair: true
18/10/22 13:40:47 INFO namenode.FSNamesystem: Detailed lock hold time metrics enabled: false
18/10/22 13:40:47 INFO namenode.FSNamesystem: fsOwner             = hadoop (auth:SIMPLE)
18/10/22 13:40:47 INFO namenode.FSNamesystem: supergroup          = supergroup
18/10/22 13:40:47 INFO namenode.FSNamesystem: isPermissionEnabled = true
18/10/22 13:40:47 INFO namenode.FSNamesystem: HA Enabled: false
18/10/22 13:40:47 INFO common.Util: dfs.datanode.fileio.profiling.sampling.percentage set to 0. Disabling file IO profiling
18/10/22 13:40:47 INFO blockmanagement.DatanodeManager: dfs.block.invalidate.limit: configured=1000, counted=60, effected=1000
18/10/22 13:40:47 INFO blockmanagement.DatanodeManager: dfs.namenode.datanode.registration.ip-hostname-check=true
18/10/22 13:40:47 INFO blockmanagement.BlockManager: dfs.namenode.startup.delay.block.deletion.sec is set to 000:00:00:00.000
18/10/22 13:40:47 INFO blockmanagement.BlockManager: The block deletion will start around 2018 十月 22 13:40:47
18/10/22 13:40:47 INFO util.GSet: Computing capacity for map BlocksMap
18/10/22 13:40:47 INFO util.GSet: VM type       = 64-bit
18/10/22 13:40:47 INFO util.GSet: 2.0% max memory 889 MB = 17.8 MB
18/10/22 13:40:47 INFO util.GSet: capacity      = 2^21 = 2097152 entries
18/10/22 13:40:47 INFO blockmanagement.BlockManager: dfs.block.access.token.enable=false
18/10/22 13:40:47 WARN conf.Configuration: No unit for dfs.heartbeat.interval(3) assuming SECONDS
18/10/22 13:40:47 WARN conf.Configuration: No unit for dfs.namenode.safemode.extension(30000) assuming MILLISECONDS
18/10/22 13:40:47 INFO blockmanagement.BlockManagerSafeMode: dfs.namenode.safemode.threshold-pct = 0.9990000128746033
18/10/22 13:40:47 INFO blockmanagement.BlockManagerSafeMode: dfs.namenode.safemode.min.datanodes = 0
18/10/22 13:40:47 INFO blockmanagement.BlockManagerSafeMode: dfs.namenode.safemode.extension = 30000
18/10/22 13:40:47 INFO blockmanagement.BlockManager: defaultReplication         = 1
18/10/22 13:40:47 INFO blockmanagement.BlockManager: maxReplication             = 512
18/10/22 13:40:47 INFO blockmanagement.BlockManager: minReplication             = 1
18/10/22 13:40:47 INFO blockmanagement.BlockManager: maxReplicationStreams      = 2
18/10/22 13:40:47 INFO blockmanagement.BlockManager: replicationRecheckInterval = 3000
18/10/22 13:40:47 INFO blockmanagement.BlockManager: encryptDataTransfer        = false
18/10/22 13:40:47 INFO blockmanagement.BlockManager: maxNumBlocksToLog          = 1000
18/10/22 13:40:47 INFO namenode.FSNamesystem: Append Enabled: true
18/10/22 13:40:47 INFO util.GSet: Computing capacity for map INodeMap
18/10/22 13:40:47 INFO util.GSet: VM type       = 64-bit
18/10/22 13:40:47 INFO util.GSet: 1.0% max memory 889 MB = 8.9 MB
18/10/22 13:40:47 INFO util.GSet: capacity      = 2^20 = 1048576 entries
18/10/22 13:40:47 INFO namenode.FSDirectory: ACLs enabled? false
18/10/22 13:40:47 INFO namenode.FSDirectory: XAttrs enabled? true
18/10/22 13:40:47 INFO namenode.NameNode: Caching file names occurring more than 10 times
18/10/22 13:40:47 INFO snapshot.SnapshotManager: Loaded config captureOpenFiles: falseskipCaptureAccessTimeOnlyChange: false
18/10/22 13:40:47 INFO util.GSet: Computing capacity for map cachedBlocks
18/10/22 13:40:47 INFO util.GSet: VM type       = 64-bit
18/10/22 13:40:47 INFO util.GSet: 0.25% max memory 889 MB = 2.2 MB
18/10/22 13:40:47 INFO util.GSet: capacity      = 2^18 = 262144 entries
18/10/22 13:40:47 INFO metrics.TopMetrics: NNTop conf: dfs.namenode.top.window.num.buckets = 10
18/10/22 13:40:47 INFO metrics.TopMetrics: NNTop conf: dfs.namenode.top.num.users = 10
18/10/22 13:40:47 INFO metrics.TopMetrics: NNTop conf: dfs.namenode.top.windows.minutes = 1,5,25
18/10/22 13:40:47 INFO namenode.FSNamesystem: Retry cache on namenode is enabled
18/10/22 13:40:47 INFO namenode.FSNamesystem: Retry cache will use 0.03 of total heap and retry cache entry expiry time is 600000 millis
18/10/22 13:40:47 INFO util.GSet: Computing capacity for map NameNodeRetryCache
18/10/22 13:40:47 INFO util.GSet: VM type       = 64-bit
18/10/22 13:40:47 INFO util.GSet: 0.029999999329447746% max memory 889 MB = 273.1 KB
18/10/22 13:40:47 INFO util.GSet: capacity      = 2^15 = 32768 entries
18/10/22 13:40:47 INFO namenode.FSImage: Allocated new BlockPoolId: BP-648750324-127.0.0.1-1540186847111
18/10/22 13:40:47 INFO common.Storage: Storage directory /home/hadoop/hadoop-2.9.1/tmp/dfs/name has been successfully formatted.
18/10/22 13:40:47 INFO namenode.FSImageFormatProtobuf: Saving image file /home/hadoop/hadoop-2.9.1/tmp/dfs/name/current/fsimage.ckpt_0000000000000000000 using no compression
18/10/22 13:40:47 INFO namenode.FSImageFormatProtobuf: Image file /home/hadoop/hadoop-2.9.1/tmp/dfs/name/current/fsimage.ckpt_0000000000000000000 of size 323 bytes saved in 0 seconds .
18/10/22 13:40:47 INFO namenode.NNStorageRetentionManager: Going to retain 1 images with txid >= 0
18/10/22 13:40:47 INFO namenode.NameNode: SHUTDOWN_MSG: 
/************************************************************
SHUTDOWN_MSG: Shutting down NameNode at localhost.localdomain/127.0.0.1
************************************************************/
[hadoop@localhost hadoop-2.9.1]$ 

```
```bash

[hadoop@localhost hadoop-2.9.1]$ tree tmp/
tmp/
└── dfs
    └── name
        └── current
            ├── fsimage_0000000000000000000
            ├── fsimage_0000000000000000000.md5
            ├── seen_txid
            └── VERSION

3 directories, 4 files
[hadoop@localhost hadoop-2.9.1]$ 



```
fsimage是NameNode元数据在内存满了后，持久化保存到的文件。

fsimage*.md5 是校验文件，用于校验fsimage的完整性。

seen_txid 是hadoop的版本

vession文件里保存：
```bash
[hadoop@localhost hadoop-2.9.1]$ cat tmp/dfs/name/current/VERSION 
#Mon Oct 22 13:40:47 CST 2018
namespaceID=1544637935
clusterID=CID-85451f7e-c811-4028-8eee-62d3202c00bc
cTime=1540186847111
storageType=NAME_NODE
blockpoolID=BP-648750324-127.0.0.1-1540186847111
layoutVersion=-63
```
6、 启动NameNode
```bash
[hadoop@localhost hadoop-2.9.1]$ hadoop-daemon.sh start namenode
starting namenode, logging to /home/hadoop/hadoop-2.9.1/logs/hadoop-hadoop-namenode-localhost.localdomain.out
```
7、 启动DataNode
```bash
[hadoop@localhost hadoop-2.9.1]$ hadoop-daemon.sh start datanode
starting datanode, logging to /home/hadoop/hadoop-2.9.1/logs/hadoop-hadoop-datanode-localhost.localdomain.out
```
8、 启动SecondaryNameNode
```bash
[hadoop@localhost hadoop-2.9.1]$ hadoop-daemon.sh start secondarynamenode
starting secondarynamenode, logging to /home/hadoop/hadoop-2.9.1/logs/hadoop-hadoop-secondarynamenode-localhost.localdomain.out
```

```bash

[hadoop@localhost hadoop-2.9.1]$ jps
7489 NameNode
8167 SecondaryNameNode
8699 Jps
8063 DataNode

  ├─java,7489 -Dproc_namenode -Xmx1000m -Djava.library.path=/home/hadoop/hadoop-2.9.1/lib -Djava.net.preferIPv4Stack=true -Dhadoop.log.dir=/home/hadoop/hadoop-2.9.1/logs -Dhadoop.log.file=hadoop.log -Dhadoop.home.dir=/home/hadoop/hadoop-2.9.1 -Dhadoop.id.str=hadoop -Dhadoop.root.logger=INFO,console -Dhadoop.policy.file=hadoop-policy.xml -Djava.net.preferIPv4Stack=true -Djava.net.preferIPv4Stack=true -Djava.net.preferIPv4Stack=true -Dhadoop.log.dir=/home/hadoop/hadoop-2.9.1/logs -Dhadoop.log.file=hadoop-hadoop-namenode-localhost.localdomain.log -Dhadoop.home.dir=/home/hadoop/hadoop-2.9.1 -Dhadoop.id.str=hadoop -Dhadoop.root.logger=INFO,RFA -Dhadoop.policy.file=hadoop-policy.xml -Djava.net.preferIPv4Stack=true -Dhadoop.security.logger=INFO,RFAS -Dhdfs.audit.logger=INFO,NullAppender -Dhadoop.security.logger=INFO,RFAS -Dhdfs.audit.logger=INFO,NullAppender -Dhadoop.security.logger=INFO,RFAS -Dhdfs.audit.logger=INFO,NullAppender -Dhadoop.security.logger=INFO,RFAS org.apache.hadoop.hdfs.server.namenode.NameNode
  
  ├─java,8063 -Dproc_datanode -Xmx1000m -Djava.library.path=/home/hadoop/hadoop-2.9.1/lib -Djava.net.preferIPv4Stack=true -Dhadoop.log.dir=/home/hadoop/hadoop-2.9.1/logs -Dhadoop.log.file=hadoop.log -Dhadoop.home.dir=/home/hadoop/hadoop-2.9.1 -Dhadoop.id.str=hadoop -Dhadoop.root.logger=INFO,console -Dhadoop.policy.file=hadoop-policy.xml -Djava.net.preferIPv4Stack=true -Djava.net.preferIPv4Stack=true -Djava.net.preferIPv4Stack=true -Dhadoop.log.dir=/home/hadoop/hadoop-2.9.1/logs -Dhadoop.log.file=hadoop-hadoop-datanode-localhost.localdomain.log -Dhadoop.home.dir=/home/hadoop/hadoop-2.9.1 -Dhadoop.id.str=hadoop -Dhadoop.root.logger=INFO,RFA -Dhadoop.policy.file=hadoop-policy.xml -Djava.net.preferIPv4Stack=true -server -Dhadoop.security.logger=ERROR,RFAS -Dhadoop.security.logger=ERROR,RFAS -Dhadoop.security.logger=ERROR,RFAS -Dhadoop.security.logger=INFO,RFAS org.apache.hadoop.hdfs.server.datanode.DataNode

  ├─java,8167 -Dproc_secondarynamenode -Xmx1000m -Djava.library.path=/home/hadoop/hadoop-2.9.1/lib -Djava.net.preferIPv4Stack=true -Dhadoop.log.dir=/home/hadoop/hadoop-2.9.1/logs -Dhadoop.log.file=hadoop.log -Dhadoop.home.dir=/home/hadoop/hadoop-2.9.1 -Dhadoop.id.str=hadoop -Dhadoop.root.logger=INFO,console -Dhadoop.policy.file=hadoop-policy.xml -Djava.net.preferIPv4Stack=true -Djava.net.preferIPv4Stack=true -Djava.net.preferIPv4Stack=true -Dhadoop.log.dir=/home/hadoop/hadoop-2.9.1/logs -Dhadoop.log.file=hadoop-hadoop-secondarynamenode-localhost.localdomain.log -Dhadoop.home.dir=/home/hadoop/hadoop-2.9.1 -Dhadoop.id.str=hadoop -Dhadoop.root.logger=INFO,RFA -Dhadoop.policy.file=hadoop-policy.xml -Djava.net.preferIPv4Stack=true -Dhadoop.security.logger=INFO,RFAS -Dhdfs.audit.logger=INFO,NullAppender -Dhadoop.security.logger=INFO,RFAS -Dhdfs.audit.logger=INFO,NullAppender -Dhadoop.security.logger=INFO,RFAS -Dhdfs.audit.logger=INFO,NullAppender -Dhadoop.security.logger=INFO,RFAS org.apache.hadoop.hdfs.server.namenode.SecondaryNameNode
  
  

```



```bash
Usage: hadoop fs [generic options]
	[-appendToFile <localsrc> ... <dst>]
	[-cat [-ignoreCrc] <src> ...]
	[-checksum <src> ...]
	[-chgrp [-R] GROUP PATH...]
	[-chmod [-R] <MODE[,MODE]... | OCTALMODE> PATH...]
	[-chown [-R] [OWNER][:[GROUP]] PATH...]
	[-copyFromLocal [-f] [-p] [-l] [-d] <localsrc> ... <dst>]
	[-copyToLocal [-f] [-p] [-ignoreCrc] [-crc] <src> ... <localdst>]
	[-count [-q] [-h] [-v] [-t [<storage type>]] [-u] [-x] <path> ...]
	[-cp [-f] [-p | -p[topax]] [-d] <src> ... <dst>]
	[-createSnapshot <snapshotDir> [<snapshotName>]]
	[-deleteSnapshot <snapshotDir> <snapshotName>]
	[-df [-h] [<path> ...]]
	[-du [-s] [-h] [-x] <path> ...]
	[-expunge]
	[-find <path> ... <expression> ...]
	[-get [-f] [-p] [-ignoreCrc] [-crc] <src> ... <localdst>]
	[-getfacl [-R] <path>]
	[-getfattr [-R] {-n name | -d} [-e en] <path>]
	[-getmerge [-nl] [-skip-empty-file] <src> <localdst>]
	[-help [cmd ...]]
	[-ls [-C] [-d] [-h] [-q] [-R] [-t] [-S] [-r] [-u] [<path> ...]]
	[-mkdir [-p] <path> ...]
	[-moveFromLocal <localsrc> ... <dst>]
	[-moveToLocal <src> <localdst>]
	[-mv <src> ... <dst>]
	[-put [-f] [-p] [-l] [-d] <localsrc> ... <dst>]
	[-renameSnapshot <snapshotDir> <oldName> <newName>]
	[-rm [-f] [-r|-R] [-skipTrash] [-safely] <src> ...]
	[-rmdir [--ignore-fail-on-non-empty] <dir> ...]
	[-setfacl [-R] [{-b|-k} {-m|-x <acl_spec>} <path>]|[--set <acl_spec> <path>]]
	[-setfattr {-n name [-v value] | -x name} <path>]
	[-setrep [-R] [-w] <rep> <path> ...]
	[-stat [format] <path> ...]
	[-tail [-f] <file>]
	[-test -[defsz] <path>]
	[-text [-ignoreCrc] <src> ...]
	[-touchz <path> ...]
	[-truncate [-w] <length> <path> ...]
	[-usage [cmd ...]]

Generic options supported are:
-conf <configuration file>        specify an application configuration file
-D <property=value>               define a value for a given property
-fs <file:///|hdfs://namenode:port> specify default filesystem URL to use, overrides 'fs.defaultFS' property from configurations.
-jt <local|resourcemanager:port>  specify a ResourceManager
-files <file1,...>                specify a comma-separated list of files to be copied to the map reduce cluster
-libjars <jar1,...>               specify a comma-separated list of jar files to be included in the classpath
-archives <archive1,...>          specify a comma-separated list of archives to be unarchived on the compute machines

The general command line syntax is:
command [genericOptions] [commandOptions]

Usage: hadoop fs [generic options] -ls [-C] [-d] [-h] [-q] [-R] [-t] [-S] [-r] [-u] [<path> ...]
[hadoop@localhost ~]$ 
```

9、 HDFS上测试创建目录、上传、下载文件
```bash
# 创建一个目录，
[hadoop@localhost ~]$ hdfs dfs -mkdir /demo1

# 上传本地文件到HDFS上
[hadoop@localhost ~]$ hdfs dfs -put hadoop-2.9.1.tar.gz  /demo1

#读取HDFS上的文件内容
[hadoop@localhost ~]$ hdfs dfs -put hadoop-2.9.1/bin/hadoop /demo1
[hadoop@localhost ~]$ hdfs dfs -ls -R /
drwxr-xr-x   - hadoop supergroup          0 2018-10-22 18:15 /demo1
-rw-r--r--   1 hadoop supergroup       6656 2018-10-22 18:15 /demo1/hadoop
-rw-r--r--   1 hadoop supergroup  361355307 2018-10-22 18:06 /demo1/hadoop-2.9.1.tar.gz
#读取HDFS上的文件内容
[hadoop@localhost ~]$ hdfs dfs -cat /demo1/hadoop

# 从HDFS上下载文件到本地
[hadoop@localhost ~]$ hdfs dfs -get /demo1/hadoop
```









## 创建hadoop系统账号
```bash
# 这里创建一个普通的linux系统账号，让hadoop运行在这个账号下面，不要直接运行在root账号下面
# 账号的home目录可以根据需要指定，这里默认使用/home/hadoop

sudo useradd -m -s /bin/bash hadoop

# 也不要设置密码，切换就用 su 来切换
sudo su - hadoop
```

## 安装 JAVA
```bash

java -version
openjdk version "1.8.0_91"
OpenJDK Runtime Environment (build 1.8.0_91-b14)
OpenJDK 64-Bit Server VM (build 25.91-b14, mixed mode)
```

## 安装 SSH
```bash


```

## 生成ssh key
```bash
ssh-keygen -t rsa -C 'hadoop-master'

# 配置免密码的ssh登录
cat .ssh/id_rsa.pub >> .ssh/authorized_keys

chmod 0600 .ssh/authorized_keys

```

## 下载 hadoop-2.9.1.tar.gz
```bash
# 有个345M 大小
http://mirrors.shu.edu.cn/apache/hadoop/common/hadoop-2.9.1/hadoop-2.9.1.tar.gz

#解压
tar xvzf hadoop-2.9.1.tar.gz

[hadoop@localhost ~]$ ls
hadoop-2.9.1  hadoop-2.9.1.tar.gz

[hadoop@localhost ~]$ ls hadoop-2.9.1
bin  etc  include  lib  libexec  LICENSE.txt  NOTICE.txt  README.txt  sbin  share


[hadoop@localhost ~]$ ll hadoop-2.9.1
总用量 152
drwxr-xr-x. 2 hadoop hadoop   4096 4月  16 2018 bin
drwxr-xr-x. 3 hadoop hadoop   4096 4月  16 2018 etc
drwxr-xr-x. 2 hadoop hadoop   4096 4月  16 2018 include
drwxr-xr-x. 3 hadoop hadoop   4096 4月  16 2018 lib
drwxr-xr-x. 2 hadoop hadoop   4096 4月  16 2018 libexec
-rw-r--r--. 1 hadoop hadoop 106210 4月  16 2018 LICENSE.txt
-rw-r--r--. 1 hadoop hadoop  15915 4月  16 2018 NOTICE.txt
-rw-r--r--. 1 hadoop hadoop   1366 4月  16 2018 README.txt
drwxr-xr-x. 3 hadoop hadoop   4096 4月  16 2018 sbin
drwxr-xr-x. 4 hadoop hadoop   4096 4月  16 2018 share

# 下载到/home/hadoop家目录下面就可以了，解压也放到这个目录下面
```


## 启动hadoop
```bash

  ├─java,28743 -Dproc_namenode -Xmx1000m -Djava.library.path=/home/hadoop/hadoop-2.9.1/lib -Djava.net.preferIPv4Stack=true -Dhadoop.log.dir=/home/hadoop/hadoop-2.9.1/logs -Dhadoop.log.file=hadoop.log -Dhadoop.home.dir=/home/hadoop/hadoop-2.9.1 -Dhadoop.id.str=hadoop -Dhadoop.root.logger=INFO,console -Dhadoop.policy.file=hadoop-policy.xml -Djava.net.preferIPv4Stack=true -Djava.net.preferIPv4Stack=true -Djava.net.preferIPv4Stack=true -Dhadoop.log.dir=/home/hadoop/hadoop-2.9.1/logs -Dhadoop.log.file=hadoop-hadoop-namenode-localhost.localdomain.log -Dhadoop.home.dir=/home/hadoop/hadoop-2.9.1 -Dhadoop.id.str=hadoop -Dhadoop.root.logger=INFO,RFA -Dhadoop.policy.file=hadoop-policy.xml -Djava.net.preferIPv4Stack=true -Dhadoop.security.logger=INFO,RFAS -Dhdfs.audit.logger=INFO,NullAppender -Dhadoop.security.logger=INFO,RFAS -Dhdfs.audit.logger=INFO,NullAppender -Dhadoop.security.logger=INFO,RFAS -Dhdfs.audit.logger=INFO,NullAppender -Dhadoop.security.logger=INFO,RFAS org.apache.hadoop.hdfs.server.namenode.NameNode


  ├─java,28994 -Dproc_datanode -Xmx1000m -Djava.library.path=/home/hadoop/hadoop-2.9.1/lib -Djava.net.preferIPv4Stack=true -Dhadoop.log.dir=/home/hadoop/hadoop-2.9.1/logs -Dhadoop.log.file=hadoop.log -Dhadoop.home.dir=/home/hadoop/hadoop-2.9.1 -Dhadoop.id.str=hadoop -Dhadoop.root.logger=INFO,console -Dhadoop.policy.file=hadoop-policy.xml -Djava.net.preferIPv4Stack=true -Djava.net.preferIPv4Stack=true -Djava.net.preferIPv4Stack=true -Dhadoop.log.dir=/home/hadoop/hadoop-2.9.1/logs -Dhadoop.log.file=hadoop-hadoop-datanode-localhost.localdomain.log -Dhadoop.home.dir=/home/hadoop/hadoop-2.9.1 -Dhadoop.id.str=hadoop -Dhadoop.root.logger=INFO,RFA -Dhadoop.policy.file=hadoop-policy.xml -Djava.net.preferIPv4Stack=true -server -Dhadoop.security.logger=ERROR,RFAS -Dhadoop.security.logger=ERROR,RFAS -Dhadoop.security.logger=ERROR,RFAS -Dhadoop.security.logger=INFO,RFAS org.apache.hadoop.hdfs.server.datanode.DataNode


  ├─java,29350 -Dproc_secondarynamenode -Xmx1000m -Djava.library.path=/home/hadoop/hadoop-2.9.1/lib -Djava.net.preferIPv4Stack=true -Dhadoop.log.dir=/home/hadoop/hadoop-2.9.1/logs -Dhadoop.log.file=hadoop.log -Dhadoop.home.dir=/home/hadoop/hadoop-2.9.1 -Dhadoop.id.str=hadoop -Dhadoop.root.logger=INFO,console -Dhadoop.policy.file=hadoop-policy.xml -Djava.net.preferIPv4Stack=true -Djava.net.preferIPv4Stack=true -Djava.net.preferIPv4Stack=true -Dhadoop.log.dir=/home/hadoop/hadoop-2.9.1/logs -Dhadoop.log.file=hadoop-hadoop-secondarynamenode-localhost.localdomain.log -Dhadoop.home.dir=/home/hadoop/hadoop-2.9.1 -Dhadoop.id.str=hadoop -Dhadoop.root.logger=INFO,RFA -Dhadoop.policy.file=hadoop-policy.xml -Djava.net.preferIPv4Stack=true -Dhadoop.security.logger=INFO,RFAS -Dhdfs.audit.logger=INFO,NullAppender -Dhadoop.security.logger=INFO,RFAS -Dhdfs.audit.logger=INFO,NullAppender -Dhadoop.security.logger=INFO,RFAS -Dhdfs.audit.logger=INFO,NullAppender -Dhadoop.security.logger=INFO,RFAS org.apache.hadoop.hdfs.server.namenode.SecondaryNameNode

  ├─java,29606 -Dproc_resourcemanager -Xmx1000m -Dhadoop.log.dir=/home/hadoop/hadoop-2.9.1/logs -Dyarn.log.dir=/home/hadoop/hadoop-2.9.1/logs -Dhadoop.log.file=yarn-hadoop-resourcemanager-localhost.localdomain.log -Dyarn.log.file=yarn-hadoop-resourcemanager-localhost.localdomain.log -Dyarn.home.dir= -Dyarn.id.str=hadoop -Dhadoop.root.logger=INFO,RFA -Dyarn.root.logger=INFO,RFA -Dyarn.policy.file=hadoop-policy.xml -Dhadoop.log.dir=/home/hadoop/hadoop-2.9.1/logs -Dyarn.log.dir=/home/hadoop/hadoop-2.9.1/logs -Dhadoop.log.file=yarn-hadoop-resourcemanager-localhost.localdomain.log -Dyarn.log.file=yarn-hadoop-resourcemanager-localhost.localdomain.log -Dyarn.home.dir=/home/hadoop/hadoop-2.9.1 -Dhadoop.home.dir=/home/hadoop/hadoop-2.9.1 -Dhadoop.root.logger=INFO,RFA -Dyarn.root.logger=INFO,RFA -classpath /home/hadoop/hadoop-2.9.1/etc/hadoop:/home/hadoop/hadoop-2.9.1/etc/hadoop:/home/hadoop/hadoop-2.9.1/etc/hadoop:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/*:/home/hadoop/hadoop-2.9.1/share/hadoop/common/*:/home/hadoop/hadoop-2.9.1/share/hadoop/hdfs:/home/hadoop/hadoop-2.9.1/share/hadoop/hdfs/lib/*:/home/hadoop/hadoop-2.9.1/share/hadoop/hdfs/*:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/*:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/*:/home/hadoop/hadoop-2.9.1/share/hadoop/mapreduce/lib/*:/home/hadoop/hadoop-2.9.1/share/hadoop/mapreduce/*:/home/hadoop/hadoop-2.9.1/contrib/capacity-scheduler/*.jar:/home/hadoop/hadoop-2.9.1/contrib/capacity-scheduler/*.jar:/home/hadoop/hadoop-2.9.1/contrib/capacity-scheduler/*.jar:/home/hadoop/hadoop-2.9.1/contrib/capacity-scheduler/*.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/*:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/*:/home/hadoop/hadoop-2.9.1/etc/hadoop/rm-config/log4j.properties:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/timelineservice/*:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/timelineservice/lib/* org.apache.hadoop.yarn.server.resourcemanager.ResourceManager

  ├─java,29930 -Dproc_nodemanager -Xmx1000m -Dhadoop.log.dir=/home/hadoop/hadoop-2.9.1/logs -Dyarn.log.dir=/home/hadoop/hadoop-2.9.1/logs -Dhadoop.log.file=yarn-hadoop-nodemanager-localhost.localdomain.log -Dyarn.log.file=yarn-hadoop-nodemanager-localhost.localdomain.log -Dyarn.home.dir= -Dyarn.id.str=hadoop -Dhadoop.root.logger=INFO,RFA -Dyarn.root.logger=INFO,RFA -Dyarn.policy.file=hadoop-policy.xml -server -Dhadoop.log.dir=/home/hadoop/hadoop-2.9.1/logs -Dyarn.log.dir=/home/hadoop/hadoop-2.9.1/logs -Dhadoop.log.file=yarn-hadoop-nodemanager-localhost.localdomain.log -Dyarn.log.file=yarn-hadoop-nodemanager-localhost.localdomain.log -Dyarn.home.dir=/home/hadoop/hadoop-2.9.1 -Dhadoop.home.dir=/home/hadoop/hadoop-2.9.1 -Dhadoop.root.logger=INFO,RFA -Dyarn.root.logger=INFO,RFA -classpath /home/hadoop/hadoop-2.9.1/etc/hadoop:/home/hadoop/hadoop-2.9.1/etc/hadoop:/home/hadoop/hadoop-2.9.1/etc/hadoop:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/*:/home/hadoop/hadoop-2.9.1/share/hadoop/common/*:/home/hadoop/hadoop-2.9.1/share/hadoop/hdfs:/home/hadoop/hadoop-2.9.1/share/hadoop/hdfs/lib/*:/home/hadoop/hadoop-2.9.1/share/hadoop/hdfs/*:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/*:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/*:/home/hadoop/hadoop-2.9.1/share/hadoop/mapreduce/lib/*:/home/hadoop/hadoop-2.9.1/share/hadoop/mapreduce/*:/home/hadoop/hadoop-2.9.1/contrib/capacity-scheduler/*.jar:/home/hadoop/hadoop-2.9.1/contrib/capacity-scheduler/*.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/*:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/*:/home/hadoop/hadoop-2.9.1/etc/hadoop/nm-config/log4j.properties:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/timelineservice/*:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/timelineservice/lib/* org.apache.hadoop.yarn.server.nodemanager.NodeManager

```