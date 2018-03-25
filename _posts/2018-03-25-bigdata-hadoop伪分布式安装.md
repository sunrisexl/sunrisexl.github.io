---
layout: post
title: '三. hadoop伪分布式安装'
subtitle: '在单机上，模拟一个分布式的环境'
date: 2018-03-25
categories: 大数据
cover: 'http://githubpics.oss-cn-beijing.aliyuncs.com/bigdata/hadoop.jpg'
tags: bigdata hadoop
---

## 伪分布模式的配置

- hadoop-2.7.3/etc/hadoop/hdfs-site.xml

    ``` xml
    <!--数据块的冗余度，默认是3-->
    <property>
      <name>dfs.replication</name>
      <value>1</value>
    </property>
    
    <!--是否开启HDFS的权限检查，默认：true-->
    <!--
    <property>
      <name>dfs.permissions</name>
      <value>false</value>
    </property>
    -->
    ```

- hadoop-2.7.3/etc/hadoop/core-site.xml

    ``` xml
    <!--NameNode的地址-->
    <property>
      <name>fs.defaultFS</name>
      <value>hdfs://bigdata11:9000</value>
    </property>	
    
    <!--HDFS数据保存的目录，默认是Linux的tmp目录-->
    <property>
      <name>hadoop.tmp.dir</name>
      <value>/root/training/hadoop-2.7.3/tmp</value>
    </property>	
    ```
- hadoop-2.7.3/etc/hadoop/mapred-site.xml

    ``` xml
    <!--MR程序运行的容器是Yarn-->
    <property>
      <name>mapreduce.framework.name</name>
      <value>yarn</value>
    </property>		
    ```
    
- hadoop-2.7.3/etc/hadoop/yarn-site.xml

    ``` xml
    <!--ResourceManager的地址-->
    <property>
      <name>yarn.resourcemanager.hostname</name>
      <value>bigdata11</value>
    </property>		
    
    <!--NodeManager运行MR任务的方式-->
    <property>
      <name>yarn.nodemanager.aux-services</name>
      <value>mapreduce_shuffle</value>
    </property>	
    ```

对NameNode进行格式化: hdfs namenode -format

启动：start-all.sh = start-dfs.sh + start-yarn.sh
