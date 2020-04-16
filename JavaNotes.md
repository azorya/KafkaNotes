## Java Notes

To run Kafka and all the Kafka utilities one needs Java. 
One possible way to deal with this is to install Java as a standard package using your package management
tool (_apt_ or _apt-get_ in case of Debian based Linux) The downside of this method is that this _default_ Java will be in your path, making switching to a _different_ Java version more difficult.
That is why we use a _less intrusive_ way by  downloading and extracting the Java version we want and pointing $JAVA_HOME to it.  

We installed Java 11 package zulu11.37.17-ca-jdk11.0.6-linux_x64.tar.gz on all our servers for this project.
In the home directory of _zconsult_ user, we have _apps_ sub-directory with the following structure:

      zconsult@kafkaqa1:~/apps$ ls -l
      total 4
      lrwxrwxrwx 1 zconsult zconsult   21 Apr  2 14:55 java -> java_versions/current
      drwxrwxr-x 3 zconsult zconsult 4096 Apr  2 14:55 java_versions

The _java_versions_ directory looks like this:
   
    zconsult@kafkaqa1:~/apps$ ls -l java_versions/
    total 4
    lrwxrwxrwx  1 zconsult zconsult   36 Apr  2 14:55 current -> zulu11.37.17-ca-jdk11.0.6-linux_x64/
    drwxrwxr-x 10 zconsult zconsult 4096 Apr  2 14:55 zulu11.37.17-ca-jdk11.0.6-linux_x64

As one can see symlink  _~/apps/java_ points to the _current_ in the _java_versions_ and symlink _current_ points to the extracted Java package.

To avoid referring to _zconsult_ home directory we use one more level of references from the _/opt_ directory.

    zconsult@kafkaqa1:~/apps$ ls /opt
    kafka  kafka_data  kafka_java
    zconsult@kafkaqa1:~/apps$ ls -l /opt/kafka_java
    lrwxrwxrwx 1 root root 24 Apr  6 18:30 /opt/kafka_java -> /home/zconsult/apps/java

Our _kafka_ related _java_ points to the _zconsult/apps/java_. If we change _current_ setting for the zconsult later on, or start using a completly different java installation, all our prepared scripts will not be changed.

Please note that to use Kafka & Kafka utilities one doesn't have to include Java in the PATH. However, one must define JAVA_HOME environment variable.

    zconsult@kafkaqa1:~/apps$ echo $JAVA_HOME

    zconsult@kafkaqa1:~/apps$ export JAVA_HOME=/opt/kafka_java
