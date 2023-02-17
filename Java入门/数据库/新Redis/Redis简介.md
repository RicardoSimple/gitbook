#### Redis简介

是最受欢迎的NoSQL数据库之一，是一个包含多种数据结构，支持网络，基于内存，可选持久性的键值对存储数据库

也符合**按照数据结构来组织、存储和管理数据的仓库**的定义

NoSQL是NOT ONLY SQL的缩写，泛指非关系型数据库

MySQL、SQLServer都是关系型数据库，一般用来存储重要信息

#### Redis使用场景

Redis的常见的、核心的使用场景是：作为数据缓存。因为数据读取速度快，能够大大提高运行效率。所以Redis大多数情况下被称为缓存

缓存，就是把数据存放在缓冲区，当查找数据时，首先会在缓存中查找，如果存在就获取，如果不存在就访问数据库，从缓存中读取数据会减少访问数据库的次数，提高运行效率

![](https://style.youkeda.com/img/ham/course/d2/d4-1-2-1.jpeg?x-oss-process=image/resize,w_584/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

读写缓存内容值Value，都是通过Key来完成。用Key进行查询的方式非常简单，不像关系型数据库可以写各种查询语句用于查询

Redis速度快的第一个直观原因就是数据存储在计算机的内存上，基于内存当然读写速度快，但是内存空间有限，所以数据不宜永久保存，计算机重启后数据消失

当然Redis也可以数据永久保存，在磁盘上

#### Redis优点

除速度快之外

1. 支持多种数据类型
   
   Redis支持Set,ZSet,List,Hash,String五种数据类型
   
   Redis是用ANSI C语言写的，不是java，不能把java对象和Redis数据类型直接相等

2. 持久化存储
   
   把数据从内存保存在磁盘上从而保证计算机掉电不丢失
   
   Redis使用RDB和AOF做数据的持久化存储。主从数据同时生成rdb文件并利用缓冲区添加新的数据更新操作做对应的同步
   
   ![](C:\Users\ricardo\AppData\Roaming\marktext\images\2022-09-13-20-13-21-image.png)

#### 缺点

1. 内存容量
   
   内存数据库，短时间内大量增加数据会导致内存不够用

2. 性能损耗
   
   进行完整持久化就会占用主机CPU，消耗网络宽带。

3. 重启速度慢
   
   硬盘中的数据加载进内存，时间比较久

#### 安装Redis

1. 安装Docker
   
   ```shell
   sudo yum -y update
   sudo yum -y install epel-release
   sudo yum -y install docker-io
   ```
   
   ![](C:\Users\ricardo\AppData\Roaming\marktext\images\2022-09-13-20-18-06-image.png)

2. 安装Redis并启动
   
   ```shell
   sudo docker pull redis:latest
   sudo docker images
   sudo docker run  --name redis -p 6379:6379 -d --restart=always redis:latest redis-server --appendonly yes --requirepass "Hello122342"
   ```
   
   6379是redis服务的端口号
   
   **Hello1223442是Redis服务器密码**

3. 验证是否成功
   
   ```shell
   sudo docker ps
   ```
