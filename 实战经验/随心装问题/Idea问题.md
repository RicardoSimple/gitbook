### 出现错误：

```java
错误: 找不到或无法加载主类
```

是idea自身的缓存问题，一般是删除pom部分依赖导致

解决：清理缓存重启IDEA

![](https://img-blog.csdnimg.cn/51428391d2c54f5498115a6f739c7776.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5q-P5aSp5Yqq5Yqb5a2m57yW56iL,size_7,color_FFFFFF,t_70,g_se,x_16)

还发现出现该问题可能是因为**启动类路径错了**或者**Jar 包的问题**

解决不了：

1. 点击IDEA右边的maven

2. 找到springboot->Lifecycle

3. 点击clean清空加载配置文件

4. 点击install重新加载相关文件

如果install出现错误，就用终端执行`mvn clean install`
