# java基础强化总结
## 数组
基本和c语言差不多
+ java数组的数组长度

    stringname.length;
+ 定义数组
```
    // 声明一个 int 数组的变量，数组大小为6
    int[] numbers = new int[6];
```
+ 函数返回类型包含数组
+ 多用递增
 ```
    public static void print(String[] names, int[] scores, int begin, int total) {
if (begin >= total) {
  return;
}

System.out.println(names[begin] + ":" + scores[begin]);
// 递增begin
begin++;
print(names,scores,begin, total);
}
```
## 字符串
+ 字符串长度
```
 stringname.length();
```
+ 取出字符串
```
    stringname.charAt(下标);
```
+ 去除左右多余的空格
```
    new String string = stringname.trim();
```
+ 查找字符串
```
    Stringname.indexOf("索引的内容");
    + 如果找到了就返回索引值的坐标；找不到就返回-1；
```
+ 查找下一个同样的内容
```
    indexof();的参数可以有两个，第二个参数是开始查找的坐标起始值；将第一个查找到的**返回值+匹配字符长度**作为起始值
```
+ 字符串拼接
 ```   
    new String str = stringname.substring();
    可以有两个参数，如果只有一个参数，那么后面的都会被拼接，
    两个参数的话会包含左边不包含右边
    返回的是字符串
```
+ 字符串开始和结束内容判断 
```
    stringname.endWith(结尾字符串);返回布尔类型
    stringname.startWith(开头字符串);返回布尔类型
```
+ 字符串替换
```
    stringname.replaceAll("被替换","替换成");
    返回新的字符串，如果后面的是空字符，那么可以起到删除的作用
```
## 字符串操作
+ 字符串分割
```
    stringname.split("分隔符");
    返回一个数组
```
如果
```
    |
    .
    *
    ```
作为分隔符，需要在前面加上\\
+ 大小写转换
```
    stringname.toUpperCase();
    stringname.toLowerCase();
    ```
+ 字符串比较
```
    stringname.equals("text");
    返回布尔类型
    ```
+ 数字和字符串转化
```  
     字符串转数字
     String text = "123";
    // 转化字符串为数字
    int a = Integer.parseInt(text);
    ----
    a = Integer.parseInt("100");
    ```
``` 
    数字转字符串
    方法1
    public static void main(String[] args) {

    int a = 100;
    //使用空字符串相加数字，会自动变成字符串类型
    String str = ""+a;
    System.out.println(str);

  }
    方法2
    public static void main(String[] args) {

    int a = 100;
    //使用valueOf强制把数字转化为字符串
    String str = String.valueOf(a);
    System.out.println(str);
  }
  ```
## 包
  + 包的导入
  import 包名+类名;包名加类名组成了完整的包路径
## 日期类 LocalDate  包路径java.time.LocalDate
```
    LocalDate now = LocalDate.now();
    //以df格式输出时间
    system.out.println(now);//输出当前的时间
    如果里面是now.toString();会将时间转为字符串输出，但是这两种结果都一样
    ```
### 日期时间和字符串转换
  + 日期格式化 DateTimeFormatter  包路径java.time.format.DateTimeFormatter
  + 创建格式化方法
```
    DateTimeFormatter df = DateTimeFormatter.ofPattern("yyyy/MM/dd");
    ```
  + 转为字符串
  ```
    String time = df.format(now);
    ```
 + 获取时间日期具体值
   LocalDate time = LocalDate.now();
```
    // 得到当前时间所在年
    int year = time.getYear();
    System.out.println("当前年份 " + year);

    // 得到当前时间所在月
    int month = time.getMonth().getValue();
    System.out.println("当前月份 " + month);

    // 得到当前时间在这个月中的天数
    int day = time.getDayOfMonth();
    System.out.println("当前日 " + day);

    // 得到当前时间所在星期数
    int dayOfWeek = time.getDayOfWeek().getValue();
    System.out.println("当前星期 " + dayOfWeek);
 ```
    https://ham.youkeda.com/articles/detail/5f37576b5e205f30b2c2b170
    有些后面跟有getValue(),有些没有，因为getMonth()和getDayOfWeek()返回的是对象不是数字

+ 字符串转化为时间类型
```
LocalDate date2 = LocalDate.parse(date);
```
date是字符串，格式必须为yyyy-MM-dd HH:mm:ss
如果格式不是这个，那就要借助DateTimeFormatter
```
String date = "2019/01/01";

    DateTimeFormatter df = DateTimeFormatter.ofPattern("yyyy/MM/dd");

    // 把字符串转化位 LocalDate 对象，并得到字符串匹配的日期
    LocalDate date2 = LocalDate.parse(date,df);
    //两个参数，后面是格式
    ```
+ 时间日期的计算
LocalDate有一个plusDays(天数)的类执行天数相加
```
    LocalDate time = LocalDate.parse(checkInTime);
    // 使用 plusDays 添加天数，得到新的时间
    LocalDate leaveTime = time.plusDays(days);
```
其他的类型计算
```
    LocalDate now = LocalDate.now();
    System.out.println("当前：" + now.toString());

    System.out.println("加法运算");
    System.out.println("加1天：" + now.plusDays(1));
    System.out.println("加1周：" + now.plusWeeks(1));
    System.out.println("加1月：" + now.plusMonths(1));
    System.out.println("加1年：" + now.plusYears(1));

    System.out.println("减法运算");
    System.out.println("减1天：" + now.minusDays(1));
    System.out.println("减1周：" + now.minusWeeks(1));
    System.out.println("减1月：" + now.minusMonths(1));
    System.out.println("减1年：" + now.minusYears(1));
```
+ 两个日期时间的判断
判断是否是同一天，前，后
isBefore，isAfter，isEqual
返回的都是布尔类型
# 包
## 包的声明
```
package 包路径;
```
包路径是该java所在的目录，一个文件只有一个package语句，在最前
```
比如Hello.java的包路径
com.youkeda.test.Hello.java
或者
com.youkeda.test.Hello
省略了后缀名
```

