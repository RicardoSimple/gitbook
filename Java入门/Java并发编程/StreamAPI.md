Java8新特性流Stream

主要作用是对集合(Collection)中的数据进行各种操作，增强了集合对象的功能

#### 流迭代

Java中，Stream是一个接口，有多个实现类

接口中提供操作数据的方法，通常叫做API

##### 创建流

方式有很多种：

1. 直接创建
   
   ```java
   import java.util.stream.Stream;
   
   Stream<String> stream = Stream.of("苹果", "哈密瓜", "香蕉", "西瓜", "火龙果");
   ```

2. 由数组转化
   
   ```java
   String[] fruitArray = new String[] {"苹果", "哈密瓜", "香蕉", "西瓜", "火龙果"};
   Stream<String> stream = Stream.of(fruitArray);
   ```
   
   本质是一样的，第一种其实就是字符串数组

3. 由集合创建
   
   ```java
   List<String> fruits = new ArrayList<>();
   fruits.add("苹果");
   fruits.add("哈密瓜");
   fruits.add("香蕉");
   fruits.add("西瓜");
   fruits.add("火龙果");
   Stream<String> stream = fruits.stream();
   ```
   
   由于源数据是有序的，所以流中的元素也是有序的

##### 迭代流

Stream提供的迭代方法也是`forEach()`

```java
Stream<String> stream = Stream.of("苹果", "哈密瓜", "香蕉", "西瓜", "火龙果");
stream.forEach(System.out::println);
```

注：为了让系统自动识别Lambda表达式参数类型，必须用泛型指定Stream中对象的类型

#### 流数据过滤

比如筛选平均分不低于80且无违规记录

```java
public class Pupil {
    private String name;
    // 平均分
    private int averageScore;
    // 违规次数
    private int violationCount;
}
```

传统：

```java
List<Pupil> pupils = new ArrayList<>();
// 这里假设小学生数据对象已经存入了

// 有资格的小学生集合
List<Pupil> qualified = new ArrayList<>();
for (Pupil pupil : pupils) {
    // 统计是否满足条件
    if (pupil.getAverageScore() >= 80 && pupil.getViolationCount() < 1) {
        qualified.add(pupil);
    }
}

// 打印符合条件的小学生姓名
for (Pupil pupil : qualified) {
    System.out.println(pupil.getName());
}
```

新特性：

```java
List<Pupil> pupils = new ArrayList<>();
// 这里假设小学生数据对象已经存入了

pupils.stream()
    .filter(pupil -> pupil.getAverageScore() >= 80 && pupil.getViolationCount() < 1)
    .forEach(pupil -> {System.out.println(pupil.getName());});
```

使用了新的api

##### filter()方法

![](https://style.youkeda.com/img/ham/course/j5/j5-2-3-2.svg)

参数为lambda表达式，后面是条件语句判断是否符合条件，符合条件的才会被留下

**注**：这里的lambda不太一样，箭头后的条件语句为非可执行语句，传统代码写在`()`里，这里可以用小括号不能用大括号

#### 流的设计思想（1）

数据流的操作过程可以看作一个管道，由多个节点构成，每个节点完成一个操作

之前的`.filter().forEach()`就构成了一个管道，每个方法都是管道的一个节点。方法调用的顺序构成了管道的节点顺序

#### 流数据映射

计算每个数据的平方并输出

```java
numbers.stream()
    .map(num -> {
        return num * num;
    })
    .forEach(System.out::println);
```

##### map()方法

称为**映射**，作用就是用新的元素替换流中原来相同位置的元素，相当于每个对象都经历一次转换

![](https://style.youkeda.com/img/ham/course/j5/j5-2-5-2.svg)

##### 映射到新数据

方法参数是一个Lambda表达式，return语句返回的对象就是转换后的对象，替代流中的参数对象

映射后的对象类型，可以与流中原始的对象类型不一致

特例简写：

替换语句简单就可以简写

```java
.map(num -> num * num)
```

完整：

```java
.map(num -> {
  ... ...
  return ... ...;
})
```

#### 流数据排序

对排序处理语句之前的优化：

```java
List<Student> students = new ArrayList<Student>();
students.add(new Student(111, "bbbb", "london"));
students.add(new Student(131, "aaaa", "nyc"));
students.add(new Student(121, "cccc", "jaipur"));

// 实现升序排序
Collections.sort(students, (student1, student2) -> {
  // 第一个参数的学号 > 第二个参数的学号
  return student1.getRollNo() - student2.getRollNo();
});

students.forEach(s -> System.out.println(s));
```

使用`Collections.sort`方法，使用StreamAPI：

```java
students.stream()
    // 实现升序排序
    .sorted((student1, student2) -> {
        return student1.getRollNo() - student2.getRollNo();
    })
    .forEach(System.out::println);
```

##### sorted()方法

完成排序的方法

注意：参数顺序

不管是StreamAPI还是之前的lambda优化，**参数(student1,student2)中的student1代指后面一个元素，student2代指前一个元素**

##### 特例写法：

排序语句简单，可以简写(自动识别return)

```java
.sorted((student1, student2) -> student1.getRollNo() - student2.getRollNo())
```

注：这两个排序方法的排序规则都是**返回非正数表示两个元素需要交换位置**，返回正数不需要

#### 流数据摘取

```java
numbers.stream()
    .sorted((n1, n2) -> n2 - n1)
    .limit(3)
    .forEach(System.out::println);
```

##### limit方法

是返回流的前n个元素，只能是**流开头的**！

#### 流的设计思想(2)

Stream编程的重点是数据的计算；

普通java代码重点是使用对象完成各种逻辑

这种特点是：**函数式风格**

filter,map,sorted都统称为聚合操作

#### 并行流的性能意外

并行流计算模式性能不是任何情况都优于串行，原因：

1. 硬件

2. 任务简单

一般任务执行超过一小时的情况考虑使用并行模式优化性能

运行时间短也能用串行
