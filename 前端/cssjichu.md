### css-美化文档
#### html内部添加样式
+ 在标签中添加声明
声明的关键字是style后接=再接""

```
<input type="text" placeholder="手机号码" style="">
```

声明位置不分先后
+ 在引号之间添加样式

```
<p style="font-size:14px;color:white"></p>
```

意思是设置p标签中的字体大小为14px，颜色为白色
#### 字体大小，字体粗细
css中，样式是由属性和值组成，冒号隔开分号收尾
![image-2.png](./image-2.png)
```
<!-- 设置字体的大小为12px -->
<p style="font-size: 12px;">
  一个轻量级和模块化的前端框架，用于开发快速和强大的web接口。
</p>
<!-- 设置字体的大小为24px -->
<p style="font-size: 24px;">
  一个轻量级和模块化的前端框架，用于开发快速和强大的web接口。
</p>
```

字体大小font-size

字体加粗font-weight
设置字体粗细时，其值可以是100,200,300,400,500,600,700,800,900中的任何一个，或者可以用英文代替，normal（正常粗细400），
lighter（细200），bold（粗700），bolder（更粗）

```
<p style="font-weight: 200;">优课达--学的比别人好一点～</p>
<p style="font-weight: lighter;">优课达--学的比别人好一点～</p>
<p style="font-weight: 400;">优课达--学的比别人好一点～</p>
<p style="font-weight: normal;">优课达--学的比别人好一点～</p>
<p style="font-weight: 700;">优课达--学的比别人好一点～</p>
<p style="font-weight: bold;">优课达--学的比别人好一点～</p>
```

#### 字体颜色
color:XXX

color的值有多种方式

+ 英文字母方式
black，blue，red。。。。

+ 十六进制颜色
十六进制颜色由#开头，后面跟三个数字，每个数字的范围由00~FF，每个数字代表一种颜色，最终颜色由三种颜色混合成
color:#DAE8FC

+ rgb形式
r(red)g(green)b(blue),最终由三种颜色混合而成，每种颜色的范围由0~255

+ rgba形式
相对rgb多了一个a，表示透明度
值在0~1之间

#### 文字对齐方式
text-align： 

center文字居中对齐
left 文字左对齐
right 文字右对齐

#### 文字行高
![行高](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-1-HTML-CSS/1.5/%E8%A7%A3%E9%87%8A%E8%A1%8C%E9%AB%981.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

虚线高度就是font-size 

20px的行高减去12px再平分两份就是4px

行高的设置格式
line-height: 30px

所以行高的作用有两个：

第一个：改变行与行之间的距离
```
<p>
  We understand every aspect of project and we put a great amount of time in
  understanding the project.
</p>

<p style="line-height:32px;">
  We understand every aspect of project and we put a great amount of time in
  understanding the project.
</p>
```


第二个： 使文字上下居中
当行高与矩形高度一样时就上下居中

文字居中的按钮：

```
<button
  style="width: 120px;height:50px;text-align: center;line-height:50px;color:white;font-size: 18px;"
>
  提交
</button>
```

#### 字间距

中文英文的字间距是不一样的
中文的字间距是每个汉字的间距

英文的字间距是字母之间的间距，不是单词之间的间距

字间距设置：
```
letter-spacing: 30px
```

#### 字体
字体设置：
```
font-family: sans-serif;
```

字体取决于使用者的电脑是否安装，所以有时设置多个字体

```
font-family: 'Goudy Bookletter 1911', sans-serif, 'Gill Sans Extrabold';
```

字体写法注意事项：
  1. 多个字体之间用逗号隔开
  1. 字体名称中有空格时要用引号隔开，单双引号都行
  1. 中文字体名称要用空格： "宋体"


