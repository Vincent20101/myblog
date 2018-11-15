
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



# 安装hadoop准备

1.创建hadoop系统账号
```bash
# 这里创建一个普通的linux系统账号，让hadoop运行在这个账号下面，不要直接运行在root账号下面
# 账号的home目录可以根据需要指定，这里默认使用/home/hadoop

sudo useradd -m -s /bin/bash hadoop

# 也不要设置密码，切换就用 su 来切换
sudo su - hadoop
```

2.安装 JAVA
```bash

java -version
openjdk version "1.8.0_91"
OpenJDK Runtime Environment (build 1.8.0_91-b14)
OpenJDK 64-Bit Server VM (build 25.91-b14, mixed mode)
```

3.安装 SSH
```bash
这个基本上是系统都自带的有了

```

4.生成ssh key
```bash
ssh-keygen -t rsa -C 'hadoop-master'

# 配置免密码的ssh登录
cat .ssh/id_rsa.pub >> .ssh/authorized_keys

chmod 0600 .ssh/authorized_keys

```

5.下载 hadoop-2.9.1.tar.gz
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


6.启动hadoop
```bash

  ├─java,28743 -Dproc_namenode -Xmx1000m -Djava.library.path=/home/hadoop/hadoop-2.9.1/lib -Djava.net.preferIPv4Stack=true -Dhadoop.log.dir=/home/hadoop/hadoop-2.9.1/logs -Dhadoop.log.file=hadoop.log -Dhadoop.home.dir=/home/hadoop/hadoop-2.9.1 -Dhadoop.id.str=hadoop -Dhadoop.root.logger=INFO,console -Dhadoop.policy.file=hadoop-policy.xml -Djava.net.preferIPv4Stack=true -Djava.net.preferIPv4Stack=true -Djava.net.preferIPv4Stack=true -Dhadoop.log.dir=/home/hadoop/hadoop-2.9.1/logs -Dhadoop.log.file=hadoop-hadoop-namenode-localhost.localdomain.log -Dhadoop.home.dir=/home/hadoop/hadoop-2.9.1 -Dhadoop.id.str=hadoop -Dhadoop.root.logger=INFO,RFA -Dhadoop.policy.file=hadoop-policy.xml -Djava.net.preferIPv4Stack=true -Dhadoop.security.logger=INFO,RFAS -Dhdfs.audit.logger=INFO,NullAppender -Dhadoop.security.logger=INFO,RFAS -Dhdfs.audit.logger=INFO,NullAppender -Dhadoop.security.logger=INFO,RFAS -Dhdfs.audit.logger=INFO,NullAppender -Dhadoop.security.logger=INFO,RFAS org.apache.hadoop.hdfs.server.namenode.NameNode


  ├─java,28994 -Dproc_datanode -Xmx1000m -Djava.library.path=/home/hadoop/hadoop-2.9.1/lib -Djava.net.preferIPv4Stack=true -Dhadoop.log.dir=/home/hadoop/hadoop-2.9.1/logs -Dhadoop.log.file=hadoop.log -Dhadoop.home.dir=/home/hadoop/hadoop-2.9.1 -Dhadoop.id.str=hadoop -Dhadoop.root.logger=INFO,console -Dhadoop.policy.file=hadoop-policy.xml -Djava.net.preferIPv4Stack=true -Djava.net.preferIPv4Stack=true -Djava.net.preferIPv4Stack=true -Dhadoop.log.dir=/home/hadoop/hadoop-2.9.1/logs -Dhadoop.log.file=hadoop-hadoop-datanode-localhost.localdomain.log -Dhadoop.home.dir=/home/hadoop/hadoop-2.9.1 -Dhadoop.id.str=hadoop -Dhadoop.root.logger=INFO,RFA -Dhadoop.policy.file=hadoop-policy.xml -Djava.net.preferIPv4Stack=true -server -Dhadoop.security.logger=ERROR,RFAS -Dhadoop.security.logger=ERROR,RFAS -Dhadoop.security.logger=ERROR,RFAS -Dhadoop.security.logger=INFO,RFAS org.apache.hadoop.hdfs.server.datanode.DataNode


  ├─java,29350 -Dproc_secondarynamenode -Xmx1000m -Djava.library.path=/home/hadoop/hadoop-2.9.1/lib -Djava.net.preferIPv4Stack=true -Dhadoop.log.dir=/home/hadoop/hadoop-2.9.1/logs -Dhadoop.log.file=hadoop.log -Dhadoop.home.dir=/home/hadoop/hadoop-2.9.1 -Dhadoop.id.str=hadoop -Dhadoop.root.logger=INFO,console -Dhadoop.policy.file=hadoop-policy.xml -Djava.net.preferIPv4Stack=true -Djava.net.preferIPv4Stack=true -Djava.net.preferIPv4Stack=true -Dhadoop.log.dir=/home/hadoop/hadoop-2.9.1/logs -Dhadoop.log.file=hadoop-hadoop-secondarynamenode-localhost.localdomain.log -Dhadoop.home.dir=/home/hadoop/hadoop-2.9.1 -Dhadoop.id.str=hadoop -Dhadoop.root.logger=INFO,RFA -Dhadoop.policy.file=hadoop-policy.xml -Djava.net.preferIPv4Stack=true -Dhadoop.security.logger=INFO,RFAS -Dhdfs.audit.logger=INFO,NullAppender -Dhadoop.security.logger=INFO,RFAS -Dhdfs.audit.logger=INFO,NullAppender -Dhadoop.security.logger=INFO,RFAS -Dhdfs.audit.logger=INFO,NullAppender -Dhadoop.security.logger=INFO,RFAS org.apache.hadoop.hdfs.server.namenode.SecondaryNameNode

  ├─java,29606 -Dproc_resourcemanager -Xmx1000m -Dhadoop.log.dir=/home/hadoop/hadoop-2.9.1/logs -Dyarn.log.dir=/home/hadoop/hadoop-2.9.1/logs -Dhadoop.log.file=yarn-hadoop-resourcemanager-localhost.localdomain.log -Dyarn.log.file=yarn-hadoop-resourcemanager-localhost.localdomain.log -Dyarn.home.dir= -Dyarn.id.str=hadoop -Dhadoop.root.logger=INFO,RFA -Dyarn.root.logger=INFO,RFA -Dyarn.policy.file=hadoop-policy.xml -Dhadoop.log.dir=/home/hadoop/hadoop-2.9.1/logs -Dyarn.log.dir=/home/hadoop/hadoop-2.9.1/logs -Dhadoop.log.file=yarn-hadoop-resourcemanager-localhost.localdomain.log -Dyarn.log.file=yarn-hadoop-resourcemanager-localhost.localdomain.log -Dyarn.home.dir=/home/hadoop/hadoop-2.9.1 -Dhadoop.home.dir=/home/hadoop/hadoop-2.9.1 -Dhadoop.root.logger=INFO,RFA -Dyarn.root.logger=INFO,RFA -classpath /home/hadoop/hadoop-2.9.1/etc/hadoop:/home/hadoop/hadoop-2.9.1/etc/hadoop:/home/hadoop/hadoop-2.9.1/etc/hadoop:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/*:/home/hadoop/hadoop-2.9.1/share/hadoop/common/*:/home/hadoop/hadoop-2.9.1/share/hadoop/hdfs:/home/hadoop/hadoop-2.9.1/share/hadoop/hdfs/lib/*:/home/hadoop/hadoop-2.9.1/share/hadoop/hdfs/*:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/*:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/*:/home/hadoop/hadoop-2.9.1/share/hadoop/mapreduce/lib/*:/home/hadoop/hadoop-2.9.1/share/hadoop/mapreduce/*:/home/hadoop/hadoop-2.9.1/contrib/capacity-scheduler/*.jar:/home/hadoop/hadoop-2.9.1/contrib/capacity-scheduler/*.jar:/home/hadoop/hadoop-2.9.1/contrib/capacity-scheduler/*.jar:/home/hadoop/hadoop-2.9.1/contrib/capacity-scheduler/*.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/*:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/*:/home/hadoop/hadoop-2.9.1/etc/hadoop/rm-config/log4j.properties:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/timelineservice/*:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/timelineservice/lib/* org.apache.hadoop.yarn.server.resourcemanager.ResourceManager

  ├─java,29930 -Dproc_nodemanager -Xmx1000m -Dhadoop.log.dir=/home/hadoop/hadoop-2.9.1/logs -Dyarn.log.dir=/home/hadoop/hadoop-2.9.1/logs -Dhadoop.log.file=yarn-hadoop-nodemanager-localhost.localdomain.log -Dyarn.log.file=yarn-hadoop-nodemanager-localhost.localdomain.log -Dyarn.home.dir= -Dyarn.id.str=hadoop -Dhadoop.root.logger=INFO,RFA -Dyarn.root.logger=INFO,RFA -Dyarn.policy.file=hadoop-policy.xml -server -Dhadoop.log.dir=/home/hadoop/hadoop-2.9.1/logs -Dyarn.log.dir=/home/hadoop/hadoop-2.9.1/logs -Dhadoop.log.file=yarn-hadoop-nodemanager-localhost.localdomain.log -Dyarn.log.file=yarn-hadoop-nodemanager-localhost.localdomain.log -Dyarn.home.dir=/home/hadoop/hadoop-2.9.1 -Dhadoop.home.dir=/home/hadoop/hadoop-2.9.1 -Dhadoop.root.logger=INFO,RFA -Dyarn.root.logger=INFO,RFA -classpath /home/hadoop/hadoop-2.9.1/etc/hadoop:/home/hadoop/hadoop-2.9.1/etc/hadoop:/home/hadoop/hadoop-2.9.1/etc/hadoop:/home/hadoop/hadoop-2.9.1/share/hadoop/common/lib/*:/home/hadoop/hadoop-2.9.1/share/hadoop/common/*:/home/hadoop/hadoop-2.9.1/share/hadoop/hdfs:/home/hadoop/hadoop-2.9.1/share/hadoop/hdfs/lib/*:/home/hadoop/hadoop-2.9.1/share/hadoop/hdfs/*:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/*:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/*:/home/hadoop/hadoop-2.9.1/share/hadoop/mapreduce/lib/*:/home/hadoop/hadoop-2.9.1/share/hadoop/mapreduce/*:/home/hadoop/hadoop-2.9.1/contrib/capacity-scheduler/*.jar:/home/hadoop/hadoop-2.9.1/contrib/capacity-scheduler/*.jar:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/*:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/lib/*:/home/hadoop/hadoop-2.9.1/etc/hadoop/nm-config/log4j.properties:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/timelineservice/*:/home/hadoop/hadoop-2.9.1/share/hadoop/yarn/timelineservice/lib/* org.apache.hadoop.yarn.server.nodemanager.NodeManager

```

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
STARTUP_MSG:   classpath = *.jar
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

