#### MyBatis映射对象

所有的ORM框架都需要有一个java对象来映射数据库的表

一般吧这种对象称为`DO`对象，对象名称规范为`表名+DO`

比如`User`表对应的类对象名称就是`UserDO`

##### DO对象包规则

一般情况都会把DO对象存在`xxx.xxx.dataobject`

##### DO对象数据类型

DO对象和普通的POJO对象类似，注意的就是属性类型要和数据库类型进行匹配

JDBC的规范：

![](C:\Users\ricardo\AppData\Roaming\marktext\images\2022-09-07-13-33-48-image.png)

注：日期类型对应的包为`java.util.Date`

##### 创建DO对象

把user表映射为UserDO

```java
package com.youkeda.comment.dataobject;

import java.time.LocalDateTime;

public class UserDO {

    private long id;

    private String userName;

    private String pwd;

    private String nickName;

    private String avatar;

    private LocalDateTime gmtCreated;

    private LocalDateTime gmtModified;

    public long getId() {
        return id;
    }

    public void setId(long id) {
        this.id = id;
    }

    public String getUserName() {
        return userName;
    }

    public void setUserName(String userName) {
        this.userName = userName;
    }

    public String getPwd() {
        return pwd;
    }

    public void setPwd(String pwd) {
        this.pwd = pwd;
    }

    public String getNickName() {
        return nickName;
    }

    public void setNickName(String nickName) {
        this.nickName = nickName;
    }

    public String getAvatar() {
        return avatar;
    }

    public void setAvatar(String avatar) {
        this.avatar = avatar;
    }

    public LocalDateTime getGmtCreated() {
        return gmtCreated;
    }

    public void setGmtCreated(LocalDateTime gmtCreated) {
        this.gmtCreated = gmtCreated;
    }

    public LocalDateTime getGmtModified() {
        return gmtModified;
    }

    public void setGmtModified(LocalDateTime gmtModified) {
        this.gmtModified = gmtModified;
    }
}
```

和普通的POJO类类似

#### MyBatis DAO

java工程化中，一般把数据层的服务称为DAO层，会包含对数据库操作的接口和实现类

MyBatis只需定义接口就可以完成对数据的增删改查，因为MyBatis做了很多动态的处理

##### 创建DAO层

即包

##### 定义DAO接口

以user表为例，接口就为`UserDAO`

```java
package com.youkeda.comment.dao;

import org.apache.ibatis.annotations.Mapper;

@Mapper
public interface UserDAO {


}
```

加上`@Mapper`注解

##### 引用DAO

完成定义后spring会自动加载接口并创建一个bean，只需注入即可

比如在Controller中

```java
package com.youkeda.comment.control;

import com.youkeda.comment.dao.UserDAO;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;

@Controller
public class UserController {

    @Autowired
    private UserDAO userDAO;

}
```

使用`@Autowired`实现自动注入

#### MyBatis查询

在`UserDao`接口类中只需补充SQL查询代码接口

1. 添加接口方法
   
   ```java
   package com.youkeda.comment.dao;
   
   import org.apache.ibatis.annotations.Mapper;
   import UserDO;
   import java.util.List;
   
   @Mapper
   public interface UserDAO {
   
       public List<UserDO> findAll();
   
   }
   ```
   
   添加了一个findAll方法,查询多条记录时使用List作为返回类型

2. 添加`@Select`注解
   
   ```java
   package com.youkeda.comment.dao;
   
   import UserDO;
   import org.apache.ibatis.annotations.Mapper;
   import org.apache.ibatis.annotations.Select;
   
   import java.util.List;
   
   @Mapper
   public interface UserDAO {
   
       @Select("SELECT id,user_name as userName,pwd,nick_name as nickName,avatar,gmt_created as gmtCreated,gmt_modified as gmtModified FROM user")
       List<UserDO> findAll();
   
   }
   ```
   
   `@Select`注解完整包路径：`org.apache.ibatis.annotations.Select;`
   
   默认参数就是SQL语句，和普通的SQL差别不大

3. API测试
   
   ```java
   package com.youkeda.comment.control;
   
   import com.youkeda.comment.dao.UserDAO;
   import UserDO;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.stereotype.Controller;
   import org.springframework.web.bind.annotation.GetMapping;
   import org.springframework.web.bind.annotation.RequestBody;
   import org.springframework.web.bind.annotation.ResponseBody;
   
   import java.util.List;
   
   @Controller
   public class UserController {
   
     @Autowired
     private UserDAO userDAO;
   
     @GetMapping("/users")
     @ResponseBody
     public List<UserDO> getAll() {
       return userDAO.findAll();
     }
   
   }
   ```
   
   由于MyBatis转化对象的时候是按照名称一一对应的，表字段和DO字段名称不一致，数据就会映射失败
   
   可以借助SQL别名

#### MyBatis插入

1. 添加接口方法
   
   ```java
   int insert(UserDO userDO);
   ```
   
   执行SQL插入语句时会返回插入行数，一般成功是返回1，所以返回值类型为int
   
   判断是否插入成功可以通过`返回值>0`来判断
   
   插入数据时，一般直接传递数据对象，所以参数为UserDO类型

