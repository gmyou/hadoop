# hadoop

## Requirement
* java 1.6~1.7 recommended but 1.8 successed (hard coding on `./etc/hadoop/hadoop-env.sh`)
```
export JAVA_HOME=/usr/lib/jvm/java-8-oracle
```
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

