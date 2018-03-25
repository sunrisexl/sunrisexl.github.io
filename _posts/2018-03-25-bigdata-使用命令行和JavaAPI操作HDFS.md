---
layout: post
title: '四.使用命令行和JavaAPI操作HDFS'
subtitle: 'HDFS的操作'
date: 2018-03-25
categories: 大数据 
cover: 'http://githubpics.oss-cn-beijing.aliyuncs.com/bigdata/hadoop.jpg'
tags: bigdata hadoop
---

- Web Console 的端口： 50070
- 使用命令行操作hdfs

    普通命令
    
    - 创建目录
    ``` shell
    hdfs dfs -mkdir /aaa
    hdfs dfs -mkdir -p /bbb/aaa #如果父目录不存在，先创建父目录
    ```
    - 查看目录
    ``` shell
    hdfs dfs -ls /aaa
    hdfs dfs -ls -R /aaa #查看目录及子目录
    ```
    - 上传数据
    ``` shell
    hdfs dfs -put data.txt /data/input
    hdfs dfs -copyFromLocal
    hdfs dfs -moveFromLocal #相当于ctrl+x
    ```
    - 下载数据
    ``` shell
    hdfs dfs -copyToLocal
    hdfs dfs -get /input/data.txt . #下载到当前目录
    ```
    - 删除目录
    ``` shell
    hdfs dfs -rm /aaa
    hdfs dfs -rmr /aaa #删除目录和子目录
    ```
    - 把某个目录下的文件合并后在下载
    ``` shell
    hdfs dfs -getmerge /students ~/allstudents.txt
    ```
    - 拷贝
    ``` shell
    hdfs dfs -cp /input/data.txt /input/data2.txt
    ```
    - 移动
    ``` shell
    hdfs dfs -mv /input/data.txt /aaa/a.txt
    ```
    - 统计hdfs对应目录下的目录个数，文件个数，文件总大小(字节)
    ``` shell
    hdfs dfs -count /students
    hdfs dfs -du /students #更详细的内容
    ```
    - 查看文本的内容
    ``` shell
    hdfs dfs -cat /input/data.txt
    hdfs dfs -text /input/data.txt
    ```
    - 平衡操作，使数据均匀
    ``` shell
    hdfs balancer
    ```
    
    管理命令
    
    - 打印HDFS的报告
    ``` shell
    hdfs dfsadmin -report
    ```
    - 安全模式
    ```shell
    hdfs dfsadmin -safemode get
    hdfs dfsadmin -safemode enter #在安全模式下，hdfs是只读的
    hdfs dfsadmin -safemode leave
    ```
