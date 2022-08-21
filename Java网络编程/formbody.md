```
public class FormPoster {

  public String postContent(String url, Map<String, String> formData) {
    // okHttpClient 实例
    OkHttpClient okHttpClient = new OkHttpClient();

    //post方式提交的数据
    Builder builder = new FormBody.Builder();


    // 放入表单数据
    for (String key : formData.keySet())
    {builder.add(key,formData.get(key));}

    // 构建 FormBody 对象
    FormBody formBody = builder.build();
    // 指定 post 方式提交FormBody
    Request request = new Request.Builder().url(url).post(formBody)
            // addHeader("Referer", ...) 这个知识点在后面的章节中学到，目前不要纠结
            .addHeader("Referer", "https://www.taobao.com")
            .build();

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
    String url = "http://tcc.taobao.com/cc/json/mobile_tel_segment.htm";
    Map<String, String> formData = new HashMap();
    formData.put("tel","18180181601" );

    FormPoster poster = new FormPoster();
    String content = poster.postContent(url, formData);

    System.out.println("API调用结果");
    System.out.println(content);
  }
}
```
