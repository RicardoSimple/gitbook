JsonFormat是另一个json库，作用与fastJson类似，spring中自动加入了依赖

数据量不大时，一般将全部数据查询出来，调用mongoTemplate的findAll方法

```java
List<Singer> singers = mongoTemplate.findAll(Singer.class);
```

模型/服务设计思路：先抽象模型，比如歌曲，歌手；在构建service及实现类，再测试类

主题设计，观察前端页面的内容并进行抽象，比如

![](https://style.youkeda.com/img/ham/course/j13/j13-3-3-3.png?x-oss-process=image/resize,w_1600/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

将不同的板块抽象成一个类，并且可以高度抽象，相似板块也可以抽象为一个类，比如心情，年代，风格

最终高度抽象

#### 爬虫分析方法

除了找到需要爬取的网页外，还需要单独打开一次，打开调试观察Request Headers中的内容，特别是四个：

+ user-agent

+ referer

+ host

+ cookie

程序header务必一样

![](https://style.youkeda.com/img/ham/course/j13/j13-4-1-2.png?x-oss-process=image/resize,w_1481/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

#### 通用HttpUtil

不同的api的header值不一样

```java
package fm.douban.util;

import okhttp3.Call;
import okhttp3.OkHttpClient;
import okhttp3.Request;
import okhttp3.Request.Builder;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.stereotype.Component;

import javax.annotation.PostConstruct;
import java.io.IOException;
import java.util.HashMap;
import java.util.Map;
import java.util.concurrent.TimeUnit;

@Component
public class HttpUtil {
    private static Logger logger = LoggerFactory.getLogger(HttpUtil.class);

    // okHttpClient 实例
    // 连接 2 分钟超时，读取 4 分钟超时
    private static OkHttpClient okHttpClient = new OkHttpClient.Builder().build();

    @PostConstruct
    public void init() {
        logger.info("okHttpClient init successful");
    }

    /**     
     * 默认的 http header。     
     *     
     * @return
     */
    public Map<String, String> buildHeaderData(String referer, String host, String cookie) {
        Map<String, String> headers = new HashMap<>();

        // 注意替换成自己从浏览器观察到的值
        headers.put("User-Agent", "");

        // 不同的爬取目标，有不同的值
        if (referer != null) {
            headers.put("Referer", referer);
        }
        if (host != null) {
            headers.put("Host", host);
        }
        if (cookie != null) {
            headers.put("Cookie", cookie);
        }

        return headers;
    }

    /**     * 根据输入的url，读取页面内容并返回     */
    public String getContent(String url, Map<String, String> headers) {
        // 定义一个request
        Builder reqBuilder = new Request.Builder().url(url);

        // 如果传入 http header ，则放入 Request 中
        if (headers != null && !headers.isEmpty()) {
            for (String key : headers.keySet()) {
                reqBuilder.addHeader(key, headers.get(key));
            }
        }

        Request request = reqBuilder.build();
        // 使用client去请求
        Call call = okHttpClient.newCall(request);
        // 返回结果字符串
        String result = null;
        try {
            // 获得返回结果
            logger.info("request " + url + " begin . ");
            result = call.execute().body().string();
        } catch (IOException e) {
            logger.error("request " + url + " exception . ", e);
        }
        return result;
    }
}
```

这样不同的网页也可以调用这个工具组

css[解决图片防盗链](https://blog.csdn.net/weixin_33682790/article/details/88569303)

`th:replace`语法不仅可以指定布局，也可以指定页面内容片段。比如在页面内的某个节点使用`th:replace="player::player"`引入另一个内容片段，`::`前的player指的是内容片段文件`player.html`模板文件，之后指定是模板文件`th:fragment`的值

### js和thymeleaf结合

script标签要加上`th:inline="javascript"`

变量使用内联表达式
