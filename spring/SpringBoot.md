### Spring boot介绍

Spring团队重新打造的新的面向微服务的框架

更像是一个方案，Spring工程的快速上手方案，极大的降低了Java web工程的创建和运行和部署的难度

Spring boot的核心还是spring，所以无需单独管理Spring的Maven依赖，多了一些工程化方案:

+ 比如说Java Web容器的嵌入集成(所以有了spring boot就不再需要额外部署Tomcat这类的服务器了)Spring boot默认集成了Tomcat

+ Spring boot工程还自定义了工程打包格式，通过这个直接就把一个Java web工程转化为普通的Java工程，启动一个main方法就可以把Spring工程启动起来

+ Spring boot默认集成了你能想到的第三方框架和服务，比如数据库连接，NoSQL，安全等

+ Spring boot还提供了标准的属性配置文件，支持应用的参数动态配置，也让代码更灵活强大

### 创建一个Spring boot项目

[官方网站](https://start.spring.io/)可以直接创建

创建工程的时候还需要选择web依赖，就会自动集成web容器

[工程视频](https://qgt-document.oss-cn-beijing.aliyuncs.com/PY1/11/fm%E5%88%9B%E5%BB%BA%E5%B7%A5%E7%A8%8B.mp4)

### 运行Spring boot工程

Spring boot默认集成了web，所以可以通过网页访问

默认的错误也面：

![](https://style.youkeda.com/img/ham/course/j4/springboot_errorpage.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

##### Spring Boot ComponentScan

加了`@SpringBootApplication`注解的类是启动类，而Spring boot框架就会默认扫描启动类所在的包及其所有的子包

非子包的包不会自动扫描。也不会自动实例化Bean,所以会报错

###### 解决方法

为启动类的注解`@SpringBootApplication`加一个参数，告知系统需要额外扫描的包

```java
@SpringBootApplication(scanBasePackages={"fm.douban.app", "fm.douban.service"})
public class AppApplication {
  public static void main(String[] args) {
    SpringApplication.run(AppApplication.class, args);
  }
}
```

+ 参数名是：`scanBasePackages`

+ 参数值是一个字符串数组，用于指定多个需要额外自动扫描的包。需要把所有的代扫描的包的前缀都写入

###### 另一种写法：

如果不是spring boot的启动类，可以使用独立的注解`@ComponebtScan`,作用也是一样的，用于指定多个需要额外自动扫描的包

```java
@ComponentScan({"fm.service", "fm.app"})
public class SpringConfiguration {
  ... ...
}
```

###### `@RestController`

使用`@RestController`的类，所有方法都不会渲染Thymeleaf页面，而是都返回数据。等同于使用`@Controller`的类的方法上添加`@ResponseBody`注解，效果一样

#### Spring Boot Logger

加上了`@PostConstruct`注解的初始化init()方法中如果有输出语句，那么输出语句打印的内容是不确定的，在企业级的项目中都是用日志系统来记录信息

日志系统的两大优势：

+ 日志系统可以轻松控制日志是否输出

+ 日志系统可以灵活的配置日志的细节，例如输出格式

使用日志系统步骤：

1. 配置
   
   修改Spring boot系统的标准配置文件`application.properties`增加日志级别配置：`logging.level.root=info`表示所有日志(root)都为info级别
   
   也可以为不同的包定义不同的级别：
   
   `logging.level.fm.douban.app=info`就表示`fm.douban.app`包及其子包中的所有类的输出info级别的日志
   
   常用日志级别：
   
   最高/ERROR/错误信息日志，
   
   高/WARN/暂时不出错但高风险的警告信息日志，
   
   中/INFO/一般的提示语，普通的数据等不紧要的信息日志
   
   低/DEBUG/进开发阶段需要关注的调试信息日志
   
   级别的作用：
   
   `logging.level.root=error`意味着不输出更低优先级的WARN,INFO,DEBUG，只输出ERROR
   
   `logging.level.root=warn`意味着不输出更低优先级的INFO,DEBUG日志，只输出WARN和更高的ERROR日志
   
   在开发阶段配置为DEBUG，项目发布时调整为INFO或更高级别，即可做到不改代码而控制只输出关心的日志

2. 编码
   
   只需实例化日志对象即可打印日志

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.web.bind.annotation.RestController;
import javax.annotation.PostConstruct;

@RestController
public class SongListControl {
    private static final Logger LOG = LoggerFactory.getLogger(SongListControl.class);

    @PostConstruct
    public void init(){
        LOG.info("SongListControl 启动啦");
    }
}
```

先定义一个类变量LOG，然后在LOG.info()方法中的参数中输入日志内容

这里的方法名`info()`与日志级别一一对应

如果想输出警告信息就调用`LOG.error()`

###### 日志按级别输出

配置为`logging.level.root=error`时，`warn()`,`info()`,`debug()`都是无效的，都不会在Console打印日志内容

通过修改一个配置，就可以方便调节日志输出的内容

`getLogger()`方法参数为当前的类名即可

##### Spring Boot Properities

框架提供了`application.properties`配置文件

##### 配置文件格式

每一行是一条配置项：**配置项名称=配置项值**

```java
logging.level.root=info
logging.level.fm.douban.app=info
```

等号两边不加空格

配置文件遵守的规则：

+ 配置项名称能准确表达作用，含义，以点`.`分割单词

+ 相同前缀的配置项写在一起

+ 不同前缀的配置项之间空一行

###### 配置的意义

配置的主要作用是把可变的内容从代码中分离，做到在不修改代码的情况下，方便的修改这些可变的或常变的内容。这个过程称为避免硬编码，做到解耦

###### 自定义配置项

我们可以在配置文件中加入自定义的配置项

```properties
song.name=God is a girl
```

框架会自动加载并自动解析

使用配置项：

```java
import org.springframework.beans.factory.annotation.Value;

public class SongListControl {
    @Value("${song.name}")
    private String songName;
}
```

只需要使用`@Value`注解，项目启动会自动把配置文件中的song.name的值赋值给SongListControl对象实例的songName变量