配置、启动YARN

1、 配置mapred-site.xml
```bash
# 默认没有mapred-site.xml文件，但是有个mapred-site.xml.template配置模板文件。复制模板生成mapred-site.xml。
[hadoop@localhost hadoop]$ cp mapred-site.xml.template  mapred-site.xml
[hadoop@localhost hadoop]$ vi mapred-site.xml
[hadoop@localhost hadoop]$ cat mapred-site.xml
<?xml version="1.0"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<!--
  Licensed under the Apache License, Version 2.0 (the "License");
  you may not use this file except in compliance with the License.
  You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

  Unless required by applicable law or agreed to in writing, software
  distributed under the License is distributed on an "AS IS" BASIS,
  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  See the License for the specific language governing permissions and
  limitations under the License. See accompanying LICENSE file.
-->

<!-- Put site-specific property overrides in this file. -->

<configuration>指定mapreduce运行在yarn框架上。
    <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>
</configuration>
[hadoop@localhost hadoop]$

```
2、 配置yarn-site.xml
```bash
添加配置如下：
<property>
  <name>yarn.nodemanager.aux-services</name>
  <value>mapreduce_shuffle</value>
</property>
<property>
    <name>yarn.resourcemanager.hostname</name>
    <value>your ip</value>
 </property>
```

