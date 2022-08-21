### MongoDB

之前的内容都是将数据存在代码之中，但是实际的用户信息肯定是存放在数据库里

MongoDB是一个基于分布式文件存储的数据库，特点是可扩展，高性能，为WEB应用提供强大的支持

#### 安装MongoDB

使用**Docker**安装MongoDB

1. 安装指令：

```shell
sudo docker run -itd --name mongo -p 27017:27017 mongo
```

27017是MongoDB的服务端口号

2. 验证是否启动成功

```shell
sudo docker exec -it mongo mongo admin
```

使用这个指令进入MongoDB软件内，能登入就说明安装成功，看到界面：

![](C:\Users\ricardo\AppData\Roaming\marktext\images\2022-05-12-16-09-52-image.png)

3. 界面交互格式说明

光标停留在`>`后面才能输入数据库的管理命令，大多数以`db.`开头。而回车后系统返回的信息前面没有箭头

#### 创建数据库

MongoDB是一个软件，要使用数据库还必须创建具体的数据库

创建一个`practice`数据库

```shell
use practice
```

系统输出`switched to db practice`就表示成功了

#### 退出登录

在登录状态下执行`exit`可以退出MongoDB回到终端

#### 重启MongoDB

安装完成后可以执行

```shell
sudo docker ps
```

查看MongoDB的启动情况

如果看到有一条名字为mongo，端口为27017就表示当前正常启动了

#### 启动不正常

如果没有看到，就表示当前MongoDB软件启动失败，先执行

```shell
sudo docker ps -a
```

如果看到有名字为mongo的记录，说明当前处于暂停状态，此时只执行

```shell
sudo docker start mongo
```

如果还是没有记录，就重新安装

### Spring data MongoDB配置

在springboot配置MongoDB

#### pom.xml依赖

如果是官网创建工程只需添加选项

