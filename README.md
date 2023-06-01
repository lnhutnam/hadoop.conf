# hadoop.conf

## Install Java

Recommended Java 8

```bash
sudo pacman -S jdk8-openjdk
```

## Install and configure OpenSSH

```bash
sudo pacman -S openssh
```

```bash
ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
chmod 0600 ~/.ssh/authorized_keys
```

## Download Hadoop

```bash
 wget https://dlcdn.apache.org/hadoop/common/hadoop-3.3.4/hadoop-3.3.4.tar.gz
 shasum -a 512 hadoop-3.3.4.tar.gz
 tar xzf hadoop-3.3.4.tar.gz
```

## Configuration for .zshrc and .bashrc

For .zshrc

```bash
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk
export HADOOP_HOME="/home/lnhutnam/opt/hadoop-3.3.4"
export HADOOP_CONF_DIR=${HADOOP_HOME}/etc/hadoop
export HADOOP_INSTALL=$HADOOP_HOME
export HADOOP_MAPRED_HOME=$HADOOP_HOME
export HADOOP_COMMON_HOME=$HADOOP_HOME
export HADOOP_HDFS_HOME=$HADOOP_HOME
export YARN_HOME=$HADOOP_HOME
export HADOOP_COMMON_LIB_NATIVE_DIR="$HADOOP_HOME/lib/native"
export PATH="$PATH:$HADOOP_HOME/sbin:$HADOOP_HOME/bin"
export HADOOP_OPTS="-Djava.library.path=$HADOOP_HOME/lib/native"
```

For .bashrc

```bash
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk
export HADOOP_HOME="/home/lnhutnam/opt/hadoop-3.3.4"
export HADOOP_CONF_DIR=${HADOOP_HOME}/etc/hadoop
export HADOOP_INSTALL=$HADOOP_HOME
export HADOOP_MAPRED_HOME=$HADOOP_HOME
export HADOOP_COMMON_HOME=$HADOOP_HOME
export HADOOP_HDFS_HOME=$HADOOP_HOME
export YARN_HOME=$HADOOP_HOME
export HADOOP_COMMON_LIB_NATIVE_DIR="$HADOOP_HOME/lib/native"
export PATH="$PATH:$HADOOP_HOME/sbin:$HADOOP_HOME/bin"
export HADOOP_OPTS="-Djava.library.path=$HADOOP_HOME/lib/native"
```

## Configuration for core-site.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<configuration>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://localhost:9000</value>
    </property>
</configuration>
```

## Configuration for hadoop-env.sh

```bash
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk
export HADOOP_HOME="/home/lnhutnam/opt/hadoop-3.3.4"
export HADOOP_CONF_DIR=${HADOOP_HOME}/etc/hadoop
export HADOOP_OS_TYPE=${HADOOP_OS_TYPE:-$(uname -s)}

export HDFS_NAMENODE_USER=root
export HDFS_DATANODE_USER=root
export HDFS_SECONDARYNAMENODE_USER=root
export YARN_RESOURCEMANAGER_USER=root
export YARN_NODEMANAGER_USER=root
```

## Configuration for hdfs-site.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<configuration>
    <property>
      <name>dfs.permissions</name>
      <value>false</value>
    </property>
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
    <property>
      <name>dfs.namenode.http-address</name>
      <value>localhost:50070</value>
    </property>
</configuration>
```

## Configuration for mapred-site.xml

```xml
<?xml version="1.0"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<configuration>
    <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>
    <property>
        <name>mapreduce.application.classpath</name>
        <value>$HADOOP_MAPRED_HOME/share/hadoop/mapreduce/*:$HADOOP_MAPRED_HOME/share/hadoop/mapreduce/lib/*</value>
    </property>
</configuration>
```

## Configuration for yarn-site.xml

```xml
<?xml version="1.0"?>
<configuration>
    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
    <property>
        <name>yarn.nodemanager.env-whitelist</name>
        <value>JAVA_HOME,HADOOP_COMMON_HOME,HADOOP_HDFS_HOME,HADOOP_CONF_DIR,CLASSPATH_PREPEND_DISTCACHE,HADOOP_YARN_HOME,HADOOP_HOME,PATH,LANG,TZ,HADOOP_MAPRED_HOME</value>
    </property>
</configuration>
```

## Test

Check your namenode at [http://localhost:50070/dfshealth.html#tab-overview](http://localhost:50070/dfshealth.html#tab-overview)

Check your resource manager at [http://localhost:8088/cluster](http://localhost:8088/cluster)

```bash
hdfs namenode -format

sudo start-dfs.sh
sudo start-yarn.sh
```
