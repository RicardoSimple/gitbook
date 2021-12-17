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
