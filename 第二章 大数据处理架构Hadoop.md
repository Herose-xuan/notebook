# 第二章 大数据处理架构Hadoop

## 2.0Hadoop的由来

​		**2003**年，**Google**发表了一篇技术学术论文，公开介绍了自己的谷歌文件系统**GFS**（**Google File System**）。这是**Google**公司为了存储海量搜索数据而设计的专用文件系统。
  第二年，也就是**2004**年，**Doug Cutting**基于**Google**的**GFS**论文，实现了**分布式文件存储系统**，并将它命名为**NDFS**（**Nutch Distributed File System**）

​		还是**2004**年，**Google**又发表了一篇技术学术论文，介绍自己的**MapReduce**编程模型，主要用于大规模数据集（大于**1TB**）的并行分析运算。
  第二年（**2005**年），**Doug Cutting**又基于**MapReduce**，在**Nutch**搜索引擎实现了上述功能

​		或许是为了给新上任的自己先冲点业绩，加盟**Yahoo**之后，**Doug Cutting**将**NDFS**和**MapReduce**进行了升级改造，并重新命名为**Hadoop**（**NDFS**也改名为**HDFS**，全称是**Hadoop Distributed File System**）。

  这就是后来大名鼎鼎的大数据框架系统**Hadoop**的由来。而**Doug Cutting**，则被人们称为**Hadoop**之父

## 2.1概述

#### 1、Hadoop简介