3、 启动Resourcemanager
```bash
[hadoop@localhost hadoop]$ yarn-daemon.sh start resourcemanager
starting resourcemanager, logging to /home/hadoop/hadoop-2.9.1/logs/yarn-hadoop-resourcemanager-localhost.localdomain.out
```

4、 启动nodemanager
```bash

[hadoop@localhost hadoop]$ yarn-daemon.sh start nodemanager
starting nodemanager, logging to /home/hadoop/hadoop-2.9.1/logs/yarn-hadoop-nodemanager-localhost.localdomain.out

```
5、 查看是否启动成功
```bash
[hadoop@localhost hadoop]$ jps
7255 ResourceManager
6105 DataNode
5834 NameNode
7532 NodeManager
6412 SecondaryNameNode
7678 Jps  # 可以看到ResourceManager、NodeManager已经启动成功了。
```
6、 YARN的Web页面
```bash
YARN的Web客户端端口号是8088，

http://10.0.63.48:8088/cluster


```




# 完全分布式安装








# 高可用模式安装
```bash

<configuration>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://sxt</value>
    </property>
    <property>
        <name>hadoop.tmp.dir</name>
        <value>/home/hadoop/hadoop-2.9.1/data</value>
    </property>

    <property>
        <name>ha.zookeeper.quorum</name>
        <value>namenode1:2181,namenode2:2181,namenode3:2181</value>
    </property>
</configuration>

```

