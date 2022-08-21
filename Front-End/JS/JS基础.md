#### js的书写位置

与css类似，分为html内部和外部

+ JavaScript写在html内部
  1. 使用<script></script>标签可以将JavaScript代码嵌入HTML内部

具体嵌入方式：

```
// script标签嵌入JavaScript代码
<script>
    // JavaScript代码
    let name = "Bob";
    function(){
        console.log("我的名字叫："+name);
    }
</script>
```

  有这种：

```
<script type="text/javascript" charset="utf-8"></script>
```

  这两种是一样的，`type = "text/javascript"`表示文档类型是javascript，`charset = "utf-8"`表示编码是utf-8

1. script标签在html中的位置
   body标签内部的末尾(规范)
   
   其实位置在哪里都无所谓，但是学习DOM时，位置不注意会报错
+ javascript写在html外部

代码写在index.js中，然后用script标签引入

```
<script src='index.js'></script>
```

不是url或href，而是src

#### js注释

单行注释  `//单行注释`
块级注释

```
/*
 * 注释
 * 注释
 */
```

#### 字符串

用引号引起来的就是字符串，单双引号都行

```
// 双引号
"字符串";

// 单引号
'字符串';
// 双引号
"Tom";

// 单引号
'Tom';

// 双引号
"T";

// 单引号
'T';
// 双引号，字符串
"12";

// 单引号，字符串
'1';
```

#### console访问控制台

JavaScript的结果不是在页面中显示，而是在控制台中显示

console表示访问控制台，`log()`表示在控制台中输出信息，中间用点号连接

完整写法：

```
console.log("要输出的内容");
```

#### 模板字符串

在一般的字符串中，我们需要将字符串和变量拼接起来，要用+

```
let firstName = "胡";
let lastName = "雪岩";

let say = "大家好，我姓" + firstName + "，名" + lastName;

console.log(say);
```

模板字符串的核心是反引号`(``)`和占位符`(${})`，反引号的作用是将变量和字符串包裹起来，占位符的作用就是在字符串中插入变量

占位符语法：`${变量名}`

改后的代码：

```
let firstName = "胡";
let lastName = "雪岩";

let say = `大家好，我姓${firstName}，名${lastName}`;

console.log(say);
```

#### 转义符`\`

双引号里面写双引号就要用转义字符

```
let str = "华为正式发布操作系统---\"鸿蒙OS\"";
console.log(str);
```

#### 多行字符串拼接

一般的字符串需要`\n`表示换行
使用模板字符串就直接回车

```
let str = "春眠不觉晓\n" + "处处闻啼鸟\n" + "夜来风雨声\n" + "花落知多少\n";
console.log(str);
```

模板字符串

```
let str = `春眠不觉晓
处处闻啼鸟
夜来风雨声
花落知多少`;

console.log(str);
```

#### 在字符串中使用表达式

拼接：

```
let number1 = 20;
let number2 = 10;
console.log(
  "两个数的和是：" +
    (number1 + number2) +
    "\n两个数的差是：" +
    (number1 - number2) +
    "。"
);
```

使用占位符：

```
let number1 = 20;
let number2 = 10;
console.log(`两个数的和是：${number1 + number2} 
两个数的差是：${number1 - number2} 。`);
```

#### 模板字符串中使用三元表达式

```
let str = `这里是${false ? "浙江" : "江苏"}`;

console.log(str); // 江苏
```

加大难度：

```
let str = `这里是${true ? "江苏" : "浙江"}-${true ? "南京" : "常州"}`;

console.log(str); // 这里是江苏-南京
```

#### 使用场景

```
// 定义屏幕的宽度，当然这个宽度是根据window的api去获取的
let screen = 760;

// 判断屏幕是大屏还是小屏，这里我们认为大于760px的就是大屏
function isLargeScreen() {
  return screen > 800;
}

// 定义元素的排列方式，大屏row排列，小屏column排列
// 具体什么排列方式，是根据屏幕大小决定的
let item = {
  isCollapsed: screen > 800
};

// 这里我们就要根据上面的信息来动态的获取类名（多个）
const classes = `header ${
  isLargeScreen() ? "" : `icon-${item.isCollapsed ? "column" : "row"}`
}`;

console.log(classes);
```

组装代码：

```
let htmlCode = `
    <img src='' />
    ${
      true
        ? `<img src='https://ss0.bdstatic.com/70cFuHSh_Q1YnxGkpoWK1HF6hhy/it/u=1906469856,4113625838&fm=26&gp=0.jpg' />`
        : `<img src='' />`
    }
`;
console.log(htmlCode);
// <img src='' />
//    <img src='https://ss0.bdstatic.com/70cFuHSh_Q1YnxGkpoWK1HF6hhy/it/u=1906469856,4113625838&fm=26&gp=0.jpg' />
```