​       Hadoop是Apache软件基金会旗下的一个开源分布式计算平台，为用户提供了系统底层细节透明的分布式基础架构。**Hadoop是基于**Java**语言开发的，具有很好的**跨平台特性**，并且可以**部署在廉价的计算机集群**中。
  Hadoop的核心是**分布式文件系统HDFS**（Hadoop Distributed File System）和 **MapReduce**。HDFS是对谷歌文件系统(Google File System，GFS）的开源实现，是面向普通硬件环境的分布式文件系统，具有较高的读写速度、很好的容错性和可伸缩性，支持大规模数据的分布式存储，其冗余数据存储的方式，很好地保证了数据的安全性。MapReduce是针对谷歌MapReduce的开源实现，允许用户在不了解分布式系统底层细节的情况下开发并行应用程序，采用MapReduce来整合分布式文件系统上的数据，可保证分析和处理数据的高效性。借助于Hadoop，程序员可以轻松地编写分布式并行程序，可将其运行于廉价计算机集群上，完成海量数据的存储与计算

#### 2、hadoop的特性

 Hadoop是一个能够对大量数据进行分布式处理的软件框架，并且是以一种可靠、高效、可伸缩的方式进行数据处理，它具有以下几个方面的特性：

- **高可靠性：**采用冗余数据存储方式，即使一个副本发生故障，其他副本也可以保证正常对外提供服务。Hadoop按位存储和处理数据的能力，值得人们信赖。
- **高效性：**作为并行分布式计算平台，Hadoop采用分布式存储和分布式处理两大核心技术，能够高效地处理PB级数据。Hadoop能够在节点之间动态地移动数据，并保证各个节点的动态平衡，因此处理速度非常快。
- **高可扩展性：**Hadoop的设计目标是可以高效稳定地运行在廉价的计算机集群上，可以扩展到数以千计的计算机节点。
- **高容错性：**采用冗余数据存储方式，自动保存数据的多个副本，并且能够自动将失败的任务进行重新分配。
- **成本低：**Hadoop采用廉价的计算机集群，成本较低，普通用户也很容易用自己的PC上搭建Hadoop运行环境。与一体机、商用数据仓库以及QlikView、Yonghong Z-Suite等数据集市相比，Hadoop是开源的，项目的软件成本因此会大大降低。
- **运行在Linux平台上：**Hadoop是基于Java语言开发的，可以较好地运行在Linux平台上。
- **支持多种编程语言：**Hadoop上的应用程序也可以使用其他语言编写，如C++。

## 2.2 Hadoop的项目架构

![](https://datawhalechina.github.io/juicy-bigdata/images/ch02/ch2.2.png)

- Common
    Common是为Hadoop其他子项目提供支持的常用工具，它主要包括**FileSystem、RPC和串行化库**，它们为在廉价的硬件上搭建云计算环境提供了基本的服务，并为运行在该平台上的软件开发提供了所需的API。
- Avro
    用于数据库序列化的系统，它提供了丰富的数据结构类型、快速可压缩的二进制数据格式、存储持久性数据的文件集、远程调用RPC的功能和简单的动态语言集成功能，其中代码生成器即不需要读写文件数据，也不需要使用或者实现RPC协议，它只是一个**可选的对静态类型语言的实现**。Hadoop的其他子项目（如HBase和Hive）的客户端与服务端之间的数据传输都采用了Avro。
      **Avro系统依赖于模式**，数据的读和写是在模式之下完成的，这样可以减少写入数据的开销，提高序列化的速度并缩减其大小，同时也可以方便动态脚本语言的使用，因为数据连同其模式都是自描述的。
      在RPC中，Avro系统客户端和服务器端通过握手协议进行模式交换，因此当客户端和服务器拥有彼此所有的模式时，不同模式下相同命名字段、丢失字段和附加字段等信息的一致性问题得以解决。
- HDFS
    Hadoop分布式文件系统（Hadoop Distributed File System，HDFS），它是针对谷歌文件系统（Google File System，GFS）的开源实现。HDFS具有**处理超大数据、流式处理、可以运行在廉价商用服务器上**等优点。它可以通过提供高吞吐率来访问应用程序的数据，适合那些具有超大数据集的应用程序，HDFS放宽了可移植操作系统接口的要求，这样可以**通过流的形式访问文件系统中的数据**，HDFS原本是开源的Apache项目Nutch的基础结构，最后它却成为了Hadoop基础架构之一。
- HBase
    HBase是一个**提供高可靠性、高性能、可伸缩、实时读写和分布式的列式数据库**，一般采用HDFS作为其底层数据存储。HBase是针对谷歌BigTable的开源实现。HBase不同于一般的数据库，原因有两个：其一、HBase是一个**适合于非结构化数据存储的数据库**；其二，HBase是**基于列而不是基于行**的存储模式，HBase和BigTable使用相同的数据模型，用户将数据存储在一个表里，一个数据行拥有一个可选择的键和任务数量的列，由于HBase表是疏松的，用户可以给行定义各种不同类型的列，HBase主要用于需要随机访问、实时读写的大数据（Big Data）。
- Pig
    Pig是一种数据流语言和运行环境，适合于使用Hadoop和MapReduce的平台来**查询大型半结构化数据集**。虽然MapReduce应用程序的编写不是十分复杂，但毕竟也是需要一定的开发经验。Pig的出现大大简化了Hadoop常见的工作任务，它在MapReduce的基础上创建了更简单的过程语言抽象，为Hadoop应用程序提供了一种更加**接近结构化查询语言(SQL)的接口**。
      Pig是一个相对简单的语言，它可以执行语句，因此，当我们需要从大型数据集中搜索满足某个给定搜索条件的记录时，采用Pig要比MapReduce具有明显的优势，前者只需要编写一个简单的脚本在集群中自动并行处理与分发，而后者则需要编写一个单独的MapReduce应用程序。
      Pig是一个对大型数据集进行分析、评估的平台，最突出的优势是**它的结构能够经受住高度并行化的检验**，这个特性使得它能够处理大型的数据集。**Pig的底层由编译器组成**，运行的时候会产生一些MapReduce程序序列。
- Sqoop
    **Sqoop可以改进数据的互操作性，主要用来在Hadoop和关系数据库之间交换数据。**通过Sqoop，我们可以方便地将数据从MySQL、Oracle、PostgreSQL等关系数据库中导入Hadoop（可以导人 HDFS、HBase或 Hive）、或者将数据从Hadoop导出到关系数据库，使得两者之间的数据迁移变得非常方便。**Sqoop主要通过JDBC（Java DataBase Connectivity）与关系数据库进行交互**，理论上，支持JDBC的关系数据库都可以用Sqoop与Hadoop进行数据交互。Sqoop是专门为大数据集设计的，支持增量更新，可以将新记录添加到最近一次导出的数据源上，或者指定上次修改的时间戳。
- Chukwa
    Chukwa是开源的**数据收集系统**，用于**监控和分析**大型分布式系统的数据，Chukwa是在Hadoop的HDFS和MapReduce框架之上搭建的，集成了Hadoop的可扩展性和健壮性，通过HDFS来存储数据，并依赖MapReduce任务处理数据。Chukwa中也附带了灵活且强大的工具，用于显示、监视和分析数据结果，以便更好地利用已收集的数据。
- Zookeeper
    Zookeeper是一个为分布式应用所涉及的开源协调服务，主要为用户**提供同步、配置管理、分组和命名等服务**，减轻分布式应用程序所承担的协调任务，Zookeeper的文件系统使用了我们所熟悉的**目录树结构**，Zookeeper是主要使用Java语言编写，同时支持C语言。

## 2.3 实验一  Hadoop 伪分布的安装

1. #### 实验准备

在开始具体操作之前，首先需要选择一个合适的操作系统。尽管Hadoop本身可以运行在Linux、Windows以及其他一些类UNIX系统（如FreeBSD、OpenBSD、Solaris等）之上，但是，**Hadoop官方真正支持的运行平台只有Linux**。这就导致其他平台在运行Hadoop时，往往需要安装很多其他的包来提供一些Linux操作系统的功能，以配合Hadoop的执行。例如，Windows在运行Hadoop时，需要安装Cygwin等软件。

我们这里选择Linux作为Hadoop的运行平台，用于演示在计算机上如何安装Hadoop、运行程序并得到最终结果。当然，其他平台仍然可以作为开发平台使用。对于正在使用Windows操作系统的小伙伴，可以通过在Windows操作系统中安装Linux虚拟机的方式完成实验。

在Linux发行版的选择上，我们倾向于使用企业级、稳定的操作系统作为实验的系统环境，同时，考虑到易用性以及是否免费等方面的问题，我们排除了OpenSUSE和RedHat等发行版，最终选择免费的Ubuntu发行版作为推荐的操作系统

**Windows**

清华源Ubuntu下载：https://mirrors.tuna.tsinghua.edu.cn/ubuntu-releases/

 最新版VMware安装Ubuntu20.04教程：https://zhuanlan.zhihu.com/p/141033713

**Mac：**

下载VMware：https://customerconnect.vmware.com/cn/downloads/get-download?downloadGroup=FUS-PUBTP-2021H1

系统镜像下载链接：http://old-releases.ubuntu.com/releases/22.04/ubuntu-22.04-live-server-arm64.iso

参考教程：[Mac M1芯片 安装vmware 和ubuntu 以及换源全过程](https://blog.csdn.net/nuomituansama/article/details/125909957)

教程以Windows版为主，Mac版后续操作可能略有不同，自行百度

#### 2、实验内容

**实验环境：**Linux Ubuntu 22.04
**实验要求：**在Linux系统的虚拟机上安装Hadoop软件，基本安装配置主要包括以下几个步骤：

1. 创建Hadoop用户
2. 安装Java
3. 设置SSH登录权限。
4. 单机安装配置。
5. 伪分布式安装配置。

#### 3、实验步骤

##### 注：**把安装包下载到`/data/hadoop`文件夹下，并解压到`/opt`下**

###### 1）创建Hadoop用户

 为方便操作，我们创建一个名为`xuan`的用户来运行程序，这样可以使不同用户之间有明确的权限区别。同时，也可以防止Hadoop的配置操作影响到其他用户的使用。对于一些大的软件（如 MySQL)，在企业中也常常为其单独创建一个用户。
  创建用户的命令是`adduser`：会自动为创建的用户指定主目录、系统shell版本，会在创建时输入用户密码。

```shell
sudo adduser xuan # 创建用户
```

切换用户为`xuan`用户，在该用户环境下进行操作。

```shell
su xuan # 切换用户
```

###### 2)  java 的安装

 由于Hadoop本身是使用Java语言编写的，因此Hadoop的开发和运行都需要Java的支持，一般要求Java 6或者更新的版本。对于Ubuntu 22.04本身，系统上可能已经预装了Java 7，JDK版本为openjdk，路径为`/usr/lib/jvm/java-1.7.0-openjdk`，后文中需要配置的 `JAVA_HOME` 环境变量就可以设置为这个值。
  对于Hadoop而言，采用更为广泛应用的Oracle公司的Java版本，在功能上可能会更稳定一些，因此用户也可以根据自己的爱好安装Oracle版本的Java。在安装过程中，请记住JDK的文件路径，即`JAVA_HOME`的位置，这个路径的设置将用在后文Hadoop的配置文件中，目的是让Hadoop程序可以找到相关的Java工具。

##### 安装jdk

在创建的用户下

/data/hadoop/文件夹下

利用命令下载jdk

```
sudo wget https://download.java.net/openjdk/jdk8u41/ri/openjdk-8u41-b04-linux-x64-14_jan_2020.tar.gz
```

将`/data/hadoop`目录下openjdk-8u41-b04-linux-x64-14_jan_2020.tar.gz   解压缩到`/opt`目录下

```
sudo tar -xzvf openjdk-8u41-b04-linux-x64-14_jan_2020.tar.gz  -C /opt
```

其中，`tar -xzvf` 对文件进行解压缩，-C 指定解压后，将文件放到/opt目录下

**注意：**如果`sudo`命令无法使用，请直接切换到`root`用户，`su root`或`sudo -i`

下面将jdk目录重命名为java，执行如下命令

```
sudo mv /opt/java-se-8u41-ri/ /opt/java
```

修改`java`目录的所属用户

```
sudo chown -R xuan:xuan /opt/java

```

##### 修改系统环境变量

打开`/etc/profile`文件，命令如下：

```shell
sudo vim /etc/profile
```

按i 在该文件末尾，添加如下内容：

```shell
#java
export JAVA_HOME=/opt/java
export PATH=$JAVA_HOME/bin:$PATH
```

点击esc ，使用`Shift+:`，输入`wq`后回车，保存并关闭编辑器。

输入以下命令，使得环境变量生效：

```shell
source /etc/profile
```

 执行完上述命令之后，可以通过`JAVA_HOME`目录找到`java`可使用的命令。 通过查看版本号的命令验证是否安装成功，命令如下：

```shell
java -version
```

 执行结果如下：

```
openjdk version "1.8.0_41"
OpenJDK Runtime Environment (build 1.8.0_41-b04)
OpenJDK 64-Bit Server VM (build 25.40-b25, mixed mode)
```

##### 3) SSH登录权限设置

需要切换为xuan用户，命令如下:

```
su xuan
```

对于Hadoop的伪分布和全分布而言，Hadoop名称节点（NameNode）需要启动集群中所有机器的Hadoop守护进程，这个过程可以通过SSH登录来实现。Hadoop并没有提供SSH输入密码登录的形式，因此，为了能够顺利登录每台机器，需要将所有机器配置为名称节点，可以通过SSH无密码的方式登录它们。
  为了实现SSH无密码登录方式，首先需要让`NameNode`生成自己的`SSH`密钥，命令如下

```shell
ssh-keygen -t rsa # 执行该命令后，遇到提示信息，一直按回车就可以
```

![image-20231019140050403](%E7%AC%AC%E4%BA%8C%E7%AB%A0%20%E5%A4%A7%E6%95%B0%E6%8D%AE%E5%A4%84%E7%90%86%E6%9E%B6%E6%9E%84Hadoop.assets/image-20231019140050403.png)

`NameNode`生成密钥之后，需要将它的公共密钥发送给集群中的其他机器。我们可以将`id_dsa.pub`中的内容添加到需要SSH无密码登录的机器的`~/ssh/authorized_keys`目录下，然后就可以无密码登录这台机器了。对于无密码登录本机而言，可以执行以下命令

```shell
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
```

 或者执行以下命令：

```shell
cat /home/xuan/.ssh/id_rsa.pub >> /home/xuan/.ssh/authorized_keys
```

这时可以通过`ssh localhost`命令来检测一下是否需要输入密码。 测试ssh连接，看到“sucessful login”，则配置成功，命令如下：

```shell
ssh localhost
```

​	

**注**：如果遇到ssh连接localhost被拒绝，可能是没有安装`openssh-server`

ssh: connect to host localhost port 22: Connection refused

```
sudo apt-get install openssh-server
```

##### 4) 安装单机版Hadoop

这里使用的Hadoop版本为2.10.1。在readme文件中提供的链接地址下载也可

```
sudo wget --no-check-certificate https://mirrors.bfsu.edu.cn/apache/hadoop/common/hadoop-2.10.1/hadoop-2.10.1.tar.gz
```

解压缩到`/opt`目录下，命令如下：

```
sudo tar -zxvf hadoop-2.10.1.tar.gz -C /opt/
```

为了便于操作，我们也将其重命名为hadoop，命令如下

```
sudo mv /opt/hadoop-2.10.1 /opt/hadoop
```

修改hadoop目录的所属用户和所属组，命令如下

```shell
sudo chown -R xuan:xuan /opt/hadoop
```

##### 修改系统环境变量

打开`/etc/profile`文件，命令如下：

```shell
sudo vim /etc/profile
```

在文件末尾，添加如下内容：

```
#hadoop
export HADOOP_HOME=/opt/hadoop
export PATH=$HADOOP_HOME/bin:$PATH

```

点击esc ，使用`Shift+:`，输入`wq`后回车，保存并关闭编辑器。

输入以下命令，使得环境变量生效：

```shell
source /etc/profile
```

通过查看版本号命令验证是否安装成功，命令如下：

```
hadoop version
```

```
Hadoop 2.10.1
Subversion https://github.com/apache/hadoop -r 1827467c9a56f133025f28557bfc2c562d78e816
Compiled by centos on 2020-09-14T13:17Z
Compiled with protoc 2.5.0
From source with checksum 3114edef868f1f3824e7d0f68be03650
This command was run using /opt/hadoop/share/hadoop/common/hadoop-common-2.10.1.jar
```

##### 3）修改hadoop-env.sh文件配置

对于单机安装，首先需要更改`hadoop-env.sh`文件，用于配置Hadoop运行的环境变量，命令如下：

```shell
cd /opt/hadoop/
vim etc/hadoop/hadoop-env.sh
```

在文件末尾，添加如下内容：

```shell
export JAVA_HOME=/opt/java/
```

保存并退出

##### 案例一：wordcount

 Hadoop文档中还附带了一些例子来供我们测试，可以运行`WordCount`的示例，检测一下Hadoop安装是否成功。运行示例的步骤如下：

1. 在`/opt/hadoop/`目录下新建`input`文件夹，用来存放输入数据；
2. 将`etc/hadoop/`文件夹下的配置文件拷贝至`input`文件夹中；
3. 在`hadoop`目录下新建`output`文件夹，用于存放输出数据；
4. 运行`wordCount`示例
5. 查看输出数据的内容。  

```shell
mkdir input
cp etc/hadoop/*.xml input
bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-3.3.1.jar grep input output 'dfs[a-z.]+'
cat output/*
```

 输出数据结果：

```log
1   dfsadmin
```

![image-20231019192158794](%E7%AC%AC%E4%BA%8C%E7%AB%A0%20%E5%A4%A7%E6%95%B0%E6%8D%AE%E5%A4%84%E7%90%86%E6%9E%B6%E6%9E%84Hadoop.assets/image-20231019192158794.png)这意味着，在所有的配置文件中，只有一个符合正则表达式`dfs[a-z.]+`的单词，输出结果正确。

#### 5) Hadoop伪分布式安装

​         伪分布式安装是指在一台机器上模拟一个小的集群。当Hadoop应用于集群时，不论是伪分布式还是真正的分布式运行，都需要通过配置文件对各组件的协同工作进行设置。
  对于伪分布式配置，我们需要修改`core-site.xml`、`hdfs-site.xml`、`mapred-site.xml`和`yarn-site.xml`这4个文件。 

##### 修改core-site.xml文件配置

 打开`core-site.xml`文件，命令如下：

```shell
vim /opt/hadoop/etc/hadoop/core-site.xml
```

添加下面配置到`<configuration>与</configuration>`标签之间，添加内容如下：

```html
<property>
    <name>fs.defaultFS</name>
    <value>hdfs://localhost:9000</value>
</property>
```

`core-site.xml`配置文件的格式十分简单，`<name>`标签代表了配置项的名字，`<value>`项设置的是配置的值。对于该文件，我们只需要在其中指定HDFS的地址和端口号，端口号按照官方文档设置为9000即可。

##### 修改hdfs-site.xml文件配置

打开`hdfs-site.xml`文件，命令如下：

```shell
vim /opt/hadoop/etc/hadoop/hdfs-site.xml
```

  添加下面配置到`<configuration>与</configuration>`标签之间

```html
<property>
    <name>dfs.replication</name>
    <value>1</value>
</property>
```

对于`hdfs-site.xml`文件，我们设置`replication`值为1，这也是Hadoop运行的默认最小值，用于设置HDFS文件系统中同一份数据的副本数量。

##### 修改mapred-site.xml文件配置

打开`mapred-site.xml`文件，命令如下：

```shell
vim /opt/hadoop/etc/hadoop/mapred-site.xml
```

添加下面配置到`<configuration>与</configuration>`标签之间

```html
<property>
    <name>mapreduce.framework.name</name>
    <value>yarn</value>
</property>
<property>
    <name>mapreduce.application.classpath</name>
    <value>$HADOOP_MAPRED_HOME/share/hadoop/mapreduce/*:$HADOOP_MAPRED_HOME/share/hadoop/mapreduce/lib/*</value>
</property>
```

##### 修改yarn-site.xml文件配置

```shell
vim /opt/hadoop/etc/hadoop/yarn-site.xml
```

添加下面配置到`<configuration>与</configuration>`标签之间。

```
<property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
    <property>
        <name>yarn.nodemanager.env-whitelist</name>
        <value>JAVA_HOME,HADOOP_COMMON_HOME,HADOOP_HDFS_HOME,HADOOP_CONF_DIR,CLASSPATH_PREPEND_DISTCACHE,HADOOP_YARN_HOME,HADOOP_MAPRED_HOME</value>
    </property>
```

##### 格式化分布式文件系统

需要切换为datawhale用户，命令如下：

```shell
su xuan
```

在配置完成后，首先需要初始化文件系统，由于Hadoop的很多工作是在自带的 HDFS文件系统上完成的，因此，需要将文件系统初始化之后才能进一步执行计算任务。执行初始化的命令如下：

```shell
hadoop namenode -format
```

![image-20231019194132576](%E7%AC%AC%E4%BA%8C%E7%AB%A0%20%E5%A4%A7%E6%95%B0%E6%8D%AE%E5%A4%84%E7%90%86%E6%9E%B6%E6%9E%84Hadoop.assets/image-20231019194132576.png)

出现以上结果，说明初始化成功。

如果为3.x的版本输入：

```shell
hdfs namenode -format
```

 在看到运行结果中出现“successfully formatted”之后，则说明初始化成功

##### 启动Hadoop

使用如下命令启动Hadoop的所有进程，可以通过提示信息得知，所有的启动信息都写入到对应的日志文件。如果出现启动错误，则可以查看相应的错误日志。

```shell
/opt/hadoop/sbin/start-all.sh
```

##### 查看Hadoop进程

运行之后，输入`jps`命令可以查看所有的`Java`进程。正常启动后，可以得到如下类似结果：

```log
2072524 SecondaryNameNode
2073019 ResourceManager
2072169 NameNode
2073158 NodeManager
2072291 DataNode
2073923 Jps
```

##### Hadoop WebUI管理界面

此时，可以通过`http://localhost:8088`访问Web界面，查看Hadoop的信息。

localhost 可以换为虚拟机ip ，通过 下面的命令查看IP  

```
ip    addr
```

![image-20231019194825719](%E7%AC%AC%E4%BA%8C%E7%AB%A0%20%E5%A4%A7%E6%95%B0%E6%8D%AE%E5%A4%84%E7%90%86%E6%9E%B6%E6%9E%84Hadoop.assets/image-20231019194825719.png)

可以通过http://localhost:50070打开 Hadoop 的 Web UI 界面，该界面提供了有关 HDFS 集群的信息和管理功能。

Hadoop版本为3.x则通过，http://localhost:9870

##### 测试HDFS集群以及MapReduce任务程序

 利用Hadoop自带的`WordCount`示例程序进行检查集群，并在主节点上进行如下操作，创建执行MapReduce任务所需的HDFS目录：

```shell
hadoop fs -mkdir /user
hadoop fs -mkdir /user/datawhale
hadoop fs -mkdir /input
```

创建测试文件，命令如下：

```shell
vim /home/xuan/test
```

 在`test`文件中，添加以下内容：

```log
Hello world!
```

 将测试文件上传到Hadoop HDFS集群目录，命令如下：

```shell
hadoop fs -put /home/xuan/test /input
```

执行wordcount程序，命令如下：

```shell
hadoop jar /opt/hadoop/share/hadoop/mapreduce/hadoop-mapreduce-examples-2.10.1.jar wordcount /input /out
```

通过以下命令，查看执行结果：

```shell
hadoop fs -ls /out
```

执行结果如下：

```log
Found 2 items
-rw-r--r--    1 root supergroup       0 time /out/_SUCCESS
-rw-r--r--    1 root supergroup      17 time /out/part-r-00000 
```

可以看到，结果中包含`_SUCCESS`文件，表示Hadoop集群运行成功。

 查看具体的输出结果，命令如下：

```shell
hadoop fs -text /out/part-r-00000
```

 **官网安装**请参考：[Hadoop单节点集群安装](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-common/SingleCluster.html#Execution)

**安装问题**：[Hadoop中DataNode没有启动](https://www.cnblogs.com/mtime2004/p/10008325.html)

## 2.4 实验二：Hadoop3.3.1集群模式安装

java与hadoop的安装与伪分布式流程一致，此处不再赘述，后面的配置文件有所不同。

#### 1、 修改hadoop hadoop-env.sh文件配置

```
vim /opt/hadoop/etc/hadoop/hadoop-env.sh
```

末端添加如下内容：

```
export JAVA_HOME=/opt/java/
```

#### 2 修改hadoop core-site.xml文件配置

```
vim /opt/hadoop/etc/hadoop/core-site.xml
```

添加下面配置到`<configuration>与</configuration>`标签之间。

```
<property>
    <name>fs.defaultFS</name>
    <value>hdfs://master:9000</value>
</property>
```

#### 3 修改hadoop hdfs-site.xml文件配置

```
vim /opt/hadoop/etc/hadoop/hdfs-site.xml
```

添加下面配置到`<configuration>与</configuration>`标签之间。

```
<property>
    <name>dfs.replication</name>
    <value>3</value>
</property>
```

#### 4 修改hadoop yarn-site.xml文件配置

```
vim /opt/hadoop/etc/hadoop/yarn-site.xml
```

添加下面配置到`<configuration>与</configuration>`标签之间。

```
<property>
    <name>yarn.nodemanager.aux-services</name>
    <value>mapreduce_shuffle</value>
</property>
<property>
    <name>yarn.nodemanager.env-whitelist</name>
   <value>JAVA_HOME,HADOOP_COMMON_HOME,HADOOP_HDFS_HOME,HADOOP_CONF_DIR,CLASSPATH_PREPEND_DISTCACHE,HADOOP_YARN_HOME,HADOOP_MAPRED_HOME</value>
</property>
```

#### 5 mapred-site.xml文件配置

```
vim /opt/hadoop/etc/hadoop/mapred-site.xml
```

添加下面配置到`<configuration>与</configuration>`标签之间

```
<configuration>
<property>
    <name>mapreduce.framework.name</name>
    <value>yarn</value>
</property>
</configuration>
```

#### 6 修改hadoop workers文件配置

```
vim /opt/hadoop/etc/hadoop/workers
```

覆盖写入主节点映射名和从节点映射名：

```
master
slave1
slave2
```

#### 7 修改hosts文件

查看master ip地址



```
ip addr
```

记录下显示的ip，例：192.168.24.135

打开slave1 节点，做如上操作，记录下显示的ip，例：192.168.24.134

打开slave2 节点，做如上操作，记录下显示的ip，例：192.168.24.136

编辑/etc/hosts文件：

```
sudo vim /etc/hosts
```

添加master IP地址对应本机映射名和其它节点IP地址对应映射名(如下只是样式，请写入实验时您的正确IP)：

```
192.168.24.135 master
192.168.24.134 slave1
192.168.24.136 slave2
```

#### 8 创建公钥

在用户下创建公钥：（若上边做伪分布式时已生成过密钥则不必再做）

```
ssh-keygen -t rsa
```

出现如下内容：

Enter file in which to save the key (/home/datawhale/.ssh/id_rsa):

回车即可，出现如下内容：

Enter passphrase (empty for no passphrase):

直接回车，出现内容：

Enter same passphrase again:

直接回车，创建完成

#### 9 拷贝公钥

提示：命令执行过程中需要输入“yes”和密码“datawhale”。三台节点请依次执行完成。

```
ssh-copy-id master
```

```
ssh-copy-id slave1
```

```
ssh-copy-id slave2
```

修改文件权限：（master和slave均需修改）

```
chmod 700 /home/master/.ssh
```

```
chmod 700 /home/master/.ssh/*
```

测试连接是否正常：

```
ssh master
```

#### 10 拷贝文件到所有从节点

```
scp -r /opt/java/ /opt/hadoop/ slave1:/tmp/
```

```
scp -r /opt/java/ /opt/hadoop/ slave2:/tmp/
```

至此，主节点配置完成。

现在，请去slave1和slave2依次完成节点配置。

以下内容在所有从节点配置完成之后回来继续进行!

#### 11 格式化分布式文件系统

```
hadoop namenode -format
```

#### 12 启动Hadoop

```
/opt/hadoop/sbin/start-all.sh
```

重新启动前记得要先关闭

```
/opt/hadoop/sbin/stop-all.sh
```

#### 13 查看Hadoop进程

在hadoop主节点执行：

```
jps
```

输出结果必须包含6个进程，结果如下：

```
2529 DataNode
2756 SecondaryNameNode
3269 NodeManager
3449 Jps
2986 ResourceManager
2412 NameNode
```

在hadoop从节点执行同样的操作：

```
jps
```

输出结果必须包含3个进程，具体如下：

```
2529 DataNode
3449 Jps
2412 NameNode
```

#### 14 在命令行中输入以下代码，打开Hadoop WebUI管理界面：

```
firefox http://master:8088
```

#### 15 测试HDFS集群以及MapReduce任务程序

利用Hadoop自带的WordCount示例程序进行检查集群；在主节点进行如下操作，创建HDFS目录：

```
hadoop fs -mkdir /datawhale/
hadoop fs -mkdir /datawhale/input
```

创建测试文件

```
vim /home/datawhale/testCopy to clipboardErrorCopied
```

添加下面文字

```
datawhale
```

保存并关闭编辑器

将测试文件上传到到Hadoop HDFS集群目录：

```
hadoop fs -put /home/datawhale/test /datawhale/inputCopy to clipboardErrorCopied
```

执行wordcount程序：

```
hadoop jar /opt/hadoop/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.3.1.jar wordcount /datawhale/input/ /datawhale/out/Copy to clipboardErrorCopied
```

查看执行结果：

```
hadoop fs -ls /datawhale/out/Copy to clipboardErrorCopied
```

如果列表中结果包含”_SUCCESS“文件，代码集群运行成功。

查看具体的执行结果，可以用如下命令：

```
hadoop fs -text /datawhale/out/part-r-00000Copy to clipboardErrorCopied
```

到此，集群安装完成。

Hadoop被视为事实上的大数据处理标准，本章主要介绍了Hadoop的发展历程，并阐述了Hadoop的高可靠性、高效性、高可扩展性、高容错性、成本低、运行在Linux平台上、支持多种编程语言等特性。
  Hadoop目前已经在各个领域得到了广泛的应用，如雅虎、Facebook、百度、淘宝、网易等公司都建立了自己的Hadoop集群。
  经过多年发展，Hadoop项目已经变得非常成熟和完善，包括Common、Avro、Zookeeper、HDFS、MapReduce、HBase、Hive、Chukwa、Pig等子项目。其中，HDFS和 MapReduce是Hadoop的两大核心组件。