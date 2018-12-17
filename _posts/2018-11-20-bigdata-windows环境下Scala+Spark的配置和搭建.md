---
layout: post
title: '五.windows环境下Scala+Spark的配置和搭建'
subtitle: 'JDK的安装，Scala的安装，Hadoop的下载和配置，Spark的安装'
date: 2018-11-20
categories: 大数据
cover: 'http://githubpics.oss-cn-beijing.aliyuncs.com/bigdata/hadoop.jpg'
tags: bigdata Scala Spark 
---

### 注意事项
* 所有的安装目录最好不要出现空格，尤其注意“Program Files”文件夹中间的空格。
* 下文中提到的安装介质如果无法下载可以点击[此处（安装介质，提取码：hg01）][1]进行下载。

## 一.JDK的安装

打开cmd命令行窗口键入命令：
```shell
java -version
```
如果可以正确输出Java版本信息则JDK已经安装成功，无需再次安装。如果不能，点击[此处链接（Oracle Java Archive）][2]下载适合的JDK版本进行安装。JDK在windows下的安装十分简单，不做赘述了。

我的环境配置使用的版本是 1.8.0_131。

## 二.Scala的安装

点击Scala的[下载地址（ALL AVAILABLE VERSIONS）][3]进行下载。此处选择的版本是“Scala 2.12.0”，如下图所示
![enter description here][4]

点击后在下一个页面向下翻，找到“Other ways to install Scala”，然后点击“Download the Scala binaries for windows ”，即可下载。
![enter description here][5]

下载好的文件名为“scala-2.12.0.msi”，双击进行安装。我安装到了如下目录“D:\ProgramFiles\scala”。

一般来说安装完成后环境变量会自动配置好，在cmd命令行窗口中输入```
scala -version``` 查看版本号 ，输入```scala``` 进入scala的环境。
![enter description here][6]
如果不能查看，就需要手动配置scala的环境变量。在“PATH”中添加scala下的bin目录“D:\ProgramFiles\scala\bin”
![enter description here][7]
![enter description here][8]
scala示例：
![enter description here][9]

## 三.Hadoop的下载和配置

由于Spark是依赖于Hadoop的，因此先安装Hadoop。点击Hadoop的[下载地址（Hadoop Releases）][10]，进入页面后往下翻，越往下的版本越新。此处选择的是Hadoop 2.7.3 版本（**注意此处选择hadoop2.7的版本，后面的spark应该选择与之对应的版本，比如2.0之后的版本**）进行下载。然后点击“hadoop-2.7.3.tar.gz ”，即可下载。
![enter description here][11]

下载后将压缩包解压到相应目录下，例如“D:\ProgramFiles\hadoop”。然后配置环境变量。添加：

“PATH”：“D:\ProgramFiles\hadoop\bin”

“HADOOP_HOME”：“D:\ProgramFiles\hadoop”

然后下载**winutils.exe**，点击[链接][12]，找到对应的Hadoop版本，并在bin目录下找到winutils.exe文件进行下载。此处使用hadoop2.7.1的winutils也可以。（如果下载不下来在上面分享的网盘连接中也有）。下载后将winutils.exe复制到hadoop的bin目录下。


下面需要修改Hadoop的权限，在cmd中输入：
```shell
D:\ProgramFiles\hadoop\bin\winutils.exe chmod 777 /tmp/hive
```
别忘了将winutils的绝对路径修改为自己的路径。

## 四.Spark的安装

点击Spark的[下载地址（Download Apache Spark）][13]，选择“2.1.0”版本进行下载。
![enter description here][14]

下载后无需安装，直接将压缩包解压到相应目录，例如“D:\ProgramFiles\spark”，然后配置spark的环境变量。在“PATH”中添加spark下的bin目录“D:\ProgramFiles\spark\bin”。
![enter description here][15]
以上的步骤若没有出现问题，接下来就可以测试一下Spark的交互环境。在cmd命令行中输入```spark-shell```就可以进入spark的命令行。正常的运行界面如下图所示，warn信息可以忽略
![enter description here][16]

如果出现报错，考虑winutlis有没有安装成功，或者有没有修改权限。


  [1]: https://pan.baidu.com/s/1WWqZfbE9oB4xhREzPfqUNA
  [2]: https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
  [3]: https://www.scala-lang.org/download/all.html
  [4]: https://githubpics.oss-cn-beijing.aliyuncs.com/bigdata/win10SparkScala/sacla%E4%B8%8B%E8%BD%BD01.JPG
  [5]: https://githubpics.oss-cn-beijing.aliyuncs.com/bigdata/win10SparkScala/sacla%E4%B8%8B%E8%BD%BD02.JPG
  [6]: https://githubpics.oss-cn-beijing.aliyuncs.com/bigdata/win10SparkScala/sacla%E9%85%8D%E7%BD%AE01.JPG
  [7]: https://githubpics.oss-cn-beijing.aliyuncs.com/bigdata/win10SparkScala/sacla%E9%85%8D%E7%BD%AE02.JPG
  [8]: https://githubpics.oss-cn-beijing.aliyuncs.com/bigdata/win10SparkScala/sacla%E9%85%8D%E7%BD%AE03.JPG
  [9]: https://githubpics.oss-cn-beijing.aliyuncs.com/bigdata/win10SparkScala/sacla%E6%BC%94%E7%A4%BA01.JPG
  [10]: https://archive.apache.org/dist/hadoop/common/
  [11]: https://githubpics.oss-cn-beijing.aliyuncs.com/bigdata/win10SparkScala/hadoop%E4%B8%8B%E8%BD%BD01.JPG
  [12]: https://github.com/steveloughran/winutils
  [13]: http://spark.apache.org/downloads.html
  [14]: https://githubpics.oss-cn-beijing.aliyuncs.com/bigdata/win10SparkScala/spark%E4%B8%8B%E8%BD%BD01.JPG
  [15]: https://githubpics.oss-cn-beijing.aliyuncs.com/bigdata/win10SparkScala/spark%E9%85%8D%E7%BD%AE01.JPG
  [16]: https://githubpics.oss-cn-beijing.aliyuncs.com/bigdata/win10SparkScala/spark%E6%B5%8B%E8%AF%9501.JPG