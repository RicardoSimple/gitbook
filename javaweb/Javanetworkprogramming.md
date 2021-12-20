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
