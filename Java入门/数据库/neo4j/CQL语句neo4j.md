### 使用数据库

`create database 数据库名`

`use 数据库名`

### 创建数据

```sql
CREATE(
    <node-name>:<label-name>
    {
    <property1-name>:<property1-value>
    ....
}
)
```

创建节点

+ node-name是节点名

+ label-name是标签名

+ property-name:property-value键值对
  
  + title属性是图形化显示默认显示的名字

+ CREATE可以换成MERGE

### 创建关系

```sql
CREATE (p1:profile1)-[r1:link]->(p2:profile2)
```

比如

```sql
CREATE (m:Movie{title:'冰雪奇缘',year:'2020'})
-[r1:links{maker:'company maker'}]
->(c:Campany{name:'迪士尼',location:'USA'})
```

会同时创建新的node

查询节点，并创建关系

```sql
MATCH (<node1-label-name>:<node1-name>),(<node2-label-name>:<node2-name>)
WHERE <condition>
CREATE
(<node1-label-name>)-[<relationship-label-name>:<relation-name>{
<relationship-properties>
}->(<node2-label-name>)
```

比如

```sql
MATCH(m1:Movie),(m2:Movie)
WHERE m1.year=m2.year
CREATE (m1)-[r:SAME_YEAR{is_same_year:'yes'}]->(m2)
RETURN r
```

### 删

DELETE用来删除节点和关系

```sql
MATCH (m:movie)
WHERE m.name='头脑特工队'
DELETE m
```

需要先删除关系，才能删除节点

```sql
MATCH (m1:Movie)-[r:LINKS]-(m2:Movie)
WHERE m1.name='冰雪奇缘'
DELETE r,m1,m2
```

REMOVE用来删除标签和属性

```sql
MATCH(m1:movie)
WHERE m1.name='冰雪奇缘'
REMOVE m1.year
```

### SET

改变标签和属性

```sql
MATCH(m1:movie)
WHERE m1.name='冰雪奇缘'
SET m1.year='2018'
```

不存在的属性会添加，存在的会替换

### 查询

```sql
MATCH (col {name:"tom hanks"}) RETURN col
```

这里的col是自定义列名

```sql
MATCH(m1:Movie)
WHERE m1.name='冰雪奇缘'
RETURN m1.year
LIMIT 10
```

### 其他

ORDER BY 和sql一样

LIMIT也和sql一样，还可以用SKIP 5获得底部5条数据

可以RETURN不存在的属性，这时就是null

RETURN 后面可以跟DISTINCT去重

RETURN 后面加AS重新命名

WHERE条件可以是IN

### UNION

两个子句中间放UNION语句用来合并两个结果，UNION会自动去重，UNION ALL不会去重

### 查询关系

```sql
MATCH (m1:Movie)-[r]-(m2:Movie) WHERE m1.title=‘头脑特工队’ RETURN r
```

### 案例

```sql
# 清除全部带关系的节点、关系
MATCH (a)-[r]->(b) DELETE r,a,b

# 清除节点，如果节点还连着关系，会报错
MATCH (a) DELETE a
```

如果是一次提交的语句，冒号前面的名字会识别成同一个节点。如果是分两次提交，会识别成不同的节点

```sql
CREATE (m:Movie{title:'冰雪奇缘',year:'2020'})
CREATE (m2:Movie{title:'头脑特工队',year:'2018'})
CREATE (c:Campany{name:'迪士尼',location:'USA'})
CREATE (m)-[r1:links{maker:'company maker'}]->(c),
(m2)-[r2:links{maker:'company maker'}]->(c)
```

m和c就认为是同一个，如果分开提交就会被认为是不同的

```sql
# * 关键字代表一切关系
match (a)-[*]-(b) return a,b

# 还有一种特殊写法
MATCH (a)-[:links]->(c)<-[:links]-(a1) RETURN a,a1
```
