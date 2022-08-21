### 鼠标移动事件

分类：

1. mousemove这个是鼠标移动事件，比较简单
2. mouseenter/mouseleave这个是鼠标进入和离开，但是仅仅作用于当前DOM节点，不会作用于后代节点
3. mouseover/mouseout也是鼠标进入和离开事件，但是与enter/leave不同，此事件除了作用于当前节点外，还作用于其后代节点

opacity可设置元素透明度(0-1),transiton:opacity 0.2s(意思是透明变化过程为0.2秒)

### 表单元素事件

能捕捉表单元素的状态变化和内容值的修改

#### 焦点事件

focus和blur，获取焦点和失去焦点

比如注册页昵称输入框加入焦点事件

```
const nick = document.querySelector('input.nick');
nick.addEventListener('focus', function() {
  console.log('获取焦点');
});

nick.addEventListener('blur', function() {
  console.log('失去焦点');
});
```

#### 内容值变化

对于表单元素，有两种事件可以监听元素内容变化input和change

只要在输入框内输入值就会触发input，而要等输入框失去焦点才会触发change

区别：

```
事件     介绍                                             案例
change   当用户提交对元素的更改时触发，不一定每次都触发       checkbox值修改后，select选择后，input失去焦点后
input    只要value的值发生变化就会触发
```

#### 滚动事件

场景：无尽滚动，动态效果

+ 事件描述
  scroll

```
window.addEventListener('scroll', function () {
  console.log(window.scrollY);
});
```

通过scrollY访问Y方向上的滚动距离

+ 无尽滚动

当页面快滚动到底部时，添加新的文章内容到body中,代码

```
window.addEventListener('scroll', function () {
  // 可以通过clientHeight获取内容高度
  const height = document.body.clientHeight;

  // 通过screen.height获取浏览器的高度
  const screenHeight = window.screen.height;

  // 当距离底部的距离小于500时，触发页面新增内容
  if (height - window.scrollY - screenHeight < 500) {
    console.log('加载新文章内容');
    // 在底部添加10张图片
    const div = document.createElement('div');
    let str = '';
    for (let i = 0; i < 10; i++) {
      str += `
       <img
        class="first"
        alt=""
        src="https://document.youkeda.com/P3-1-HTML-CSS/1.8/1.jpg?x-oss-process=image/resize,h_300"
      />
      `;
    }
    div.innerHTML = str;
    document.body.appendChild(div);
  }
});
```

主要技术：

1. 内容高度`document.body.clientHeight`
2. 浏览器高度`window.screen.height`
3. 滚动距离`window.scrollY`
4. 滚动距离底部距离`内容高度-浏览器高度-滚动距离`

#### 其他事件

[键盘事件](https://developer.mozilla.org/zh-CN/docs/Web/API/KeyboardEvent#%E5%B1%9E%E6%80%A7)

[transform](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform)
transition:transform 0.2s(设置转换过程时间)
