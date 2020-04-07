

to check:
    
       zconsult@kafkaqa1:~$ export JAVA_HOME=/opt/kafka_java
       zconsult@kafkaqa1:~$ /opt/kafka/bin/schema-registry-start -daemon /opt/kafka/etc/schema-registry/schema-registry.properties 
       zconsult@kafkaqa1:~$ ps -eaf | grep schema
       zconsult 32128     1 65 17:24 pts/1    00:00:09 /opt/kafka_java/bin/java -Xmx512M -server -XX:+UseG1GC -XX:MaxGCPauseMillis=20 -XX:InitiatingHeapOccupancyPercent=35 -XX:+ExplicitGCInvokesConcurrent -Djava.awt.headless=true -Dcom.sun.management.jmxremote -Dcom.sun.management.jmxremote.authenticate=false -Dcom.sun.management.jmxremote.ssl=false -Dschema-registry.log.dir=/opt/kafka/bin/../logs -Dlog4j.configuration=file:/opt/kafka/bin/../etc/schema-registry/log4j.properties -cp :/opt/kafka/bin/../package-schema-registry/target/kafka-schema-registry-package-*-development/share/java/schema-registry/*:/opt/kafka/bin/../share/java/confluent-security/schema-registry/*:/opt/kafka/bin/../share/java/confluent-common/*:/opt/kafka/bin/../share/java/rest-utils/*:/opt/kafka/bin/../share/java/schema-registry/* io.confluent.kafka.schemaregistry.rest.SchemaRegistryMain /opt/kafka/etc/schema-registry/schema-registry.properties
       zconsult 32207 31764  0 17:24 pts/1    00:00:00 grep --color=auto schema


    zconsult@kafkaqa1:~$ /opt/kafka/bin/kafka-topics --bootstrap-server localhost:9092 --list
    T1
    __consumer_offsets
    _schemas_qa123
