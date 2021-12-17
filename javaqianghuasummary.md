# java基础强化总结
## 数组
基本和c语言差不多
+ java数组的数组长度

    stringname.length;
+ 定义数组

    // 声明一个 int 数组的变量，数组大小为6
    int[] numbers = new int[6];
+ 函数返回类型包含数组
+ 多用递增
    public static void print(String[] names, int[] scores, int begin, int total) {
if (begin >= total) {
  return;
}

System.out.println(names[begin] + ":" + scores[begin]);
// 递增begin
begin++;
print(names,scores,begin, total);
}

## 字符串
+ 字符串长度
    stringname.length();
+ 取出字符串
    stringname.charAt(下标);
+ 去除左右多余的空格
    new String string = stringname.trim();
+ 查找字符串
    Stringname.indexOf("索引的内容");
    + 如果找到了就返回索引值的坐标；找不到就返回-1；
+ 查找下一个同样的内容
    indexof();的参数可以有两个，第二个参数是开始查找的坐标起始值；将第一个查找到的**返回值+匹配字符长度**作为起始值
+ 字符串拼接