- 使用JavaAPI操作hdfs

    - 依赖的jar包：
    ```shell
    /root/training/hadoop-2.7.3/share/hadoop/common
    /root/training/hadoop-2.7.3/share/hadoop/common/lib
    /root/training/hadoop-2.7.3/share/hadoop/hdfs
    /root/training/hadoop-2.7.3/share/hadoop/hdfs/lib
    ```
    - 创建目录
    ```java
    @Test
	public void test1() throws Exception{
		//设置执行程序的用户是root，此时就有了写权限
		System.setProperty("HADOOP_USER_NAME", "root");
		//指定NameNode地址
		Configuration conf = new Configuration();
		//如果要使用主机名，要配置windows的hosts文件
		conf.set("fs.defaultFS", "hdfs://192.168.203.11:9000");
		
		//创建一个HDFS的客户端
		FileSystem client = FileSystem.get(conf);
		client.mkdirs(new Path("/folder1"));
		
		client.close();
	}
    ```
    
    - 上传数据
    
    ```java
    @Test
	public void testUpload1() throws Exception{
		//指定NameNode地址
		Configuration conf = new Configuration();
		conf.set("fs.defaultFS", "hdfs://bigdata11:9000");
		
		//创建一个HDFS的客户端
		FileSystem client = FileSystem.get(conf);	
		
		//构造一个输出流，指向HDFS
		OutputStream out = client.create(new Path("/folder1/a.tar.gz"));
		
		//构造一个输入流，从本地文件读入数据
		InputStream in = new FileInputStream("d:\\temp\\hadoop-2.7.3.tar.gz");
		
		//下面是Java的基本IO
		//缓冲区
		byte[] buffer = new byte[1024];
		//长度
		int len = 0;
		while((len=in.read(buffer)) > 0){
			//表示读入了数据，再输出
			out.write(buffer,0,len);
		}
		
		out.flush();
		
		out.close();
		in.close();
	}
	
	@Test
	public void testUpload2() throws Exception{
		//使用HDFS的工具类来简化程序
		//指定NameNode地址
		Configuration conf = new Configuration();
		conf.set("fs.defaultFS", "hdfs://bigdata11:9000");
		
		//创建一个HDFS的客户端
		FileSystem client = FileSystem.get(conf);	
		
		//构造一个输出流，指向HDFS
		OutputStream out = client.create(new Path("/folder1/b.tar.gz"));
		
		//构造一个输入流，从本地文件读入数据
		InputStream in = new FileInputStream("d:\\temp\\hadoop-2.7.3.tar.gz");
		
		//使用HDFS的工具类来简化程序
		IOUtils.copyBytes(in, out, 1024);
	}
    ```
    
    - 下载数据
    
    ```java
    @Test
	public void testDownload() throws Exception{
		//指定NameNode地址
		Configuration conf = new Configuration();
		conf.set("fs.defaultFS", "hdfs://bigdata11:9000");
		
		//创建一个HDFS的客户端
		FileSystem client = FileSystem.get(conf);	
		
		//从HDFS获取一个输入流
		InputStream in = client.open(new Path("/folder1/a.tar.gz"));
		
		//创建一个输出流，指向本地进行下载
		OutputStream out = new FileOutputStream("d:\\temp\\aaa.tar.gz");
		
		//下面是Java的基本IO
		//缓冲区
		byte[] buffer = new byte[1024];
		//长度
		int len = 0;
		while((len=in.read(buffer)) > 0){
			//表示读入了数据，再输出
			out.write(buffer,0,len);
		}
		
		out.flush();
		
		out.close();
		in.close();		
	}
	
	@Test
	public void testDownload1() throws Exception{
		//指定NameNode地址
		Configuration conf = new Configuration();
		conf.set("fs.defaultFS", "hdfs://bigdata11:9000");
		
		//创建一个HDFS的客户端
		FileSystem client = FileSystem.get(conf);	
		
		//从HDFS获取一个输入流
		InputStream in = client.open(new Path("/folder1/a.tar.gz"));
		
		//创建一个输出流，指向本地进行下载
		OutputStream out = new FileOutputStream("d:\\temp\\bbb.tar.gz");
		
		//使用HDFS的工具类来简化程序
		IOUtils.copyBytes(in, out, 1024);
    ```
    
    - 查询数据的元信息
    
    ```java
    @Test
	public void test1() throws Exception{
		//获取HDFS的目录信息
		//指定NameNode地址
		Configuration conf = new Configuration();
		conf.set("fs.defaultFS", "hdfs://bigdata11:9000");
		
		//创建一个HDFS的客户端
		FileSystem client = FileSystem.get(conf);	
		
		FileStatus[] fsList =  client.listStatus(new Path("/folder1"));
		for(FileStatus s: fsList){
			System.out.println("文件还是目录？" + (s.isDirectory()?"目录":"文件"));
			System.out.println(s.getPath().toString());
		}
	}
	
	@Test
	public void test2() throws Exception{
		//获取文件的数据块信息
		
		//指定NameNode地址
		Configuration conf = new Configuration();
		conf.set("fs.defaultFS", "hdfs://bigdata11:9000");
		
		//创建一个HDFS的客户端
		FileSystem client = FileSystem.get(conf);	
		
		//获取文件：/folder1/a.tar.gz数据块的信息
		FileStatus fs = client.getFileStatus(new Path("/folder1/a.tar.gz"));
		
		//通过文件的FileStatus获取相应的数据块信息
		/*
		 * 0: 表示从头开始获取
		 * fs.getLen(): 表示文件的长度
		 * 返回值：数据块的数组
		 */
		BlockLocation[] blkLocations = client.getFileBlockLocations(fs, 0, fs.getLen());
		for(BlockLocation b: blkLocations){
			//数据块的主机信息: 数组，表示同一个数据块的多个副本（冗余）被 保存到了不同的主机上
			System.out.println(Arrays.toString(b.getHosts()));
			
			//获取的数据块的名称
			System.out.println(Arrays.toString(b.getNames()));
		}
		
		client.close();
	}
    ```
    
    