函数是一段可以反复调用的代码块

JS中，函数是头等对象(first-class)也可以称其方法

#### 获取随机数 Math.random()

```
const num = Math.random();
```

这是JS的内置函数，Math是一个内置对象，用于执行数学任务

这行代码可以获得0到1(不包括1)之间的随机值

Math.floor(x)是返回小于x的最大整数

#### 自定义函数

自定义函数需要自行声明

+ 用function命令声明
  ![function](https://document.youkeda.com/P3-4-HTML-CSS/6/1.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)
  函数命名一般用小驼峰命名法

通过函数名调用函数

+ 用函数表达式声明
  ![函数表达式声明](https://document.youkeda.com/P3-4-HTML-CSS/6/2.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

这种写法将一个匿名函数赋值给变量，赋值等号右边为匿名函数，又称函数表达式

这种方式function后面不带有函数名

调用时也是通过函数名，这里函数名指变量名

函数表达式可以用箭头函数简写：

```
let print = () => {
  console.log("JavaScript 真有趣");
};
```

+ 函数声明的提升
  
  ```
  f();
  function f() {}
  ```

不会报错，因为在采用function函数声明时，整个函数会被提到代码头部，虽然看起来像是调用在前，声明在后，其实等价于：

```
// 被提升到头部
function f() {}
f();
```

但是采用函数表达式声明函数时就不存在函数声明提升

+ 两种声明的区别
  函数表达式结尾有分号

有无函数提升

+ 函数的重复声明

如果一个函数被多次重复声明，那么就会覆盖前面的声明

```
function print() {
  console.log("JavaScript 真有趣");
}

function print() {
  console.log("JavaScript 真有趣...个鬼嘞");
}

print();//JavaScript 真有趣...个鬼嘞
```

+ 立即执行函数

当函数只使用一次时，通常使用IIFE：

```
(function() {
  console.log("这个函数只执行一次");
})();
```

会在函数声明后立即执行

#### 函数参数

```
// 在圆括号运算符中加入参数 figure，用来接收外部传入的数据
function code(figure) {
  const num1 = Math.random() * 0.9 + 0.1;
  const num2 = Math.floor(num1 * Math.pow(10, figure));

  console.log(num2);
}

// 不要忘记调用函数，调用函数后函数才会执行
code(4);
code(6);
```

加入参数，Math.pow(x,y)是求x的y次幂的函数

+ 多个参数

函数可以接受多个参数，以逗号分开

传入值时需要按顺序

+ 参数数量和传入数量不匹配

```
// 参数 figure(位数) txt(文本) 
function code(figure, txt) {
  const num1 = Math.random() * 0.9 + 0.1;
  const num2 = Math.floor(num1 * Math.pow(10, figure));

  console.log(txt, num2);
}

code(6, "六位随机数：", "第三个参数");
```

不会报错，因为JS允许传入任意个参数而不影响调用，因此传入参数比定义的参数多也没有问题

```
// 参数 figure(位数) txt(文本) 
function code(figure, txt) {
  const num1 = Math.random() * 0.9 + 0.1;
  const num2 = Math.floor(num1 * Math.pow(10, figure));

  console.log(txt, num2);
}

code(6);
```

打印结果：

```
undefined 556540
```

因为对应的txt未传入参数，所以打印undefined

+ 参数默认值

防止出现undefined可以给参数设置默认值

```
// 参数 figure(位数) txt(文本) 
function code(figure, txt = "随机数：") {
  const num1 = Math.random() * 0.9 + 0.1;
  const num2 = Math.floor(num1 * Math.pow(10, figure));

  console.log(txt, num2);
}

code(6);
```

+ 函数的返回值

用return返回值，return后语句不会执行

#### 内置函数--计时器1

+ 延迟执行setTimeout()

setTimeout()用来指定某个函数或者某段代码，在多少毫秒之后执行，它返回一个整数，表示定时器的编号，以后可以用来取消这个定时器

基础语法：
![setTimeout](https://document.youkeda.com/P3-4-HTML-CSS/6/3.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

setTimeout()函数接受两个参数，第一个参数func|code是将要推迟执行的函数名或者一段代码，第二个参数delay是推迟执行的毫秒数

三种写法：

```
console.log(1);

/**
 * 第一个参数是代码，注意代码需用引号包裹，否则会立即执行代码
 * 第二个参数是 1000，即 1000ms 后执行 console.log(2)
 */
setTimeout('console.log(2)', 1000);

/**
 * 第一个参数是匿名函数
 * 第二个参数是 2000，即 2s 后执行 console.log(3)
 */
setTimeout(function () {
  console.log(3);
}, 2000);

// 第一个参数是函数名，注意函数名后不要加小括号“()”，否则会立即执行 print4
setTimeout(print4, 3000);

console.log(5);

function print4() {
  console.log(4);
}
```

每隔一秒打印出剩余秒数

```
// 首先定义计时总秒数，单位 s
let i = 60;

// 定义变量用来储存定时器的编号
let timerId;

// 写一个函数，这个函数即每次要执行的代码，能够完成上述的 1、2、3
function count() {
  console.log(i);
  i--;
  if (i > 0) {
    timerId = setTimeout(count, 1000);
  } else {
    // 清除计时器，这个例子不是必要
    clearTimeout(timerId);
  }
}

// 首次调用该函数，开始第一次计时
count();
```

clearTimeout()是清除计时器

+ 回调函数
  比如，取商店预定中秋月饼，预留手机号，到货后通知你

回调函数：预留的手机号；条件：到货；到货后打电话：调用回调函数

之前的例子将函数指针作为参数(回调函数)

#### 内置函数--计时器2

+ 无限调用setInterval()
  基础语法：
  ![setInterval](https://document.youkeda.com/P3-4-HTML-CSS/6/4.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

用法与setTimeout()用法一致，只是setInterval()是隔一段时间就调用一次，无限调用，比如：
隔一秒打印一次1：

```
let timer = setInterval(print, 1000);

function print() {
  console.log(1);
}
```

第一次打印1是在一秒后，而计时器则是直接打印，再间隔

```
let i = 60;
print();
let timer = setInterval(print, 1000);

function print() {
  console.log(i);
  i--;
  if (i < 1) {
    clearInterval(timer);//这是必要的
  }
}
```

clearInterval()清除计时器
