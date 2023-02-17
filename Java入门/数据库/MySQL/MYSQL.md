## 数据库

将大量数据保存起来，通过计算机加个而成的可以高效访问的数据集合DataBase简称DB

用来管理数据库的计算机系统成为数据库管理系统DBMS

从事管理和维护数据库管理系统的相关人员成为数据库管理员DBA

### 数据库分类

三种：层次式数据库，网络式数据库，关系型数据库

#### 关系型数据库

把复杂的数据结构归结为简单的二元关系

![](https://qgt-style.oss-cn-hangzhou.aliyuncs.com/newcoursep4/d1/d1-1/%E7%94%BB%E6%9D%BF.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

表的结构和excel类似

表的关系就是存在有关系的数据

关系就是数据能够对应的匹配，在关系数据库中正式名称为联结，对应英文名称为join

#### SQL

SQL是structured query language结构化查询语言，我们使用SQL来对数据库进行操作

#### NoSQL非关系型数据库

泛指非关系型数据库，不保证关系数据的ACID特性

[Y 栈ACID](https://ham.youkeda.com/articles/detail/5f3752bb5e205f30b2c2a986)

NoSQL中有redis和mongodb

### 安装并启动MySQL

使用docker安装

执行

```shell
sudo docker run -itd -p 3306:3306  \
    -e MYSQL_ALLOW_EMPTY_PASSWORD="root" \
    --name mysql \
    mysql \
    --character-set-server=utf8 \
    --collation-server=utf8_general_ci \
    --default-authentication-plugin=mysql_native_password
```

3306是MySQL的端口号

验证是否启动成功

`sudo docker ps`

出现

![](https://style.youkeda.com/newcoursep4/d1/d1-1-4-1.1.png?x-oss-process=image/resize,w_1024/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

#### 进入服务

`sudo docker exec -it mysql bash`

看到

`root@xxxxxxxxx:/#`

#### 登录MySQL

输入`mysql -uroot`

看到

![](C:\Users\ricardo\AppData\Roaming\marktext\images\2022-07-12-15-49-49-image.png)

#### 创建数据库

```shell
CREATE DATABASE youkedadb;
```

![](C:\Users\ricardo\AppData\Roaming\marktext\images\2022-07-12-15-50-20-image.png)

数据库名称为youkedadb

#### 登出MySQL

`\quit`

#### 退出服务

`exit`

#### 重启MySQL

`sudo docker ps`如果没有mysql

sudo docker ps -a看看有没有，如果有就是处于暂停，执行`sudo docker start mysql`

### 表格的结构

##### 表格：

+ 表由表名、行、列、列名组成

+ 表名是表的名字

+ 表格实质上是一个二维数组，行列都是从0开始数

##### 数据库和表

在云服务器上可以创建很多DB，公用一个云服务

##### 表名

表名就是表格名，在mysql中一般用小写英文字母加下划线

##### 字段

数据库表中每一列都是一个字段，第一行是字段名，下面都是字段的值，字段必须是唯一的，不能同名的字段。字段的值可以为null

##### 主键

每一张数据库表都可以有一个主键，主键最大的作用就是来标识数据

+ 主键是一个特殊字段

+ 表格可以没有主键但是最多只能拥有一个主键

+ 主键不能为NULL

+ 主键的值必须绝对唯一

+ 我们一般使用主键和其他的表联系

##### SQL常用数据类型

VARCHAR(可变的长字符串，类比string)

INT(整型)

DOUBLE(浮点型)

DATETIME(时间类型长度为0，格式为YY-MM0DD HH：MM：SS)

BIGINT(长整型，long)

##### 数据

数据库的一条数据就像是value，主键就像key

##### CRUD

![](C:\Users\ricardo\AppData\Roaming\marktext\images\2022-07-12-16-05-27-image.png)

### CRUD

#### 创建表格

需要提供表名、字段名、字段的数据类型

```SQL
CREATE TABLE `user`(
`id` INT(10)NOT NULL,
`mobile` VARCHAR(11) NOT NULL,
`nickname` VARCHAR(40) NOT NULL,
`gmt_created` datetime ,
`gmt_modified` datetime NOT NULL,
PRIMARY KEY ( `id` )
)ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

第一行

```SQL
CREATE TABLE  `user`()
```

意思是创建一个user表格

##### 创建字段

```SQL
`id` INT(10)NOT NULL,
`mobile` VARCHAR(11) NOT NULL,
`nickname` VARCHAR(40) NOT NULL,
`gmt_created` datetime ,
`gmt_modified` datetime NOT NULL,
```

 语法结构都是一样的，符合

`字段名+数据类型+长度+是否为null`

比如第一句：

+ id是字段名

+ INT是数据类型

+ (10)表示id最长为10位

+ dateime类型没有长度，不用定义长度

+ NOT NULL字段不能为空，必须输入值否则报错，如果这个值可以不输入值，那么可以不加NOT NULL

##### 约定主键

```SQL
PRIMARY KEY ( `id` )
```

意思是这张表的主键为id

主键：

+ 主键必须是已约定的字段

+ 主键不能为空

+ 主键的值不能重复

+ 主键最大的作用就是标识所以最好为计算机生成，人工不加干预生成的值

有时候会把INT类型的主键语句改为

```SQL
`id` INT UNSIGNED AUTO_INCREMENT
```

意思是id会从1开始自增，第二个为2，第三个为3

UNSIGNED是指无符号的，排除负数，但是数据库默认从1开始累计，所以关键词可以省略

企业级开发往往使用BIGINT作为主键因为数据过多

##### 设置储存引擎和编码方式

```sql
ENGINE=InnoDB DEFAULT CHARSET=utf8
```

意思是储存引擎为InnoDB，默认编码方式为utf-8

`InnoDB是MySQL的默认储存引擎`

##### 符号

包裹字段名的是反引号

定义字段语句间有`,`最后一句没有，SQL语句以分号结尾

##### 删除表格

```SQL
drop table table_name;
```

或者

```SQL
DROP TABLE IF EXISTS table_name;
```

不可逆的

##### 查看表的结构

线上命令：

```shell
mysql -h 192.168.0.1 -uroot -Dyoukedadb -e 'desc winner;'
```

+ 192.168.0.1是服务器ip

+ -uroot表示使用root用户

+ -Dyoukeddadb表示数据库是youkedadb

+ -e是执行后面单引号内的sql语句

desc winner是查看表结构的语句，前面的是指定自己的MySQL服务并执行

#### 备份数据

```shell
mysqldump -h 192.168.0.1 -uroot youkedadb > youkedadb.sql
```

之后执行ls可以看到多了`youkedadb.sql`文件再git提交

#### 恢复数据

检查备份文件，再执行

```shell
mysql -h 192.168.0.1 -uroot -Dyoukedadb<youkedadb.sql
```

+ `<youkedadb.sql`表示运行当前目录下的youkdedadb.sql文件

+ `192.168.0.1`是ip

+ `-uroot`表示使用root用户

+ 如果sql语句过长适合这种方式替代
