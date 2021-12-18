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
