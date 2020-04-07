**Basic Installation Notes.**

We have the same [Confluent](https://www.confluent.io/) community Kafka installation on all our boxes.
Currently we use confluent-community-5.4.1-2.12.tar.gz <a name="bin_flink_zero"/> package.

We use similar approach to install Kafka that we use for [Java](./JavaNotes.md).

Our steps are :

1. [Prepare all the directories and links](#bin_flink_one)
2. [Configure Zookeeper]()
3. [Configure Kafka]()
4. [Configure Schema Registry]()




***Directories and links.*** <a name="bin_flink_one"/>
As was mentioned above we use manual installation. We extract our [package](#bin_flink_zero) to the home directory of _zconsult_ user. Please note _kafka_ and [_kafka_data_](#bin_flink_two) direcories.

    zconsult@kafkaqa3:~$ ls ~
    apps  bin  kafka  kafka_data  test

In the _kafka_ directory habe have _current_ link pointing to our extracted package:

    zconsult@kafkaqa3:~$ ls -l kafka
    total 280828
    drwxr-xr-x 8 zconsult zconsult      4096 Apr  3 06:08 confluent-5.4.1
    -rw-rw-r-- 1 zconsult zconsult 287561707 Apr  2 14:42 confluent-community-5.4.1-2.12.tar.gz
    lrwxrwxrwx 1 zconsult zconsult        16 Apr  2 14:43 current -> confluent-5.4.1/

In the _kafka_ directoy we have the standard directory structure:

    zconsult@kafkaqa3:~$ cd ~/kafka/current
    zconsult@kafkaqa3:~/kafka/current$ ls -l
    total 28
    drwxr-xr-x  3 zconsult zconsult 4096 Apr  2 14:43 bin
    drwxr-xr-x 13 zconsult zconsult 4096 Apr  2 14:45 etc
    drwxr-xr-x  3 zconsult zconsult 4096 Apr  2 14:43 lib
    drwxrwxr-x  2 zconsult zconsult 4096 Apr  7 12:07 logs
    -rw-r--r--  1 zconsult zconsult  871 Apr  2 14:43 README
    drwxr-xr-x  6 zconsult zconsult 4096 Apr  2 14:43 share
    drwxr-xr-x  2 zconsult zconsult 4096 Apr  2 14:43 src

The _bin_ directory contains all the _shell files_ we are goinf to use to launch our _daemons/services_ as well as command line tools. That is it all our executables are in fact shell scripts that will launch java applications. The actual _.jar_ files are located in the _share/java_ subdirectory.

The _etc_ directory contains all our configuration files. The _lib_ dir contains examples of systemd service files. The _logs_ dir will cbe the place where zookeeper, kafka & schema-registry deamons will write their log files.

Finally _src_ directory has a few archives, which will be used later in the development.

****Kafka data**** <a name="bin_flink_two"/>

Kafka_data directory in our configuration is the place where kafka and zookeeper keep/write their data (messages for the Kafka and its database like structures for the zookeeper).

kafka_data dir has two sub-dirs _kafka_ and _zoo_:

    consult@kafkaqa3:~/kafka_data$ ls -l
    total 8
    drwxrwxr-x 5 zconsult zconsult 4096 Apr  7 13:29 kafka
    drwxrwxr-x 3 zconsult zconsult 4096 Apr  3 06:08 zoo

In the _kafka_ dir is for Kafka daemon, _zoo_ is for the zookeeper one.

In the production environment it is much better to configure Kafka to write to the separate hdd, but on some of our boxes we currently have only one. Also please note that zookeeper could be configured to keep its data in a completely separate place. To simplify our installation/configuration process we put them (kafka & zookeeper) data directories in one place.




 
