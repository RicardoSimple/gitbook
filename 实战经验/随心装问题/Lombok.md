### lombok简介

lombok可以让pojo类代码更加简洁，通过注释完成构造方法，get，set

[Lombok_百度百科 (baidu.com)](https://baike.baidu.com/item/Lombok/23780246?fr=aladdin)

mybatisplus ： 基础的数据库crud、分页等可以自动生成

swaggerUI: 接口文档自动生成，对接前端和测试更加方便

创建springboot项目引入springweb的依赖

<!--简化bean代码的工具包 会生成一系列的get和set方法-->

```xml
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <optional>true</optional>
        <version>1.18.6</version>
        <scope>provided</scope>  provided 表示只是在编译的时候生效，而不会在项目的jar包
    </dependency>
```

lombok在编译时会讲带有lombok注解的java文件编译为完整的class文件

#### 添加idea对lombok的支持

点击file--setting--plugin--搜索lombok 然后安装

开启注解处理 enable annotation processing  之后需要重启idea；

```java
@Setter 注解在类或字段，注解在类时为所有字段生成setter方法，注解在字段上时只为该字段生成setter方法。
@Getter 使用方法同上，区别在于生成的是getter方法。
@ToString 注解在类，添加toString方法。
@EqualsAndHashCode 注解在类，生成hashCode和equals方法。
@NoArgsConstructor 注解在类，生成无参的构造方法。
@RequiredArgsConstructor 注解在类，为类中需要特殊处理的字段生成构造方法，比如final和被@NonNull注解的字段。
@AllArgsConstructor 注解在类，生成包含类中所有字段的构造方法。
@Data 注解在类，生成setter/getter、equals、canEqual、hashCode、toString方法，如为final属性，则不会为该属性生成setter方法。
@Slf4j 注解在类，生成log变量，严格意义来说是常量。private static final Logger log = LoggerFactory.getLogger(UserController.class);
```
