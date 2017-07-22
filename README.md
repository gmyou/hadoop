# hadoop

## Requirement
* java 1.6~1.7 recommended but 1.8 successed (hard coding on **./etc/hadoop/hadoop-env.sh**)
```
export JAVA_HOME=/usr/lib/jvm/java-8-oracle
```
## Installation
* install
* hduser user create and add role on **/usr/local/hadoop**
```bash

```
* ssh key-gen and add on authorized_keys
* disable IPv6

## Test
```bash
hduser@ubuntu:/usr/local/hadoop$ bin/hadoop jar ./share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.3.jar wordcount /user/hduser/gutenberg /user/hduser/gutenberg-output
```
** remove and re-test
```bash
hduser@ubuntu:/usr/local/hadoop$ bin/hadoop fs -rmr /user/hduser/gutenberg-output
```
