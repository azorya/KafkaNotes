**Kafka Settings**

We will confugure a three node kafka cluster. That is the smallest possible setup which can work as a _cluster_. The main differnce between a _cluster_ and a _standalone kafka_ is that it is possible to work with some of our nodes down. 

We have to:

1.  [Configure a single node](#ks_flink_one)
2.  [Propagate  this changes to other nodes](#ks_flink_two)
3.  [Launch and test our cluster](#ks_flink_three)



***kafka node config*** <a name="ks_flink_one"/>

