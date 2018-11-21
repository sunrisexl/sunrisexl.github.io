---
layout: post
title: '六.windows环境下Hadoop+Eclipse的部署'
subtitle: 'Hadoop在windows下的配置，使用API操作HDFS'
date: 2018-11-21
categories: 大数据
cover: 'http://githubpics.oss-cn-beijing.aliyuncs.com/bigdata/hadoop.jpg'
tags: bigdata hadoop 
---

### 注意事项
* 所有的安装目录最好不要出现空格，尤其注意“Program Files”文件夹中间的空格。
* 下文中提到的安装介质如果无法下载可以点击[此处（安装介质，提取码：w8p6）][1]进行下载。

## 一.准备工作：JDK的安装、Hadoop的安装
再次注意自己的JDK安装路径中有没有空格！

Hadoop的下载和安装在上一篇中已经介绍完毕，检查Hadoop的环境变量配置：
```
PATH:D:\ProgramFiles\hadoop\bin
HADOOP_HOME:D:\ProgramFiles\hadoop
```

## 二.修改Hadoop配置文件
1.  下载[（安装介质，提取码：w8p6）][2]中的“hadooponwindows-master.zip”文件，解压后，用文件中的“bin”文件夹替换掉本地Hadoop的安装目录“D:\ProgramFiles\hadoop”中的“bin”文件夹。
2.  在Hadoop的安装目录“D:\ProgramFiles\hadoop\etc\hadoop”中找到如下文件，并按照提示添加内容。（在windows下推荐使用**notepad++**修改xml文件，notepad++的安装包也在网盘中提供）

**D:\ProgramFiles\hadoop\etc\hadoop\core-site.xml**

```xml
<configuration>
<!--NameNode的地址-->
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://localhost:9000</value>
    </property>
</configuration>
```

**D:\ProgramFiles\hadoop\etc\hadoop\mapred-site.xml**

```xml
<configuration>
<!--MR程序运行的容器是Yarn-->
    <property>
       <name>mapreduce.framework.name</name>
       <value>yarn</value>
    </property>
</configuration>
```

**D:\ProgramFiles\hadoop\etc\hadoop\hdfs-site.xml**

```xml
<configuration>
<!--数据块的冗余度，默认是3-->
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
    <property>
       <name>dfs.namenode.name.dir</name>
       <value>/hadoop/data/namenode</value>
    </property>
    <property>
       <name>dfs.datanode.data.dir</name>
       <value>/hadoop/data/datanode</value>
    </property>
</configuration>
```

**D:\ProgramFiles\hadoop\etc\hadoop\yarn-site.xml**

```xml
<configuration>
<!--NodeManager运行MR任务的方式-->
    <property>
       <name>yarn.nodemanager.aux-services</name>
       <value>mapreduce_shuffle</value>
    </property>
    <property>
       <name>yarn.nodemanager.aux-services.mapreduce.shuffle.class</name>
       <value>org.apache.hadoop.mapred.ShuffleHandler</value>
    </property>
</configuration>
```

**D:\ProgramFiles\hadoop\etc\hadoop\hadoop-env.cmd**
(修改为自己的JDK安装目录)

```shell
set JAVA_HOME=D:\java\jdk
```

## 三.格式化HDFS文件系统

```shell
hdfs namenode -format
```

如下图所示内容基本就成功了。
![enter description here][3]

## 四.启动与终止命令
在cmd命令行中切换到“D:\ProgramFiles\hadoop\sbin”路径下，或者在该路径下按住shift同时右击鼠标，选择“在此处打开命令窗口”，然后输入```start-all```启动。（start-all = start-dfs + start-yarn）
然后会弹出四个命令行窗口，分别为：Namenode、Datanode、YARN resourcemanager、YARN nodemanager，表示四个进程启动成功，如下图
![enter description here][4]

终止命令：```stop-all```

打开浏览器，在地址栏中输入```localhost:8088```和```localhost:50070```
![enter description here][5]
![enter description here][6]
看到如上图所示则说明Hadoop配置成功。
测试：在命令行中输入```hdfs dfs -mkdir /aaa```,可以在hdfs中创建一个”aaa“目录。
![enter description here][7]
然后在```localhost:50070```网站中点击Utilities，选择Browse the file system，会看到刚才创建的目录。
![enter description here][8]

