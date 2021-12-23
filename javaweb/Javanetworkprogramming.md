## java网络编程
### http协议原理
#### 协议
网络协议的简称，计算机遵守这个协议才能交流，主要有http和https两种
+ 区别

![图片](https://qgt-document.oss-cn-beijing.aliyuncs.com/PY2/py2-1/ht%3Ahts%E5%8C%BA%E5%88%AB.jpg?x-oss-process=image/resize,w_1024/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)
#### url
地址栏输入的地址就叫做url，英文全称Uniform Resource Locator
+ 格式规范
![规范](https://style.youkeda.com/img/ham/course/py2/py2-1-2.png?x-oss-process=image/resize,w_1024/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)
协议类型://域名/路径/?参数

多个参数之间用&分隔，参数用键值对表示key=value

和Windows文件路径\分割符不一样
+ 端口号
域名后的**:443**表示端口号

因为http默认端口号为80，https默认端口号443，默认的端口号可以省略，其他的要写
+ 路径
相对路径

不是以/开头的路径表示相对路径，以/开头的为绝对路径

不输入路径打开的是默认路径
### 简单api调用
#### get请求无参数
安装依赖库okhttp3
+ 安装方式，在pom.xml增加依赖
```
<!-- https://mvnrepository.com/artifact/com.squareup.okhttp3/okhttp -->
<dependency>
 <groupId>com.squareup.okhttp3</groupId>
 <artifactId>okhttp</artifactId>
 <version>4.1.0</version>
</dependency>
```
##### 使用okhttp3完成页面请求
+ 实例化
```
OkHttpClient okHttpClient = new OkHttpClient();
```

+ 执行调用
调用之前先实例化一个request
```
Request request = new Request.Builder().url(url).build();
```

构建调用对象
```
Call call = okHttpClient.newCall(request);
```

最后执行调用
call.execute()返回的是一个执行的结果的对象，需要转换成字符串
```
call.execute().body().string();
```
##### api
api是应用程序接口，是预先定义的函数
url本质上就是api
#### get请求有参数
只需把有参数的url直接传入方法中即可
#### post表单
提交数据至服务器进行增加，删除，修改操作都是post

post操作数据放在表单中，不是url
+ 创建表单对象FormBody
```
Builder builder = new FormBody.Builder();
// 设置数据，第一个参数是数据名，第二个参数是数据值
builder.add("", "");
FormBody formBody = builder.build();

Request request = new Request.Builder().url(url).post(formBody).build();
```

构建Request对象时用.post(formbody)，与api不同

用for循环添加表单数据
```
for(String key:formData.keySet())
{builder.add(key,formDate.get(key));}
```

#### post json数据
post的参数不是formbody,改为requestbody

[json](https://ham.youkeda.com/articles/detail/5f3757fd5e205f30b2c2b1f9)

FastJson依赖
```
<!-- 在下列地址查询最新的版本：https://mvnrepository.com/artifact/com.alibaba/fastjson -->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
    <version>1.2.62</version>
</dependency>
```



+ 先定义一个最终提交的常量数据
```
pravite static final MediaType JSON_TYPE = MediaType.parse("application/json;charset=utf-8");
```

utf-8是码表，[查询](https://www.cnblogs.com/csguo/p/7402034.html)

[完整代码](javaweb/postjson.md)

#### response 网页
除了返回值，还经常关注http状态码

常见状态码200表示成功，404表示出错

[更多状态码](https://ham.youkeda.com/articles/detail/5f3758675e205f30b2c2b2a4)

取得状态码的代码
```
call.execute().code()
```

但是即要读取相应内容，又要读取状态，而且不是再次请求，那么代码应该优化为
```
import okhttp3.Response;

// 执行请求
Response rep = call.execute();
// 获取响应状态码
int code = rep.code();
// 获取响应内容
String content = rep.body().string();

```

就是将原本的call.excute()保存为一次结果，再获取其中的内容
#### response 非文本文件

非文本文件获取不是用string()方法，而是用
```
response.body().bytes();
```

返回二进制编码，再用软件解析

#### response json
json是一段文本，也就是java的字符串，要解析内容需转换为java对象
```
JSON.parseObject()
```

#### 解析json对象
普通的直接转换，遇到多次嵌套的
```
{
  "code": 0,
  "data": {
    "ip": "117.89.35.58",
    "country": "中国",
    "area": "",
    "region": "江苏",
    "city": "南京",
    "county": "XX",
    "isp": "电信",
    "country_id": "CN",
    "area_id": "",
    "region_id": "320000",
    "city_id": "320100",
    "county_id": "xx",
    "isp_id": "100017"
  }
}
```

就转换后取出内部的再转换

```
Map contentObj = JSON.parseObject(content, Map.class);
Map dataObj = (Map)contentObj.get("data");
String city = (String)dataObj.get("city");
```

[json格式化工具](http://www.ab173.com/json/jsonviewernew.php)

#### user-agent
有时网站的会未成功，因为网站会校验是否是真实的浏览器发出的请求，api服务器认为不是真实的浏览器
+ 判断是否是真实的浏览器，需要从HTTP消息头（Headers）中取得（User-Agent）
[HTTP Headers文档](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers)
##### 代码中加上User-Agent信息来模拟真实浏览器发出的请求
+ 模拟win7 + chrome：
    Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/535.1 (KHTML, like Gecko) Chrome/14.0.835.163 Safari/535.1
+ 加入header信息
```
Request request = new Request.Builder()
    .url(url)
    .addHeader("User-Agent", "")
    .build();
```
addheader第一个参数是名称，第二个参数是值
##### referer
图片防盗链

在headers中加入referer信息

在浏览器能访问是因为没有了“来源”

+ 加入referer
```
Request request = new Request.Builder()
    .url(url)
    .addHeader("Referer", "https://ham.youkeda.com/course/j14/0")
    .build();
```

可把referer的信息设置为本站的信息




