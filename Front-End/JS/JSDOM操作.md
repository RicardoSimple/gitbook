### DOM操作

#### DOM样式修改

需要哪些技术：

```
1. 如何使用JS创建节点
2. 如何设置节点的样式、属性？
3. 如何在已存在的节点内部添加子节点
4. 如何清空节点内部子节点
```

![](https://style.youkeda.com/img/course/f4/9/17.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)
达到上述效果

html

```
<!DOCTYPE html>
<head>
  <meta charset="utf-8" />
  <link rel="stylesheet" type="text/css" href="./post.css" />
  <title>优课达</title>
</head>
<body>
  <section class="box">
    <img
      class="java"
      src="https://document.youkeda.com/new-learn-path/Bitmap.png"
    />
    <div class="title">Java</div>
    <div class="select"></div>  
  </section>

  <script src="./post.js"></script>
</body>
```

JS:

```
// 保存当前是否选中的状态
let isSelected = false;

// 获取整个元素的节点
const box = document.querySelector('.box');

// 获取select框节点
const select = document.querySelector('.select');

// 给整个元素添加点击事件【大家可以先忽略这部分】
box.addEventListener('click', function () {
  // 点击以后触发这个函数

  // 修改当前选中状态，取反即可
  isSelected = !isSelected;

  // 如果当前是选中状态、则添加img到select中
  if (isSelected) {
    // 创建一个img标签节点
    const img = document.createElement('img');

    // 设置img的src属性和样式，让其撑满select框
    img.src = 'https://style.youkeda.com/img/sandwich/check.png';
    img.setAttribute('style', 'width: 100%; height: 100%;');

    // 将这个节点添加到select框中
    select.appendChild(img);
  } else {
    // 如果不是选择状态，则清空内部子元素
    select.innerHTML = '';
  }
});
```

+ 知识点：
  
  1. 创建标签节点
     document.createElement(tagName),此方法用于创建一个由标签名称tagName指定的HTML元素，也就是上节课提到的元素(标签)节点
  
  比如创建一个div标签：`const div = document.createElement('div');`
  
  1. 创建文本节点
     document.creatTextnode(string),如果想继续在这个标签内部添加纯文本内容，可以用
  
  ```
  const div = document.createElement('div');
  const txt = document.createTextNode('优课达-学的比别人好一点');
  div.appendChild(txt);
  document.body.appendChild(div);
  ```
  
  1. 添加新节点
  
  apendChild(newNode),此方法可以往该节点中插入儿子节点(排最后)
  
  inserBefore(newNode,referenceNode),此方法是在某个目标儿子节点之前添加，newNode表示新节点，referenceNode表示目标节点
  
  ```
  <!DOCTYPE html>
  <head>
  <meta charset="utf-8" />
  <title>优课达</title>
  </head>
  <body>
  <ul class="root">
    <li class="sars">SARS</li>
  </ul>
  <script src="./index.js"></script>
  </body>
  ```

JS:

```
const root = document.querySelector('ul.root');
const sars = document.querySelector('li.sars');

// 创建 H1N1
const H1N1 = document.createElement('li');
const H1N1Txt = document.createTextNode('H1N1');
H1N1.appendChild(H1N1Txt);
root.appendChild(H1N1);

// 创建 新型冠状病毒
const nCoV = document.createElement('li');
const nCoVTxt = document.createTextNode('新型冠状病毒');
nCoV.appendChild(nCoVTxt);
root.insertBefore(nCoV, sars);
```

最终顺序：

```
<ul class="root">
  <li>新型冠状病毒</li>
  <li>SARS</li>
  <li>H1N1</li>
</ul>
```

1. 设置样式，属性`img.setAttribute('style', 'width: 100%; height: 100%;');`

和直接在HTML中写style是一样的

dom.style是Map对象，所以还可以单独设置样式`dom.style.color = 'xxxx';`

setAttribute()不仅仅可以设置style，所有html属性都可以用它设置比如id，src等

1. classList

classList可以获取到DOM上所有的类，所有我们可以把样式写进css，然后只用增加或删除class

[classList](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/classList)

1. innerHTML

使用`innerHTML = ''`清空节点所有的后代节点，那也可以用innerHTML添加内容

```
function createDisease(txt) {
  const dom = document.createElement('li');

  // 我们可以直接用innerHTML设置其纯文本
  dom.innerHTML = txt;
  return dom;
}
```

#### 案例介绍

监听input输入事件

```
const input = document.querySelector('input');

// 监听键盘事件
input.addEventListener('keyup', function() {
  // this 是DOM节点，this.value可以获取input内输入的值
  console.log(this.value);
});
```

先将登录查看历史设置为可见，将搜索结果设置为不可见

接着动态设置显示和隐藏

```
const input = document.querySelector('input');

const login = document.querySelector('.login');
const searchResult = document.querySelector('.search-result');

// 监听键盘事件
input.addEventListener('keyup', function() {
  // this 是DOM节点，this.value可以获取input内输入的值
  if (this.value === '肺炎') {
    login.style.display = 'none';
    searchResult.style.display = 'block';
  } else {
    login.style.display = 'block';
    searchResult.style.display = 'none';
  }
});
```

肺炎搜索结果动态显示

搜索结果的数据是由JS发起网络请求返回的数据然后利用动态生成节点的方式插入页面

暂时先模拟

```
let data = [
  '<em>肺炎</em>实时疫情动态',
  '<em>肺炎</em>的症状有哪些症状',
  '<em>肺炎</em>武汉',
  '<em>肺炎</em>症状',
  '<em>肺炎</em>最新',
  '<em>肺炎</em>是怎么引起的',
  '<em>肺炎</em>最新消息',
  '<em>肺炎</em>实时',
  '<em>肺炎</em>症状及表现',
  '<em>肺炎</em>最新情况'
];
```

利用上述数据生成多个li节点

封装函数

```
function createSearchItem(txt) {
  const item = document.createElement('li');
  item.innerHTML = `<i class="search"></i><p>${txt}</p><i class="edit"></i>`;
  return item;
}
```

整体代码：

```
let data = [
  '<em>肺炎</em>实时疫情动态',
  '<em>肺炎</em>的症状有哪些症状',
  '<em>肺炎</em>武汉',
  '<em>肺炎</em>症状',
  '<em>肺炎</em>最新',
  '<em>肺炎</em>是怎么引起的',
  '<em>肺炎</em>最新消息',
  '<em>肺炎</em>实时',
  '<em>肺炎</em>症状及表现',
  '<em>肺炎</em>最新情况'
];

function createSearchItem(txt) {
  const item = document.createElement('li');
  item.innerHTML = `<i class="search"></i><p>${txt}</p><i class="edit"></i>`;
  return item;
}

const input = document.querySelector('input');

const login = document.querySelector('.login');
const searchResult = document.querySelector('.search-result');

// 监听键盘事件
input.addEventListener('keyup', function() {
  // this 是DOM节点，this.value可以获取input内输入的值
  if (this.value === '肺炎') {
    // 先把原始内容清空
    searchResult.innerHTML = '';
    for (let i = 0; i < data.length; i++) {
      searchResult.appendChild(createSearchItem(data[i]));
    }

    login.style.display = 'none';
    searchResult.style.display = 'block';
  } else {
    login.style.display = 'block';
    searchResult.style.display = 'none';
  }
});
```

+ 总结解决思路：

```
1. 首先我们在不考虑动态效果的情况下，把页面中涉及到的所有元素都用静态页面的形式写出来

2. 其次利用JS控制区域的显示和隐藏，达到动态效果

3. 最后根据写好的静态页面模板和数据，动态创建DOM节点
```

css样式属性[transform](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform)
