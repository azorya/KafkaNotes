
**Zookeeper configuration notes.**

We will confugure and check three node _zookeeper ensemble_. That is the prefered way to use zookeeper with kafka cluster. The main differnce is that all the data a shared between all the members of the _ensemble_ making it possible to work with some of our nodes down. 

We have to:

1.  [Configure a single node](#zs_flink_one)
2.  [Propagate  this changes to other nodes](#zs_flink_two)
3.  [Launch and test our ensemble](#zs_flink_three)

Do not forget that JAVA_HOME environment veriable must be set.

        zconsult@kafkaqa1:~$ export JAVA_HOME=~/apps/java

***Zookeeper node changes.*** <a name="zs_flink_one"/>

To configure a zookeper node we are going to: 
*  [make the following changes](#zs_flink_one_one) in the zookeeper.properties file.
*  [create a myid file](#zs_flink_one_one).

****zookeeper.properties****

In our case _zookeeper.properties_ file is visible through _/opt_

     zconsult@kafkaqa1:/opt/kafka/etc/kafka$ cd /opt/kafka/etc/kafka
     zconsult@kafkaqa1:/opt/kafka/etc/kafka$ ls -l zookeeper.properties
    -rw-r--r-- 1 zconsult zconsult 1372 Apr  3 05:59 zookeeper.properties

Let us examine our changes using **git diff** command: <a name="zs_flink_one_one"/>
     
    git diff zookeeper.properties
    diff --git a/kafka/zookeeper.properties b/kafka/zookeeper.properties
    index 90f4332..d48d7de 100644
    --- a/kafka/zookeeper.properties
    +++ b/kafka/zookeeper.properties
    @@ -13,11 +13,20 @@
    # See the License for the specific language governing permissions and
    # limitations under the License.
    # the directory where the snapshot is stored.
    -dataDir=/tmp/zookeeper
    +dataDir=/opt/kafka_data/zoo
    # the port at which the clients will connect
    clientPort=2181
    # disable the per-ip limit on the number of connections since this is a non-production config
    maxClientCnxns=0
    +
    +# to be changed later to proper names
    +server.1=10.111.30.26:2888:3888
    +server.2=10.111.30.27:2888:3888
    +server.3=10.111.30.28:2888:3888
    +
    +
    +initLimit=20
    +syncLimit=5
    # Disable the adminserver by default to avoid port conflicts.
    # Set the port to something non-conflicting if choosing to enable this
    admin.enableServer=false
    zconsult@kafkaqa1:/opt/kafka/etc/kafka$

We changed the _DataDir_ location from /tmp/zookeeper to the _/opt/kafka_data/zoo_, add  three lines describing our servers:
_server.1=10.111.30.26:2888:3888_, and added a couple of more params.

****myid file**** <a name="zs_flink_one_two"/>
Now we have to add myid file to the /opt/kafka_data/zoo directory. 

    zconsult@kafkaqa1:/opt/kafka/etc/kafka$ ls /opt/kafka_data/zoo
    myid  version-2
    
The content of this file is a single number (id of our zookeeper node).

    zconsult@kafkaqa1:/opt/kafka/etc/kafka$ cat /opt/kafka_data/zoo/myid 
    1


***propagate changes to all the nodes.*** <a name="zs_flink_two"/> 

We have to make similar changes (or simply _scp_ them) to all our nodes. 
Please note that the content of _myid_ file is unique for each node.

On the kafkaqa2:

    zconsult@kafkaqa2:/home$ cat /opt/kafka_data/zoo/myid
    2

On the kafkaqa3:

    zconsult@kafkaqa3:~$ cat /opt/kafka_data/zoo/myid
    3

***Launch and test.*** <a name="zs_flink_three"/> 
to test




