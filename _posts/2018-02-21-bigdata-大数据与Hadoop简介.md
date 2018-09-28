---
layout: post
title: '一. 大数据与hadoop简介'
subtitle: '大数据与hadoop的背景知识'
date: 2018-02-21
categories: 大数据
cover: 'https://githubpics.oss-cn-beijing.aliyuncs.com/bigdata/hadoop.jpg'
tags: bigdata hadoop
---

此系列博客将记录大数据学习的相关知识，亦作备忘

## 大数据

1. 大数据的技术特点：
- Volume（大体量）：从数百TB到PB，甚至EB的规模
- Variety（多样性）：包含各种格式和形态的数据（结构化和非结构化）
- Velocity（时效性）：很多大数据需要在一定的时间限度下得到处理
- Value（大价值）：包含很多深度的价值，但是价值密度低

2. 大数据研究的主要目标：

    以有效的信息技术手段和计算方法，获取、处理和分析各种行业的大数据，发现和提取数据的深度价值，为行业提供高附加值的应用和服务。
	
3. 大数据研究的基本原则：

	以应用需求为导向、以领域交叉为桥梁、以技术综合为支撑
	
4. 基本途径：
	- 寻找新算法降低复杂度
	- 寻找和使用降低数据尺度的算法
	- **分而治之**的并行化处理

[大数据][1]的各种介绍已经很多了，以上不全面的地方可以自行[谷歌][2]。

## OLTP与OLAP

当今的数据处理大致可以分成两大类：

联机事务处理OLTP（on-line transaction processing）

联机分析处理OLAP（On-Line Analytical Processing）

OLTP是传统的关系型数据库的主要应用，主要是基本的、日常的事务理，例如银行交易。OLAP是数据仓库系统的主要应用，支持复杂的分析操作，侧重决策支持，并且提供直观易懂的查询结果。

下表列出了OLTP与OLAP之间的比较:

|    --    | 联机事务处理OLTP                   |联机分析处理OLAP|
|-------   |:------:                            |:--------:|
|用户      |操作人员,低层管理人员               | 决策人员,高级管理人员|
|功能      |日常操作处理                        |      分析决策|
|DB 设计   |  面向应用                          |      面向主题|
|数据      |当前的, 最新的细节的, 二维的分立的  |历史的, 聚集的, 多维的集成的, 统一的|
|存取      |读/写数十条记录                     |读上百万条记录|
|工作单位  |   简单的事务                       |              复杂的查询|
|用户数    |上千个                              |       上百个|
|DB 大小   |100MB-GB                            |         100GB-TB|

## 数据仓库
数据库与数据仓库的[区别][3]，主要在于数据库是传统的关系型数据库的主要应用，主要是基本的、日常的事务处理，例如银行交易。数据仓库系统的主要应用主要是OLAP（On-Line Analytical Processing），支持复杂的分析操作，侧重决策支持，并且提供直观易懂的查询结果。

## MapReduce的基本概念和由来
- MapReduce是一个面向大数据并行处理的计算模型、框架和平台。包含了以下三层含义：
	1. 是一个基于集群的高性能并行计算平台（使用市场上普通的商用服务器构成一个包含数千节点的分布和并行计算集群）
	2. 是一个并行计算与运行软件框架（自动完成任务的并行化处理，自动划分任务、自动分配和执行任务以及收集结果。将数据分布存储、数据通信、容错处理全部交给系统处理）
	3. 是一个并行程序设计模型和方法（使用Map和Reduce两个函数实现基本的并行计算任务，提供了抽象的操作和并行编程接口）

- MapReduce的由来

	MapReduce最早是Google公司研究提出的一种面向大规模数据处理的并行计算模型和方法。初衷是解决其搜索引擎中大规模网页数据的并行化处理。后来DongCutting模仿Google MapReduce，基于Java设计开发了一个称为Hadoop的开源MapReduce并行计算框架和系统。自此，Hadoop成为Apache开源组织下最重要的项目。

- 设计思想
	1. 分而治之
	
		对数据分片，将每个数据分片交给一个节点去处理，最后汇总处理结果
	
	2. 抽象模型：Map和Reduce
	
		提供了高层的并行编程抽象模型和接口，程序员只需实现这两个基本接口即可快速完成并行化程序的设计
	
	3. 统一架构，为程序员隐藏系统层细节
	
		实现自动化并行计算，为程序员隐藏系统层细节
	
## hadoop的思想起源
Hadoop主要包括了分布式存储和并行计算两部分，主要是参考了谷歌在处理大数据的时使用的方法，包括文件系统GFS、倒排索引、PageRank、BigTable等

- GFS（[谷歌文件系统][4]）
GFS是一个可扩展的分布式文件系统，用于大型的、分布式的、对大量数据进行访问的应用。它运行于廉价的普通硬件上，并提供容错功能。它可以给大量的用户提供总体性能较高的服务。
![此处输入图片的描述][5]

- [倒排索引][6]
用来存储在全文搜索下某个单词在一个文档或者一组文档中的存储位置的映射。它是文档检索系统中最常用的数据结构。

- [PageRank][7]
PageRank的核心思想非常简单：
 - 如果一个网页被很多其他网页链接到的话说明这个网页比较重要，也就是PageRank值会相对较高
 - 如果一个PageRank值很高的网页链接到一个其他的网页，那么被链接到的网页的PageRank值会相应地因此而提高

- [BigTable][8]
BigTable是一种压缩的、高性能的、高可扩展性的，基于Google文件系统（Google File System，GFS）的数据存储系统，用于存储大规模结构化数据，适用于云计算。
  


  [1]: https://en.wikipedia.org/wiki/Big_data
  [2]: https://www.google.com.hk/search?num=20&newwindow=1&safe=strict&q=big%20data&spell=1&sa=X&ved=0ahUKEwiP2PevgbfZAhUS9mMKHQwtA30QBQgkKAA&biw=1280&bih=679
  [3]: https://www.zhihu.com/question/20623931
  [4]: https://baike.baidu.com/item/GFS/1813072
  [5]: https://gss3.bdstatic.com/-Po3dSag_xI4khGkpoWK1HF6hhy/baike/c0=baike92,5,5,92,30/sign=3513d1f13c6d55fbd1cb7e740c4b242f/9825bc315c6034a84d5b05aeca13495409237667.jpg
  [6]: https://zh.wikipedia.org/wiki/%E5%80%92%E6%8E%92%E7%B4%A2%E5%BC%95
  [7]: https://en.wikipedia.org/wiki/PageRank
  [8]: https://baike.baidu.com/item/BigTable