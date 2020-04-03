Do not forget that JAVA_HOME environment veriable must be set.

        zconsult@kafkaqa1:~$ export JAVA_HOME=~/apps/java

To set up a zookeeper node for the kafka server we do the following:

1.  Connect to the zookeeper.
2.  Create a node.
3.  Check result.

Connect to the zookeeper.

        zconsult@kafkaqa1:~$ ~/kafka/current/bin/zookeeper-shell localhost:2181
        Connecting to localhost:2181
        Welcome to ZooKeeper!
        JLine support is disabled

        WATCHER::

        WatchedEvent state:SyncConnected type:None path:null

Create a node.
When we connect to the newly created _zookeeper ensemble_ it has only one node: _zookeeper_. We can check it using _ls_ command.

    ls /
    [zookeeper]

We also can use _help_ command to get a list of zookeepers' commands

    help
    ZooKeeper -server host:port cmd args
        addauth scheme auth
        close
        config [-c] [-w] [-s]
        connect host:port
        create [-s] [-e] [-c] [-t ttl] path [data] [acl]
        delete [-v version] path
        deleteall path
        delquota [-n|-b] path
        get [-s] [-w] path
        getAcl [-s] path
        history
        listquota path
        ls [-s] [-w] [-R] path
        ls2 path [watch]
        printwatches on|off
        quit
        reconfig [-s] [-v version] [[-file path] | [-members serverID=host:port1:port2;port3[,...]*]] | [-add serverId=host:port1:port2;port3[,...]]* [-remove serverId[,...]*]
        redo cmdno
        removewatches path [-c|-d|-a] [-l]
        rmr path
        set [-s] [-v version] path data
        setAcl [-s] [-v version] [-R] path acl
        setquota -n|-b val path
        stat [-w] path
        sync path
       Command not found: Command not found help

Create a new _kafka_server_ node for our kafka_server.

    create /kafka_server data
    Created /kafka_server

Check the result.
   
   ls /
   [kafka_server, zookeeper]
