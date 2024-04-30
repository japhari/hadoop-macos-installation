# INSTALL HADOOOP ON MACOS

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

### Step 6: Configure Hadoop

Navigate to Hadoop's configuration directory and modify XML files like `core-site.xml`, `hdfs-site.xml`, and `mapred-site.xml`.

### Step 7: Format Hadoop Filesystem

Format HDFS before starting Hadoop services:

```
hdfs namenode -format
```

### Step 8: Start Hadoop Services

Start Hadoop services:

```
start-all.sh
```

### Step 9: Verify Installation

Access Hadoop Namenode web interface at http://localhost:9870 and verify the cluster status.

### Step 10: Test Hadoop

Run sample MapReduce jobs to ensure Hadoop is functioning correctly.

That's it! You've successfully installed Hadoop on macOS.
