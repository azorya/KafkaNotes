
**Zookeeper configuration notes.**

We will confugure and check three node _zookeeper ensemble_. That is the prefered way to use zookeeper with kafka cluster. The main differnce is that all the data a shared between all the members of the _ensemble_ making it possible to work with some of our nodes down. 

We have to:

1.  [Configure a single node](#zs_flink_one)
2.  [Propagate  this changes to other nodes]()
3.  [Launch and test our ensemble]()

Do not forget that JAVA_HOME environment veriable must be set.

        zconsult@kafkaqa1:~$ export JAVA_HOME=~/apps/java

To configure a zookeper node we are going to: <a name="zs_flink_one"/>
*  make the [following changes](#zs_flink_one_one) in the zookeeper.properties file.
*  create a [myid file](#zs_flink_one_one).

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
_server.1=10.111.30.26:2888:3888_, and add a couple of more params.

Now we have to add myid file to the /opt/kafka_data/zoo directory. <a name="zs_flink_one_two"/>

    zconsult@kafkaqa1:/opt/kafka/etc/kafka$ ls /opt/kafka_data/zoo
    myid  version-2
    zconsult@kafkaqa1:/opt/kafka/etc/kafka$ cat /opt/kafka_data/zoo/myid 
    1
