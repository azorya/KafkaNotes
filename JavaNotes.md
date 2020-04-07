To run Kafka and all the Kafka utils one needs Java. 
One possible way to deal with this is to install Java as a standard package using your package management
tool (_apt_ or _apt-get_ in case of Debian based Linux) The downside of this method is that this _default_ Java will be in your path and if one wants to switch to the _different_ Java version it it will become more difficult.
That is way we use less intrusive way by simply downloading and extracting Java version we want and pointing out to it.  


Currently we use the following Java 11 package of zulu11.37.17-ca-jdk11.0.6-linux_x64.tar.gz on all our boxes.
In the home directory of our _zconsult_ user we have _apps_ subdir with the following structure:

      zconsult@kafkaqa1:~/apps$ ls -l
      total 4
      lrwxrwxrwx 1 zconsult zconsult   21 Apr  2 14:55 java -> java_versions/current
      drwxrwxr-x 3 zconsult zconsult 4096 Apr  2 14:55 java_versions
