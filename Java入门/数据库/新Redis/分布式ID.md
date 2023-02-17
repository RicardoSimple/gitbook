#### 分布式ID

ID是数据的唯一标识，常见是数据库自动生成`AUTO_INCREMENT`

##### 解决的问题

随业务的发展，数据量会越来越多，一张表或一个库无法容纳，会对数据进行分表分库，这样数据按照自己的节奏来自增，会造成ID冲突，需要一个单独的机制来负责生成唯一的ID

##### 应用场景

交易订单号等

这样的ID带有日期特征

可以根据当天时间及订单生成的序号来生成一个唯一的ID

Redis有特点就是**所有命令操作都是单线程的**，速度够快

Redis提供了自增原子命令，能保证生成ID是唯一有序的

引入关键类:

```java
import org.redisson.api.RAtomicLong;
import org.redisson.api.RedissonClient;
```

代码：

```java
//格式化格式为年月日
DateTimeFormatter dateTimeFormatter = DateTimeFormatter.ofPattern("yyyyMMdd");
//获取当前时间
String now = LocalDate.now().format(dateTimeFormatter);
//通过redis的自增获取序号
RAtomicLong atomicLong = redissonClient.getAtomicLong(now);
//设置一天过期
atomicLong.expire(1, TimeUnit.DAYS);
long number = atomicLong.incrementAndGet();
//拼装订单号
String orderId = now + "" + number;
```

通过redis获取一个自增的ID也需要一个key

先获取一个自增的实例对象：

```java
RAtomicLong atomicLong = redissonClient.getAtomicLong(now);
```

在取得实际自增值

```java
long number = atomicLong.incrementAndGet();
```

设置一天过期是合理的，第二天重新从1开始

### format扩展用法

转换日期

```java
DateTimeFormatter dateTimeFormatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
String now = LocalDate.now().format(dateTimeFormatter);
```

位数补充

```java
String.format("%04d", 12);
```

结果是`0012`

[string.format()详解](https://www.cnblogs.com/mark5/p/11430245.html)
