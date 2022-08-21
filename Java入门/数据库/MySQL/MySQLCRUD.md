### 插入语句INSERT

MySQL中使用INSERT INTO SQL语句来插入数据

#### 语法

```SQL
INSERT INTO table_name(field1,field2,...fieldN)
VALUES
(value1,value2,...valueN);
```

意思是向指定的表插入若干字段和对应的值，比如

```SQL
INSERT INTO
  `user` (`id`, `mobile`, `nickname`, `gmt_created`)
VALUES
  (1, '13426069530', '叶冰', now());
```

+ user是表名

+ id,mobile等是字段名

+ id的值是数字可以直接写

+ mobile值是VARCHAR要用单引号包含

+ gmt_created是datetime类型一般用now()函数获取服务器当前时间

#### 简化

1. 如果主键设置为自增那么可以不插入主键和对应的数据

2. 如果插入的是所有的字段可以省略字段名，直接插入值，但是类型必须一致，比如
   
   ```SQL
   INSERT INTO table_name
   VALUES
   (value1,value2,...valueN);
   ```

#### 批量插入数据

一次性插入大量数据可以使用

```SQL
INSERT INTO table_name
VALUES
(value1,value2,...valueN),
(value1,value2,...valueN);
```

如果NOT NULL没有给到值会报错

### 查询SELECT

#### 语法

```SQL
SELECT field1,field2,.... FROM table_name;
```

意思是从指定表中查询指定列的信息

```SQL
SELECT
  id,
  hero_name
FROM
  timi_adc;
```

如果查询所有的字段

```SQL
SELECT * FROM timi_adc;
```

`*`表示所有的字段

#### WHERE子句

实际查询中很少直接限定字段查找，会加限定条件

MySQL中使用WHERE语句来限定条件

相等用`=`

##### 语法

```SQL
SELECT * FROM table_name WHERE condition;
```

condition是指条件

比如

```SQL
SELECT
  *
FROM
  timi_adc
where
  win_rate > 0.5;
```

#### Limit子句

##### 语法

```sql
SELECT * FROM table_name LIMIT parameter
```

parameter是LIMIT的参数，分情况：

###### 查询第x-y行

```sql
SELECT
*
FROM
timi_adc
LIMIT
5,6;
```

这个意思是查询timi_adc的第6-11行，**第一个参数5表示从第六行开始查，第二个参数6表示一共查询6行**

数据库的表格类似数组，从第0开始，所以5表示第六行

LIMIT语句一般是配合分页使用的

###### 查询第0-x行

```sql
SELECT
*
FROM
timi_adc
LIMIT
5;
```

意思是查询timi_adc表的第0-5行，等价于

`SELECT * FROM timi_adc LIMIT 0,5`

所以从0开始查询就可以省略第一个参数

###### 查询第x行

```sql
SELECT
*
FROM
timi_adc
LIMIT 
4,1;
```

限制第二个参数为1就可以

###### 和WHERE子句联合使用

LIMIT语句会放在WHERE语句后面，比如

```sql
SELECT
*
FROM
timi_adc
WHERE
appearance_rate>0.1
LIMIT
5;
```

#### 排序（ORDER BY子句）

##### 语法

```sql
SELECT * FROM table_name ORDER BY field_name;
```

比如

```sql
SELECT
*
FROM
timi_adc
ORDER BY
win_rate;
```

默认排序按照升序排列，对于int，double是按照从小到大，对于varchar是字母A-Z，对于datetime是过去到现在

##### DESC关键词

默认是正序排列，关键词为`ASC`一般不写，可以加上关键词`DESC`将排序逆序，比如

```sql
SELECT
*
FROM
timi_adc
ORDER BY
win_rate DESC;
```

##### 和其他句子连用

和LIMIT子句一样，ORDER BY子句连用：(**先排序再LIMIT**)

```sql
SELECT
*
FROM
timi_adc
ORDER BY
win_rate DESC
LIMIT
3;
```

### 更新/删除

#### 更新UPDATE语句

##### 语法

```sql
UPDATE 表名称 SET 列名称 = 新值 WHERE 列名称 = 某值
```

**UPDATE语句必须加入WHERE限定条件，否则就会对整列起作用**

比如

```sql
UPDATE
timi_adc
SET
ban_rate = 0.01
WHERE
hero_name='艾琳';
```

#### 删除DELETE语句

##### 语法

```sql
DELETE FROM table_name [WHERE Clause]
```

删除语句是不可恢复的，所以务必要添加WHERE语句否则会删除整张表的数据

不同的情况：

###### 删除user表中id为4的行

`DELETE FROM user WHERE id=4`

###### 删除user表中所有id小于20的数据

`DELETE FROM user WHERE id<20`

###### 删除user表中的所有数据

`DELETE FROM user`

DELETE语句只会删除表中的数据，如果要删除表格，用之前的`DROP TABLE +表名`
