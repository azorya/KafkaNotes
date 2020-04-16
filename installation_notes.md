## Installation Notes

We use [Confluent](https://www.confluent.io/) community Kafka installation on all our boxes.
Current version is confluent-community-5.4.1-2.12.tar.gz <a name="bin_flink_zero"/>.

We use similar approach to install the Kafka that we use for [Java](./JavaNotes.md).

Installation involves the following steps:

1. [Prepare all directories and links](#bin_flink_one)
2. [Configure Zookeeper](./zookeeper_settings.md)
3. [Configure Kafka](./kafka_settings.md)
4. [Configure Schema Registry](./schema_registry_settings.md)


### Directories and links <a name="bin_flink_one"/>
As was mentioned above, we use manual installation. We extract our [package](#bin_flink_zero) to the home directory of _zconsult_ user. Please note that the _kafka_ and [_kafka_data_](#bin_flink_two) directories hold most of code and data for this project.

    zconsult@kafkaqa3:~$ ls ~
    apps  bin  kafka  kafka_data  test

In the _kafka_ directory, the _current_ link points to the extracted Confluent package:

    zconsult@kafkaqa3:~$ ls -l kafka
    total 280828
    drwxr-xr-x 8 zconsult zconsult      4096 Apr  3 06:08 confluent-5.4.1
    -rw-rw-r-- 1 zconsult zconsult 287561707 Apr  2 14:42 confluent-community-5.4.1-2.12.tar.gz
    lrwxrwxrwx 1 zconsult zconsult        16 Apr  2 14:43 current -> confluent-5.4.1/

Similar to [java](./JavaNotes.md) we made our installation visible and accessable via _/opt_ dir:

    consult@kafkaqa3:~$ ls -l /opt
    total 0
    lrwxrwxrwx 1 root root 28 Apr  3 05:37 kafka -> /home/zconsult/kafka/current
    lrwxrwxrwx 1 root root 26 Apr  3 04:43 kafka_data -> /home/zconsult/kafka_data/
    lrwxrwxrwx 1 root root 24 Apr  6 18:35 kafka_java -> /home/zconsult/apps/java

So _/opt/kafka_ refers to the _current_ kafka installation in the zconsult user home dir. All our configuration changes must reference this _externally visible_ path via _/opt_.

In the _kafka_ directory we have the following directory structure:

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

The _bin_ directory contains all the _shell files_ we are going to use to launch our _daemons/services_ as well as command line tools. That is all our Kafka management executables are in fact shell scripts that will launch java applications. The actual _.jar_ files are located in the _share/java_ subdirectory.

The _etc_ directory contains all kafka configuration files. The _lib_ dir contains examples of systemd service files. The _logs_ dir will cbe the place where zookeeper, kafka & schema-registry deamons will write their application log files (not to be confused with log file that are Kafka backbone and contain records of all messages).

Finally _src_ directory has a few archives, which will be used later in the development.

Confluence comes with default settings for running Kafka on a localhost. In the following we will use __git__ to make our changes to the config easily visible.
First let us _cd_ to the _etc_ directory and run and look.

    zconsult@kafkaqa1:~/kafka/current/etc$ cd ~/kafka/current/etc
    zconsult@kafkaqa1:~/kafka/current/etc$ ls -l
    total 40
    drwxr-xr-x 2 zconsult zconsult 4096 Apr  2 14:33 confluent-common
    drwxr-xr-x 2 zconsult zconsult 4096 Apr  6 19:11 kafka
    drwxr-xr-x 2 zconsult zconsult 4096 Apr  2 14:33 kafka-connect-elasticsearch
    drwxr-xr-x 2 zconsult zconsult 4096 Apr  2 14:33 kafka-connect-jdbc
    drwxr-xr-x 2 zconsult zconsult 4096 Apr  2 14:33 kafka-connect-s3
    drwxr-xr-x 2 zconsult zconsult 4096 Apr  2 14:33 kafka-connect-storage-common
    drwxr-xr-x 2 zconsult zconsult 4096 Apr  2 14:33 kafka-rest
    drwxr-xr-x 2 zconsult zconsult 4096 Apr  2 14:33 ksql
    drwxr-xr-x 2 zconsult zconsult 4096 Apr  2 14:33 rest-utils
    drwxr-xr-x 2 zconsult zconsult 4096 Apr  2 14:33 schema-registry

All the kafka daemons (kafka, zookeeper, schema-registry) and _connectors_ configurations are stored in this directories.

Once repository is setup using _"git init"_ command,  and changes committed with_"git add *"_ and _"git commit"_ commands, all modified  configuration files will be visible via _git status_ command:
 
    zconsult@kafkaqa1:~/kafka/current/etc$ git status
    On branch master
    Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   kafka/server.properties
	modified:   kafka/zookeeper.properties

Further,  _git diff_ will show exactly _what_ was changed. Example below is a partial output:

    zconsult@kafkaqa1:~/kafka/current/etc$ git diff  kafka/server.properties
    diff --git a/kafka/server.properties b/kafka/server.properties
    index fc9ba00..b2fcd8f 100644
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
 
All the changes are easily visible.

We are ready to configure and launch [zookeeper](./zookeeper_settings.md), [kafka](./kafka_settings.md) and [schema_registry](./schema_registry_settings.md).

#### Kafka data location<a name="bin_flink_two"/>

Kafka_data directory in our configuration is the place where kafka and zookeeper keep/write their data (messages for the Kafka and its database like structures for the zookeeper).

kafka_data dir has two sub-dirs _kafka_ and _zoo_:

    consult@kafkaqa3:~/kafka_data$ ls -l
    total 8
    drwxrwxr-x 5 zconsult zconsult 4096 Apr  7 13:29 kafka
    drwxrwxr-x 3 zconsult zconsult 4096 Apr  3 06:08 zoo

_kafka_ dir is for the Kafka daemon, _zoo_ is for the zookeeper one.

In the production environment it is better to configure Kafka to write to the separate hdd, but on some of our boxes we currently have only one. 

Also, please note that zookeeper can be configured to keep its data in a completely separate place. To simplify our installation/configuration process, we put the kafka & zookeeper data directories in the one place.




 
