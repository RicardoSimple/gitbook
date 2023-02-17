#### MyBatis简介

最主流的持久层框架，操作数据库的框架，非常轻量，只需要XML或者注解就可以完成数据映射和操作数据

##### ORM介绍

ORM 对象关系映射

是一种程序设计用于实现面向对象编程语言里不同类型系统的数据之间的转换

![](https://style.youkeda.com/img/ham/course/j6/orm.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

数据库映射成对象：

```txt
数据库的表（table） --> 类（class）
记录（record，行数据）--> 对象（object）
字段（field）--> 对象的属性（attribute）
```

Java领域所有数据库框架都是基于JDBC，最底层的

##### 集成MyBatis

创建时可勾选

![](C:\Users\ricardo\AppData\Roaming\marktext\images\2022-09-06-15-52-50-image.png)

依赖：

```xml
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>2.1.2</version>
</dependency>
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <scope>runtime</scope>
</dependency>
```

##### 配置MySQL连接

出现报错：

```shell
***************************
APPLICATION FAILED TO START
***************************

Description:

Failed to configure a DataSource: 'url' attribute is not specified and no embedded datasource could be configured.

Reason: Failed to determine a suitable driver class


Action:

Consider the following:
  If you want an embedded database (H2, HSQL or Derby), please put it on the classpath.
  If you have database settings to be loaded from a particular profile you may need to activate it (no profiles are currently active).
```

因为没有连接数据源，未填或填错都会报错

配置：

```properties
spring.datasource.url=jdbc:mysql://mysql数据库地址:数据库端口/数据库名称?serverTimezone=GMT%2B8
spring.datasource.username=用户名
spring.datasource.password=密码
```

#### 评论组件概要设计

评论有添加、回复等

软件工程角度，应该对所有的产品先做模型设计，再看数据库的设计

![](https://style.youkeda.com/img/ham/course/j6/comments-1.svg)

需要**用户模型**和**评论模型**

回复评论这个动作需要设计应该Tree结构才能满足存储。

比如多个人回复评论A时可以把回复的评论当作评论A的子，就可以形成Tree结构

![](https://style.youkeda.com/img/ham/course/j6/comments-uml.svg)

评论组件可以在很多地方用，比如文章。笔记。博客。需要关联到一个主键，就是`refId`

Comment里的**children字段就是回复的评论**

##### 数据库设计

满足上面的模型就需要两个表`User,comment`

![](https://style.youkeda.com/img/ham/course/j6/comment-er.svg)

![](C:\Users\ricardo\AppData\Roaming\marktext\images\2022-09-06-17-03-27-image.png)
