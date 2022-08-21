### BOM

页面交互过程

#### Bom

我们知道为了让浏览器显示出来，必须要在浏览器中键入网址，敲击回车，浏览器能自动帮我们渲染网页内容，和浏览器渲染有关的对象，我们叫做浏览器对象模型(BOM)

BOM是由一系列相关对象构成，每个对象都提供了很多方法和属性

但BOM缺乏标准，BOM属于约定俗成，比如chrome怎么实现，IE怎么实现，所以不同的浏览器不完全相同。

在前端有一门高级技术叫浏览器兼容处理

现在主要以chrome为准

##### BOM对象

最重要的对象有四个：

+ window(窗口)
  window是整个窗口的框架，每个网页的内容都是装载在window里面

+ navigator(浏览器)
  navigator里面储存浏览器相关信息

+ history(历史)
  我们知道每个网页可以前进和后退，history便储存整个网页栈的

+ screen(显示屏幕)
  screen包含我们显示屏幕的信息，这个是硬件信息

+ Location(地址)
  location包含我们当前访问的地址(网址)信息

![Bom](https://style.youkeda.com/img/course/f4/8/1.jpeg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

+ 强调：
  ```
1. screen是整个电脑唯一的

2. navigator是整个浏览器唯一的，如果有多个浏览器就会有多个navigator

3. window是每个网页唯一的，每个网页都有一个独立的window

4. history和location是每个网页的信息，当然也是网页唯一的
   
   ```
   
   ```

#### window

在所有的BOM对象中最重要的是window对象

+ HTML中嵌入JS

在body内底部用script标签加入JS代码

在JS中最好的学习和排查方法就是console.log

可以打印一下window看一下到底是什么
![window](https://style.youkeda.com/img/course/f4/8/2.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

window对象中有很多方法，还有一些属性对象[MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Window)

```
1. window对象表示一个浏览器窗口或一个frame框架，它处于对象层次的最顶端，它提供了处理浏览器窗口的方法和属性
2. window对象是浏览器中的默认对象，所以可以隐式地引用window对象的属性和方法。在浏览器环境中，添加到window对象中的方法，属性其作用域是全局的。
```

默认对象，比如console.log等同于window.console.log,navigator等同于window.navigator,甚至自定义的顶层函数(直接定义的函数),也是挂载在window下；甚至Math对象，setTimeout函数等，都是在window下

总结：window是默认对象，如果是调用window上面的方法，可以省略，也可以称为隐式调用window上面的方法和属性

窗口内部高度和宽度：

```
window.innerWidrh
window.innerHeight
```

打开一个新窗口：

```
window.open()
三个参数，第一个是要在新打开窗口中加载的url；第二个是新窗口名称；第三个是可选，新窗口的特征
```

#### Location/History

##### Location

用来保存当前网页位置的信息
[MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Location)

+ Location属性

常用属性：href，hostname，host，protocol，origin，pathname，search

![用图表示](https://style.youkeda.com/img/course/f4/8/3.jpeg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

+ Location方法：

reload(),可以设置一个定时器延时调用

```
setTimeout(function () {
  window.location.reload();
}, 3000);
```

这个是每隔3秒刷新一次，虽然是setTimeout函数，但是每次刷新后会重修执行JS内的代码，所以会无限刷新

+ 跳转到新的地址

可以修改location的代码，将网页地址赋值给Location即可

```
window.location = 'https://www.youkeda.com';
```

##### History

如果原始页面为`https://www.youkeda.com`,History就储存为

```
// 会话记录
['https://www.youkeda.com'];
```

如果在网页中点击某个链接或者用location跳转到`https://www.baidu.com`,History就储存为

```
// 会话记录
['https://www.youkeda.com', 'https://www.baidu.com'];
```

以此类推

这是一个数组，(或者说列表)在实际存储中用到的数据结构和数组特别类似，叫做栈

+ History方法

back()和forward()对应浏览器左上角的返回和前进按钮

#### Navigator/Screen

##### Navigator

navigator表示用户代理的状态和标识，也就是浏览器基本信息

+ userAgent属性

代表当前浏览器的用户代理

输出信息：

```
Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_2) AppleWebKit/537.36 (KHTML, like
Gecko) Chrome/79.0.3945.130 Safari/537.36
```

Mozilla是一个基金会，表示这是一个主流浏览器，Inter Mac OS X表示电脑信息为Mac，Chrome/79.0表示浏览器的版本

+ Screen

查[MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/screen)