html代码作为输出时，需要用反括号括起来

### 基础数据类型

#### 变量

JS中定义变量的关键字有两个：let和const
![定义规则](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-4-HTML-CSS/1/2j/%E5%8F%98%E9%87%8F%E8%A7%A3%E9%87%8A.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

定义变量并输出：

```
let name = "Will Smith";
console.log(name);
```

其他资料中出现的var关键字定义变量，这是一种过时的关键字，会导致很多错误，现在不常用

let定义的变量可以多次赋值，const定义的常量不能多次赋值，声明时就要初始化

#### 数值类型

常用的数值类型包括 整数，浮点数和NaN(Not a number 非数值)

+ 整数
  不同进制的数
+ 浮点数
  包含小数点，小数点前面可以不写数字，也可以用科学计数法

浮点数的精度丢失现象
浮点数最高17位小数，但是算术运算中不如整数，比如：

```
let number1 = 0.1;
let number2 = 0.2;
console.log(number1 + number2); // 0.30000000000000004
```

所以不能这样判断：

```
if (a + b == 0.3) {
  console.log('输出成功');
}
```

+ NaN(非数值)
  简单说就是两个变量执行一个运算，例如加减乘除中的一种，返回的结果仍然是数字类型，但是执行的数字运算没有成功
  
  ```
  let a = 'number';
  let b = 10;
  let c = a / b;
  console.log(c); // NaN
  console.log(typeof c); // number
  ```
  
  typeof的作用是用来判断变量类型的，最后的返回结果是数字类型

其他出现NaN的情况：

```
1. 0/0
2. 字符串乘以数字
3. NaN和任何数字运算
```

例如：

```
let a = 'number';
let b = 10;
let c = a / b;
// 此时`c`和任何数进行运算结果都是`NaN`
```

#### 类型转换/字符串拼接

+ 类型转换
  分为隐式转换和强制类型转换

数字字符串和数字做加法，数字被隐式转换为数字字符串

```
console.log(20 + "20"); // 2020
// 调换位置亦可
console.log("20" + 20); // 2020
```

数字字符串与数字做非加法运算，数字字符串被隐式转换为数字

```
console.log("20" - 10); // 10
console.log(10 * "10"); // 100
console.log(10 / "2"); // 5
```

数字字符串与数字字符串做非加法运算，隐式转换为数字：

```
console.log("20" - "10"); // 10
console.log("20" / "10"); // 2
console.log("20" * "10"); // 200
```

+ 强制类型转换
  parseInt(将小数字符串，整数字符串和小数转换为整数)

整数字符串：

```
let number = "20";
//  将number转换为整数类型
let converNumber = parseInt(number);
console.log(converNumber); // 20
// 判断转换后的数据类型
console.log(typeof converNumber); // number
```

小数字符串：

```
let number = "20.5";
let converNumber = parseInt(number);
console.log(converNumber); // 20  不足21一律按照20算
console.log(typeof converNumber); // number
```

小数：

```
let number = 20.5;
let converNumber = parseInt(number);
console.log(converNumber); // 20
```

parseFloat(将小数字符串转换为小数)

```
let number = "20.9";
let converNumber = parseFloat(number);
console.log(converNumber); // 20.9
console.log(typeof converNumber); // number
```

#### 字符串拼接

用加号

变量不加引号

#### 运算符

+ 相等/全等
  相等(==),全等(===)

区别是，前者是判断值是否相同，后者还要判断类型是否相同

```
let number1 = "45";
let number2 = 45;
console.log(number1 == number2); // true
console.log(number1 === number2); // false
```

相等在比较之前会做隐式类型转换，为了保证代码数据类型的完整性，推荐使用全等

+ 自增/自减
  就是++/--

```
let a = 10;
const c = a++;//c的值为10
```

#### 布尔类型

+ 布尔类型
  布尔类型就是真(true)和假(false)

区分大小写

+ 布尔运算
  除全等和相等外的布尔运算：
  大于(>),小于(<),大于等于(>=),小于等于(<=),不等(!==),非(!)

+ 两种布尔运算的简便写法
  非零数字和非空字符串转换为true，""(空字符串)和0转换为false

+ if条件语句
  
  ```
  if (条件) {
  // 待执行代码
  }
  ```
  
  与c一样

+ 逻辑或(||)/逻辑与(&&)
  与c一样

#### 条件判断

+ if-else
  与c一样

+ switch
  
  ```
  let weather = 'rain';
  ```

switch (weather) {
  case 'snow':
    console.log('堆雪人');
    break;
  case 'windy':
    console.log('呆在家里');
    break;
  case 'rain':
    console.log('雨中漫步');
    break;
  default:
    console.log('工作');
    break;
}

```
default是所有case都不匹配再执行
与c类似

#### 数组
+ 数组定义
```

let arr = [1,'第一名',true,[2,'第二名',false]];

```
创建另一种方法：
```

// 创建一个空数组并赋值
let arr = new Array();

// 创建一个有内容的数组
let arr2 = new Array(1,2,'arr');

```
数组的索引与c一样，从0开始

可以添加(较大的索引)
```

let arr = ['张三','李四','王五','Lisa'];

// 修改值
arr[0] = 'Three';
arr[1] = 'Four';
arr[2] = 'Five';
arr[3] = '丽莎';
console.log(arr);

// 添加值
arr[4] = 'Polly';

console.log(arr);

```
#### 数组元素操作(增、删、查、改)
+ 增
push方法(从尾部加)
```

变量名.push('要添加的值');

```
也可以添加多个值
```

schools.push('河海大学');
schools.push('大连理工大学');
schools.push('哈尔滨工业大学');

// 上述三步操作可以一次性完成
schools.push('河海大学', '大连理工大学', '哈尔滨工业大学');

```
unshift方法(从头加)，与push方法类似
+ 删
pop方法(从后往前删)
与push对应
```

let schools = ['清华大学', '北京大学', '浙江大学', '同济大学'];

// 在末尾添加“河海大学”
schools.push('河海大学');
console.log(schools); // 清华大学','北京大学','浙江大学','同济大学','河海大学'

// 从末尾删除一个元素
schools.pop();
console.log(schools); // 清华大学','北京大学','浙江大学','同济大学'

```
shift方法(从前往后删)

splice方法(删除指定位置的值)
splice方法括号内可以写一个参数也可以写两个参数

一个参数：表示删除从指定位置开始(包括开始位置)到结束位置的所有元素，并返回被删除的元素
```

let schools = ['清华大学', '北京大学', '浙江大学', '同济大学'];

// 删除从下标为1的位置到结束位置的值
let deleteSchools = schools.splice(1);
// 删除之后，原数组中的剩余内容
console.log(schools); // ["清华大学"]
// 删除的内容
console.log(deleteSchools); // ["北京大学", "浙江大学", "同济大学"]

```
两个参数：第二个参数表示删除个数
```

let schools = ['清华大学', '北京大学', '浙江大学', '同济大学'];

// 从下标为0开始,往后数两个元素,删除
let deleteSchools = schools.splice(0, 2);
console.log(schools); // ['浙江大学', '同济大学']
console.log(deleteSchools); // ['清华大学', '北京大学']

```
+ 改
splice方法(三个参数)

第一个参数，整数类型，表示起始位置

第二个参数，整数类型，表示步长(往后选几个元素，1表示一个元素)

第三个参数，要替换的数组的值
```

let schools = ['清华大学', '北京大学', '浙江大学', '同济大学'];

schools.splice(2, 0, '江西理工大学');
console.log(schools); //  ["清华大学", "北京大学", "江西理工大学", "浙江大学", "同济大学"]

```
再有：
```

let schools = ['清华大学', '北京大学', '浙江大学', '同济大学'];

schools.splice(2, 2, '江西理工大学');
console.log(schools); // ["清华大学", "北京大学", "江西理工大学"]

```
+ 查
indexOf方法，可以写两个参数

一个参数时，就直接查
```

let schools = ['清华大学', '北京大学', '浙江大学', '同济大学'];

let result = schools.indexOf('大连理工');
console.log(result); // -1

```
找到就返回元素下标，找不到就返回-1

两个参数时，第一个参数是我们要找的值，第二个是开始寻找的位置
```

let schools = ['清华大学', '北京大学', '浙江大学', '同济大学'];

let result = schools.indexOf('浙江大学', 3);
console.log(result); // -1

```
![下标](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-4-HTML-CSS/4/splice.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

#### 二维数组
两个方括号[[]]

#### 循环
+ for循环
![for](https://document.youkeda.com/P3-4-HTML-CSS/5/1.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

for..in..
```

let peppaFamily = ["佩奇", "乔治", "猪妈妈", "猪爸爸"];

for (let i in peppaFamily) {
  console.log(peppaFamily[i]);
}
// 输出:
// 佩奇
// 乔治
// 猪妈妈
// 猪爸爸

```
for..in..循环会访问数组中的每一项，这里的i代表每一项的下标，for..in..不但可以循环数组，还可以循环对象

for..of..循环
```

let peppaFamily = ["佩奇", "乔治", "猪妈妈", "猪爸爸"];

for (let item of peppaFamily) {
  console.log(item);
}
// 输出:
// 佩奇
// 乔治
// 猪妈妈
// 猪爸爸

```
item表示每一项的值

+ while循环
![while](https://document.youkeda.com/P3-4-HTML-CSS/5/2.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

+ do..while循环
![dowhile](https://document.youkeda.com/P3-4-HTML-CSS/5/5.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

+ 跳出循环break/continue
```
