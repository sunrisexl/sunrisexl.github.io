---
layout: post
title: '一. 大数据与hadoop简介'
subtitle: '大数据与hadoop的背景知识'
date: 2018-02-21
categories: 大数据
cover: 'https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1519225753088&di=f614d63f4b6167ac7137d725af4fe02f&imgtype=0&src=http%3A%2F%2Fm.c.lnkd.licdn.com%2Fmpr%2Fmpr%2Fp%2F3%2F005%2F049%2F0bf%2F2835fc4.jpg'
tags: bigdata hadoop
---

此系列博客将记录大数据学习的相关知识，亦作备忘
## 1. 大数据
[大数据][1]的各种介绍已经很多了，可以自行[谷歌][2]。
## 2. OLTP与OLAP
当今的数据处理大致可以分成两大类：联机事务处理OLTP（on-line transaction processing）、联机分析处理OLAP（On-Line Analytical Processing）。OLTP是传统的关系型数据库的主要应用，主要是基本的、日常的事务处理，例如银行交易。OLAP是数据仓库系统的主要应用，支持复杂的分析操作，侧重决策支持，并且提供直观易懂的查询结果。
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

## 3. 数据仓库
数据库与数据仓库的[区别][3]，主要在于数据库是传统的关系型数据库的主要应用，主要是基本的、日常的事务处理，例如银行交易。数据仓库系统的主要应用主要是OLAP（On-Line Analytical Processing），支持复杂的分析操作，侧重决策支持，并且提供直观易懂的查询结果。

## 4. hadoop的思想起源
Hadoop的主要是参考了谷歌在处理大数据的时使用的方法，包括文件系统GFS、倒排索引、PageRank、BigTable等

### GFS（[谷歌文件系统][4]）
GFS是一个可扩展的分布式文件系统，用于大型的、分布式的、对大量数据进行访问的应用。它运行于廉价的普通硬件上，并提供容错功能。它可以给大量的用户提供总体性能较高的服务。
![此处输入图片的描述][5]

### [倒排索引][6]
用来存储在全文搜索下某个单词在一个文档或者一组文档中的存储位置的映射。它是文档检索系统中最常用的数据结构。

### [PageRank][7]
PageRank的核心思想非常简单：
 - 如果一个网页被很多其他网页链接到的话说明这个网页比较重要，也就是PageRank值会相对较高
 - 如果一个PageRank值很高的网页链接到一个其他的网页，那么被链接到的网页的PageRank值会相应地因此而提高

### [BigTable][8]
BigTable是一种压缩的、高性能的、高可扩展性的，基于Google文件系统（Google File System，GFS）的数据存储系统，用于存储大规模结构化数据，适用于云计算。
  


  [1]: https://en.wikipedia.org/wiki/Big_data
  [2]: https://www.google.com.hk/search?num=20&newwindow=1&safe=strict&q=big%20data&spell=1&sa=X&ved=0ahUKEwiP2PevgbfZAhUS9mMKHQwtA30QBQgkKAA&biw=1280&bih=679
  [3]: https://www.zhihu.com/question/20623931
  [4]: https://baike.baidu.com/item/GFS/1813072
  [5]: https://gss3.bdstatic.com/-Po3dSag_xI4khGkpoWK1HF6hhy/baike/c0=baike92,5,5,92,30/sign=3513d1f13c6d55fbd1cb7e740c4b242f/9825bc315c6034a84d5b05aeca13495409237667.jpg
  [6]: https://zh.wikipedia.org/wiki/%E5%80%92%E6%8E%92%E7%B4%A2%E5%BC%95
  [7]: https://en.wikipedia.org/wiki/PageRank
  [8]: https://baike.baidu.com/item/BigTable