对象是JS的核心概念，也是最重要的数据结构

#### 什么是对象

简单说，对象就是一组键值对(key-value)的集合，是一种无序的复合数据集合
![对象](https://document.youkeda.com/P3-4-HTML-CSS/7/1.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

大括号：定义一个对象

person：定义的对象被赋值给person，则person指向这个对象
name:'henry'/age=18:键值对，键值用:分开

一个对象可以包含多个键值对，键值对用,分开

person保存的是对象的地址，不是对象本身，这种方式叫引用

这种定义的方式称为"字面量"

#### 键名

对象的键名大多都是字符串，加不加引号都行

```
let person = {
  name: 'henry',
  age: 18
}

// 和上面的写法意思一样
let person = {
  'name': 'henry',
  'age': 18
}
```

#### 方法

对象的键名又称为属性，它的键值可以是任何数据类型，当它是函数时，我们把这个属性称为"方法",可以像函数那样调用

```
let person = {
  name: 'henry',
  age: 18,
  run: function() {
    console.log('running');
  }
}

person.run();
```

#### 对象的创建

除了字面量方法，还可以通过构造函数创建新对象，使用构造函数创建对象的步骤：

```
1. 创建一个构造函数，构造函数的名称常根据大驼峰命名法命名；
2. 通过new创建对象实例
```

其中，构造函数可以声明对象的名称，属性和方法

用构造函数：

```
// 第一步：创建构造函数
function People(name, age) {
  this.name = name;
  this.age = age;
}

// 第二步：通过 new 创建对象实例
let person = new People('henry', 18);
console.log(person);
```

#### 自定义对象的属性操作

+ 属性的读取
  JS中有两个方法读取一个对象的属性:点运算符和方括号运算符：

```
let person = {
  name: 'henry',
  age: 18
}

console.log(person.name);
console.log(person['name']);
```

输出：

```
henry
henry
```

两种方式的效果一样，但是方括号中可以是一个变量

```
let person = {
  name: 'henry',
  age: 18
}

let variable = 'name';
console.log(person[variable]);

variable = 'age';
console.log(person[variable]);
```

如果键值是对象：

```
let person = {
  name: 'henry',
  age: 18,
  parents: {
    papa: 'jack',
    mama: 'mary'
  }
}

console.log(person.parents.papa);
console.log(person['parents']['mama']);
```

还是使用点运算符和方括号运算符，输出：

```
jack
mary
```

称为链式引用

+ 属性的赋值

赋值和读取一样，通过点运算符和方括号运算符

```
let person = {
  name: 'henry',
  age: 18
}

person.name = 'tom';
person['age'] = 10

console.log(person.name);
console.log(person.age);
```

+ 属性的查看
  查看一个对象的所有属性，可以用Object.keys()方法:

```
let person = {
  name: 'henry',
  age: 18
}

console.log(Object.keys(person));
```

上述的Object是JS提供的基本对象，JS中所有其他对象都继承自Object对象，即都是Object的实例，keys是Object的静态方法

+ 属性的删除和增加

如果要删除对象中的某个属性，可以使用delete命令：

```
let person = {
  name: 'henry',
  age: 18
}

delete person.name;

console.log(person);
```

输出：

```
{ age: 18 }
```

增加一个属性：

```
let person = {
  name: 'henry',
  age: 18
}

person.gender = 'male';
```

输出：

```
{ name: 'henry', age: 18, gender: 'male' }
```

+ 遍历对象属性

如果想要在一个对象里找到某个符合条件的属性，我们就需要遍历属性了

JS中通过for...in或者Object.keys()来实现

1. 使用for...in...来

```
let person = {
name: 'henry',
age: 18,
}

for (let key in person) {
console.log('键名：' + key + '；键值：' + person[key]);
}
```

1. 借助Object.keys()遍历属性
   由于Object.keys()返回的是对象所有属性组成的数组，我们可以借助这一点来遍历属性

```
  let person = {
  name: 'henry',
  age: 18,
}

let keys = Object.keys(person);

for (let i = 0; i < keys.length; i++) {
  console.log('键名：' + keys[i] + '；键值：' + person[keys[i]]);
}
```

#### 对象的继承

之前的自定义对象就是继承于Object对象

Object对象是JS提供的及基本对象，JS中的所有其他对象都是继承于Object对象，keys是Object的一个静态方法

因此，除了字面量构造对象外，还可以用JS提供的构造函数Object()或"继承"来创建对象

```
// 字面量
let o1 = {
  name: 'alice',
};

// 构造函数
let o2 = new Object();
let o3 = new Object();

// 继承
funtion O4() {
}; 
O4.prototype = o1; 
let o4 = new O4();
```

最后一种方法创建的o4对象继承自o1对象，那么o1就是o4的原型

+ 原型
  原型其实也是JS的一个对象

一个对象称呼他上一级对象为原型

+ 属性是否存在：in

我们可以用in运算符来判断一个对象中是否拥有某个属性

```
let person = {
  name: 'henry',
  age: 18,
};

'name' in person;
'gender' in person;
'toString' in person;
```

输出：

```
true;
false;
true;
```

'toString'是Object对象的一个属性，person继承自Object，所以也有这个属性

由于继承的存在，一个对象的属性被分为了两类：自身属性和继承属性

Object.keys()返回的属性就包含这两种属性

+ 判断自身属性是否存在：hasOwnProperty

```
let person = {
  name: 'henry',
  age: 18,
};

person.hasOwnProperty('name');
person.hasOwnProperty('gender');
person.hasOwnProperty('toString');
```

输出：

```
true;
false;
false;
```

+ Object于Map、Json的区别

```
JSON格式与JS对象的转换
1. JSON.parse():JSON格式=> JS对象
// 一个 JSON 字符串
const jsonStr =
  '{"sites":[{"name":"Runoob", "url":"www.runoob.com"},{"name":"Google", "url":"www.google.com"},{"name":"Taobao", "url":"www.taobao.com"}]}';

// 转成 JavaScript 对象
const obj = JSON.parse(jsonStr);

2. JSON.stringify(): JS对象=>JS格式
const jsonStr2 = JSON.stringify(obj)；
```

+ Map

Map与Object很相似，都能保存键值对，区别：

```
1. 一个Object的键通常是字符串，但是一个Map的键可以是任意值，包括函数，对象，基本类型
2. Map中的键值是有序的，而添加到对象中的键则不是
3. Map的键值对个数可以直接获取，Object则需要用Object.keys()等间接获取
4. Map可以直接进行迭代，Object则需要借助Object.keys()等
5. Map不存在键名与原型键名冲突问题，可以直接覆盖，而Object不行
```

#### 内置对象--Math、Storage

+ Math对象

提供各种数学函数，该对象不是构造函数，不能生成实例，所以属性和方法都需要在Math对象上调用

常量：

```
Math.E // 常数e。
Math.LN2 // 2 的自然对数。
Math.LN10 // 10 的自然对数。
Math.LOG2E // 以 2 为底的e的对数。
Math.LOG10E // 以 10 为底的e的对数。
Math.PI // 常数π。
Math.SQRT1_2 // 0.5 的平方根。
Math.SQRT2 // 2 的平方根。
```

用的多的是Math.PI

静态方法：

```
Math.abs() // 绝对值
Math.ceil() // 向上取整
Math.floor() // 向下取整
Math.round() // 四舍五入取整
Math.max() // 最大值
Math.min() // 最小值
Math.pow() // 指数运算
Math.sqrt() // 平方根
Math.log() // 自然对数
Math.exp() // e的指数
Math.random() // 随机数
```

除了Math.random()都需要传入合适的参数。注意几个取整的方法：
Math.ceil() // 向上取整
Math.floor() // 向下取整
Math.round() // 四舍五入取整

+ Storage对象

Storage接口用于脚本在浏览器保存数据。两个对象部署了这个接口：windows.sessionStorage和windows.localStorage

sessionStorage保存的数据用于浏览器的一次会话(session),当会话结束(通常是窗口关闭)，数据被清空;localStorage保存的数据长期存在，
下一次访问该网站的时候，网页可以直接读取以前保存的数据

主要看一下windows.localStorage的用法

数据的存入：setltem
写法：`window.localStorage.setItem('myLocalStorage', 'storage Value');`

windows.localStorage.setItem('key','value')方法接受两个参数：key键名，value键值
两个参数都是字符串，不是字符串的参数会被转成字符串后存入浏览器

浏览器开发者工具的application中可以查看

如果要存入的数据不是字符串类型，最好先转换成字符串类型，比如：

```
const obj = {
  name: 'henry',
  age: 18
}
const value = JSON.stringify(obj);
window.localStorage.setItem('myLocalStorage', value);
```

读取数据:getItem 写法：`window.localStorage.getItem('myLocalStorage');`

接受一个参数，键名

清除缓存：clear，写法：`window.localStorage.clear();`

可以清除所有保存的数据

#### 内置对象--String

JS原生提供的三个包装对象：String，Number，Boolean

包装对象：原生对象可以把原始类型的值包装成对象`let v2 = new String('abc');`

包装对象的最大目的：1.使得JS的对象涵盖所有的值，2.使得原始类型的值可以方便的调用某些方法

+ 字符串长度：length

```
let len = 'here is an apple'.length;
```

返回的结果包括空格

+ 查找字符indexOf()

```
let str = 'here is an apple';
const index = str.indexOf('an');
console.log(index);
```

参数是要查找的子字符串(内容)，返回开始的下标，查找不到就返回-1

+ 去掉两端的空格：trim()

```
// 'here' 之前有一个空格，'apple' 之后有三个空格
let str = ' here is an apple   ';
const trimedStr = str.trim();
console.log(str.length);
console.log(trimedStr.length);
```

去掉两端的空格，不改变原字符串，返回处理后的空格

+ 截取字符串：substring/substr

如果要截取一个字符串中的一部分，可以用substring或substr

比如现在有个url：`https://www.youkeda.com/userhome#collect`,要截取其中#后面的内容

```
let url = 'https://www.youkeda.com/userhome#collect';

// 首先找到 # 后第一个字母的下标
const index = url.indexOf('#') + 1;

// 有 hash 才能进行截取，没有就直接提示不存在
if (index) {
  // 用 substring 截取字符串
  const hash1 = url.substring(index, url.length);

  // 计算 hash 的长度
  const lenHash = url.length - index;
  // 用 substr 截取字符串
  const hash2 = url.substr(index, lenHash);

  console.log(hash1);
  console.log(hash2);
} else {
  console.log('不存在 hash');
}
```

substring(start,end):start是开始的下标，end是结束的下标

substr(start,len):start是开始的下标，len是要截取的字符串的长度

如果第二个参数不写，就会一直截取到结尾

不会改变原字符串

+ 分割字符串：split

split方法按照给定的规则分割字符串，返回一个由分割出来的字符串组成的数组

```
const splitedStr = 'a|b|c'.split('|');
console.log(splitedStr);
```

可以接受两个参数，第一个参数可以是分隔符或正则表达式，第二个参数可写可不写，用到不多(表示返回数组内元素的最大个数)

#### 内置对象Array

Array是JS的原生对象之一，为数组提供了很多实用的方法

+ 连接数组：join()

join()方法可以以指定参数作为分隔符，将所有数组成员连接为一个字符串。如果不提供参数，默认以逗号隔开

```
let arr = [1, 2, 3, 4];

arr.join(" "); // '1 2 3 4'
arr.join(" | "); // "1 | 2 | 3 | 4"
arr.join(); // "1,2,3,4"
```

这个方法和字符串中的split刚好是一对相反的方法

```
let str = "a|b|c";

const splited = str.split("|");
console.log(splited);

const joined = splited.join("|");
console.log(joined);
```

join()不会改变原数组

+ 倒序排列：reverse()

reverse()用于颠倒排列的数组，返回改变后的数组

```
let arr = ["a", "b", "c"];

arr.reverse(); // ["c", "b", "a"]
arr; // ["c", "b", "a"]
```

该方法将改变数组

+ 排序sort()

sort方法对数组成员排序，默认按照字典顺序排序，如果想让sort按自定义的顺序排序，可以传入一个函数作为参数

```
let arr = [
  { name: "jenny", age: 18 },
  { name: "tom", age: 10 },
  { name: "mary", age: 40 },
];

arr.sort(function (a, b) {
  return a.age - b.age;
});

console.log(arr);
```

这里传入了一个函数，这个函数有两个参数即进行比较的两个数组成员，原数组中a排在b前面，

这个函数有个返回值，返回值大于0时，表示第一个成员应该排在第二个成员之后，否则相反

该方法将改变原数组

+ 遍历：map/forEach

遍历数组我们之前用的是for，但是JS提供了两个方便的遍历方法：map/forEach

有返回值的遍历：map，方法使用：它接受一个函数，然后将数组的所有成员依次传入这个参数函数，最后把每一次的执行结果组成一个新的数组返回

```
let arr = [
  { name: "jenny", age: 18 },
  { name: "tom", age: 10 },
  { name: "mary", age: 40 },
];

// elem: 数组成员
// index: 成员下标
// a: 整个数组
const handledArr = arr.map(function (elem, index, a) {
  elem.age += 1;
  console.log(elem, index, a);
  return elem.name;
});

console.log(arr);
console.log(handledArr);
```

map的参数函数可以有3个参数：elem,index,a；elem表示依次传入的数组成员，index表示数组成员对应的下标，a表示整个数组，map方法的返回值是由return后的内容一个一个组成的

无返回值的遍历：forEach

forEach的用法和map几乎一致，不过forEach没有返回值

```
const handledArr = arr.forEach(function (elem, index, a) {
  elem.age += 1;
  console.log(elem, index, a);
  return elem.name;
});

console.log(handledArr);
```

输出undefined

#### 内置对象Date

JS提供一个原生的时间库：Date对象，以国际标准时间(UTC)1970年1月1日00：00：00作为时间的零点，可以表示的范围是前后各一亿天

+ 获取当前时间：new Date()

可以把Date看作是一个构造函数，用new命令生成一个时间的实例，在不加参数的情况下返回的是当前的时间

```
let now = new Date();
console.log(now);
```

传入一些参数可以生成特定的时间对象，可以传入数字，字符串，毫秒数

```
// 传入表示“年月日时分秒”的数字
let dt1 = new Date(2020, 0, 6, 0, 0, 0);
console.log(dt1);

// 传入日期字符串
let dt2 = new Date("2020-1-6");
console.log(dt2);

// 传入距离国际标准时间的毫秒数
let dt3 = new Date(1578240000000);
console.log(dt3);
```

都输出`2020-01-05T16:00:000Z`,第一个因为我们这边是东八区，而且月份范围是0-11

##### 日期运算

+ 时间差：毫秒数

两个时间对象可以直接相减，返回相差的毫秒数

```
let dt1 = new Date(2020, 2, 1);
let dt2 = new Date(2020, 3, 1);

// 求差值
let diff = dt2 - dt1;

// 一天的毫秒数
let ms = 24 * 60 * 60 * 1000;

console.log(diff / ms); // 31
```

+ 早晚比较

比较两个时间的早晚可以直接用`<`或者`>`

```
let dt1 = new Date(2020, 2, 1);
let dt2 = new Date(2020, 3, 1);

console.log(dt1 > dt2); // false
console.log(dt1 < dt2); // true
```

+ 解析日期字符串parse()

Date.parse()用来解析日期字符串，返回该时间距离时间零点的毫秒数

```
let dt = Date.parse("2020-1-6");
console.log(dt); // 1578240000000
```

##### 三大类方法：to方法，get方法，set方法

+ 时间对象转时间字符串：to方法
  toJSON()方法，转换为时间字符串

```
let dt = new Date();
let dtStr = dt.toJSON();

console.log(dtStr); // 2020-01-03T09:44:18.220Z
```

我们这里东八区比国际标准时间快八个小时，所以打出来的时间慢八个小时

+ 获取时间对象的年/月/日：get方法

```
let dt = new Date();
dt.getTime(); // 返回实例距离1970年1月1日00:00:00的毫秒数。
dt.getDate(); // 返回实例对象对应每个月的几号（从1开始）。
dt.getDay(); // 返回星期几，星期日为0，星期一为1，以此类推。
dt.getFullYear(); // 返回四位的年份。
dt.getMonth(); // 返回月份（0表示1月，11表示12月）。
dt.getHours(); // 返回小时（0-23）。
dt.getMilliseconds(); // 返回毫秒（0-999）。
dt.getMinutes(); // 返回分钟（0-59）。
dt.getSeconds(); // 返回秒（0-59）。
```

返回的都是整数，

+ 设置时间对象：set方法

```
let dt = new Date();
dt.setTime(ms); // 设置实例距离1970年1月1日00:00:00的毫秒数。
dt.setDate(date); // 设置实例对象对应每个月的几号（从1开始）。
dt.setFullYear(year); // 设置四位的年份。
dt.setMonth(month); // 设置月份（0表示1月，11表示12月）。
dt.setHours(hour); // 设置小时（0-23）。
dt.setMilliseconds(ms); // 设置毫秒（0-999）。
dt.setMinutes(min); // 设置分钟（0-59）。
dt.setSeconds(sec); // 设置秒（0-59）。
```

set方法没有setDay，因为星期几是由计算得到的

![Date](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-4-HTML-CSS/7/3.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)
