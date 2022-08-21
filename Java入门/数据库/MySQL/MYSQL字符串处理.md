### 字符串处理

mysql中也有对字符串处理的函数

#### CONCAT函数

##### 语法

```sql
SELECT column_name1,CONCAT(column_name2,str,column_name3).column_name4 
FROM table_name;
```

分析语法：

1. 查询语句

2. CONCAT函数可以拼接列名，字符串

3. 使用CONCAT函数时可以同时查询其他的列

4. CONCAT函数之间的参数用英文逗号隔开

比如

```sql
SELECT
id,
concat(hero_name,'的胜率为',win_rate)
FROM
timi_adc;
```

查询结果有一列为CONCAT,输出xxx的胜率为xx

**如果拼接的值有NULL，则结果一律为NULL**

###### 配合WHERE语句查询

比如

```SQL
SELECT
  concat(hero_name, '的胜率是', win_rate)
FROM
  timi_adc
WHERE
  id = 3;
```

###### 别名

优化拼接的结果，起一个别名，比如结果的列名叫result，可以写

```sql
SELECT
concat(hero_name.'的胜率为',win_rate) as result
FROM
timi_adc
WHERE
id=3;
```

**别名也可以应用在其他的列名上**

#### TRIM函数

使用trim函数来处理数据

##### 语法

```sql
TRIM(str)
```

很简单，就是把需要去除空格的数据放在TRIM()函数的括号里，比如

```sql
INSERT INTO
  timi_adc
VALUES
  (
    20,
    '      鲁班七号',
    'T1      ',
    0.5111,
    0.2300,
    0.0944,
    now(),
    now()
  ),
  (
    21,
    '     后羿      ',
    'QT1Q',
    0.5111,
    0.2300,
    0.0944,
    now(),
    now()
  );
```

这是插入有空格的

```sql
SELECT
trim(hero_name),
trim(fever)
FROM
timi_adc
WHERE
id=20;
```

这样得到的就是没有空格的，但是不会修改原数据，如果修改原数据需要配合UODATE或者DELETE

##### 语法拓展

trim函数也可以去掉前面或者后面的空格或其他字符

```sql
TRIM( BOTH|LEADING|TRAILING removed_str FROM str);
```

+ TRIM函数可以加上LEADING来只去除前面的空格，或者加上TRAILING来只去除后面的空格，默认都去掉

+ 可以删除指定的字符串内容，不加就默认去除空格

比如：

```sql
SELECT
  TRIM(
    TRAILING 'Q'
    FROM
      fever
  )
FROM
  timi_adc
WHERE
  id = 21;
```

去掉fever末尾的Q

#### REPLACE()函数

TRIM不能去掉字符串中间的值，修改中间的值用REPLACE()函数

##### 语法

```sql
UPDATE table_name
SET column_name=
REPLACE(column_name,string_find,string_to_replace)
WHERE conditions;
```

可以把找到的字符串替换成另一个字符串
