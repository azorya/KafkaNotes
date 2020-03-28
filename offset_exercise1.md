### Offset exercise

In this excersise we will:
* [Create a topic](#flink_one)
* [Populate it with records with kafka_console_producer](#flink_two)
* [Examine the log file directlly](#flink_three)
* [Examine the consumer groups](#flink_fore)
* [Manually adjust the offset](#flink_five)

Let us create a topic T2. <a name="flink_one"/>  

    ~/kafka/current/bin/kafka-topics --bootstrap-server localhost:9092 --create --topic T2 --partitions 3
Let us polulate it with 4 records. <a name="flink_two"/>
 
     ~/kafka/current/bin/kafka-console-producer --broker-list localhost:9092 --topic T2 --property  "parse.key=true" --property "key.separator=:"
    >one:First Record
    >two:Second Record
    >three:Third One
    >fore:Forth Record 
 Let us have a look directly at the kafka log files with kafka-dump-log. <a name="flink_three"/>
 
    alex@ubuntus1:~/test$ ~/kafka/current/bin/kafka-dump-log --deep-iteration --files ~/kafka_data/kafka/T2-0/00000000000000000000.log --print-data-log
    Dumping /home/alex/kafka_data/kafka/T2-0/00000000000000000000.log
    Starting offset: 0
    baseOffset: 0 lastOffset: 0 count: 1 baseSequence: -1 lastSequence: -1 producerId: -1 producerEpoch: -1    partitionLeaderEpoch: 0 isTransactional: false isControl: false position: 0 CreateTime: 1585240376402 size: 84 magic: 2 compresscodec: NONE crc: 3809967938 isvalid: true
    | offset: 0 CreateTime: 1585240376402 keysize: 3 valuesize: 13 sequence: -1 headerKeys: [] key: two payload: Second Record
 
     alex@ubuntus1:~/test$ ~/kafka/current/bin/kafka-dump-log --deep-iteration --files ~/kafka_data/kafka/T2-1/00000000000000000000.log --print-data-log
    Dumping /home/alex/kafka_data/kafka/T2-1/00000000000000000000.log
    Starting offset: 0
    baseOffset: 0 lastOffset: 0 count: 1 baseSequence: -1 lastSequence: -1 producerId: -1 producerEpoch: -1 partitionLeaderEpoch: 0 isTransactional: false isControl: false position: 0 CreateTime: 1585240390882 size: 82 magic: 2 compresscodec: NONE crc: 676068487 isvalid: true
    | offset: 0 CreateTime: 1585240390882 keysize: 5 valuesize: 9 sequence: -1 headerKeys: [] key: three payload: Third One
    baseOffset: 1 lastOffset: 1 count: 1 baseSequence: -1 lastSequence: -1 producerId: -1 producerEpoch: -1 partitionLeaderEpoch: 0 isTransactional: false isControl: false position: 82 CreateTime: 1585240416299 size: 84 magic: 2 compresscodec: NONE crc: 1961464386 isvalid: true
    | offset: 1 CreateTime: 1585240416299 keysize: 4 valuesize: 12 sequence: -1 headerKeys: [] key: fore payload: Forth Record
    
    alex@ubuntus1:~/test$ ~/kafka/current/bin/kafka-dump-log --deep-iteration --files ~/kafka_data/kafka/T2-2/00000000000000000000.log --print-data-log
    Dumping /home/alex/kafka_data/kafka/T2-2/00000000000000000000.log
    Starting offset: 0
    baseOffset: 0 lastOffset: 0 count: 1 baseSequence: -1 lastSequence: -1 producerId: -1 producerEpoch: -1    partitionLeaderEpoch: 0 isTransactional: false isControl: false position: 0 CreateTime: 1585240360805 size: 83 magic: 2 compresscodec: NONE crc: 1176011849 isvalid: true
    | offset: 0 CreateTime: 1585240360805 keysize: 3 valuesize: 12 sequence: -1 headerKeys: [] key: one payload: First Record
As we can see ore records got distributed in the following way:

Key | Kafka Partition
----|----------------
one  | 2 (T2-2)
two  | 0 (T2-0)
three, fore | 1 (T2-1)

Now we will check consumer groups with kafka-consumer-groups. <a name="flink_fore"/>

    alex@ubuntus1:~/test$ ~/kafka/current/bin/kafka-consumer-groups --bootstrap-server localhost:9092 --list
    KMOffsetCache-ubuntus1

We can ingnore *KMOffsetCache-ubuntus1* record. It is a CMAK (former Kafka Manager) artifact. Apart of it our group list is empty.

Let us read our records with kafka-console-consumer. Pay attantion to the *--from-beginning* option.

    ~/kafka/current/bin/kafka-console-consumer --bootstrap-server localhost:9092 --topic T2 --group G2 --from-beginning
    First Record
    Third One
    Forth Record
    Second Record

As expected we got all fore records. *Without stopping* our consumer let us look at the group list.
      
    alex@ubuntus1:~/test$ ~/kafka/current/bin/kafka-consumer-groups --bootstrap-server localhost:9092 --list
    KMOffsetCache-ubuntus1
    G2
We have a new group *G2*. Let us examine it:

    alex@ubuntus1:~/test$ ~/kafka/current/bin/kafka-consumer-groups --bootstrap-server localhost:9092 --group G2 --describe

    GROUP           TOPIC           PARTITION  CURRENT-OFFSET  LOG-END-OFFSET  LAG             CONSUMER-ID                                        HOST            CLIENT-ID
    G2              T2              0          1               1               0               consumer-G2-1-80d4cbcf-4347-49f3-9a06-2f447ea466f7 /127.0.0.1      consumer-G2-1
    G2              T2              1          2               2               0               consumer-G2-1-80d4cbcf-4347-49f3-9a06-2f447ea466f7 /127.0.0.1      consumer-G2-1
    G2              T2              2          1               1               0               consumer-G2-1-80d4cbcf-4347-49f3-9a06-2f447ea466f7 /127.0.0.1      consumer-G2-1

Now let us stop our consumer and look at our group once again.

    alex@ubuntus1:~/test$ ~/kafka/current/bin/kafka-consumer-groups --bootstrap-server localhost:9092 --group G2 --describe

    Consumer group 'G2' has no active members.

    GROUP           TOPIC           PARTITION  CURRENT-OFFSET  LOG-END-OFFSET  LAG             CONSUMER-ID     HOST            CLIENT-ID
    G2              T2              2          1               1               0               -               -               -
    G2              T2              1          2               2               0               -               -               -
    G2              T2              0          1               1               0               -               -               -

As we can see when there are no active readers *CONSUMER-ID*, *HOST*, and *CLIENT-ID* collumns are empty.

If we try to rerun our consumer even with the **--from-beginning** option but *using the same group name G2*, it will not read any messages, because for that group our *LAG*s are 0 on all three partitions. (We had read everything)

    alex@ubuntus1:~/test$ ~/kafka/current/bin/kafka-console-consumer --bootstrap-server localhost:9092 --topic T2 --group G2 --from-beginning

<a name="flink_five"/>

We can manually adjust an offset using *--reset-offsets* option. Since it is a danager operation we have two modes aka two options: *--dry-run* to check what the outcome would be (below) and *--execute* to perform a real action.

Let us adjust our offsets on the partion 1 (T2:1) where we have two records with the keys *three* & *fore*. First let us have a look with --dry-run:

    alex@ubuntus1:~/test$ ~/kafka/current/bin/kafka-consumer-groups --bootstrap-server localhost:9092 --group G2 --reset-offsets --topic T2:1 --to-offset 1 --dry-run

    GROUP                          TOPIC                          PARTITION  NEW-OFFSET     
    G2                             T2                             1          1              

And then do it with --execute.

    alex@ubuntus1:~/test$ ~/kafka/current/bin/kafka-consumer-groups --bootstrap-server localhost:9092 --group G2 --reset-offsets --topic T2:1 --to-offset 1 --execute

    GROUP                          TOPIC                          PARTITION  NEW-OFFSET     
    G2                             T2                             1          1              

We run our consumer again

    alex@ubuntus1:~/test$ ~/kafka/current/bin/kafka-console-consumer --bootstrap-server localhost:9092 --topic T2 --group G2 --from-beginning
    Forth Record
and as expected we got the last record. The key of that recod is *fore*, the value is *Forth Record*.
