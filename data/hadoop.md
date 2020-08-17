# Hadoop, YARN, HDFS, Spark

Install the necessary dependencies and setup the environment. For a local environment the following are sufficient:

1. Download Hadoop distribution (2.x)
2. Download Spark distribution (2.4.x) without Hadoop
3. Setup the following environment variables:

```bash
JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-amd64
HADOOP_HOME=/opt/hadoop
HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop
HIVE_HOME=/opt/hive
SPARK_HOME=/opt/spark
PATH=$JAVA_HOME/bin:$HADOOP_HOME/bin:$HADOOP_HOME/sbin:$HIVE_HOME/bin:$SPARK_HOME/bin:$SPARK_HOME/sbin:$PATH
SPARK_DIST_CLASSPATH=$(hadoop classpath)

export JAVA_HOME HADOOP_HOME HADOOP_CONF_DIR HIVE_HOME SPARK_HOME SPARK_DIST_CLASSPATH PATH
```

## HDFS

### core-site.xml

```xml
<configuration>
   <property>
      <name>fs.default.name</name>
      <value>hdfs://localhost:9000</value> 
   </property>
</configuration>
```

### hdfs-site.xml

```xml
<configuration>
   <property>
      <name>dfs.replication</name>
      <value>1</value>
   </property>
   <property>
      <name>dfs.name.dir</name>
      <value>file:///var/hadoop/hdfs/namenode</value>
   </property>
   <property>
      <name>dfs.data.dir</name>
      <value>file:///var/hadoop/hdfs/datanode</value> 
   </property>
</configuration>
```

**NOTE**: The hadoop process should have write permissions to the directories above.

### yarn-site.xml

```xml
<configuration>
   <property>
      <name>yarn.nodemanager.vmem-check-enabled</name>
      <value>false</value>
   </property>
</configuration>
```

**NOTE:** This configuration proved necessary to submit Spark jobs in local mode

### Format the filesystem

```bash
$ hdfs namenode -format
```

### Start services

```bash
$ start-dfs.sh
$ start-yarn.sh
```

HDFS monitoring interface will be available by default at:
- Namenode: `http://localhost:50070`
- Datanote: `http://localhost:50075`

YARN monitoring interface will be available by default at:

- Resource manager: `http://localhost:8088`
- Node manager: `http://localhost:8042`

**NOTE:**: Before starting ensure that `JAVA_HOME` is set in `hadoop-env.sh`.

## Working with Spark

### Submit a Spark job

```bash
$ spark-submit --class org.apache.spark.examples.SparkPi \
    --master yarn \
    --deploy-mode cluster \
    --driver-memory 2g \
    --executor-memory 2g \
    --executor-cores 1 \
    $SPARK_HOME/examples/jars/spark-examples*.jar 10
```

**NOTE:** HADOOP_CONF_DIR must be set (http://spark.apache.org/docs/latest/running-on-yarn.html#launching-spark-on-yarn). If a spark distribution without hadoop is used SPARK_DIST_CLASSPATH=$(hadoop classpath)
must be set as well (https://spark.apache.org/docs/latest/hadoop-provided.html) .

### Start a notebook session in YARN mode

```scala
import org.apache.spark.sql._
import org.apache.spark.sql.functions._

//Spark log can be very verbose
import org.apache.log4j.{Level, Logger}
Logger.getLogger("org").setLevel(Level.OFF)

val spark = {
  NotebookSparkSession.builder()
    .master("yarn")
    .config("spark.executor.instances", 2)
    .config("spark.executor.memory", "1g")
    .getOrCreate()
}

import spark.implicits._
```

## Hive

```
$ schematool -initSchema -dbType derby
```

```xml
<configuration>
  <property>
    <name>hive.exec.scratchdir</name>
    <value>/tmp</value>
  </property>
  <property>
    <name>hive.metastore.warehouse.dir</name>
    <value>/user/hive/warehouse</value>
  </property>
  <property>
    <name>javax.jdo.option.ConnectionURL</name>
    <value>jdbc:derby:/tmp/metastore_db;databaseName=metastore_db;create=true</value>
  </property>
</configuration>
```