```bash

<configuration>
    <!-- http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HDFSHighAvailabilityWithQJM.html -->
    <property>
        <name>dfs.nameservices</name>
        <value>sxt</value>
    </property>

    <property>
        <name>dfs.ha.namenodes.sxt</name>
        <value>namenode1,namenode2</value>
    </property>

    <!-- 配置rpc通信接口的 -->
    <property>
        <name>dfs.namenode.rpc-address.sxt.namenode1</name>
        <value>namenode1:8020</value>
    </property>
    <property>
        <name>dfs.namenode.rpc-address.sxt.namenode2</name>
        <value>namenode2:8020</value>
    </property>

    <property>
        <name>dfs.namenode.http-address.sxt.namenode1</name>
        <value>namenode1:50070</value>
    </property>
    <property>
        <name>dfs.namenode.http-address.sxt.namenode2</name>
        <value>namenode2:50070</value>
    </property>

    <!-- -->
    <property>
        <name>dfs.namenode.shared.edits.dir</name>
        <value>qjournal://namenode1:8485;namenode2:8485;namenode3:8485/sxt</value>
    </property>

    <property>
        <name>dfs.client.failover.proxy.provider.sxt</name>
        <value>org.apache.hadoop.hdfs.server.namenode.ha.ConfiguredFailoverProxyProvider</value>
    </property>

    <property>
        <name>dfs.journalnode.edits.dir</name>
        <value>/home/hadoop/hadoop-2.9.1/journal</value>
    </property>

     <property>
        <name>dfs.ha.automatic-failover.enabled</name>
        <value>true</value>
    </property>



    <property>
        <name>dfs.ha.fencing.methods</name>
        <value>sshfence</value>
    </property>

    <property>
        <name>dfs.ha.fencing.ssh.private-key-files</name>
        <value>/home/hadoop/.ssh/id_rsa</value>
    </property>
```
