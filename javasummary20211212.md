# java第五章总结
## 异常
使用try/catch 环绕，后面的代码不受影响
## 文件读
### 配置Apache commons-io
在pom.xml 的 <dependencies>节点
<dependency>
  <groupId>commons-io</groupId>
  <artifactId>commons-io</artifactId>
  <version>2.10.0</version>
</dependency>
### 文件的对象 File (java.io) 实例化
File filename = new File("文件路径");文件路径为相对路径，.表示项目下为根目录
也可写为File filename = new File("父路径","子路径");
### FileUtils 是Apache commons-io提供的
FileUtils.readFileToString(文件名,"utf-8");
//要用try/catch环绕，但是里面的语句块是独立的，接受的变量是String类型，若定义在外部需要初始化为null
### 逐行读文件
FileUtils.readLines(文件对象名,"utf-8");返回的是数组/集合，
### 逐行读后转换为对象
+ 用循坏
分隔符 Stringname.spilt("分隔符");返回的是数组，最后要add();
### 读出的文件字符串转换为时间
+ 用LocalDateTime.parse()
### json数据
+ 基本格式数据用键值对 name:value
+ 多条数据用逗号分开
+ 字符串在双引号中
+ 时间类型的注解
@JsonDeserialize(using = LocalDateTimeDeserializer.class)
@JsonSerialize(using = LocalDateTimeSerializer.class)
@JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyy-MM-dd HH:mm:ss")
private LocalDateTime gmtCreated;
#### 转换为字符串
+ ObjectMapper mapper = new ObjectMapper();
mapper.writeValueAsString(name);得到一个字符串
#### 将json字符串转换为java对象
先读文件，
+ mapper.readValue(stringname,User.class);后面的是转换的对象，返回的是后面转换为的类型
