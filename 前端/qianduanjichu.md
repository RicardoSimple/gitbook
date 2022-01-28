## 前端
前端开发是创建web页面或app等前端页面呈现给用户看的过程，通过HTML,CSS,及JavaScript以及衍生出来的各种技术
框架解决方案等来实现互联网产品和用户界面的交互。
### 前端三板斧
+ HTML 超文本标记语言
html描述了一个网站的结构，是一种标记语言而非编程语言
浏览器使用html标签和脚本来诠释网页内容
+ CSS 层叠样式表
主要职能就是确定布局和元素的表现
css不能单独使用
+ JavaScript (JS,高级的，解释型的编程语言)
支持面向对象编程，命令式编程，函数式编程
与java不同
### Html元素的结构
#### html标签
+ 由尖括号包围关键词组成，比如：
<p>,<h1>,<div>
+ 通常成对出现，一个开始一个结尾，结尾比开始多/，比如：
<div>,</div>
+ 并不是所有的标签都有结尾标签，比如：
<input>,<img>等，都是单独出现的
+ html标签中大小写不敏感，但是一般要求小写

![html元素](https://document.youkeda.com/P3-1-HTML-CSS/1.2/2.jpg)

剖析一个html元素，可以看到一般由
开始标签，内容，结尾标签组成

如果是单独出现的标签，一个标签就是一个元素

#### html中的嵌套
html中的元素可以嵌套
```
<div><p>HTML是一门伟大的语言！</p></div>
```

![html嵌套](https://document.youkeda.com/P3-1-HTML-CSS/1.2/16.png)

#### 完整的html文档结构
![html示例](https://document.youkeda.com/P3-1-HTML-CSS/1.2/4.jpg)
1. <!DOCTYPE html>
+ 告知浏览器该页面文件的文档类型，指示浏览器该用html的哪个版本编写
+ 位于文档第一行
+ 对大小写不敏感，没有结束标签
1. <html lang="en">...</html>
+ 告知浏览器该文档是html文档
+ 限定文档的开始和结尾，在他们之间是文档的头部和主体，头部由<head>标签定义，主体是<body>标签定义
+ lang属性(语言属性)，当搜索引擎或浏览器拿到语言属性时可能做一些针对语言属性的辅助操作，"en"表示英语
1. 标签属性
+ 一个标签可以拥有零个或多个标签属性
+ 标签属性采用
key="value"
+ 常见标签属性： class,id,style,lang,src等
1. 文档的头部
+ head元素定义文档的头部，我们通常在这里引用样式表，提供元信息等
+ 文档头部中的
<title>...</title>定义文档的标题，网页上体现为网页标签的标题
+ 一个head 元素必须且只能包含一个title元素
1. 文档主体
+ 包括文档的所有内容（图片，文字，超链接等）
#### html文档中的注释
```
<!-- -->
```
表示注释，不会显示
#### MDN
[MDN网站](https://developer.mozilla.org/zh-CN/docs/Web/HTML)


```
图片<img>
音频<audio>
进度条<progress>
```

#### 块状标签和内联标签
内联标签常常嵌套在块状标签中

+ 内联标签不会为自己的内容占据新的一行
<span><img><strong>
+ 块状标签会为自己的内容占据新的一行
段落：<p>  标题：<h1>，<div>

#### 标签标题
<h1>到<h6>数字越大级别越小

尽量不要跨级别写标签

#### 常用文本标签
##### 段落标签<p>
##### 内联标签<strong>
加粗，
行内标签，包含的内容具有重要性
<b>标签也可以加粗语句，但是没有语义，只是单纯加粗样式
##### 内联标签<span>
本身如果没有加样式处理，就不会有变化，而且span可以使网页更加有条理

##### 图片标签<img url="" alt="">

url是图片地址，绝对地址或相对地址，也可以是gif文件，alt是替换文字

##### 链接标签<a>
![示例](https://document.youkeda.com/P3-1-HTML-CSS/1.3/12.png)
<a href="">wenzi</a>
里面不仅能放文字，还能是图片等
属性：
+ href是链接指向的地址
+ title属性给出链接的说明信息
鼠标悬停在链接上方时，浏览器将这个属性的值以提示块的形式显示出来
+ target属性指定如何展示打开的链接
target属性的值可以是"_self","_blank","_parent","_top"之一
self是当前页面打开，blank是新页面打开

##### 列表标签
+ 无序列表标签<ul>
+ 有序列表标签<ol>
+ 列表每一项内容用标签<li>

#### html表单标签
##### <form>标签
form块状标签
+ 属性
action 一个处理此表单信息的程序所在的url，所述表单信息将在表单提交时被发送到定义的地址

method 它的值可以是GET或POST，用来规定如何发送表单信息


```
<!-- <form>是块状标签，要注意：<form>标签不能嵌套<form>标签 -->
<form action="">
  <!-- 这里会有一些表单控件 -->
</form>
```

##### 单行文本输入框<input>

```
<!-- action=""则表单信息将提交到当前页面 -->
<form action="">
  <input type="text" />
</form>
```
没有结束标签

属性：
type
占位文本placeholder
输入框名字name
输入框的值value
readonly只读
disabled无法更改

disabled和readonly的区别
![image.png](./image.png)

##### 多行文本输入框和密码输入框
+ 用<textarea>标签来写多行文本输入框

```
<!-- name属性表示表单元素的名称，placeholder属性表示表单元素的占位文本 -->
<textarea
  name="sign"
  rows="5"
  cols="30"
  placeholder="请输入个性签名"
></textarea>
```

rows和cols表示文本框的行数和文本域的可视宽度

右下角的斜线拖动可改变文本框的大小

+ 密码输入框

密码输入框输入的内容会变成小黑点

只需将type = "text" 改为type = "password"

##### 单选框和复选框
+ 单选框
```
<!-- type属性表示表单元素的类型，name属性表示表单元素的名称，value属性表示表单元素的值 -->
<input type="radio" name="gender" value="male" />
<input type="radio" name="gender" value="female" />

```

单选框就是把控件类型type = "text"改为了type = "radio"
大部分表单类型就是通过改标签属性type实现的

同一道单选题的单选选项应该具有相同的name属性值

在input标签后直接加内容可以显示

但是普通的只能点击圆点进行选择，要将选择范围变大可以用<label>标签

```
<label> <input type="radio" name="gender" value="male" />男 </label>
<label> <input type="radio" name="gender" value="female" />女 </label>

```
另一种写法,增加了id属性，label的for属性和input的id属性应该保持一致
```
<input id="male" type="radio" name="gender" value="male" />
<label for="male">男</label>
<input id="female" type="radio" name="gender" value="female" />
<label for="female">女</label>s
```
+ 复选框
复选框的标签属性是checkbox

```
<input type="checkbox" />
``` 

再配合name，value，label等

```
<label> <input type="checkbox" name="interest" value="coding" />编程 </label>
<label> <input type="checkbox" name="interest" value="other" />其他 </label>
```








