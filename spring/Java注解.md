### Java注解

之前的例子：

```java
@Service
public class MessageServiceImpl implements MessageService{

    public String getMessage() {
         return "Hello World!";
    }

}
```

这个语句之前的语法`@service`就是注解

### Annotation(注解)

本质上来说注解是java推出的一种注释机制，后面统一称为Annotation，和普通注释有显著区别就是注解可以在编译，运行阶段读取

能在编译运行阶段读取信息就给了我们很多扩展空间，而且不会污染源代码，sping中就重度使用注解，通过运行阶段动态获取Annotation从而完成很多自定义行为

从另一个角度，Annotation也是一个java类

上面Service的源代码：

![Service源代码](https://style.youkeda.com/img/ham/course/j4/service1.svg)

### 需掌握五个小点：

+ Target
  
  java.lang.annotation.Target自身也是一个注解，只有一个数组属性，用于设定该注解的目标范围，比如说可以作用于类或者方法等
  
  具体可以作用的类型配置在java.lang.annotation.ElementType枚举类中，比如：
  
  1. ElementType.TYPE可以作用于类，接口类，枚举类上
  
  2. ElementType.FIELD可以作用于类的属性上
  
  3. ElementType.METHOD可以作用于类的方法上
  
  4. ElementType.PARAMETER可以作用于类的参数
     
     如果想同时作用于类和方法上，可以
     
     ```java
     @Target({ElementType.TYPE,ElementType.METHOD})
     ```

其他的元注解见[java注解和反射](../kuangJava/Java注解和反射.md)

下面的注解类：

```java
@Target(ElementType.PARAMETER)
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface RequestParam {

    @AliasFor("name")
    String value() default "";


    @AliasFor("value")
    String name() default "";


    boolean required() default true;

    String defaultValue() default ValueConstants.DEFAULT_NONE;
}
```

中的`@AliasFor("name")`是别名的意思，就是这个属性用这个别名也可以访问到
