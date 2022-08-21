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
2. 字体名称中有空格时要用引号隔开，单双引号都行
3. 中文字体名称要用空格： "宋体"

### css的三种引入方式

#### 行内式

行内式需要内嵌在每一个html标签中
![行内式](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-1-HTML-CSS/1.6/line-style-css.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

这样比较麻烦，所以我们需要抽取出这些样式

#### 内部样式

![抽离过程](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-1-HTML-CSS/1.6/head-style-css.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

```
抽离步骤：
1. 首先我们将每一个css的标签抽离出来
2. 然后在head标签中声明一个<style></style>标签
3. 接下来将样式都放在style里，这不是简单的复制粘贴
```

![抽离](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-1-HTML-CSS/1.6/%E5%A4%B4%E9%83%A8%E6%A0%B7%E5%BC%8F%E6%8A%BD%E7%A6%BB.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

```
4. 将相同的标签的样式写在大括号里，大括号前面加上标签名
p {
  font-size: 16px;
  color: #ffffff;
}
```

书写规则：
![shuxie](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-1-HTML-CSS/1.6/%E6%A0%B7%E5%BC%8F%E9%87%8D%E7%82%B9%E8%A7%A3%E9%87%8A.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

1. 不要忘记写style标签
2. 样式要用大括号括起来
3. 每个样式后面要用分号

#### 外部样式

随着代码量的增加，css的代码会比html多，会出现头重脚轻的现象，所以要进一步抽离

![外部样式](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-1-HTML-CSS/1.6/%E5%A4%96%E9%83%A8%E6%A0%B7%E5%BC%8F%E5%BC%95%E5%85%A5%E6%AD%A5%E9%AA%A4.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

抽离步骤：

```
1. 新建一个index.css文件
2. 将html中head标签中的style标签内的全部样式复制到index.css中
3. 建立html和css的联系，用link标签引入css，注意link标签一定在head中
```

#### css注释

html的注释方式是```<!--注释-->```,而css的注释不同，是```/*注释*/```

+ 内部样式注释
  
  ```
  <style>
  /* 写CSS的基础样式 */
  .base{
  /* 基础字体大小 */
  font-size: 14px;
  /* 基础字体颜色 */
  color:#000000;
  }
  </style>
  ```

+ 外部样式注释，直接在css文件中
  
  ```
  /* 写CSS的基础样式 */
  .base {
  /* 基础字体大小 */
  font-size: 14px;
  /* 基础字体颜色 */
  color: #000000;
  }
  ```

#### link标签的属性

```
<link rel="stylesheet" type="text/css" href="index.css" />
```

+ rel属性
  规定了当前文档与被联系文档之间的关系
  但是stylesheet的值被所有浏览器支持，所以记住一个值就行

+ type属性
  规定了被联系文档的MIME（多用途互联网邮件扩展类型）最常见的值就是text/css

+ href属性
  表示被联系文档的地址

### 相对路径/绝对路径

绝对路径就是文件在硬盘上的真实路径

```
<img src="E:\book\网页布局\bg.jpg" /> 
```

但是为了避免在其他的设备上的文件丢失，一般使用相对路径
就是相对于这个文件去找其他文件

./就是当前文件夹目录
../就是回到上级文件夹目录
可以多次返回上级文件夹

### 常用选择器

#### 标签选择器

之前css中的就是标签选择器

选择器的叠层性

如果在{}中添加新的样式，可能会有下面的结果

```
1. 添加新的效果(如果添加的是一个新的属性)
2. 改变之前已经存在的效果(如果该属性之前就存在)
```

#### 类选择器

类选择器定义

```
<p class="article">
  class是定义类的关键字，article是类名，类名可以任意，但是要符合规范
</p>
```

选择器的使用

```
.article {
  color: red;
  font-size: 14px;
}
```

写在style里或css文件里

定义时一个类名还可以写多个属性

```
<p class="common color font-size">
  common设置通用样式，color设置特殊颜色，font-size设置特殊字体大小
</p>
```

#### id选择器

在标签中定义id

```
<p id="p-item">这是一段文字</p>
```

使用id选择器

```
#p-item {
  font-size: 24px;
  font-weight: 400;
}
```

注意事项：

```
1. id选择器在文档中只会出现一次(id不能重复)
2. 不能定义多个id
·
```

### 高级选择器

#### 后代选择器(空格)

```p a```选择所有p标签中的a标签，书写规则：用空格隔开

```
/* 选择id名为password的标签内部所有类名为box的元素内部的所有p标签 */
#password .box p{}
/* 选择所有p标签内部的所有span标签 */
p span{}
/* 选择所有p标签内部的所有类名为spanItem的标签 */
p .spanItem{}
```

随着复杂程度增加，选择器的层数可以增加

```
/* 方法一： */
a{}
/* 方法二 */
ul a{}
/* 方法三 */
ul li p a{}
/* 方法四 */
ul li p span a{}
```

多层的话可以添加类名减少复杂度

#### 交集选择器

书写规则：

```
a.special{}
```

```
<a href="#" class="special">超链接</a>
<a href="#">超链接</a>
<a href="#">超链接</a>
<a href="#">超链接</a>
```

意思是，在所有的a标签内类名为special的标签
比如：

```
<ul>
    <li><a href="" class="special">电子产品</a></li>
    <li><a href="">家居服饰</a></li>
    <li><a href="">电竞手办</a></li>
    <li><a href="" class="special">家装服务</a></li>
    <li><a href="">房屋出租</a></li>
</ul>
```

```
ul li {
    list-style: none;
    font-size: 22px;
}

ul li a {
    /* 去除a标签的下划线 */
    text-decoration: none;
    /* 这里的颜色一定要在a标签上设置，因为a标签默认会去设置字体颜色，会层叠掉默认的黑色 */
    color: black;
}

ul li a.special {
    color: orangered;
}
```

#### 子选择器

书写规则：

```
p>span {
    color: orangered;
}
```

与后代选择器类似但是后代选择器突出的是后代，子选择器突出的是子

#### 并集选择器

书写规则：

```
.box,p,h3,.phone{}
```

用逗号隔开
如果要给不同的标签添加相同的样式，就要用到并集选择器，表示的是"和"

#### 选择器的优先级

+ 单个选择器的优先级
  id选择器>类选择器>标签选择器
  (非优先级相同的情况与选择器的顺序无关)

+ 文字属性的继承性
  在之前的内容中，发现除了h标签，其他的标签都有默认的字体大小和颜色等，这是因为继承了body标签的字体大小和颜色，这就是文字属性的继承性

+ 高级选择器的优先级
  多个选择器叠加的优先级要用到选择器的权重的叠加性，假设id选择器的权重为100，类选择器的权重为10，标签选择器的权重为1
  
  ```
  .param #item span {
  }
  ```
  
  就可以算出这个高级选择器的权重为111

权重就可以判断高级选择器的优先级(要看标签)

```
<ul class="ul-item">
  <li>
    <p>文字的颜色到底是什么颜色？</p>
  </li>
</ul>
```

```
.ul-item li {
  color: blue;
}
p {
  color: red;
}
```

最终显示的是红色，与权重比较结果不同
