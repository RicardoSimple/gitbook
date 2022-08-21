### css定位

#### position-static(默认定位)

css的一个非常关键的属性-position，这个属性在css中用于定位dom元素，修改dom元素的布局

static是遵循默认的文档流，从上到下，从左到右
除了这个static还有四个值

relative(相对定位)

absolute(绝对定位)

fixed(固定定位)

sticky(粘性定位)

#### position-relative(相对定位)

![列子](https://document.youkeda.com/P3-1-HTML-CSS/1.8/2-relative/1.jpeg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

如果想图片相对于其他内容发生偏移，可以用relative(使用margin会引起文档流的变化)

在此情况下就可以使用top，left，right，bottom属性
实现图片中的效果的代码：

```
.first {
  position: relative;
  left: 50px;
  top: 50px;
}
```

#### position-absolute(绝对定位)

绝对定位脱离了文档流，相对定位是未脱离文档流且相对于文档流原位置发生偏移，
而绝对定位是通过指定元素相对于最近的非static定位祖先元素的偏移，来确定元素位置的

#### position-fixed(固定定位)

有些时候文章的标题会一直停留在窗口的顶部，不会随着滚动而变化位置

所以fixed就是固定的意思

固定定位和绝对定位类似，但是元素的包含块是屏幕窗口

固定定位不会为元素预留空间，而是通过指定元素相对于屏幕窗口的位置来确定元素位置

+ z-indedx
  在之前的演示中，我们知道html页面是由多个图层组成的

图层优先级就是由z-index决定

```
默认的非static元素的z-index都为0
z-index越大，则越在最上面，离观察者最近
同样的z-index，则在html中元素越靠后，离观察者越近
```

#### position-sticky(粘性定位)

![sticky](https://document.youkeda.com/P3-1-HTML-CSS/1.8/6-sticky/1.mp4)

就是当mountain滚到顶部时，会粘在顶部，

滚动回去时会回到之前的位置

```
h1 {
  position: sticky;
  color: yellowgreen;
  top: 50px;
  z-index: 1;
}
```

#### float

新标签nav，main

nav：一般表示此区域是导航区域
main：一般用户表示此区域是网页的主体区域

+ float
  中文是浮动

有两个最基本的值： left，right
比如导航栏的设置中logo和头像：

```
.logo {
  float: left;
}

.avatar {
  float: right;
}
```

#### 模态框

模态框总是在浏览器的中心，浏览器随意的放大缩小，模态框还是在浏览器中心

模态框总有一个半透明的背景

+ 元素水平居中
  1. 如果内部是行内元素，我们可以在父容器中使用text-align: center
  2. 如果内部是块状元素，我们可以在子容器中使用margin: 0 auto(如果不是块状元素，需要设置display: block)
+ 元素垂直居中
  可以使用margin完成居中效果
  margin-top=（modal高度-img高度）/2

left,top定位的都是左上角，所以还要用margin来调整.3333333333 

#### 搜索框

由两个组件构成，一个搜索框，一个搜索结果列表

+ 搜索框
  一般将导航栏右边的元素都用容器包起来，设置float
  搜索用input设置
  搜索图标可以用绝对定位设置

+ 搜索结果
  搜索结果宽度和头部搜索栏的宽度一致

搜索结果脱离文档流，是浮在所有元素上面

+ 阴影
  box-shadow
  [mdn阴影](https://developer.mozilla.org/zh-CN/docs/Web/CSS/box-shadow)

+ 修改placeholder的样式
  
  ```
  ::-webkit-input-placeholder {
  color: red;
  }
  ```
