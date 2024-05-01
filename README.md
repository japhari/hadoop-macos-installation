## INSTALL HADOOOP ON MACOS

### Step 1: Install Homebrew

Homebrew simplifies software installation on macOS. Open Terminal and run:

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### Step 2: Install Java

Hadoop requires Java. Install OpenJDK using Homebrew:

```
brew install openjdk
```

### Step 3: Set Java Home

Set the JAVA_HOME environment variable in `~/.bash_profile` or `~/.zshrc`:

```
export JAVA_HOME=/usr/local/opt/openjdk
```

### Step 4: Verify Java Installation

Verify Java installation by running:

```
java -version
```

### Step 5: Download and Install Hadoop

Download and install Hadoop using Homebrew:

```
brew install hadoop

```

Confirm the Installation folder

```
cd /usr/local/Cellar/hadoop/3.4.0/libexec/etc/hadoop
pwd
/usr/local/Cellar/hadoop/3.4.0/libexec/etc/hadoop

```

This is the installation path - According to this version
/usr/local/Cellar/hadoop/3.4.0/libexec/etc/hadoop

Configure environment varriable in ~/ .bash_profile

open .bash_profile by using any editor of your choice.

```
sudo vim ~/.bash_profile

```

Change HADOOP_HOME to match the installation path

```
# Hadoop
export HADOOP_HOME=/usr/local/Cellar/hadoop/3.4.0/libexec/etc/hadoop
export HADOOP_INSTALL=$HADOOP_HOME
export HADOOP_MAPRED_HOME=$HADOOP_HOME
export HADOOP_COMMON_HOME=$HADOOP_HOME
export HADOOP_HDFS_HOME=$HADOOP_HOME export YARN_HOME=$HADOOP_HOME
export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
export PATH=$PATH:$HADOOP_HOME/sbin:$HADOOP_HOME/bin
export HADOOP_OPTS="-Djava.library.path=$HADOOP_HOME/lib/nativ"

```

Refresh Environment by running

```
source ~/.bash_profile

```

### Step 6: Configure Hadoop

Navigate to Hadoop's configuration directory and modify XML files like `core-site.xml`, `hdfs-site.xml`, and `mapred-site.xml`.

Change Directory to Installation folder to finish the configurations

```
cd /usr/local/Cellar/hadoop/3.4.0/libexec/etc/hadoop

```

Add JAVA_HOME to the Hadoop Environment

```
sudo vim hadoop-env.sh

```

Uncomment export JAVA_HOME and add path to your JAVA_HOME path - How to know your JAVA_HOME path

```
echo $JAVA_HOME
/Library/Java/JavaVirtualMachines/jdk1.8.0_202.jdk/Contents/Home

```

So after change the file to
export JAVA_HOME =/Library/Java/JavaVirtualMachines/jdk1.8.0_202.jdk/Contents/Home
N:B Match your JAVA_HOME path
After that save the file

Then edit core-site.xml

```
sudo vim core-site.xml

```

Add the following configuration in the <configuration><configuration>
N:B If port 9000 is used on your device change to another port 9001 or any

```
<configuration>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://localhost:9000</value>
    </property>
</configuration>

```

Save then
Edit hdfs-site.xml

```
sudo vim hdfs-site.xml

```

Add the following configuration in the <configuration><configuration>

```
<configuration>
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
</configuration>

```

Save then
Edit mapred-site.xml

```
sudo vim mapred-site.xml

```

Add the following configuration in the <configuration><configuration>

```
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

Save then
Edit yarn-site.xml

```
sudo vim yarn-site.xml

```

Add the following configuration in the <configuration><configuration>

```
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

### Step 7: Enable SSH to localhost

Go to System Setting -> General -> Remote Login and click i to allow access for only these Users as shown here
![Access](/images/access.png)

Create security for SSH - But if you have SSH key dont run first command

```
ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa

```

```
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys

```

```
chmod 0600 ~/.ssh/id_rsa.pub

```

```
ssh localhost


```

If you dont get any error youre good to go

### Step 8: Format Hadoop Filesystem

Format HDFS before starting Hadoop services:

```
hdfs namenode -format
```

### Step 9: Start Hadoop Services

Start Hadoop services:

```
start-all.sh
```

Run jps to check if all datanodes and namenodes are running

