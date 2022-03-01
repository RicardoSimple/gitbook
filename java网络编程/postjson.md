## postjson完整代码
```
package com.youkeda.test.http;

import com.alibaba.fastjson.JSON;
import okhttp3.*;

import java.io.IOException;
import java.util.HashMap;
import java.util.Map;

public class JsonPoster {

  public static final MediaType JSON_TYPE = MediaType.parse("application/json;charset=utf-8");

  /**
   * 向指定的 url 提交数据，以 json 的方式
   */
  public String postContent(String url, Map<String, String> datas) {
    // okHttpClient 实例
    OkHttpClient okHttpClient = new OkHttpClient();

    // 数据对象转换成 json 格式字符串
    String string = JSON.toJSONString(datas);


    //post方式提交的数据
    RequestBody requestBody = RequestBody.create(JSON_TYPE,string);

    Request request = new Request.Builder().url(url).post(requestBody).build();

    // 使用client去请求
    Call call = okHttpClient.newCall(request);
    // 返回结果字符串
    String result = null;
    try {
      // 获得返回结果
      result = call.execute().body().string();
    } catch (IOException e) {
      // 抓取异常
      System.out.println("request " + url + " error . ");
      e.printStackTrace();
    }
    return result;
  }

  public static void main(String[] args) {
    String url = "https://www.fastmock.site/mock/3d95acf3f26358ef032d8a23bfdead99/api/posts";
    Map<String, String> datas = new HashMap();
    datas.put("num","20190101");
    datas.put("name","王陆");
    datas.put("gender","男");
    datas.put("faculty","灵剑派");
    datas.put("discipline","无相剑骨");
    datas.put("class","一年一班");
    datas.put("startYear","2019");
    JsonPoster jsonPoster = new JsonPoster();


    String content = jsonPoster.postContent(url,datas);

    System.out.println("API调用结果");
    System.out.println(content);
  }
}
```

