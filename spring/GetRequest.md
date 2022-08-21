### Get Request

http网络中用的最多的两个协议就是get，post

### 使用Spring MVC来支持http服务的get协议

get请求参数`https://www.baidu.com/s?wd=test`

参数不同得到的内容也不同

#### 获取Http URL的参数

每个Http URL都可以设定自定义参数，跟上面百度的`wd`参数一样。

##### 需求：

自定义参数，希望可以根据歌单参数访问到不同的歌单页面

如果要解决这个需求，就需要先定义一个能够表达歌单的参数。歌单的id是可以索引到唯一一个歌单的，所以可以用歌单的id作为URL参数，所以我们的歌单URL：

```java
https://域名/songlist?id=xxxx
```

如果是本地电脑：

```java
http://localhost:8080/songlist?id=xxxx
```

##### 定义参数：

只需要在方法上面添加对应的参数和参数注解就可以了

代码：

```java
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.*;

@Controller
public class SongListControl {

    @RequestMapping("/songlist")
    public String index( @RequestParam("id") String id){
        return "html/songList.html";
    }

}
```

在之前的基础上添加了：

```java
@RequestParam("id") String id
```

要注意RequestParam注解的参数`"id"`这个值必须要和URL的param key一样，因为我们在url中定义的是id，所以我们这里写id

比如，如果我们访问的URL是

```html
https://域名/songlist?listId=xxxx
```

那么代码就是

```java
@RequestMapping("/songlist")
public String index( @RequestParam("listId") String id){
    return "html/songList.html";
}
```

#### RquestParam注解的包路径是

```java
org.springframework.web.bind.annotation.RequestParam
```

由于Spring MVC的注解都是在

```html
org.springframework.web.bind.annotation
```

包内，所以import的时候，直接用

`import org.springframework.web.bind.annotation.*;`

默认情况下，URL的参数传递到服务端都是变成字符串的，所以使用String来获取参数值肯定没问题

##### 操作参数

代码添加分支：

```java
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.*;

@Controller
public class SongListControl {

    @RequestMapping("/songlist")
    public String index( @RequestParam("id") String id){
        if("38672180".equals(id)){
            return "html/songList.html";
        }else{
            return "html/404.html";
        }
    }
}
```

调用url：

```html
http://xxx.agent.youkeda.com/songlist?id=123
```

现在访问到的是404页面

##### 获取多个参数

就是添加多个参数即可

```java
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.*;

@Controller
public class SongListControl {

  @RequestMapping("/songlist")
  public String index(@RequestParam("id") String id,  @RequestParam("pageNum") int pageNum){
     return "html/songList.html";
  }
}
```

基础的boolean，int，String数据类型是可以直接自动转化的

如果我们在Request方法声明了多个参数，那么URL访问的时候就必须要传递该参数，不如就访问不到

#### @GetMapping

一开始学习了`@RequestMapping`注解用于解析URL请求路径，这个路径默认是支持所有的HTTP Method。放开所有Http Method这样不是很安全，一般我们还是会明确制定Method，比如get请求

我们可以使用`@GetMapping`来替换注解`@RequestMapping`包路径一样，之前的代码就写成：

```java
import org.springframework.web.bind.annotation.*;


@GetMapping("/songlist")
public String index(@RequestParam("id") String id,@RequestParam("pageNum") int pageNum){
  return "html/songList.html";
}
```

#### 非必须传递参数

默认情况下，访问的URL中必须包含我们在Request服务里设定的参数，如果不想某个参数必须传递，就可以修改参数的注解

```java
@GetMapping("/songlist")
public String index(@RequestParam(name="pageNum",required = false) int pageNum,@RequestParam("id") String id){
  return "html/songList.html";
}
```

带上`required = false`就代表非必须

#### 输出JSON数据

有的时候作为服务端，只想返回数据，不返回HTML内容，目前通用的Web数据格式就是JSON，在Spring中配置：

```java
@GetMapping("/api/foos")
@ResponseBody
public String getFoos(@RequestParam("id") String id) {
  return "ID: " + id;
}
```

在方法上加上了`@ResponseBody`注解，这个注解的包和RequestParam的包一样

返回Java对象：

```java
public class User{
  private String id;
  private Stirng name;
  // 省略了 getter、setter
}


public class UserControl{
  //缓存 User 数据
  private static Map<String,User> users = new HashMap();

  /**   * 初始化数据    */
  @PostConstruct
  public void init(){
    User user = new User();
    user.setId("100");
    user.setName("ykd");
    users.put(user.getId(),user);
  }

  @GetMapping("/api/user")
  @ResponseBody
  public User getUser(@RequestParam("id") String id) {
    return users.get(id);
  }
}
```

Spring MVC会自动把对象转化为JSON字符串输出到网页

一般我们会把这种输出JSON数据的方法称为API