```
jps
```

![Jps](/images/jps.png)

### Step 10: Verify Installation

Access Hadoop Namenode web interface at http://localhost:9870 and verify the cluster status.
Go to Utilities -> Browse File System
At Beginning you might see empty, so back to the terminal and create
N:B After User you can enter any name - depends on you.
![Jps](/images/hadoop_user.png)

```
hdfs dfs -mkdir -p /user/Hadoop/twitter_data
```

To load Data go to the csv folder and change demo to name of your csv

```
hadoop fs -put demo.csv /user/Hadoop/twitter_data

```

![Jps](/images/csv_data.png)

### Step 11: Test Hadoop

list the contents of the Hadoop file system root directory

```
hadoop fs -ls hdfs:///

```

```
hadoop fs -ls /

```

Display the File Size

```
hdfs dfs -ls -h /user

```

List of Directories

```
hdfs dfs -ls -R /user

```

Display Content of a file

```
hdfs dfs -cat /user/Hadoop/twitter_data/demo.csv

```

Run sample MapReduce jobs to ensure Hadoop is functioning correctly.

That's it! You've successfully installed Hadoop on macOS.

## INSTALL PIG ON Mac

Before Installing Apache Pig make sure the jps is running

```
jps

```

```
1633
52979 ResourceManager
53063 NodeManager
67961 Jps
53368 DataNode
53272 NameNode
53470 SecondaryNameNode

```

Download the Apache Pig from the Official Website https://dlcdn.apache.org/pig/pig-0.17.0/pig-0.17.0.tar.gz (pig-0.17.0)
N:B Replace the appropriate version
Extract folder and move pig-0.17.0 from Downloads to /Users/<your-mac-user>/pig-0.17.0

Then Open Configuration Environment and add

sudo vim ~/.bash_profile

```
#PIG VARIABLES
export PIG_HOME=/Users/<your-mac-user>/pig-0.17.0
export PATH=$PATH:$PIG_HOME/bin
export PIG_CLASSPATH=$PIG_HOME/conf:$HADOOP_INSTALL/etc/hadoop/bin
export PIG_CONF_DIR=$PIG_HOME/conf
export PIG_CLASSPATH=$PIG_CONF_DIR
#PIG VARIABLES END

```

N:B Change <your-mac-user> with your user
Then go to terminal and type

```
source ~/.bashrc

```

### Test Apache Pig

```
pig

```

![Jps](/images/pig.png)
If it show grunt>
Then Done Apache Pig is Up

## INSTALL Apache Sqoop ON Mac

Download the Apache Sqoop from the Official Website https://archive.apache.org/dist/sqoop/1.4.4/sqoop-1.4.4.bin__hadoop-0.20.tar.gz
N:B Replace the appropriate version
Extract folder and move sqoop-1.4.4 from Downloads to /Users/<your-mac-user>/sqoop-1.99.7

Then Open Configuration Environment and add

sudo vim ~/.bash_profile

```
#APACHE SCOOP  VARIABLES
export SQOOP_HOME=/Users/<your-mac-user>/sqoop-1.4.4
export PATH=$PATH:$SQOOP_HOME/bin

```

N:B Change <your-mac-user> with your user
Then go to terminal and type

```
source ~/.bashrc

```

Then Open Sqoop Environment Environment and add

```
sudo cp /Users/<your-mac-user>/sqoop-1.4.4/conf/sqoop-env-template.sh sqoop-env.sh
sudo vim /Users/<your-mac-user>/sqoop-1.4.4/conf/sqoop-env.sh

```

Add this two path

```
export HADOOP_COMMON_HOME=/Users/<your-mac-user>/hadoop-3.4.0
export HADOOP_MAPRED_HOME=/Users/<your-mac-user>/hadoop-3.4.0

```

Then Save
N:B Hadoop Version must match with your hadoop version
If it on other file link the correct path

Then Download mysql-connector https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-j-8.4.0.tar.gz
Extract the file and copy the jar file to the lib in the scoop folder - Remember it must be inside the lib

Then Now you can test it
N:B Make sure you have mysql user with access to test it

```
cd /Users/<your-mac-user>/sqoop-1.4.4.4/bin
sqoop list-databases --connect jdbc:mysql://localhost/ --username root -P

```

![Jps](/images/sqoop.png)
