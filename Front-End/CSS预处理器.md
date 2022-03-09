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
