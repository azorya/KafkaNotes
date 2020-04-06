We have to create a zookeeper node for CMAK app

     zconsult@kafkaqa3:/opt$ /opt/kafka/bin/zookeeper-shell localhost:2181 
     Connecting to localhost:2181
     Welcome to ZooKeeper!
     JLine support is disabled

     WATCHER::

    WatchedEvent state:SyncConnected type:None path:null
    ls /
   [apps, zookeeper]
   ls /apps
  [kafka_server]
  create /apps/cmak data
  Created /apps/cmak