## 五.在eclipse中使用Java API 操作hdfs
在eclipse中创建一个Java项目，在项目中创建一个”lib“文件夹。如图：
![enter description here][9]
从Hadoop的安装目录中找到如下四个文件夹中的后缀名为".jar"的所有文件，将其复制到eclipse中的”lib”目录下。
D:\ProgramFiles\hadoop\share\hadoop\common\
D:\ProgramFiles\hadoop\share\hadoop\common\lib
D:\ProgramFiles\hadoop\share\hadoop\hdfs
D:\ProgramFiles\hadoop\share\hadoop\hdfs\lib
（在网盘中也有提供）
然后在“lib“目录中单击第一个jar包，按住shift，再单击最后一个jar包，将其全部选中，右键buildpath->add to buildpath.看到其变成小奶瓶的样子就导入成功了。
![enter description here][10]
然后新建class文件，输入如下内容：(测试创建文件夹)
```java
import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.FileSystem;
import org.apache.hadoop.fs.Path;
import org.junit.Test;

public class Demo {

	@Test
	public void test1() throws Exception{
		
		//指定NameNode地址
		Configuration conf = new Configuration();
		//如果要使用主机名，需要配置Windows的host文件
		//C:\Windows\System32\drivers\etc\hosts文件
		conf.set("fs.defaultFS", "hdfs://localhost:9000");
		
		/*
		 * 还有一种写法：IP地址
		 * conf.set("fs.defaultFS", "hdfs://192.168.157.11:9000");
		 */
		
		//创建一个HDFS的客户端
		FileSystem client = FileSystem.get(conf);
		//创建目录
		client.mkdirs(new Path("/folder1"));
		
		//关闭客户端
		client.close();
	}

}
```
![enter description here][11]
如果看到左侧junit测试结果为绿色则创建成功。


  [1]: https://pan.baidu.com/s/1k_fURV8mb2uruBlMsKqpVw
  [2]: https://pan.baidu.com/s/1k_fURV8mb2uruBlMsKqpVw
  [3]: https://githubpics.oss-cn-beijing.aliyuncs.com/bigdata/win10EclipseHadoop/%E6%A0%BC%E5%BC%8F%E5%8C%96namenode.JPG
  [4]: https://githubpics.oss-cn-beijing.aliyuncs.com/bigdata/win10EclipseHadoop/%E5%90%AF%E5%8A%A8%E4%BF%A1%E6%81%AF.JPG
  [5]: https://githubpics.oss-cn-beijing.aliyuncs.com/bigdata/win10EclipseHadoop/hadoop%E5%90%AF%E5%8A%A8%E7%95%8C%E9%9D%A2.JPG
  [6]: https://githubpics.oss-cn-beijing.aliyuncs.com/bigdata/win10EclipseHadoop/hdfs%E5%90%AF%E5%8A%A8%E7%95%8C%E9%9D%A2.JPG
  [7]: https://githubpics.oss-cn-beijing.aliyuncs.com/bigdata/win10EclipseHadoop/%E5%88%9B%E5%BB%BA%E7%9B%AE%E5%BD%95.JPG
  [8]: https://githubpics.oss-cn-beijing.aliyuncs.com/bigdata/win10EclipseHadoop/%E6%98%BE%E7%A4%BA%E5%88%9B%E5%BB%BA%E7%9A%84%E7%9B%AE%E5%BD%95.JPG
  [9]: https://githubpics.oss-cn-beijing.aliyuncs.com/bigdata/win10EclipseHadoop/lib%E7%9B%AE%E5%BD%95.JPG
  [10]: https://githubpics.oss-cn-beijing.aliyuncs.com/bigdata/win10EclipseHadoop/jar%E5%AF%BC%E5%85%A5.JPG
  [11]: https://githubpics.oss-cn-beijing.aliyuncs.com/bigdata/win10EclipseHadoop/%E6%B5%8B%E8%AF%95%E6%88%90%E5%8A%9F.JPG