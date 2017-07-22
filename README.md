# hadoop
* [Running Hadoop on Ubuntu Linux (Single-Node Cluster)](http://www.michael-noll.com/tutorials/running-hadoop-on-ubuntu-linux-single-node-cluster/)
* [Hadoop: Setting up a Single Node Cluster](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-common/SingleCluster.html)

## Requirement
* Ubuntu 16.04
* Java 1.8
* Hadoop 2.7.3

## Installation
* install hadoop
```bash
$ sudo wget http://apache.mirror.cdnetworks.com/hadoop/core/hadoop-2.7.3/hadoop-2.7.3.tar.gz
$ sudo tar -xvzf hadoop-2.7.3.tar.gz
$ sudo mv hadoop-2.7.3 hadoop
```
* disable IPv6 on `/etc/sysctl.conf`
```
# disable ipv6
net.ipv6.conf.all.disable_ipv6 = 1
net.ipv6.conf.default.disable_ipv6 = 1
net.ipv6.conf.lo.disable_ipv6 = 1
```
* check command ( **0** means IPv6 is enabled, **1** means disabled )
```bash
$ cat /proc/sys/net/ipv6/conf/all/disable_ipv6
```


* hduser user create and add role on `/usr/local/hadoop`
```bash
$ sudo addgroup hadoop
$ sudo adduser --ingroup hadoop hduser
$ sudo hown -R hduser:hadoop /usr/local/hadoop 
```
* ssh key-gen and add on authorized_keys
```bash
$ su - hduser
$ ssh-keygen -t rsa -P ""
$ cat $HOME/.ssh/id_rsa.pub >> $HOME/.ssh/authorized_keys
```

## Configuration
* java 1.6~1.7 recommended but 1.8 successed (hard coding on `./etc/hadoop/hadoop-env.sh`)
```
export JAVA_HOME=/usr/lib/jvm/java-8-oracle
```
* create directory and set the required ownerships and permissions.
```bash
$ sudo mkdir -p /app/hadoop/tmp
$ sudo chown hduser:hadoop /app/hadoop/tmp
# ...and if you want to tighten up security, chmod from 755 to 750...
$ sudo chmod 750 /app/hadoop/tmp
```
* `core-site.xml`
* `mapred-site.xml`
* `hdfs-site.xml`

## Test
```bash
hduser@ubuntu:/usr/local/hadoop$ bin/start-dfs.sh
hduser@ubuntu:/usr/local/hadoop$ bin/hadoop jar ./share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.3.jar wordcount /user/hduser/gutenberg /user/hduser/gutenberg-output
hduser@ubuntu:/usr/local/hadoop$ bin/stop-dfs.sh
```
* remove output for re-test
```bash
hduser@ubuntu:/usr/local/hadoop$ bin/hadoop fs -rmr /user/hduser/gutenberg-output
```

* view output
```bash
$ bin/hadoop dfs -ls /user/hduse
$ bin/hadoop dfs -ls /user/hduser/gutenberg-output
$ bin/hadoop dfs -cat /user/hduser/gutenberg-output/part-r-00000
```

## Web Interface
```bash
$ sudo netstat -plten | grep java
```

* [Total](http://{IP}:50070)
* [DataNode](http://{IP}:50075/)
* [Status](http://{IP}:50090)
