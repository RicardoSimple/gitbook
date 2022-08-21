#### Lamda表达式

使用Lamda表达式的好处：

+ 避免匿名内部类定义过多

+ 可以让代码看起来很简洁

+ 去掉了一堆没有意义的代码，只留下核心的逻辑

+ 实质属于函数式编程

#### 函数式接口

理解Functional Interface(函数式接口)是学习lamda表达式的关键

函数式接口的定义：

+ 任何接口，如果只包含**唯一一个**抽象方法，那么它就是一个函数式接口，比如：
  
  ```java
  public interface Runnable{
      public abstract void run();
  }
  ```

+ 对于函数式接口，我们可以通过lamda表达式来创建接口的对象

lamda表达式推导：

普通实现接口-->静态内部类-->局部内部类-->匿名内部类-->lamda表达式

```java
//先有一个函数式接口
interface ILike{
    void lamda();
}
//lamda表达式
ILike like = ()->{
    System.out.println("i like lamda");
}
```

有参数的

```java
//函数式接口
interface ILove{
    void lamda(int a);
}
//lamda表达式
ILove love = (int a)->{
    System.out.println("a");
}
```

简化1：去掉参数类型(多个参数也能去掉，要去掉就都去掉)

```java
ILove love = (a)->{
   System.out.println("a");
}
```

简化2：去掉括号(一个参数)

```java
ILove love = a->{
   System.out.println("a");    
}
```

简化3：去掉花括号(一行代码才能简化)

```java
ILove love = a->System.out.println(a);
```
