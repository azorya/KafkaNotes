## Perf test

One can use _kafka-producer-perf-test_ util to stress test.

A couple of examples:

We send 1000000 records requesting one ack

       zconsult@kafkaqa3:~$  /opt/kafka/bin/kafka-producer-perf-test --topic T1 --num-records 1000000  --throughput -1 --record-size 1024 --producer-props acks=1 bootstrap.servers=kafkaqa2:9092
       387460 records sent, 77492.0 records/sec (75.68 MB/sec), 337.4 ms avg latency, 845.0 ms max latency.
       422064 records sent, 84412.8 records/sec (82.43 MB/sec), 355.4 ms avg latency, 1030.0 ms max latency.
       1000000 records sent, 79598.821937 records/sec (77.73 MB/sec), 363.55 ms avg latency, 1030.00 ms max latency, 202 ms 50th, 985 ms 95th, 1018 ms 99th, 1027 ms 99.9th.


The same but smaller size

        zconsult@kafkaqa3:~$  /opt/kafka/bin/kafka-producer-perf-test --topic T1 --num-records 1000000  --throughput -1 --record-size 64 --producer-props acks=1 bootstrap.servers=kafkaqa2:9092
        1000000 records sent, 620347.394541 records/sec (37.86 MB/sec), 2.55 ms avg latency, 289.00 ms max latency, 2 ms 50th, 9 ms 95th, 12 ms 99th, 14 ms 99.9th


The small size but 3 acks
    
     zconsult@kafkaqa3:~$ /opt/kafka/bin/kafka-producer-perf-test --topic T1 --num-records 1000000  --throughput -1 --record-size 64 --producer-props acks=all bootstrap.servers=kafkaqa2:9092
     1000000 records sent, 302755.071147 records/sec (18.48 MB/sec), 355.54 ms avg latency, 889.00 ms max latency, 277 ms 50th, 823 ms 95th, 872 ms 99th, 886 ms 99.9th.


