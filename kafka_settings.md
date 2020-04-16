## Kafka Settings

We will confugure a three node kafka cluster. That is the smallest possible setup which can work as a _cluster_. The main differnce between a _cluster_ and a _standalone kafka_ is that it is possible to work with some of our nodes down. 

We have to:

1.  [Configure a single node](#ks_flink_one)
2.  [Propagate this changes to other nodes](#ks_flink_two)
3.  [Launch and test our cluster](#ks_flink_three)



### server.properties file <a name="ks_flink_one"/>

As with zookeeper settings we have a link to this config file in the  _/opt_

    zconsult@kafkaqa1:/opt$ ls -l /opt/kafka/etc/kafka/server.properties 
    -rw-r--r-- 1 zconsult zconsult 8417 Apr  8 19:45 /opt/kafka/etc/kafka/server.properties

To view the changes we use the **git** command:

    diff --git a/kafka/server.properties b/kafka/server.properties
    index fc9ba00..dbd9378 100644
    --- a/kafka/server.properties
    +++ b/kafka/server.properties
    @@ -18,7 +18,7 @@
    ############################# Server Basics #############################
 
    # The id of the broker. This must be set to a unique integer for each broker.
    -broker.id=0
    +broker.id=1
 
    ############################# Socket Server Settings #############################
 
    @@ -57,7 +57,7 @@ socket.request.max.bytes=104857600
    ############################# Log Basics #############################
 
    # A comma separated list of directories under which to store log files
    -log.dirs=/tmp/kafka-logs
    +log.dirs=/opt/kafka_data/kafka
 
    # The default number of log partitions per topic. More partitions allow greater
    # parallelism for consumption, but this will also result in more files across
    @@ -68,11 +68,12 @@ num.partitions=1
    # This value is recommended to be increased for installations with data dirs located in RAID array.
    num.recovery.threads.per.data.dir=1
 
    +auto.create.topics.enable=false
    ############################# Internal Topic Settings  #############################
    # The replication factor for the group metadata internal topics "__consumer_offsets" and "__transaction_state"
    # For anything other than development testing, a value greater than 1 is recommended to ensure availability such as 3.
    -offsets.topic.replication.factor=1
    -transaction.state.log.replication.factor=1
    +offsets.topic.replication.factor=3
    +transaction.state.log.replication.factor=3
    transaction.state.log.min.isr=1
 
    ############################# Log Flush Policy #############################
    @@ -100,6 +101,7 @@ transaction.state.log.min.isr=1
    # from the end of the log.
 
    # The minimum age of a log file to be eligible for deletion due to age
    +log.cleanup.policy=delete
     log.retention.hours=168
 
    # A size-based retention policy for logs. Segments are pruned from the log unless the remaining
    @@ -120,7 +122,7 @@ log.retention.check.interval.ms=300000
    # server. e.g. "127.0.0.1:3000,127.0.0.1:3001,127.0.0.1:3002".
    # You can also append an optional chroot string to the urls to specify the
    # root directory for all kafka znodes
    
    -zookeeper.connect=localhost:2181
    +zookeeper.connect=kafkaqa1:2181,kafkaqa2:2181,kafkaqa3:2181/apps/kafka_server
 
    # Timeout in ms for connecting to zookeeper
    zookeeper.connection.timeout.ms=6000
    @@ -143,7 +145,7 @@ zookeeper.connection.timeout.ms=6000
    # then the feature to collect and report support metrics
    # ("Metrics") is enabled.  If set to false, the feature is disabled.
    #
    -confluent.support.metrics.enable=true
    +confluent.support.metrics.enable=false
 
 We made a few changes:
 *  _broker.id_ is the unique id of the kafka node.
 *  _log.dirs_  is the location(s) where kafka stores the data. Look [here](./installation_notes.md#bin_flink_two) for more info.
 *  _auto.create.topics.enable_ was set to _false_. (_true_ is the default.) It prevents from accidental topic creation by misspelling the topic name.
 * _offsets.topic.replication.factor_ & _transaction.state.log.replication.factor_ are settings for the _replication_ of kafka internal(system) topics.
 * _log.cleanup.policy=delete_ means that after kafka log file is exipred it must be deleted.
 * _zookeeper.connect_ is a way our kafka node finds a _zookeeper enseble_. Please note that we use a zookeeper nodes we [created](./zookeeper_node_for_kafka.md#zs_flink_last) before.
 * finally we disable _confluent.support.metrics.enable_
 

### Propagate changes to all the nodes <a name="ks_flink_two"/> 

As with zookeeper we have to make similar changes (or simply _scp_ them) one all our nodes. 

The other and probably better way to deal with the configuration would be to have a central git repository.
In this repo we could make all the changes for the entire cluster, check it out on each node, and make only local _corrections like broker_id_ locally.
The only downside in this case that only local changes are immediatly visible.

    
### Launch and test <a name="ks_flink_three"/> 

There are three different ways to start kafka related server processes. 

* Run them from the command line.
* Run them from the command line as _daemons_.
* Run them as _systemd services_.

The difference between first two options is that a _daemon_ will be running even if the terminal it was launched from is closed.

Do not forget that JAVA_HOME environment veriable must be set one way or another.

        zconsult@kafkaqa1:~$ export JAVA_HOME=~/apps/java
        
To launch a kafka server process on a node use _zookeeper-server-start_ command from the kafka _bin_ directory.

    ### Launch and test <a name="zs_flink_three"/> 

There are three different ways to start kafka related server processes. 

* Run them from the command line.
* Run them from the command line as _daemons_.
* Run them as _systemd services_.

The difference between first two options is that a _daemon_ will be running even if the terminal it was launched from is closed.

Do not forget that JAVA_HOME environment veriable must be set one way or another.

        zconsult@kafkaqa1:~$ export JAVA_HOME=~/apps/java
        
To launch a zookeeper process on a node use _kafka-server-start_ command from the kafka _bin_ directory.

    /opt/kafka/bin/kafka-server-start -daemon /opt/kafka/etc/kafka/server.properties
    

One of the quick ways to check if kafka process is running is to run _ps_ command _grep_ either for _kafka_ or, keeping in mind that all the _daemons_ are java processes, for _java_

There are also a few log files kafka server writes to. The first one to look is **kafkaServer.out**

    zconsult@kafkaqa3:/opt/kafka/logs$ ls -l kafkaServer.out 
    -rw-rw-r-- 1 zconsult zconsult 151288 Apr  9 08:59 kafkaServer.out

One of the key points here is that _kafka server dumps its configuration in this file while starting. That is a very convinient way to check if the config changes you made did really take place._ For example here is a small fragment:

    [2020-04-08 20:09:13,271] INFO KafkaConfig values:
            advertised.host.name = null
            advertised.listeners = null
            advertised.port = null
            alter.config.policy.class.name = null
            alter.log.dirs.replication.quota.window.num = 11
            alter.log.dirs.replication.quota.window.size.seconds = 1
            authorizer.class.name =
            auto.create.topics.enable = false
            auto.leader.rebalance.enable = true
            background.threads = 10
            broker.id = 3

Please note _auto.create.topics.enable = false_ string.

Another file to look at is **server.log** . That is a _common_ file for our servicies. It means that the kafka server _shares_ it with zookeeper, so in case of some problems one can see error messages from both servers which can help to find the orinigal cause.

There are a few more log file: state-change.log, log-cleaner.log, etc.

Please also note that the **log settings are controlled by log4j** system. The detailed description of it is a way out of scope here, but here are a few hints. Let us check the _java options_ our kafka server is running with.  

    zconsult@kafkaqa1:~$ ps -eaf | grep "server.prop"
    zconsult  3232     1  1 Apr08 ?        00:09:02 /opt/kafka_java/bin/java -Xmx1G -Xms1G -server -XX:+UseG1GC -XX:MaxGCPauseMillis=20 -XX:InitiatingHeapOccupancyPercent=35 -XX:+ExplicitGCInvokesConcurrent -Djava.awt.headless=true -Xlog:gc*:file=/opt/kafka/bin/../logs/kafkaServer-gc.log:time,tags:filecount=10,filesize=102400 -Dcom.sun.management.jmxremote -Dcom.sun.management.jmxremote.authenticate=false -Dcom.sun.management.jmxremote.ssl=false -Dkafka.logs.dir=/opt/kafka/bin/../logs -Dlog4j.configuration=file:/opt/kafka/bin/../etc/kafka/log4j.properties -cp /opt/kafka/bin/../share/java/kafka/*:/opt/kafka/bin/../support-metrics-client/build/dependant-libs-2.12.10/*:/opt/kafka/bin/../support-metrics-client/build/libs/*:/usr/share/java/support-metrics-client/* io.confluent.support.metrics.SupportedKafka /opt/kafka/etc/kafka/server.properties
 
Please note the following string _-Dlog4j.configuration=file:/opt/kafka/bin/../etc/kafka/log4j.properties_

let us have a look at this (log4j.properties) file
    
    zconsult@kafkaqa3:/opt/kafka/etc/kafka$ ls -l log4j.properties 
    -rw-r--r-- 1 zconsult zconsult 4675 Apr  2 14:43 log4j.properties

Here is a part of it:

    # Unspecified loggers and loggers with additivity=true output to server.log and stdout
    # Note that INFO only applies to unspecified loggers, the log level of the child logger is used otherwise
    log4j.rootLogger=INFO, stdout, kafkaAppender

    log4j.appender.stdout=org.apache.log4j.ConsoleAppender
    log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
    log4j.appender.stdout.layout.ConversionPattern=[%d] %p %m (%c)%n

    log4j.appender.kafkaAppender=org.apache.log4j.DailyRollingFileAppender
    log4j.appender.kafkaAppender.DatePattern='.'yyyy-MM-dd-HH
    log4j.appender.kafkaAppender.File=${kafka.logs.dir}/server.log
    log4j.appender.kafkaAppender.layout=org.apache.log4j.PatternLayout
    log4j.appender.kafkaAppender.layout.ConversionPattern=[%d] %p %m (%c)%n

This way one can controll everything: the level of output messages (ERROR, INFO, DEBUG), the format, the output files, etc.

To test our kafka cluster let us [stress it](/perf_test.md) and do some [exercise.](./offset_exercise1.md)
