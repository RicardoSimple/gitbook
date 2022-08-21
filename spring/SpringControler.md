通过Spring boot就快速搞定了Spring MVC工程

Spring MVC是Java Web的一种实现框架

Java web的规范就是Servlet技术，所以所有的Java web都实现了Servlet API的定义

### Web服务

![web服务](https://style.youkeda.com/img/ham/course/j4/springmvc1.svg)

基本所有的网页加载都是这样的过程，在Spring boot方案里，一个网页请求到了服务器后，首先我们进入的是Java Web服务器，然后进入到Spring boot应用，最后匹配到某一个Spring  controller(这其实也是一个Spring Bean)然后路由到某一个具体的Bean方法，执行完后返回结果，输出到客户端来

其实只要掌握了Spring Controller就可以自己提供web服务了

### Spring controller技术有三个核心点：

+ Bean的配置：Controller注解运用

+ 网络资源的加载：加载网页

+ 网址路由的配置：RequestMapping注解运用

### Controller注解

Spring Controller本身也是一个Spring bean，只是它多提供了web能力，我们只需要在类上提供一个`@Controller`注解就行

```java
import org.springframework.stereotype.Controller;

@Controller
public class HelloControl {


}
```

Spring Controller一般不需要特定实现接口

### 加载网页

在Spring boot应用中，一般把网页放在`src/main/resources/static`目录下

在controller中会自动加载static下的内容，所以通过Spring boot建设网页也很容易

```java
import org.springframework.stereotype.Controller;

@Controller
public class HelloControl {

    public String say(){
        return "hello.html";
    }

}
```

say方法：

+ 定义返回类型为`String`

+ `return "index.html"`;返回的其实是文件路径

+ 当执行这段代码时，Spring boot实际加载的是`src/main/resources/static/index.html`文件

注意，<mark>文件路径不需要额外添加static</mark>

如果有子目录就要写子目录路径，比如`html/index.html`

### RequestMapping注解

我们之前看到的都是从浏览器或者客户端去请求url，对于web服务器来说，必须要实现的一个能力就是解析url，并提供资源内容给调用者。这个过程一般称为路由

Spring MVC支持路由能力而且简化了配置

只需要在需要提供web访问的方法上添加一个`@RequestMapping`注解就可以完成配置，比如：

```java
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class HelloControl {

    @RequestMapping("/hello")
    public String say(){
        return "html/hello.html";
    }

}
```

本机电脑访问是`127.0.0.1:端口:/路径`

端口默认都是8080，如果想修改就在配置文件`application propeties`里

`serve.port=端口`

路径是`@RequestMapping`注解中的路径

一般将controller类存放在control子包里
