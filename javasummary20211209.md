# 第四章总结
## 封装对象
创建一个类，getter和setter
## LocalDateTime
### 有三种
LocalDate   LocalTime  LocalDateTime
### 获取当前时间
.now()
### 指定时间格式
路径 java.time.format.DateTimeFormatter
DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
### 将字符串转换成时间类型
DateTimeFormatter name = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
LocalDateTime.parse();括号里可以直接是字符串，但是字符串必须满足iso（2021-01-23T12：09：56）
也可以两参数，后面是指定格式如LocalDateTime.parse(str.dtf);
### List接口
List<类型> name = new ArrayList<>();
### 循环 
for 用集合.for可快速创建
### static静态变量
