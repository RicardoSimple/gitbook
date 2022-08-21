#### Cookie

cookie主要用于识别用户身份，客户端与网站服务端通讯过程：

![](https://style.youkeda.com/img/ham/course/j4/j4-7-1.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

Spring工程有读写两种操作

##### 读Cookie

给control类的方法增加一个`HttpServletRequest`参数，通过request.getCookies()取得cookie数组

```java
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServletRequest;

@RequestMapping("/songlist")
public Map index(HttpServletRequest request) {
  Map returnData = new HashMap();
  returnData.put("result", "this is song list");
  returnData.put("author", songAuthor);

  Cookie[] cookies = request.getCookies();
  returnData.put("cookies", cookies);

  return returnData;
}
```

[cookie](https://ham.youkeda.com/articles/detail/5f37597b5e205f30b2c2b32f)有很多属性值

##### 使用注解读取cookie

如果知道cookie的名字就可以通过注解的方式读取，不需要再遍历cookie数组

给control类添加`@CookieValue("xxxx") String xxxx` 参数即可，注意使用时要填入正确的，系统会自动解析和传递值

```java
import org.springframework.web.bind.annotation.CookieValue;

@RequestMapping("/songlist")
public Map index(@CookieValue("JSESSIONID") String jSessionId) {
  Map returnData = new HashMap();
  returnData.put("result", "this is song list");
  returnData.put("author", songAuthor);
  returnData.put("JSESSIONID", jSessionId);

  return returnData;
}
```

如果系统解析找不到指定名字的cookie就会报错

##### 写cookie

通过request参数调用addCookie方法传入Cookie实例对象

```java
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServletResponse;

@RequestMapping("/songlist")
public Map index(HttpServletResponse response) {
  Map returnData = new HashMap();
  returnData.put("result", "this is song list");
  returnData.put("name", songName);

  Cookie cookie = new Cookie("sessionId","CookieTestInfo");
  // 设置的是 cookie 的域名，就是会在哪个域名下生成 cookie 值
  cookie.setDomain("youkeda.com");
  // 是 cookie 的路径，一般就是写到 / ，不会写其他路径的
  cookie.setPath("/");
  // 设置cookie 的最大存活时间，-1 代表随浏览器的有效期，也就是浏览器关闭掉，这个 cookie 就失效了。
  cookie.setMaxAge(-1);
  // 设置是否只能服务器修改，浏览器端不能修改，安全有保障
  cookie.setHttpOnly(false);
  response.addCookie(cookie);

  returnData.put("message", "add cookie successfule");
  return returnData;
}
```

四个设置cookie

```java
 // 设置的是 cookie 的域名，就是会在哪个域名下生成 cookie 值
  cookie.setDomain("youkeda.com");
  // 是 cookie 的路径，一般就是写到 / ，不会写其他路径的
  cookie.setPath("/");
  // 设置cookie 的最大存活时间，-1 代表随浏览器的有效期，也就是浏览器关闭掉，这个 cookie 就失效了。
  cookie.setMaxAge(-1);
  // 设置是否只能服务器修改，浏览器端不能修改，安全有保障
  cookie.setHttpOnly(false);
```

##### Spring Session API

把用户ID、登录状态等重要信息放入cookie会带来安全隐患

采用session会话机制可以解决这个问题，用户ID，登录状态等重要信息不存放在客户端，而是存放在服务端,从而避免安全隐患

![](https://style.youkeda.com/img/ham/course/j4/j4-7-2.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

名字为JSESSIONID的cookie，是专门用来记录用户session的。JSESSIONIDS是标准的、通用的名字

也分读写两种

##### 读操作

于cookie相似，从`HttpServletRequest`对象取得`HttpSession`对象，使用`getSession()`方法

返回的是对象，不是数组。在`attribute`属性中用key->value形式存储多个数据

比如存储登录信息的key是`userLoginInfo`，那么语句就是

```java
session.getAttribute("userLoginInfo");
```

##### 登录信息类

登录信息实例对象因为要在网络上传输，就必须实现序列化接口`Serializable`，否则不实现会报错

比如：

```java
import java.io.Serializable;

public class UserLoginInfo implements Serializable {
  private String userId;
  private String userName;
}
```

##### 操作代码

```java
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

@RequestMapping("/songlist")
public Map index(HttpServletRequest request, HttpServletResponse response) {
  Map returnData = new HashMap();
  returnData.put("result", "this is song list");

  // 取得 HttpSession 对象
  HttpSession session = request.getSession();
  // 读取登录信息
  UserLoginInfo userLoginInfo = (UserLoginInfo)session.getAttribute("userLoginInfo");
  if (userLoginInfo == null) {
    // 未登录
    returnData.put("loginInfo", "not login");
  } else {
    // 已登录
    returnData.put("loginInfo", "already login");
  }

  return returnData;
}
```

##### 写操作

记录登录信息到`Session`

调用`setAttribute()`方法

完成登录过程：(略去了校验用户名和密码的步骤)

```java
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

@RequestMapping("/loginmock")
public Map loginMock(HttpServletRequest request, HttpServletResponse response) {
  Map returnData = new HashMap();

  // 假设对比用户名和密码成功
  // 仅演示的登录信息对象
  UserLoginInfo userLoginInfo = new UserLoginInfo();
  userLoginInfo.setUserId("12334445576788");
  userLoginInfo.setUserName("ZhangSan");
  // 取得 HttpSession 对象
  HttpSession session = request.getSession();
  // 写入登录信息
  session.setAttribute("userLoginInfo", userLoginInfo);
  returnData.put("message", "login successfule");

  return returnData;
}
```

##### 注意：

Cookie存放在客户端，一般不能超过4kb，而Session存放在服务端，没有限制，不过也不能放太多东西

##### Spring Session配置

系统会自动把默认的`JSESSIONID`放在默认的`cookie`中

但cookie作为`sessionid`的载体，也能修改属性

`application.properties`是`SpringBoot`的标准配置文件，配置一些简单的属性。

但同时`SpringBoot`也提供了编程式的配置方式，主要用于配置`Bean`

基本格式：

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class SpringHttpSessionConfig {
  @Bean
  public TestBean testBean() {
    return new TestBean();
  }
}
```

在类上添加`@Configuration`注解，就表示这是一个**配置类**，系统会自动扫描并处理

在方法上添加`@Bean`注解，表示把**此方法返回的对象实例**注册成`Bean`

##### Session配置

依赖库

```xml
<!-- spring session 支持 -->
<dependency>
    <groupId>org.springframework.session</groupId>
    <artifactId>spring-session-core</artifactId>
</dependency>
```

##### 配置类

在类上额外添加一个注解`@EnableSpringHttpSession`开启`session`然后注册两个`bean`

+ `CookieSerializer`读写Cookies中的SessionId信息

+ `MapSessionRepository`Session信息在服务器上的存储仓库

```java
import org.springframework.session.MapSessionRepository;
import org.springframework.session.config.annotation.web.http.EnableSpringHttpSession;
import org.springframework.session.web.http.CookieSerializer;
import org.springframework.session.web.http.DefaultCookieSerializer;

import java.util.concurrent.ConcurrentHashMap;

@Configuration
@EnableSpringHttpSession
public class SpringHttpSessionConfig {
  @Bean
  public CookieSerializer cookieSerializer() {
    DefaultCookieSerializer serializer = new DefaultCookieSerializer();
    serializer.setCookieName("JSESSIONID");
    // 用正则表达式配置匹配的域名，可以兼容 localhost、127.0.0.1 等各种场景
    serializer.setDomainNamePattern("^.+?\\.(\\w+\\.[a-z]+)$");
    serializer.setCookiePath("/");
    serializer.setUseHttpOnlyCookie(false);
    // 最大生命周期的单位是秒
    serializer.setCookieMaxAge(24 * 60 * 60);
    return serializer;
  }

  // 当前存在内存中
  @Bean
  public MapSessionRepository sessionRepository() {
    return new MapSessionRepository(new ConcurrentHashMap<>());
  }
}
```

[Spring Session - Custom Cookie](https://docs.spring.io/spring-session/docs/2.3.0.RELEASE/reference/html5/guides/java-custom-cookie.html)
