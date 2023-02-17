#### MyBatis XML 条件语句

##### if语句

```xml
<update id="update" parameterType="com.youkeda.comment.dataobject.UserDO">
  update user set nick_name=#{nickName},gmt_modified=now() where id=#{id}
</update>
```

如果nickname为null，就会出错，因为字段不应该为null

所以结合if语句

```xml
<update id="update" parameterType="com.youkeda.comment.dataobject.UserDO">
  update user set
   <if test="nickName != null">
    nick_name=#{nickName},gmt_modified=now()
   </if>
   where id=#{id}
</update>
```

这样if语句就会对数据提前做判断

但是如果nickname为null，SQL语句就会变成

```sql
update user set  where id=?
```

##### set语句

所以结合set语句防止这样的错误

```xml
<update id="update" parameterType="com.youkeda.comment.dataobject.UserDO">
  update user
  <set>
    <if test="nickName != null">
      nick_name=#{nickName},
    </if>
    <if test="avatar != null">
      avatar=#{avatar},
    </if>
    gmt_modified=now()
  </set>
   where id=#{id}
</update>
```

任何情况下修改操作一定更新gmt_modified就可以避免所有列值都为null，引起语法错误

注：`<set>`语句，系统会自动去除最后一个逗号，不用担心到底哪一个列才是最后一个

##### if+select

if语句也可以用于查询语句，很多时候查询语句都是动态的，比如想模糊查找某个时间后注册的用户

```java
List<UserDO> search(@Param("keyWord")String keyWord,      @Param("time")LocalDateTime time);
```

XML：

```xml
<select id="search" resultMap="userResultMap">
  select * from user where
    <if test="keyWord != null">
      user_name like CONCAT('%',#{keyWord},'%')
        or nick_name like CONCAT('%',#{keyWord},'%')
    </if>
    <if test="time != null">
      and  gmt_created <![CDATA[ >= ]]> #{time}
    </if>
</select>
```

注：`>=,<,<=,>,>=,&`这类的表达式会导致mybatis解析失败，

需要用`<![CDATA[ key ]]>`来包围住

而且这里的time参数是LocalDateTime类型，但是url传来的都是字符串类型。所以API调用要特殊处理：

```java
 @GetMapping("/user/search")
    @ResponseBody
    public List<UserDO> search(@RequestParam("keyWord") String keyWord,    @RequestParam("time")
    @DateTimeFormat(pattern = "yyyy-MM-dd HH:mm:ss")
    LocalDateTime time) {
        return userDAO.search(keyWord, time);
    }
```

在`LocalDateTime time`参数额外增加`@DateTimeFormat`注解，用于把字符串参数转化为日期类型

```java
org.springframework.format.annotation.DateTimeFormat
```

支持自定义日期数据格式，一般建议使用`yyyy-MM-dd HH:mm:ss`

所以传入参数的时候格式就不能写错

```
time=2020-01-01 00:00:00
```

##### where

如果上面的查询注册时间前的用户

1. keyWord为null
   
   ```sql
   select * from user where
      and  gmt_created >= ?
   ```

2. 都为null
   
   ```sql
   select * from user where
   ```

都是错误的SQL语句

所以如果把where改成XML中的where节点

```xml
<select id="search" resultMap="userResultMap">
  select * from user
   <where>
      <if test="keyWord != null">
          user_name like CONCAT('%',#{keyWord},'%')
            or nick_name like CONCAT('%',#{keyWord},'%')
      </if>
      <if test="time != null">
        and  gmt_created <![CDATA[ >= ]]> #{time}
      </if>
   </where>
</select>
```

Mybatis XML其他条件条件语句：

choose，when，otherwise，trim

[mybatis &#x2013; MyBatis 3 | 动态 SQL](https://mybatis.org/mybatis-3/zh/dynamic-sql.html)

#### MyBatis XML 循环语句

遇到批量插入，就需要foreach语句

比如批量插入user：

1. 创建DAO
   
   ```java
   @Mapper
   public interface UserDAO {
   
      int batchAdd(@Param("list") List<UserDO> userDOs);
   
   }
   ```

2. foreach语法
   
   ```xml
   <insert id="batchAdd" parameterType="java.util.List" useGeneratedKeys="true" keyProperty="id">
       INSERT INTO user (user_name, pwd, nick_name,avatar,gmt_created,gmt_modified)
       VALUES
       <foreach collection="list" item="it" index="index" separator =",">
           (#{it.userName}, #{it.pwd}, #{it.nickName}, #{it.avatar},now(),now())
       </foreach >
   </insert>
   ```
   
   相当于执行了java的for循环，属性：
   
   collection：指定集合的上下文参数名称，比如list，对应`@Param("list")`
   
   item：指定遍历中的每一个数据的变量，一般用it命名，就可以用`it.userName`获取到具体的值
   
   index：集合的索引值，从0开始
   
   separator：遍历每条记录并添加分隔符

上面的SQL会执行为：

```sql
INSERT INTO user (user_name, pwd, nick_name,avatar,gmt_created,gmt_modified)
    VALUES
    (?, ?, ?,?,now(),now()),
    (?, ?, ?,?,now(),now()),
    (?, ?, ?,?,now(),now())
```

自动优化逗号

另外使用foreach的场景：使用SQL in查询比如查询多个用户信息

1. 创建DAO方法
   
   ```java
   public interface UserDAO {
   
       List<UserDO> findByIds(@Param("ids") List<Long> ids);
   
   }
   ```

2. 完善XML
   
   ```xml
   <select id="findByIds" resultMap="userResultMap">
       select * from user
       <where>
           id in
           <foreach item="item" index="index" collection="ids"
                       open="(" separator="," close=")">
               #{item}
           </foreach>
       </where>
   </select>
   ```

多了两个参数：

open：表示的是节点开始时自定义的分隔符

close：表示的是节点结束时自定义分隔符

执行后会变为

```sql
select * from user where id in (?,?,?)
```
