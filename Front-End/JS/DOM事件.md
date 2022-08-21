### DOM事件

#### DOM绑定事件

之前接触的事件：

```
// 监听Input输入事件
dom.addEventListener("input", function () {});

// 监听鼠标放置，移动事件
dom.addEventListener("mouseover", function () {});

// etc...
```

DOM可以通过`addEventListener(eventName, callback)`绑定eventName事件

有两种不恰当写法：

1. 事件嵌套到HTML代码中`<div onclick="console.log('xxx')"></div>`这种写法会导致JS代码嵌在HTML中，不利于文件分离

2. 事件方法替换`dom.onclick = function () {};`这种写法只能在这个DOM上绑定一个点击事件，下一个设置会顶替前面的设置

#### DOM事件

打印DOM事件对象，这个事件对象会通过回调函数参数的方式传递给我们

```
const h1 = document.querySelector("h1");
h1.addEventListener("click", function (e) {
  console.log(e);
});
```

![click](https://style.youkeda.com/img/course/f4/10/9.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

从结果看，可以看出，这是一个MouseEvent，关键属性：

```
属性         值                                 解释
target      <h1>优课达-学的比别人好一点</h1>      点击事件触发的DOM节点
type         click                              事件名称
pageX/pageY  92/47                              鼠标事件触发的页面坐标
```

[事件MDN](https://developer.mozilla.org/zh-CN/docs/Web/Events)

分类介绍：

```
焦点事件 
focus:表单组件(input,textarea等)获取焦点事件
blur:表单组件失去焦点事件

鼠标事件
click：点击事件
dblclick：双击事件
mousedown：在元素上按下任意鼠标按键
mouseenter：指针移到有事件监听的元素内
mouseleave：指针移出元素范围外(不冒泡)
mousemove：指针在元素内移动时持续触发
mouseover：指针移到有事件监听的元素或它的子元素内
mouseout:指针移出元素或者移到它的子元素上
mouseup：在元素上释放任意鼠标按键

键盘事件
keydown：键盘按下事件
keyup：键盘释放事件

视图事件
scroll：文档滚动事件
resize：窗口缩放事件

资源
load：资源加载成功事件
```

#### 点击事件

DOM事件中最常见的就是点击事件

首先给按钮添加点击事件，并且利用变量标识是否喜欢

```
// 默认是未点击喜欢
let hasLike = false;

const likeBtn = document.querySelector(".like-btn");
likeBtn.addEventListener("click", function () {
  // 点击事件
  hasLike = !hasLike;
  console.log(hasLike);
});
```

然后根据hasLike的值修改页面信息

```
// 默认是未点击喜欢
let hasLike = false;

const likeBtn = document.querySelector(".like-btn");
const likeBtnRight = likeBtn.querySelector(".right");
likeBtn.addEventListener("click", function () {
  // 点击事件
  hasLike = !hasLike;
  if (hasLike) {
    likeBtn.classList.add("has-like");
    likeBtnRight.innerHTML = parseInt(likeBtnRight.innerText.trim()) + 1;
  } else {
    likeBtn.classList.remove("has-like");
    likeBtnRight.innerHTML = parseInt(likeBtnRight.innerText.trim()) - 1;
  }
});
```

案例总结：

监听DOM事件，使用DOM操作，修改DOM属性

CSS中有短横线的属性在JS中用小驼峰表示（style.backgroundImage)

#### 冒泡，捕获，委托

冒泡就是点击事件会从最顶层元素一直到根元素，先触发最内层的点击事件

修复上面冒泡导致的异常，可以用`阻止冒泡-e.stopPropagation()`，比如：

```
// ......省略
likeBtn.addEventListener('click', function(e) {
  // 点击事件
  e.stopPropagation()

// ......省略
```

捕获和冒泡是完全相反的，冒泡是当前元素沿着祖先元素往上冒，而捕获是从根节点开始依次移动到当前节点

如果想在捕获阶段监听事件，我们需要传递第三个参数为true，比如

```
dom.addEventListener('click', function() {}, true);
```

委托其实是冒泡事件的一种应用

如果你想在大量子元素中单击任何一个都可以运行一段代码，那么可以将监听器设置在父节点上，并让子节点发生的事件冒泡到父节点，而不是每个子节点单独设置事件监听器

之前的function(e)中e表示MouseEvent，打印出来
![MouseEvent](https://style.youkeda.com/img/course/f4/10/10.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

会发现MouseEvent的target却在img上面，真实响应的DOM节点，所以之前的代码：

```
const box = document.querySelector('.box');

box.addEventListener('click', function(e) {
  // 注意box区域比img大，如果点击在空白间隔区域，那么返回的节点将不会是IMG，需要特殊处理一下
  if (e.target.nodeName === 'IMG') {
    document.body.style.backgroundImage = `url(${e.target.src})`;
  }
});
```