2. 添加`@Insert`注解
   
   ```java
   package com.youkeda.comment.dao;
   
   import UserDO;
   import org.apache.ibatis.annotations.Insert;
   import org.apache.ibatis.annotations.Mapper;
   import org.apache.ibatis.annotations.Select;
   
   import java.util.List;
   
   @Mapper
   public interface UserDAO {
   
     @Insert("INSERT INTO user (user_name, pwd, nick_name,avatar,gmt_created,gmt_modified) VALUES(#{userName}, #{pwd}, #{nickName}, #{avatar},now(),now())")
     int insert(UserDO userDO);
   
     @Select("SELECT id,user_name as userName,pwd,nick_name as nickName,avatar,gmt_created as gmtCreated,gmt_modified as gmtModified FROM user")
     List<UserDO> findAll();
   
   }
   ```
   
   SQL语句中value值换成了`#{变量名}`格式，是MyBatis动态获取值的方式，执行时自动从上下文参数获取变量的值
   
   ```java
   VALUES(#{userName}, #{pwd}, #{nickName}, #{avatar},now(),now())
   ```
   
   `#{userName}`实际上是执行`userDO.getUserName()`方法来获取变量值，MyBatis会自动更新生成正式的SQL语句去数据库执行

3. API测试
   
   ```java
   @Controller
   public class UserController {
   
     @Autowired
     private UserDAO userDAO;
   
     @GetMapping("/users")
     @ResponseBody
     public List<UserDO> getAll() {
       return userDAO.findAll();
     }
   
     @PostMapping("/user")
     @ResponseBody
     public UserDO save(@RequestBody UserDO userDO) {
       userDAO.insert(userDO);
       return userDO;
     }
   
   }
   ```
   
   注意数据插入都用POST请求，使用JSON方式传递数据，为了接收JSON参数，需要在参数中添加`@RequestBody`注解
   
   但是这样输出的内容是没有主键值的，需要在`@Insert`注解上添加`@Option`主键
   
   完整路径：`org.apache.ibatis.annotations.Options`
   
   ```java
    @Insert("INSERT INTO user (user_name, pwd, nick_name,avatar,gmt_created,gmt_modified) VALUES(#{userName}, #{pwd}, #{nickName}, #{avatar},now(),now())")
     @Options(useGeneratedKeys = true, keyColumn = "id", keyProperty = "id")
     int insert(UserDO userDO);
   ```
   
   Options注解有三个参数：
   useGeneratedKeys:设置为true表示允许数据库使用自增主键
   
   keyColunm：设置表的主键字段名称，一般为id
   
   keyProperty：设置DO模型的主键字段，一般为id

#### MyBatis修改

修改数据方法

1. 接口方法
   
   ```java
    int update(UserDO userDO);
   ```
   
   会返回影响的行数，所以定义返回值为int
   
   和insert一样，Update参数为对象

2. `@Update`注解
   
   ```java
   @Update("update user set nick_name=#{nickName},gmt_modified=now() where id=#{id}")
   ```
   
   只修改了nick_name字段，根据主键id来执行`update`
   
   注：任何对数据的修改，都需要同步修改时间`gmt_modified`
   
   ```java
   @Update("update user set nick_name=#{nickName},gmt_modified=now() where id=#{id}")
       int update(UserDO userDO);
   ```

3. API测试
   
   ```java
   @PostMapping("/user/update")
       @ResponseBody
       public UserDO update(@RequestBody UserDO userDO) {
           userDAO.update(userDO);
           return userDO;
       }
   ```

注：什么字段能够修改，应该看会不会产生数据错乱的影响，比如`user_id`,`ref_id`就不能修改

#### MyBatis删除

1. 接口方法
   
   ```java
   int delete(long id);
   ```
   
   根据主键删除，SQL执行删除语句时会返回删除的行数，所以返回值设置为int型
   
   MyBatis除了支持DO对象之外还支持接收普通参数，但是为了在SQL中完成普通参数透析，需要对参数增加一个注解`@param`
   
   ```java
   public interface UserDAO {
       int delete(@Param("id") long id);
   }
   ```
   
   `@param`完整路径：`arg.apache.ibatis.annotations.Param`

2. `Delete`注解
   
   ```java
   @Delete("delete from user where id=#{id}")
   ```
   
   这里的`#{id}`和param中的id是一样的，比如如果是`@Param("key")`，那么SQL语句为：
   
   ```java
   @Delete("delete from user where id=#{key}")
   ```

3. 通过验证返回值是否大于0判断是否删除成功
   
   ```java
    @GetMapping("/user/del")
       @ResponseBody
       public boolean delete(@RequestParam("id") Long id) {
           return userDAO.delete(id) > 0;
       }
   ```
   
   ```java
   return userDAO.delete(id) > 0;
   ```

#### MyBatis简单查询

比如从数据库查询用户名来查询记录

设计接口方法：

```java
public interface UserDAO {

  UserDO findByUserName(@Param("userName") String name);

}
```

方法名最好能一眼看出功能

`@Select`注解

只需要写合适的SQL语句即可

```sql
select * from user where user_name=? limit 1
```

确保只取出一条数据

```java
public interface UserDAO {

  @Select("select id,user_name as userName,pwd,nick_name as nickName,avatar,gmt_created as gmtCreated,gmt_modified as gmtModified  from user  where user_name=#{userName} limit 1")
  UserDO findByUserName(@Param("userName") String name);

}
```

再加上测试API

```java
 @GetMapping("/user/findByUserName")
  @ResponseBody
  public UserDO findByUserName(@RequestParam("userName") String userName) {
    return userDAO.findByUserName(userName);
  }
```