![](https://style.youkeda.com/img/ham/course/j4/j4-8-2-1.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

已创建的工程可以手动增加依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-mongodb</artifactId>
</dependency>
```

#### 配置

修改配置文件，增加配置

```shell
# 购买的云服务器的公网 IP
spring.data.mongodb.host=192.168.0.1
# MongoDB 服务的端口号
spring.data.mongodb.port=27017
# 创建的数据库及用户名和密码
spring.data.mongodb.database=practice
```

这是固定的

#### 云服务器的安全设置

需要检查云服务器的安全设置，必须开放27017端口，有些默认关闭了27017

### Spring Data CRUD

对数据库的操作一定要放在`@Service`类中，而不是放在`@Controller`中；且`@Controller`类可以调用`@Service`类的方法，反之不行

+ `@Service`类主要用于不易变的核心业务逻辑

+ `@Controller`类与前端页面紧密配合，调用`@Service`服务读写数据，从而响应前端请求

##### 新增数据

就是向数据库中插入一条数据，在Java中万物皆对象，所谓数据就是实例对象

```java
import org.springframework.data.mongodb.core.MongoTemplate;

//自动注入MongoTemplate实例
  @Autowired
  private MongoTemplate mongoTemplate;

  public void test() {
    Song song = new Song();
    song.setSubjectId("s001");
    song.setLyrics("...");
    song.setName("成都");

    mongoTemplate.insert(song);
  }
```

调用`insert()`方法就能把对象存入数据库

请求页面返回

```java
{"songResult":{"id":"5e55db3a461b9b3c3e1d6a56","name":"成都","lyrics":"...","subjectId":"s001"}}
```

系统会自动为id属性(主键)赋值

##### 查询数据

一条语句

```java
mongoTemplate.findById(songId, Song.class)
```

`findById()`方法第一个参数就是主键id，第二个参数是具体的类

##### 修改数据

修改操作包括两个部分：

+ 修改哪条数据

+ 哪个字段修改成说明值

所以修改操作也分为部分：修改条件和修改的字段

```java
// 修改 id=1 的数据
Query query = new Query(Criteria.where("id").is("1"));

// 把歌名修改为 “new name”
Update updateData = new Update();
updateData.set("name", "new name");

// 执行修改，修改返回结果的是一个对象
UpdateResult result = mongoTemplate.updateFirst(query, updateData, Song.class);
// 修改的记录数大于 0 ，表示修改成功
System.out.println("修改的数据记录数量：" + result.getModifiedCount());
```

即先使用条件对象`Criteria`构建条件对象`Query`实例，然后再调用修改对象`Update`的方法`.set()`设置需要修改的字段

最后调用`mongoTemplate.updateFirst(query,updateData,Song.class)`方法完成修改，第三个参数是具体的类

约定：主键不修改，且其他字段值为null表示不修改，值长度为0的字符串""表示清空此字段

##### 删除数据

只需要精确确定要删除什么数据即可

调用`mongoTemplate.remove()`即可删除数据，参数是对象，表示需要删除哪些数据

```java
Song song = new Song();
song.setId(songId);

// 执行删除
DeleteResult result = mongoTemplate.remove(song);
// 删除的记录数大于 0 ，表示删除成功
System.out.println("删除的数据记录数量：" + result.getDeletedCount());
```

创建一个对象并设置好属性值作为删除条件，符合条件的数据都将被删除，可以设置更多的属性值来提高精准度，但通过主键来删除数据是确保不误删的比较好的方法

#### Spring Data Query

显然只根据主键查询是不够的，通常情况下需要根据多条件查询

核心方法：

```java
List<Song> songs = mongoTemplate.find(query, Song.class);
```

因为可能查询到多条数据，所以结果是对象集合，第一个参数是查询对象`Query`实例，第二个参数就是查询什么样的对象

需要构建好的`Criteria`条件对象

```java
Query query = new Query(criteria);
```

而构建`Criteria`对象一般有两种情况

单一条件

- `Criteria criteria1 = Criteria.where("条件字段名").is("条件值")`

组合条件，根据或，且关系进行组合

或：

- ```java
  Criteria criteria = new Criteria();
  criteria.orOperator(criteria1, criteria2);
  ```

且：

- ```java
  Criteria criteria = new Criteria();
  criteria.andOperator(criteria1, criteria2);
  ```

组合条件可以多层组合

```java
import org.springframework.data.mongodb.core.query.Query;
import org.springframework.data.mongodb.core.query.Criteria;

public List<Song> list(Song songParam) {
    // 总条件
    Criteria criteria = new Criteria();
    // 可能有多个子条件
    List<Criteria> subCris = new ArrayList();
    if (StringUtils.hasText(songParam.getName())) {
      subCris.add(Criteria.where("name").is(songParam.getName()));
    }

    if (StringUtils.hasText(songParam.getLyrics())) {
      subCris.add(Criteria.where("lyrics").is(songParam.getLyrics()));
    }

    if (StringUtils.hasText(songParam.getSubjectId())) {
      subCris.add(Criteria.where("subjectId").is(songParam.getSubjectId()));
    }

    // 必须至少有一个查询条件
    if (subCris.isEmpty()) {
      LOG.error("input song query param is not correct.");
      return null;
    }

    // 三个子条件以 and 关键词连接成总条件对象，相当于 name='' and lyrics='' and subjectId=''
    criteria.andOperator(subCris.toArray(new Criteria[]{}));

    // 条件对象构建查询对象
    Query query = new Query(criteria);
    // 仅演示：由于很多同学都在运行演示程序，所以需要限定输出，以免查询数据量太大
    query.limit(10);
    List<Song> songs = mongoTemplate.find(query, Song.class);

    return songs;
}
```

#### Spring Data分页

分页是最常用的功能，为了防止一次查询的数据量太大

查询支持分页也比较简单，只需调用`PageRequest.of()`构建一个分页对象，然后注入到查询对象即可，方法第一个参数是页码，从0开始，第二个参数是每页的数量

```java
import org.springframework.data.domain.PageRequest;
import org.springframework.data.domain.Pageable;

Pageable pageable = PageRequest.of(0 , 20);
query.with(pageable);
```

对于分页，除了要查询结果外，还需要查询总数，才能进一步计算出总共多少页，实现完整的分页功能。所以还需两个步骤：

+ 调用`count(query,xx.class)`查询总数

+ 根据结果，分页条件，总数三个对象构建分页器对象

```java
import org.springframework.data.domain.Page;
import org.springframework.data.repository.support.PageableExecutionUtils;

// 总数
long count = mongoTemplate.count(query, Song.class);
// 构建分页器
Page<Song> pageResult = PageableExecutionUtils.getPage(songs, pageable, new LongSupplier() {
  @Override
  public long getAsLong() {
    return count;
  }
});
```

`PageableExcutionUtils.getPage()`方法第一个参数是查询结果，第二个参数是分页条件的对象，第三个是实现一个`LongSupplier`接口的匿名类，在匿名类的`getAsLong()`方法中返回结果总数，方法返回值是一个`Page`分页器对象

[分页类](https://blog.csdn.net/u014236541/article/details/50983400)

使用`getContent()`获得本页数据集
