**Kafka Settings**

We will confugure a three node kafka cluster. That is the smallest possible setup which can work as a _cluster_. The main differnce between a _cluster_ and a _standalone kafka_ is that it is possible to work with some of our nodes down. 

We have to:

1.  [Configure a single node](#ks_flink_one)
2.  [Propagate  this changes to other nodes](#ks_flink_two)
3.  [Launch and test our cluster](#ks_flink_three)



***server.properties file*** <a name="ks_flink_one"/>

As with zookeeper settings we have a link to this config file in the  _/opt_

    zconsult@kafkaqa1:/opt$ ls -l /opt/kafka/etc/kafka/server.properties 
    -rw-r--r-- 1 zconsult zconsult 8417 Apr  8 19:45 /opt/kafka/etc/kafka/server.properties

To view the changes we use the **git command**:

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
 *  _log.dirs_  is the location(s) where kafka stores the data. Please look at the last paragraph in our [basic installation](./basic_installation_notes.md)

    
    
    
