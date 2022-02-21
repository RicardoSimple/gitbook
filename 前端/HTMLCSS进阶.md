### html5/css3介绍
#### html5简而言之就是html的升级版
+ 语义化标签
![常用语义化标签](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-2-HTML-CSS/1.1/courseware/%E8%AF%AD%E4%B9%89%E5%8C%96%E6%A0%87%E7%AD%BE2.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

html代码
```
<!-- 头部 -->
<header>header</header>
<!-- 主体 -->
<main>
    <!-- 导航 -->
    <nav>nav</nav>
    <!-- 区块 -->
    <section>section</section>
    <section>section</section>
</main>
<!-- 侧边栏 -->
<aside></aside>
<!-- 底部 -->
<footer></footer>
```

  1. header 头部
  标签是用于展示介绍性内容，通常包含一组介绍性的或者是辅助导航的实用元素
  1. main 主体
  通常是页面的主体内容区域
  1. footer 底部
  表示页面页脚的版权数据，与文档相关的链接，责任声明等信息
  1. nav 导航标签
  定义文档或者页面的导航
  1. aside 侧边栏
  定义页面的侧边栏内容
  1. section 区域块
  定义文档中的节，区域块， section更像接近div的语句，就是在页面开辟一块空间

css3与html5的关系类似于 css与html的关系
#### css伪元素 ::after/::before
![首页](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-2-HTML-CSS/1.2/f2-2-1-%E5%BE%AE%E5%8D%9A%E5%A4%B4%E9%83%A8.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

实现红框中的效果

```
<span class="icon"></span><span class="words">主页</span>
```
css
```
.icon {
    display: inline-block;
    width: 24px;
    height: 24px;
    background: url(https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-2-HTML-CSS/1.2/source/first-page.png) no-repeat center;
    background-size: contain;
    vertical-align: top;
    margin-right: 8px;
}

.words {
    font-size: 18px;
    line-height: 24px;
}
```
有更好的办法实现这种效果，通过伪元素
+ 伪元素
伪元素就是利用css代码在标签内部的前面，或者后面添加一个行内元素，这个行内元素可以理解为span

写法：
```
/* before */
选择器::before{
  /* 使用空白符号占位 */
  content: '';
}

/* after */
选择器::after{
  /* 使用空白符号占位 */
  content: '';
}
```

实现案例的代码
```
<span>主页</span>
```
伪元素
```
/* 在span之前添加行内元素 */
span::before {
  content: '';
  /* 将添加的行内元素定位，并设置大小、背景 */
    position: absolute;
    left: 0px;
    width: 24px;
    height: 24px;
    background: url(https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-2-HTML-CSS/1.2/source/first-page.png) no-repeat center;
    background-size: contain;
}
```

#### css伪类——清除浮动
+ 浮动带来的问题
设置子元素的高度后，父元素会自适应，会被子元素撑起来

会影响父元素兄弟元素的布局

+ 清除浮动
让父元素包住浮动的子元素

这种技术叫做清除浮动
css代码：
```
.clearfix::after{
  content: '';
  display: block;
  clear: both;
}
```
需要在父元素中加入类名clearfix
```
<!-- 添加清除浮动类名 -->
<div class="father-one clearfix">
    <div class="son-one">son-one</div>
    <div class="son-two">son-two</div>
    <div class="son-three">son-three</div>
</div>
```
总结：哪个元素有浮动，就在哪个元素中添加清除浮动

+ 尖括号在html中会报错，所以需要用转义字符来写
```
小米&lt;小爱老师&gt;
小米<小爱老师>
```

#### css伪类——事件伪类
通俗的说就是鼠标移动上去后，字体颜色，背景颜色或者边框中的一种或多种样式发生了变化

+ 鼠标移动上去——hover
先学习鼠标移动上去的效果

要让样式发生变化，需要有一个基础样式

当鼠标移动上去后，对应的地方，样式会发生改变，就是伪类

具体代码：
```
li:hover{
    background-color: #47A0FC;
    color: white;
}
```

+ active——鼠标点击时

这里的点击是按住不松手
代码：
```
ul>li:active{
    /* 要改变的效果 */
}
```

注意： hover一定要在active之前

+ 获取焦点后——focus
focus：获取焦点的伪类，一般是用于具有焦点的元素，比如：input，
我们可以让input获取到焦点后改变颜色


```
一个标签可以添加多个hover并且都会生效

但是也与选择器权重有关系

<div>
    <span></span>
</div>

div:hover>span{
    background:blue;
}
改变直接子元素的方法
```
如果是图片发生hover，可以用背景图片



