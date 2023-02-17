Java8更新了许多新特性比如**Lambda、流和函数式编程**

例子：

```java
List<Fruit> fruits = Arrays.asList(
    new Fruit("香蕉"),
    new Fruit("苹果"),
    new Fruit("梨子"),
    new Fruit("西瓜"),
    new Fruit("荔枝")
);
```

 遍历打印名字

```java
for (int i = 0; i < fruits.size(); i++) {
  Fruit f = fruits.get(i);
  System.out.println(f.getName());
}
```

新特性：

```java
fruits.forEach(f -> {
  System.out.println(f.getName());
});
```

#### Lambda表达式

基本定义：

```java
f->{}
```

f是参数变量

比如sort排序

```java
List<Student> students = new ArrayList<Student>();
students.add(new Student(111, "bbbb", "london"));
students.add(new Student(131, "aaaa", "nyc"));
students.add(new Student(121, "cccc", "jaipur"));

// 实现升序排序
Collections.sort(students, (student1, student2) -> {
  // 第一个参数的学号 vs 第二个参数的学号
  return student1.getRollNo() - student2.getRollNo();
});

students.forEach(s -> System.out.println(s));
```

Collections.sort方法第二个参数是实现匿名类`new Comparator(){}`

注：如果参数有指定类型，那么一个参数也需要用括号包裹

```java
fruits.forEach((Fruit f) -> {
  System.out.println(f.getName());
});
```

#### lambda表达式引用外部变量

```java
List<Fruit> fruits = Arrays.asList(......);
String message = "水果名称：";

fruits.forEach(f -> {
  System.out.println(message + f.getName());
});

message = "";
```

可以引用外部变量

如果有报错：

![](C:\Users\ricardo\AppData\Roaming\marktext\images\2022-09-01-13-17-26-image.png)

因为引用的局部变量不允许被修改，**写在表达式后面也不行**

相当于final

**参数不能与局部变量同名**

#### 双冒号(::)操作符

之前只有一条执行语句的lambda表达式可以进一步简化

```java
List<String> names = Arrays.asList("zhangSan", "LiSi", "WangWu");

names.forEach(n -> {
  System.out.println(n);
});
```

```java
names.forEach(System.out::println);
```

使用`::`时，每次遍历取得的元素会自动传给`system.out.println`

##### 语法含义

![](https://style.youkeda.com/img/ham/course/j5/j5-1-5-1.svg)

##### 不同用法

1. 静态方法调用
   
   `LamdaTest::print`替代`f->LambdaTest.print(f)`

2. 调用非静态方法
   
   需要实例对象来调用；`System.out`指代的就是一个实例对象

3. 多参数
   
   ```java
   Collections.sort(students, (student1, student2) -> {
     // 第一个参数的学号 vs 第二个参数的学号
     return student1.getRollNo() - student2.getRollNo();
   });
   ```
   
   可以抽成一个方法
   
   ```java
   private static int compute(Student s1, Student s2) {
     ... ...
     ... ...
   }
   ```
   
   然后简写为
   
   ```java
   Collections.sort(students, SortTest::compute);
   ```
   
   会自动按顺序传参

4. 父类方法
   
   可以用super关键字
   
   ```java
    public void print(List<Fruit> fruits){
       fruits.forEach(super::print);
       }
   ```
