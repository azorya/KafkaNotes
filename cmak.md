## CMAK (former KafkaManager) Notes.

[CMAK](https://github.com/yahoo/CMAK) (former KafkaManager) is a useful tool for many kafka-realted tasks such as managing topics, browsing, and monitoring. As of now, CMAK is built and installed on the _kafkaqamini1_ server.

Setting CMAK up "from scratch" involves the following steps:

1.  [get CMAK](#cmak_flink_one) 
2.  [build it](#cmak_flink_two) 
3.  [install & configure](#cmak_flink_three)
4.  [run & test](#cmak_flink_fore)

#### get CMAK  <a name="cmak_flink_one"/>

To get CMAK we use **git**.

    git clone https://github.com/yahoo/CMAK.git

#### Build <a name="cmak_flink_two"/>

To build it we must have java in our PATH.

    export PATH=$PATH:/opt/kafka_java/bin

In our newly created _CMAK_ directory we have _sbt_ application.

    zconsult@kafkaqa3:~/stage/CMAK$ ls
    app        conf  LICENSE  public     sbt  target
    build.sbt  img   project  README.md  src  test

Run it like this:
         
    ./sbt clean dist

The building process takes some time. It downloads quite a lot into the ~/sbt directory, and we end up with the _cmak-3.0.0.4.zip_ file in the _target/universal_ directory.

    zconsult@kafkaqa3:~/stage/CMAK/target/universal$ ls
    cmak-3.0.0.4.zip  scripts

That archive file is what we want to deploy on the box where this app is going to run.

##### Install & Config <a name="cmak_flink_three"/>

We must create a _zookeeper node_ [like we did for kafka](./zookeeper_node_for_kafka.md) for the CMAK app

     zconsult@kafkaqa3:/opt$ /opt/kafka/bin/zookeeper-shell localhost:2181 
     Connecting to localhost:2181
     Welcome to ZooKeeper!
     JLine support is disabled

     WATCHER::

    WatchedEvent state:SyncConnected type:None path:null
    ls /
    [apps, zookeeper]
    ls /apps
    [kafka_server]
    create /apps/cmak data
    Created /apps/cmak

To configure, we edit the _application.conf_ file. (we must put it on the box were we plan to run it).
     
    zconsult@kafkaqa3:~/test/cmak-3.0.0.4/conf$ ls
    application.conf  consumer.properties  logback.xml  logger.xml  routes

Our changes are the following (just one line):
    
    zconsult@kafkaqa3:~/test/cmak-3.0.0.4/conf$ git diff
    diff --git a/application.conf b/application.conf
    index 8d61f29..aa62a35 100644
    --- a/application.conf
    +++ b/application.conf
    @@ -25,7 +25,7 @@ play.application.loader=loader.KafkaManagerLoader
    # https://github.com/yahoo/CMAK/issues/713
    kafka-manager.zkhosts="kafka-manager-zookeeper:2181"
    kafka-manager.zkhosts=${?ZK_HOSTS}
    -cmak.zkhosts="kafka-manager-zookeeper:2181"
    +cmak.zkhosts="kafkaqa2:2181/apps/cmak"
    cmak.zkhosts=${?ZK_HOSTS}

We are pointing CMAK to the zookeeper node (_kafkaqa2:2181/apps/cmak_) we created on the previous step.

#### Run & test <a name="cmak_flink_fore"/>

To run this app just go to the _bin_ dir and execute _cmak_ command.
 
    cd ~/test/cmak-3.0.0.4/bin/
    ./cmak

We can check that this app is using proper node like this:

    zconsult@kafkaqa3:/opt$ kafka/bin/zookeeper-shell localhost:2181
    Connecting to localhost:2181
    Welcome to ZooKeeper!
    JLine support is disabled

    WATCHER::

    WatchedEvent state:SyncConnected type:None path:null
    ls /
    [apps, zookeeper]
    ls /apps/cmak
    [kafka-manager]

Now we have a new zookeeper node _kafka-manager_ created (by CMAK application) under our previously created _cmak_.

Now we have our application running. To use it we have to start a web browser (firefox for example) and connenct to the port _9000_.

The _cmak_ is not without some minor glitches though. For example some of the changes made are not immediately visible and one has to go "back and forth" on the links. The previous version had some problems with _consumer groups_. It "remembered" them long after they were gone. Consider this tool as a helper only, the command line tools provide more reliable information.

