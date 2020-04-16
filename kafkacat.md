## kafkacat util

That is an extremely [useful utility](https://github.com/edenhill/kafkacat) with a lot of use [examples](https://medium.com/@coderunner/debugging-with-kafkacat-df7851d21968) on the web and supported by [confluent](https://docs.confluent.io/current/app-development/kafkacat-usage.html)

It is installed on the _kafkaqamini1_

    zconsult@kafkaqamini1:~/stage/schema-registry-ui$ which kafkacat
    /usr/bin/kafkacat

It is not java based but uses the same low level library as C#, go and Python.

It also provides access to the _old client interface_ (without groups).

Just a couple of examples:

Read data from out topic:

    zconsult@kafkaqamini1:~/stage/schema-registry-ui$ kafkacat -C  -t T2 -b kafkaqa1:9092 
    Third One
    Forth Record
    First Record
    Second Record
    % Reached end of topic T2 [1] at offset 2
    % Reached end of topic T2 [2] at offset 1
    % Reached end of topic T2 [0] at offset 1
 
 Get some info about our kafka cluster and topics.
 
 
    zconsult@kafkaqamini1:~/stage/schema-registry-ui$ kafkacat -b kafkaqa1:9092 -L 
    Metadata for all topics (from broker 1: kafkaqa1:9092/1):
    3 brokers:
    broker 2 at kafkaqa2:9092
    broker 3 at kafkaqa3:9092
    broker 1 at kafkaqa1:9092
    6 topics:
    topic "T2" with 3 partitions:
    partition 0, leader 3, replicas: 3,2,1, isrs: 3,2,1
    partition 2, leader 2, replicas: 2,1,3, isrs: 2,1,3
    partition 1, leader 1, replicas: 1,3,2, isrs: 1,3,2
    topic "T1" with 3 partitions:
    partition 0, leader 1, replicas: 1,3,2, isrs: 1,3,2
    partition 2, leader 3, replicas: 3,2,1, isrs: 1,3,2
    partition 1, leader 2, replicas: 2,1,3, isrs: 1,3,2
    topic "__consumer_offsets" with 50 partitions:
    partition 0, leader 3, replicas: 3,1,2, isrs: 1,3,2
    ..........................................
    topic "DEV_SECOND_MSG_T" with 3 partitions:
    partition 0, leader 1, replicas: 1,2,3, isrs: 1,2,3
    partition 2, leader 3, replicas: 3,1,2, isrs: 3,1,2
    partition 1, leader 2, replicas: 2,3,1, isrs: 2,3,1
    topic "_schemas_qa123" with 1 partitions:
    partition 0, leader 2, replicas: 2,3,1, isrs: 1,3,2
    topic "DEV_FIRST_MSG_T" with 3 partitions:
    partition 0, leader 2, replicas: 2,1,3, isrs: 2,1,3
    partition 2, leader 1, replicas: 1,3,2, isrs: 1,3,2
    partition 1, leader 3, replicas: 3,2,1, isrs: 3,2,1

 
