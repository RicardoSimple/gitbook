### css装饰
#### cursor
![cursor](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-2-HTML-CSS/1.3/f2-3-1-show.gif)
鼠标箭头的变化就是cursor实现的
```
<p>点击这里了解更多cursor性质</p>
```
添加方式：
```
p{
    cursor: pointer;
}
```
cursor的值很多，有：pointer，default，text，move，grab，zoom-in，zoom-out

[MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/cursor)
cursor的值不仅可以用关键字，还可以用url等，见mdn

#### box-shadow/text-shadow
![box-shadow](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-2-HTML-CSS/1.3/%E9%98%B4%E5%BD%B1%E8%A7%A3%E9%87%8A.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

```
/* x偏移量 | y偏移量 | 阴影模糊半径 | 阴影扩散半径 | 阴影颜色 */
  box-shadow: 2px 2px 2px 1px rgba(0, 0, 0, 0.2);
```
text-shadow原理类似，设置方法也类似
```
span{
    text-shadow: 0px 0px 10px rgba(0, 0, 0, 0.11);
}
```

[使用阴影制造缝合效果](https://codepen.io/HUBLine/pen/yLLRgYa)
[使用阴影制造多页效果](https://codepen.io/HUBLine/pen/YzzORYb)

#### flex布局
flex布局可以简便，完整，响应式的实现各种页面布局。
flex有强大的空间分布和对齐能力
+ flex是什么
flex是flexible box的缩写，意为弹性布局，任何一个盒模型都可以指定为flex布局


它最显著的效果就是把原本从上到下排列的块状元素变成水平排列
```
<div class="container">
  <div class="item">项目1</div>
  <div class="item">项目2</div>
  <div class="item">项目3</div>
</div>
```
css:
```
.container {
  display: flex;
  background: #D5E8D4;
  border: 1px solid #5D9E5A;
}

.item {
  width: 50px;
  height: 50px;
  background: #FFF2CC;
  border: 1px solid #B7A570;
  margin: 10px;
}
```
注:设为flex布局后，子元素的float，clear和vertical-align都将失效

直接设置flex的元素叫flex容器(container)
直接子元素叫flex项目(item)

#### flex布局调整水平方向分布属性justify-content
justify-content属性定义了项目在水平方向上的对齐方式
justify-content有六个值
```
justify-content: flex-start | flex-end | center | space-between | space-around | space-evenly;
```

flex-start(默认值):项目向水平方向的起点对齐
flex-end:项目向水平方向的终点对齐
center:项目向水平方向居中
space-between:在每行上均匀分配项目，相邻项目间距相同
space-around:在每行上均匀分布，项目间距如图
space-evenly:在每行上均匀分布，但是间距与space-between不同
![justify-content](https://document.youkeda.com/P3-2-HTML-CSS/1.4/9.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)
最常用的是space-between

#### 垂直方向上的分布align-items
align-items和justift-content都是容器的属性，设置在容器上

五个值：
```
align-items: flex-start | flex-end | center | baseline | stretch;
```

flex-start:项目向垂直方向的起点对齐
flex-end:项目向垂直方向的终点对齐
center：项目在垂直方向上居中
stretch（默认值）:项目在垂直方向上被拉伸到与容器相同的高度或高度
baseline：按项目的第一行文字的基线对齐
![align-text](https://document.youkeda.com/P3-2-HTML-CSS/1.4/10.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)
最常用的是center


#### flex-wrap
默认情况下，项目都排在水平方向上，并且是单行排列，不会换行
即使所有的项目超过了容器宽度，也不会换行，而是被压缩

如果想让项目换行，就可以用flex-wrap属性
定义项目是否换行，如何换行

flex-wrap可能取三个值：nowrap，wrap,wrap-reverse

nowrap(默认值):不换行
wrap：换行，第一行在上方
wrap-reverse:换行，第一行在下方
![flex-wrap](https://document.youkeda.com/P3-2-HTML-CSS/1.4/19.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

#### flex:none和flex:1
在前面我们已经知道如果我们容器设置了display:flex
那么当项目在一行内排不下时，就会被压缩

如果项目在一行内还有多余的位置那么项目不会被压缩也不会放大

如果在一行内排不下但是我们又要求能够在一行内排下又不被压缩，那么我们可以设置项目属性flex
可以控制项目的放大和缩小

+ 不允许项目压缩，放大 flex:none
注意：这个属性是设置在项目上的，之前的属性都是设置在容器上的

设置之后的项目不会被挤压或放大

可以指定某一个项目设置，这样只有这个项目不会被挤压或者压缩

+ 项目自动充满剩余空间
如果一行有剩余的空间，希望项目能够均匀放大，撑满水平方向，我们应该给所有项目设置flex:1

注意这里是给所有项目设置

+ 两栏自适应布局
指的是一栏固定宽度，另一栏根据浏览器的宽度自适应
html代码：
```
<div class="flex-auto">
  <div class="static"></div>
  <div class="flexible"></div>
</div>
```
css代码：
```
.flex-auto {
  display: flex;
}
.flex-auto .static {
  width: 100px;
  flex: none;
}
.flex-auto .flexible {
  flex: 1;
}
```

+ 指定项目放大
如果一行有剩余的情况下，可以指定某一个项目放大撑满剩余的空间

只要给指定的项目设置flex:1就行

#### flex-direction
与之前的三栏自适应布局类似，只不过我们需要垂直方向上三栏自适应布局

比如许多软件界面这样就可以在不同的手机上适应高度
![垂直方向三栏自适应](https://document.youkeda.com/P3-2-HTML-CSS/1.4/40.gif)

要做这种布局的思路很简单
只需要改变横向三栏自适应的中三栏的排列方向就行水平向右改为从上到下

flex-direction属性可以改变项目的排列方向

当设置flex-direction:column时三栏自适应就会从上到下排列

+ 主轴和交叉轴
![主轴和交叉轴](https://document.youkeda.com/P3-2-HTML-CSS/1.4/42.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

默认情况下主轴的方向：水平从左到右  交叉轴方向：竖直从上到下

容器中的项目默认按主轴排列

所以现在要改变项目的排列方向只需要改变主轴的方向就行

主轴和交叉轴是始终保持垂直的方向

+ flex-direction有四个有效值：row/row-reverse/column/column-reverse
row(默认值): 主轴为水平方向，起点在左边

row-reverse: 主轴为水平方向，起点在右边

column:主轴垂直，起点在上方

column-reverse:主轴垂直，起点在下方
![flex-direction](https://document.youkeda.com/P3-2-HTML-CSS/1.4/8.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

+ height:90vh
90vh,意思可以看作将浏览器高度分为100份，每一份占1vh

+ 主轴改变后的影响
