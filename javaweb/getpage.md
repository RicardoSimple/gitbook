```
public String getContent(String url) {

    // 返回结果字符串
    String result = null;
    OkHttpClient okHttpClient = new OkHttpClient();
    Request request = new Request.Builder().url(url).build();
    Call call = okHttpClient.newCall(request);
    try {
      result = call.execute().body().string();
    } catch (IOException e) {
      e.printStackTrace();
    }

    return result;
  }

```

### 打印最终相应的url
```
response.request().url().toString()
```
需要前面有定义的response
