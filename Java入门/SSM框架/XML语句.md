使用注解来执行SQL的方式还是不够的，多数情况下企业还是偏向XML来执行SQL

也是MyBatis推荐的方式，因为更灵活

底层执行原理都是一样的

#### MyBatis XML配置

想要使用XML，需要在配置文件中添加

`mybatis.mapper-locations`这个配置来指定MyBatis Mapper XML文件路径，一般来说是和DAO的包路径一样，所以配置应该为

```
mybatis.mapper-locations=classpath:com/youkeda/comment/dao/*.xml
```

这样启动MyBatis框架会扫描工程下的指定路径，加载这个路径下的所有XML文件`(*)`会匹配所有

一般会把代码之外的文件存在resources文件目录，所以上面配置对应的完整文件路径是

```
src/main/resources/com/youkeda/comment/dao/
```

注：如果有多个路径，也可以用`,`隔开，比如：

```
mybatis.mapper-locations=classpath:com/youkeda/dao/*.xml,classpath:com/youkeda/comment/dao/*.xml
```

#### MyBatis XML Mapper

约定：一个DAO类对应一个DAO.xml文件，比如UserDAO.java对应UserDao.xml

但是有的也会把DAO命名为Mapper

UserDAO.xml存放在：

```
src/main/resources/com/youkeda/comment/dao/UserDAO.xml
```

1. 头信息
   
   创建完xml文件之后，需要有头信息：固定格式
   
   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   ```

2. mapper根节点
   
   有头信息之后，继续添加mapper节点，比如：
   
   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <mapper namespace="com.youkeda.comment.dao.UserDAO">
   ```

</mapper>
   ```

   mapper节点有一个属性`namespace`，这是命名空间的含义，一般就是我们这个mapper所对应的DAO接口全称，比如：`userDAO`

3. resultMap
   
   用于处理表和DO对象的属性映射，确保表中的每个字段都有属性可以匹配
   
   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <mapper namespace="com.youkeda.comment.dao.UserDAO">
   
     <resultMap id="userResultMap" type="com.youkeda.comment.dataobject.UserDO">
       <id column="id" property="id"/>
       <result column="user_name" property="userName"/>
     </resultMap>
   
   </mapper>
   ```
   
   注：resultMap节点在mapper根节点内
   
   id：唯一标识，一般命名规则是`xxxResultMap`，比如`userResultMap`
   
   type:对应的DO类完整路径，比如`com.youkeda.comment.dataobject.UserDO`

4. resultMap子节点
   
   最主要的是`id,result`两个子节点
   
   id:设置数据库主键字段信息，column属性对应的是表的字段名称，property对应的是DO属性名称
   
   result：设置数据库的其他字段名称，column属性对应的是表的字段名称，property对应的是DO属性名称

完整的resultMap：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.youkeda.comment.dao.UserDAO">

  <resultMap id="userResultMap" type="com.youkeda.comment.dataobject.UserDO">
    <id column="id" property="id"/>
    <result column="user_name" property="userName"/>
    <result column="pwd" property="pwd"/>
    <result column="nick_name" property="nickName"/>
    <result column="avatar" property="avatar"/>
    <result column="gmt_created" property="gmtCreated"/>
    <result column="gmt_modified" property="gmtModified"/>
  </resultMap>

</mapper>
```

这样查询使用的别名`user_name as userName`写法就不用写了

#### MyBatis XML Insert语句

使用XML实现Insert功能

接口中新增add方法

```java
int add(UserDO userDO);
```

在xml文件中添加insert语句

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.youkeda.comment.dao.UserDAO">

 <resultMap id="userResultMap" type="com.youkeda.comment.dataobject.UserDO">
    <id column="id" property="id"/>
    <result column="user_name" property="userName"/>
    <result column="pwd" property="pwd"/>
    <result column="nick_name" property="nickName"/>
    <result column="avatar" property="avatar"/>
    <result column="gmt_created" property="gmtCreated"/>
    <result column="gmt_modified" property="gmtModified"/>
  </resultMap>

  <insert id="add" parameterType="com.youkeda.comment.dataobject.UserDO" >
    INSERT INTO user (user_name, pwd, nick_name,avatar,gmt_created,gmt_modified)
    VALUES(#{userName}, #{pwd}, #{nickName}, #{avatar},now(),now())
  </insert>

</mapper>
```

在mapper节点内

两个属性：

id：与DAO中的方法名要一样都为add

parameterType：传递参数类型，和add方法的参数类型要一样，add中传入的是UserDO

这里就需要`com.youkeda.comment.dataobject.UserDO`

节点内的代码与注解里使用的SQL一样

如果需要获取插入的主键id值，还需要配置`useGeneratedKeys,keyProperty`和注解`@Options`一样

#### MyBatis XML Update/Delete语句

updateXML语句也类似，只是把insert换成update

添加update节点

```xml
<update id="update" parameterType="com.youkeda.comment.dataobject.UserDO">
        update user set nick_name=#{nickName},gmt_modified=now() where id=#{id}
    </update>
```

##### delete

也是类似

添加delete节点

```xml
 <delete id="delete">
        delete from user where id=#{id}
    </delete>
```

注：delete并没有配置`parameterType`属性，因为

```java
int delete(@Param("id") long id);
```

delete参数是由`@Param`注解组成，Mybatis会把这类数据以Map传递，而parameterType默认就是Map，可以省略不写

#### MyBatis XML Select语句

XML模式的开发顺序：

1. 创建DO对象

2. 创建DAO接口，配置`@Mapper`注解

3. 创建XML文件，完成`resultMap`配置

4. 创建DAO接口方法

5. 创建对应的XML语句

##### select

```xml
<select id="findAll" resultMap="userResultMap">
        select * from user
    </select>

    <select id="findByUserName" resultMap="userResultMap">
        select * from user where user_name=#{userName} limit 1
    </select>
```

多了一个属性`resultMap`,一般配置为该XML下的resultMap节点的id值，比如userResultMap

这样就不需要使用别名

模糊查询，比如搜索时可能是user_name也可能是nick_name

```xml
<select id="query" resultMap="userResultMap">
    select * from user where user_name like CONCAT('%',#{keyWord},'%')
    or nick_name like CONCAT('%',#{keyWord},'%')
</select>
```

使用Like和concat
