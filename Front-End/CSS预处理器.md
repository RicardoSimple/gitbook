### css预处理器
#### SCSS
+ SASS
sass是一款css的预编译器
定义了一种新的编程语言，为css增加了一些编程的特性，开发者使用这种语言进行编码后，代码需要编译成css才能被浏览器理解

sass比css更像一门编程语言，它可以有函数，变量，控制语句，导入，嵌套等高级功能

类似的css预编译器还有less，stylus

有了这些功能，sass可以：
 1. 提供变量，实现一键切换主题色之类的功能
 1. 用嵌套写法减少选择器的重复书写
 1. 用混用功能解决代码复用问题
 1. 用函数进行复杂的运算
 1. 把样式代码模块化，减少不同模块的代码间不必要的相互影响，提高代码的安全性

安装
[安装sass](https://sass.bootcss.com/documentation)

写好的scss/sass文件会在运行代码后直接编译为同名的css文件

+ SASS和SCSS
scss其实就是sass 3为了兼容css引入的新语法
css的语法是选择器+声明块
声明块是由花括号和声明组成的

最早的SASS的语法格式被称为缩进格式，使用"缩进"代替"花括号"
```
#sidebar {
  width: 30%;
  background-color: #faa;
}
```

为：
```
#sidebar
  width: 30%
  background-color: #faa
```

SASS 3引入了新语法，也就是SCSS，其语法完全兼容CSS 3并且继承了SASS的强大功能，也就是说，任何标准的CSS3样式表都是具有相同语义的
有效SCSS文件

SASS和SCSS的大部分扩展，例如：变量，parent reference 和 指令 都是一样的；唯一不同的就是 SCSS 需要使用分号和花括号而不是换行和缩进

#### 变量
在css的基础上，Sass提供了一些名为SassScript的新功能。
SassScript 可作用于任何属性，允许属性使用变量，算数运算等额外功能。

变量的写法为以美元符号$开头，赋值方法和css属性一样
```
$width: 10px;
```
使用变量：
```
#main {
  width: $width;
}
```

编译为css：
```
#main {
  width: 10px;
}
```

如果变量类型为字符串，一般不需要加引号但是有些特殊情况，比如字符串中有双斜杠"//",就需要用英文状态下的双引号或者单引号包裹字符串

```
// 变量需要定义在使用之前
$position: "absolute";
$mainTxtColor: '#333';

// 应用
#main {
  position: $position;
  color: $mainTxtColor;
}
```

因为//在SASS中表示单行注释

+ 除了简单赋值，Sass还可以定义类似数组的变量
`$animals: puppy kitty chick;`这样的变量可以配合循环使用

+ 简单计算
Sass中支持在使用变量时进行简单的加减乘除
```
$width: 10px;

#main {
  width: $width / 2;
  height: $width * 2;
}
```
编译为css：
```
#main {
  width: 5px;
  height: 20px;
}
```
+ 插值法
`#{}`插值几乎可以在Sass样式表中的任何地方使用

```
$name: "mail";
$top-or-bottom: "top";
$left-or-right: "left";

.icon-#{$name} {
  background-image: url("/icons/#{$name}.svg");
  position: absolute;
  #{$top-or-bottom}: 0;
  #{$left-or-right}: 0;
}
```

编译结果：
```
.icon-mail {
  background-image: url("/icons/mail.svg");
  position: absolute;
  top: 0;
  left: 0;
}
```
就相当于替换

#### 嵌套
如下代码：
```
main .double .item .links {
  text-align: center;
}

main .double .item .links a {
  margin-right: 20px;
}
```
如果遇到比较复杂的界面，为了不影响其他元素的样式，会把选择器写的尽可能精确，会很长

sass的嵌套可以解决这个问题

比如上面的两个选择器选择的是父子元素，在Sass中就可以写成：
```
main .double .item .links {
  text-align: center;
  a {
    margin-right: 20px;
  }
}
```
把共同选择器提取了出来

+ 嵌套规则
Sass中允许将一套CSS样式嵌入进另一套CSS样式中
内层的样式将外层的选择器作为父选择器

![嵌套](https://document.youkeda.com/P3-2-HTML-CSS/1.6/3.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

+ 父选择器 &
在嵌套CSS样式时，有时需要直接使用父选择器，比如当给某个元素设置hover时：
![hover](https://document.youkeda.com/P3-2-HTML-CSS/1.6/4.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

或者特殊一点的用法：
![](https://document.youkeda.com/P3-2-HTML-CSS/1.6/5.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

#### 复用：mixin/include
很多代码都有解决复用的方案

可以用混合(mixin/include)来定义可重复使用的样式
+ 无参数混合
css中使用重复类名来解决代码的复用
css：
```
.square {
  width: 100px;
  height: 100px;
}
```
html:
```
<div class="user-avatar square">...</div>
<div class="admin-avatar square">...</div>
```

用mixin/include来解决代码复用：
```
@mixin square {
  width: 100px;
  height: 100px;
}

// 应用：
.user-avatar {
  @include square;
}
.admin-avatar {
  @include square;
}
```

上述代码定义了一个可以重复使用的块square
用@mixin 定义，@include 引用

编译结果：
```
.user-avatar {
  width: 100px;
  height: 100px;
}

.admin-avatar {
  width: 100px;
  height: 100px;
}
```

+ 有参数混合
 + 参数无默认值
 稍作修改：
 ```
 @mixin square($size) {
  width: $size;
  height: $size;
}

// 应用
.avatar {
  @include square(100px);
}
```
使用时向$size传入值
+ 参数有默认值
直接定义默认值：
```
@mixin square($size: 100px) {
  width: $size;
  height: $size;
}

// 不传参数就会使用默认的值 100px
.avatar {
  @include square;
}

// 传入参数就会使用传入的值 200px
.avatar-200 {
  @include square($size: 200px);
}
```

#### Sass文件的引入
![Sass](https://document.youkeda.com/P3-2-HTML-CSS/1.8/8.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

用@import
