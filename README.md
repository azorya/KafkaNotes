# KafkaNotes
some key points for the kafka project

Where to get [CMAK](https://github.com/yahoo/CMAK)

### Offset exercise
Let us create a topic T2.

    ~/kafka/current/bin/kafka-topics --bootstrap-server localhost:9092 --create --topic T2 --partitions 3
 Let us polulate it with fore records.
 
     ~/kafka/current/bin/kafka-console-producer --broker-list localhost:9092 --topic T2 --property  "parse.key=true" --property "key.separator=:"
    >one:First Record
    >two:Second Record
    >three:Third One
    >fore:Forth Record 
 Let us have a look directly at the kafka log files.
 
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

Let us read them.


 
