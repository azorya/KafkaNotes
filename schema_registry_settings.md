## schema registry settings

Schema registry is used when one wants to send structured messages to kafka. We have it installed on the _kafkaqa1_ node.

Our steps are:

1. [configure](#srs_flink_one)
2. [run & test](#srs_flink_two)
3. [cleanup note](#srs_flink_three)

### schema registry configuration <a name="srs_flink_one"/>

As we did with [zookeeper](./zookeeper_settings.md#zs_flink_one_one) and [kafka](./kafka_settings.md#ks_flink_one) let us use **git** to examine our schema-registry.properties config file.

    zconsult@kafkaqa1:/opt$ ls -l /opt/kafka/etc/schema-registry/schema-registry.properties 
    -rw-r--r-- 1 zconsult zconsult 1975 Apr  8 15:33 /opt/kafka/etc/schema-registry/schema-registry.properties

    zconsult@kafkaqa1:~/kafka/current/etc/schema-registry$ git diff schema-registry.properties
    diff --git a/schema-registry/schema-registry.properties b/schema-registry/schema-registry.properties
    index 3756b8f..6534b3a 100644
    --- a/schema-registry/schema-registry.properties
    +++ b/schema-registry/schema-registry.properties
    @@ -25,7 +25,7 @@ listeners=http://0.0.0.0:8081
    # (see zookeeper docs for details).
    # This is a comma separated host:port pairs, each corresponding to a zk
    # server. e.g. "127.0.0.1:3000,127.0.0.1:3001,127.0.0.1:3002".
    -kafkastore.connection.url=localhost:2181
    +#kafkastore.connection.url=localhost:2181
 
    # Alternatively, Schema Registry can now operate without Zookeeper, handling all coordination via
    # Kafka brokers. Use this setting to specify the bootstrap servers for your Kafka cluster and it
    @@ -34,10 +34,15 @@ kafkastore.connection.url=localhost:2181
    # (Note that you cannot mix the two modes; use this mode only on new deployments or by shutting down
    # all instances, switching to the new configuration, and then starting the schema registry
    # instances again.)
    -#kafkastore.bootstrap.servers=PLAINTEXT://localhost:9092
    +kafkastore.bootstrap.servers=PLAINTEXT://localhost:9092
 
    # The name of the topic to store schemas in
    -kafkastore.topic=_schemas
    +kafkastore.topic=_schemas_qa123
    +
    +schema.registry.group.id=schema_group_qa123 
 
    # If true, API requests that fail will include extra debugging information, including stack traces
    debug=false
    +
    +access.control.allow.methods=GET,POST,PUT,OPTIONS
    +access.control.allow.origin=*

We made the following changes:
* commented out _kafkastore.connection.url=localhost:2181_ string & uncommented _kafkastore.bootstrap.servers=PLAINTEXT://localhost:9092_. This way we are using _kafka instead of zookeeper_ as a [schema registry storage](#srs_flink_three).
* renamed the topic _kafkastore.topic=_schemas_qa123_ where schema registry keeps its data. That way we avoid a name conflict if one wants to use the same kafka cluster for the different schema registry services <a name="srs_blink_one"/>
* add _schema.registry.group.id_ for the same reason.
* add two lines _access.control.allow_. That is a [schema registry gui](./lenes.md) requirement.

### run & test <a name="srs_flink_two"/>

To start as a daemon use _schema-registry-start_ as shown below.
    
       zconsult@kafkaqa1:~$ export JAVA_HOME=/opt/kafka_java
       zconsult@kafkaqa1:~$ /opt/kafka/bin/schema-registry-start -daemon /opt/kafka/etc/schema-registry/schema-registry.properties 

To check if it is running:

	zconsult@kafkaqa1:~$ ps -eaf | grep schema
       zconsult 32128     1 65 17:24 pts/1    00:00:09 /opt/kafka_java/bin/java -Xmx512M -server -XX:+UseG1GC -XX:MaxGCPauseMillis=20 -XX:InitiatingHeapOccupancyPercent=35 -XX:+ExplicitGCInvokesConcurrent -Djava.awt.headless=true -Dcom.sun.management.jmxremote -Dcom.sun.management.jmxremote.authenticate=false -Dcom.sun.management.jmxremote.ssl=false -Dschema-registry.log.dir=/opt/kafka/bin/../logs -Dlog4j.configuration=file:/opt/kafka/bin/../etc/schema-registry/log4j.properties -cp :/opt/kafka/bin/../package-schema-registry/target/kafka-schema-registry-package-*-development/share/java/schema-registry/*:/opt/kafka/bin/../share/java/confluent-security/schema-registry/*:/opt/kafka/bin/../share/java/confluent-common/*:/opt/kafka/bin/../share/java/rest-utils/*:/opt/kafka/bin/../share/java/schema-registry/* io.confluent.kafka.schemaregistry.rest.SchemaRegistryMain /opt/kafka/etc/schema-registry/schema-registry.properties
       zconsult 32207 31764  0 17:24 pts/1    00:00:00 grep --color=auto schema

schema registry uses these two files as logs:

    zconsult@kafkaqa1:~/kafka/current/logs$ ls sche*
    schema-registry.log  schemaRegistry.out
 
They can be very useful while debugging which requests were sent from your client applications.

As a a part of our checking procedure let us look at the:

#### schema registry storage <a name="srs_flink_three"/>

First let us find out if we have a schema registry [topic](#srs_blink_one) 

    zconsult@kafkaqa1:~$ /opt/kafka/bin/kafka-topics --bootstrap-server localhost:9092 --list
    T1
    __consumer_offsets
    _schemas_qa123

Please note that its  name is _"_schemas_qa123"_ not _"_schemas"_, so our renaming worked.

Let us examine this topic

    zconsult@kafkaqa1:~$ /opt/kafka/bin/kafka-topics --bootstrap-server localhost:9092 --topic _schemas_qa123 --describe
    Topic: _schemas_qa123	PartitionCount: 1	ReplicationFactor: 3	Configs: cleanup.policy=compact,segment.bytes=1073741824
	Topic: _schemas_qa123	Partition: 0	Leader: 2	Replicas: 2,3,1	Isr: 2,3,1

The key points here are _ReplicationFactor: 3_ so it is replicated across our kafka cluster and _cleanup.policy=compact_ which make kafka preserve the latest values with the same keys.

### cleanup note <a name="srs_flink_three"/>

An important thing to keep in mind that even when one deletes the subjects from schema registry the old ids are preserved.
In other words the following steps: 

1. register a schema as a subject "First" on the *empty schema registry*.
2. delete it.
3. register a _new version of this schema_ with the same name again.

will give us a _different id_ for the our "First". 

There is no way to force schema registry "completely forget the past" using its commands.

The only way to do it is:
1. stop schema registry daemon
2. delete its topic ("_schemas_qa123_" in our case)
3. restart schema registry daemon.

That is one more reason to have schema registry data stored in kafka.

