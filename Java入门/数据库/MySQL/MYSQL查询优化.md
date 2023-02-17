### 查询优化

#### LIKE查询

之前的查询大都是精准查询，更多的是模糊查询

##### 语法

```sql
SELECT
*
FROM
table_name
WHERE
condition
LIKE
condition
```

##### %

SQL LIKE子句中**使用百分号来表示任意字符**，如果没有使用任何`%`就相当于等于`=`

比如

```sql
SELECT
*
FROM
timi_adc
WHERE
hero_name LIKE '%孙%';
```

两个结果公孙离，孙尚香

%的位置会决定搜索结果的不同，

`%孙%`这个字符串含`孙`

`孙%`这个字符串以孙开头

`%孙`表示以孙结尾

##### _

这个符号和%不同

如果忘记孙尚香，只记得X尚香，可以写：

```sql
SELECT
*
FROM
timi_adc
WHERE
hero_name LIKE '_尚香'
```

`_尚香`和`%尚香`的区别在前者不能匹配公孙尚香，后者可以

#### AND&OR

多个条件

##### 语法

```sql
SELECT * FROM table_name WHERE conditionA AND/OR conditonB;
```

比如

```sql
SELECT
*
FROM
timi_adc
WHERE
win_rate>0.5
AND win_rate<0.51
OR win_rate<0.47;
```

AND/OR就是且/或，有时需要加上`()`来分隔条件

#### IN/NOT IN

IN语句是一种精准查询也需要配合WHERE使用

##### 语法

```sql
SELECT * FROM table_name WHERE colume IN (conditionA,conditionB);
```

比如

```sql
SELECT
*
FROM timi_adc
WHERE
fever IN ('T0','T3');
```

等价于

```SQL
SELECT
  *
FROM
  timi_adc
WHERE
  fever = 'T0'
  OR fever = 'T3';
```

可以简化语法

##### NOT IN/NOT LIKE

就是在IN前面加一个NOT，表示非，否定这个条件，比如

```sql
SELECT
*
FROM
timi_adc
WHERE
fever NOT IN ('T0');
```

括号不能省略

NOT LIKE 子句也是否定，比如

```sql
SELECT
*
FROM
timi_adc
WHERE
hero_name NOT LIKE '%孙%';
```

#### NULL值的处理

如果当提供的查询条件字段为NULL时，SELECT命令可能无法正常工作

##### NULL值

`mysql`为处理NULL提供了三种运算符

1. IS NULL:当列值为NULL时返回true

2. IS NOT NULL:当列的值不为NULL时返回true

3. <=>:比较操作符，当比较的两个值都为NULL时或者相等时返回true

**不能使用=NULL或者!=NULL来查找NULL值！**

##### 语法

```sql
SELECT field_name1,field_name2
FROM table_name
WHERE field_name2 IS NOT NULL/IS NULL
```

可以根据这个来查询不为NULL或者为NULL的数据，比如

```SQL
SELECT
  id,
  mobile
FROM
  student
WHERE
  mobile IS NOT NULL;
```
