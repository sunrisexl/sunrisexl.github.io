---
layout: post
title: '二.linux系统安装和配置'
subtitle: '安装配置redhat'
date: 2018-03-11
categories: bigdata
cover: 'http://githubpics.oss-cn-beijing.aliyuncs.com/bigdata/red-hat-1.jpg'
tags: bigdata linux 
---

 1. linux的实验环境
    * 版本: RedHat 7.4  64位 自带netcat服务器（测试：Spark Streaming）
    * VM：12
    * 网卡：仅主机模式
    * 一共3台虚拟机：安装JDK、配置主机名、关闭防火墙
    
        192.168.157.11 SY001.hadoop

	    192.168.157.12 SY002.hadoop
	    
	    192.168.157.13 SY003.hadoop
	    
    
 2. 详细步骤
    * VMware安装配置redhat
        * 文件->新建虚拟机->自定义->兼容性（下一步）->稍后安装操作系统->linux Red Hat Enterprise Linux 7 64 位->虚拟机名称：SY001->下一步->网络类型：仅主机模式->……->完成
        * 编辑虚拟机设置->CD/DVD->使用ISO映像文件（找到下载的redhat系统镜像）->确定
        * 开启此虚拟机->Install Red Hat ……->回车->语言版本continue->Date&Time选择上海或者北京
        * SOFTWARE SELECTION->**选择Server with GUI并在右侧下拉列表中选择Development Tools**（此处设置是为了安装redis时有gc++等）->done
        * NETWORK & HOSTNAME（配置网和主机名）-> Host name：SY001->apply->网卡切换为ON->Configure->
        * ![redhat配置][2]
        * ![redhat配置][3]
        * ![redhat配置][4]
        * ![redhat配置][1]
        * Begin Installation->设置root用户密码-重启
        * 重启后完成配置
    * linux配置
        * 关闭防火墙
            查看防火墙的状态：systemctl status firewalld.service

		    关闭防火墙：      systemctl stop firewalld.service
		    
		    禁用防火墙（永久）systemctl disable firewalld.service
		    
        * 设置主机名 （配置文件） /etc/hosts
        
            vi /etc/hosts
            
            192.168.203.111 SY001 SY001.hadoop

        * 创建ambari系统用户和用户组（可选，此处直接用root用户）
            
        * 配置SSH免密码登录
        
            主节点执行以下命令：
            
                ssh-keygen -t rsa
                
                ssh-copy-id -i .ssh/id_rsa.pub root@SY002
                
                ssh-copy-id -i .ssh/id_rsa.pub root@SY003
            
        * 开启NTP服务（时间校准服务）
        
            (此时可能无法连接外网，暂不配置)
            
                yum install ntp
                
                systemctl is-enabled ntpd
                
                systemctl enabled ntpd
                
                systemctl start ntpd
            
        * 检查DNS和NSCD
            配置全域名，所以需要检查DNS。为了减轻DNS的负担, 建议在节点里用 Name Service Caching Daemon (NSCD)
            
            vi /etc/hosts
                
                192.168.203.111 SY001 SY001.hadoop
                
                192.168.203.112 SY002 SY002.hadoop
                
                192.168.203.113 SY003 SY003.hadoop
                
                
            每台节点里配置FQDN，如下以主节点为例:
            
            vi /etc/sysconfig/network
            
                NETWORKING=yes
                
                HOSTNAME=SY-001.hadoop


        * 关闭SELinux（安全策略，在安装过程中已经关闭）


  [1]: http://githubpics.oss-cn-beijing.aliyuncs.com/bigdata/redhat02.jpg
  [2]: http://githubpics.oss-cn-beijing.aliyuncs.com/bigdata/redhat03.jpg
  [3]: http://githubpics.oss-cn-beijing.aliyuncs.com/bigdata/redhat04.jpg
  [4]: http://githubpics.oss-cn-beijing.aliyuncs.com/bigdata/redhat05.jpg