### Working with topics, groups, and offsets

In this excersise we will:
* [Create a topic](#oe1_flink_one)
* [Populate it with records with kafka_console_producer](#oe1_flink_two)
* [Examine the log file directlly](#oe1_flink_three)
* [Examine the consumer groups](#oe1_flink_fore)
* [Manually adjust the offset](#oe1_flink_five)

Let us create a topic T2. <a name="oe1_flink_one"/>  

    ~/kafka/current/bin/kafka-topics --bootstrap-server localhost:9092 --create --topic T2 --partitions 3 --replication-factor 3
    
Let us polulate it with 4 records. <a name="oe1_flink_two"/> Note we configure the console producer to accept our key and payload separated by semicolon.
 
     ~/kafka/current/bin/kafka-console-producer --broker-list localhost:9092 --topic T2 --property  "parse.key=true" --property "key.separator=:"
    >one:First Record
    >two:Second Record
    >three:Third One
    >four:Fourth Record 
 Let us have a look directly at the kafka log files using kafka-dump-log. <a name="oe1_flink_three"/>

    zconsult@kafkaqa3:~$ ~/kafka/current/bin/kafka-dump-log --deep-iteration --files ~/kafka_data/kafka /T2-0/00000000000000000000.log --print-data-log
    Dumping /home/zconsult/kafka_data/kafka/T2-0/00000000000000000000.log
    Starting offset: 0
    baseOffset: 0 lastOffset: 0 count: 1 baseSequence: -1 lastSequence: -1 producerId: -1 producerEpoch: -1 partitionLeaderEpoch: 0 isTransactional: false isControl: false position: 0 CreateTime: 1586442163921 size: 84 magic: 2 compresscodec: NONE crc: 4292585582 isvalid: true
    | offset: 0 CreateTime: 1586442163921 keysize: 3 valuesize: 13 sequence: -1 headerKeys: [] key: two payload: Second Record
    zconsult@kafkaqa3:~$ ~/kafka/current/bin/kafka-dump-log --deep-iteration --files ~/kafka_data/kafka  /T2-1/00000000000000000000.log --print-data-log
    Dumping /home/zconsult/kafka_data/kafka/T2-1/00000000000000000000.log
    Starting offset: 0
    baseOffset: 0 lastOffset: 0 count: 1 baseSequence: -1 lastSequence: -1 producerId: -1 producerEpoch: -1 partitionLeaderEpoch: 0 isTransactional: false isControl: false position: 0 CreateTime: 1586442191488 size: 82 magic: 2 compresscodec: NONE crc: 3040786131 isvalid: true
    | offset: 0 CreateTime: 1586442191488 keysize: 5 valuesize: 9 sequence: -1 headerKeys: [] key: three payload: Third One
    baseOffset: 1 lastOffset: 1 count: 1 baseSequence: -1 lastSequence: -1 producerId: -1 producerEpoch: -1 partitionLeaderEpoch: 0 isTransactional: false isControl: false position: 82 CreateTime: 1586442219433 size: 84 magic: 2 compresscodec: NONE crc: 1664393495 isvalid: true
    | offset: 1 CreateTime: 1586442219433 keysize: 4 valuesize: 12 sequence: -1 headerKeys: [] key: fore payload: Forth Record
    zconsult@kafkaqa3:~$ ~/kafka/current/bin/kafka-dump-log --deep-iteration --files ~/kafka_data/kafka/T2-2/00000000000000000000.log --print-data-log
    Dumping /home/zconsult/kafka_data/kafka/T2-2/00000000000000000000.log
    Starting offset: 0
    baseOffset: 0 lastOffset: 0 count: 1 baseSequence: -1 lastSequence: -1 producerId: -1 producerEpoch: -1 partitionLeaderEpoch: 0 isTransactional: false isControl: false position: 0 CreateTime: 1586442149669 size: 83 magic: 2 compresscodec: NONE crc: 4121187872 isvalid: true
    | offset: 0 CreateTime: 1586442149669 keysize: 3 valuesize: 12 sequence: -1 headerKeys: [] key: one payload: First Record
   

As we can see ore records got distributed in the following way:

Key | Kafka Partition
----|----------------
one  | 2 (T2-2)
two  | 0 (T2-0)
three, four | 1 (T2-1)

Now we will check consumer groups with kafka-consumer-groups. <a name="oe1_flink_fore"/>
         
     zconsult@kafkaqa3:~$ ~/kafka/current/bin/kafka-consumer-groups --bootstrap-server localhost:9092 --list
     KMOffsetCache-kafkaqamini1

We can ingnore *KMOffsetCache-kafkaqamini1* record. It is a CMAK (former Kafka Manager) artifact. Apart of it our group list is empty.

Let us read our records with kafka-console-consumer. Pay attantion to the *--from-beginning* option, which instructs, only for  a _new group_, to read from zero offset, that is from very beginning, rather than from the moment consumer starts.

    ~/kafka/current/bin/kafka-console-consumer --bootstrap-server localhost:9092 --topic T2 --group G2 --from-beginning
    First Record
    Third One
    Fourth Record
    Second Record

As expected we got all four records, and now consumer sits waiting for the next message. *Without stopping* our consumer let us look at existing groups using _--list_ option.
      
    ~/kafka/current/bin/kafka-consumer-groups --bootstrap-server localhost:9092 --list
    KMOffsetCache-ubuntus1
    G2
We have a new group *G2*. Let us examine it using the _--desribe_ option

    zconsult@kafkaqa3:~$ ~/kafka/current/bin/kafka-consumer-groups --bootstrap-server localhost:9092 --group G2 --describe

    GROUP           TOPIC           PARTITION  CURRENT-OFFSET  LOG-END-OFFSET  LAG             CONSUMER-ID                                        HOST            CLIENT-ID
    G2              T2              0          1               1               0               consumer-G2-1-25e2ec44-853a-4bb5-9d63-fad055bc84ca /10.111.30.28   consumer-G2-1
    G2              T2              1          2               2               0               consumer-G2-1-25e2ec44-853a-4bb5-9d63-fad055bc84ca /10.111.30.28   consumer-G2-1
    G2              T2              2          1               1               0               consumer-G2-1-25e2ec44-853a-4bb5-9d63-fad055bc84ca /10.111.30.28   consumer-G2-1

Now let us stop our consumer (Ctrl-C) and look at our group once again.

    ~/kafka/current/bin/kafka-consumer-groups --bootstrap-server localhost:9092 --group G2 --describe

    Consumer group 'G2' has no active members.

    GROUP           TOPIC           PARTITION  CURRENT-OFFSET  LOG-END-OFFSET  LAG             CONSUMER-ID     HOST            CLIENT-ID
    G2              T2              2          1               1               0               -               -               -
    G2              T2              1          2               2               0               -               -               -
    G2              T2              0          1               1               0               -               -               -

As we can see when there are no active readers *CONSUMER-ID*, *HOST*, and *CLIENT-ID* collumns are empty.

As briefly mentioned abome, if we try to rerun our consumer even with the **--from-beginning** option but *using the same group name G2*, it will not read any messages, because for that group our *LAG*s are 0 on all three partitions. (We had read everything)

    ~/kafka/current/bin/kafka-console-consumer --bootstrap-server localhost:9092 --topic T2 --group G2 --from-beginning

<a name="oe1_flink_five"/>

We can manually adjust an offset using *--reset-offsets* option. Since it is a danager operation we have two modes aka two options: *--dry-run* to check what the outcome would be (below) and *--execute* to perform a real action.

Let us adjust our offsets on the partion 1 (T2:1) where we have two records with the keys *three* & *four*. First let us have a look with _--dry-run_ option:

    ~/kafka/current/bin/kafka-consumer-groups --bootstrap-server localhost:9092 --group G2 --reset-offsets --topic T2:1 --to-offset 1 --dry-run

    GROUP                          TOPIC                          PARTITION  NEW-OFFSET     
    G2                             T2                             1          1              

And then do it for real with option _--execute_.

    ~/kafka/current/bin/kafka-consumer-groups --bootstrap-server localhost:9092 --group G2 --reset-offsets --topic T2:1 --to-offset 1 --execute

    GROUP                          TOPIC                          PARTITION  NEW-OFFSET     
    G2                             T2                             1          1              

We run our consumer again

    ~/kafka/current/bin/kafka-console-consumer --bootstrap-server localhost:9092 --topic T2 --group G2 --from-beginning
    Fourth Record

and as expected we got the last record, with key  *four*, and value *Fourth Record*.
