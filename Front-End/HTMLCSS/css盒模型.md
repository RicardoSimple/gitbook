### 盒模型

#### content

+ 在页面中画出矩形的格子
  div标签，许多标签都是在div标签的基础上改造的

div就是一个干净透彻的矩形，没有默认，没有margin，padding，border，content

+ content
  div标签写出来是没有默认高度的，只有默认的宽度，和父标签宽度一样

设置矩形的宽高：width，height两个css属性
但是这样设置也还是看不见的，因为默认是没有颜色的
要给矩形添加新的颜色background-color背景颜色css属性

+ width新的值
  
  ```
  width: calc(100% - 480px); /*表示整个宽度100%减去480px*/
  ```

百分百尺寸

设置块元素的宽高还可以用%形式，但是是相对与父元素

#### padding内边距

![padding](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-1-HTML-CSS/1.7/google-padding.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

由绿色边框框起来的文字，距离上下左右都有一定的距离

![padding 2](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-1-HTML-CSS/1.7/padding-explore.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)
就是这样的内边距效果

+ padding分开写
  没有设置的内边距默认为0，而且padding默认是给矩形四周设置相同的内边距但是实际会有四边内边距不同的情况

```
.box {
    padding: 20px;
}
```

等价于

```
.box {
    padding-top: 20px; /*上内边距*/
    padding-right: 20px; /*右内边距*/
    padding-bottom: 20px; /*下内边距*/
    padding-left: 20px; /*左内边距*/
}
```

+ padding 简写
  最初形式是
  
  ```
  div{
    padding:20px 20px 20px 20px;
  }
  ```

如果上下一样，左右一样，可以写为

```
div{
    padding-top: 20px;
    padding-bottom: 20px;
    padding-left: 30px;
    padding-right: 30px;
}
```

或者

```
div{
    padding: 20px 30px;
}
```

后面两个值的位置不能颠倒

上下一样，左右不一样

```
div{
    padding-top: 20px;
    padding-bottom: 20px;
    padding-left: 10px;
    padding-right: 30px;
}
```

简写为

```
div{
    padding: 20px 30px 20px 10px;
}
```

上右下左的顺序

上下不一样，左右一样

```
div{
    padding-top: 30px;
    padding-bottom: 20px;
    padding-left: 10px;
    padding-right: 10px;
}
```

简写为

```
div{
    padding: 30px 10px 20px;
}
```

总结就是要按照上右下左的顺序写，如果没有，就跟对面的值一样

#### box-sizing

box-sizing规定了如何计算一个元素的总宽度和高度，它有两个值，content-box和border-box，默认是content-box

content-box计算的公式： width=内容的宽度，height=内容的高度

border-box计算的公式： width=border+padding+内容的宽度，height=border+padding+内容的高度

#### border 边框

边框就是包裹在padding外面的一层线

设置边框线：

```
.box {
  /* 设置矩形大小 */
  width: 200px;
  height: 30px;
  /* 设置边框线 */
  border-width: 2px;
  border-color: grey;
  border-style: solid;
}
```

+ css属性解释
  border-width是边框的粗细，单位是px
  border-color是边框的颜色
  border-style是边框的线型，solid是实线，dashed是虚线，这是常用的两种，还有其他的类型

+ 边框的简写
  
  ```
  .box {
  border: 2px solid blue;
  }
  ```
  
  后面的三个值要用空格分开，值的顺序可以忽略

+ 分别设置边框
  
  ```
  .box {
  /*设置顶部border*/
  border-top-color: blue;
  border-top-style: solid;
  border-top-width: 2px;
  /*那么接下来，left，right，bottom是类似的，这里忽略不写*/
  }
  ```

类似于padding分别设置，但是这种不推荐
推荐的写法：

```
.box {
  /* 添加顶部border */
  border-top: 1px solid black;
  /*添加右侧border*/
  border-right: 3px solid orange;
  /*添加底部border*/
  border-bottom: 5px dashed pink;
  /*添加左侧border*/
  border-left: 10px dashed purple;
}
```

+ 利用层叠性设置边框
  在设置边框时，遇到只有一边是与其他边不同的情况，可以用层叠性来减少代码量

```
.box {
  /*设置矩形的宽*/
  width: 300px;
  /*设置矩形的高*/
  height: 300px;
  /*设置矩形的背景颜色*/
  background-color: white;
  /*设置矩形的边框*/
  /*统一设置矩形的所有边框样式*/
  border: 2px solid black;
  /*重新设置一个下边框的样式来层叠掉统一设置的边框的样式*/
  border-bottom: 5px solid orange;
}
```

+ 无边框
  border的属性值none是无边框

+ 圆角
  圆角的设置方法
  
  ```
  .box {
  border-radius: 12px;
  }
  ```
  
  设置圆角之前需要先设置边框或背景色，圆角也能分开设置
  
  ```
  .box {
  width: 200px;
  height: 200px;
  background-color: violet;
  border-top-left-radius: 5px;
  border-top-right-radius: 10px;
  border-bottom-left-radius: 20px;
  border-bottom-right-radius: 15px;
  }
  ```
  
  是左上角等

+ 阴影
  
  ```
  .box {
  width: 200px;
  height: 200px;
  border: 1px solid #c4c4c4;
  /* x偏移量 | y偏移量 | 阴影模糊半径 | 阴影扩散半径 | 阴影颜色 */
  box-shadow: 2px 2px 2px 1px rgba(0, 0, 0, 0.2);
  border-radius: 15px;
  }
  ```
  
  阴影的实现原理可以看作是有一个同样大小的矩形在x轴y轴上偏移

x偏移量：在x轴上移动，向右为正
y偏移量：在y轴上移动，向下为正
阴影模糊半径：就是边线的清晰度
阴影扩散半径：就是向外扩散
阴影颜色：就是矩形下面那个矩形的颜色

#### margin

margin是外边距，所谓外边距，就是矩形与矩形之间的间距
![margin](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-1-HTML-CSS/1.7/margin1.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

margin的写法与padding一样

```
.box{
    /*总写*/
    margin: 20px;
    /*分开写*/
    margin-top: 20px;
    margin-right: 20px;
    margin-bottom: 20px;
    margin-left: 20px;
}
```

+ 两个盒子之间margin的计算
  水平距离是两个盒子的左右margin之和

垂直距离是两个盒子的最大设置值
![chuzhui](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-1-HTML-CSS/1.7/max_margin.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

所以在实际应用中设置两个盒子垂直间距为50px；
不推荐

```
.box1{
    margin-bottom: 20px;
}
.box2{
    margin-top: 50px;
}
```

推荐

```
.box1{
    margin-bottom: 50px;
}
```

不推荐

```
.box2{
    margin-top: 50px;
}
```

就是只设置底部

+ 盒子左右居中
  margin还有一个作用就是可以让盒子在父盒子中左右居中
  
  ```
  <div class="father"> 
    <div class="son"></div>
  </div>
  ```
  
  但是前提是必须有宽度
  
  ```
  .father{
    width:400px;
    height:200px;
    border: 1px solid #ccc;
  }
  ```

.son{
    width:200px;
    height:100px;
    margin:0 auto;
    border: 1px solid #ccc;
}

```
如果去掉son中的width就会失去居中效果

#### display:block/none
+ display:block
块元素属性1-独占一行
块元素性质2-可以设置宽高
+ 行内元素和块状元素的转换
块状元素默认的display是block
行内元素默认的display是inline

只要设置display的值就可以达到行内元素和块状元素互转的效果
+ display: none
none就是无，也就是设置了这个属性值，标签就会消失，在网页布局中最常用的就是用none和block来控制元素的显示和隐藏


#### display: inline/inline-block
行内元素不能设置宽高

行内元素可以设置padding
与块状元素的padding不太一样，会与块状元素重叠

行内元素可以设置左右margin，但是不能设置上下margin

+ inline-block
就是即具有inline的性质又具有block的性质

可以简单理解为inline-block就是可以显示在同一行的块元素
+ 空白折叠现象
并排的两个盒子之间出现了空白，但是我们并没有设置margin，原因就是两个div之间有回车

在html中，回车被当成一个文字，所以这里的空白就是文字的空白

解决方法有三种：
```

1. 去掉回车，写在同一行

2. 给父元素添加word-spacing属性
   
   <div class="father">
   <div class="box1"></div>
   <div class="box2"></div>
   </div>

.father {
  word-spacing: -50px;
}

.box1 {
  width: 200px;
  height: 50px;
  display: inline-block;
  background-color: #fff2cc;
}

.box2 {
  width: 200px;
  height: 50px;
  display: inline-block;
  background-color: #b0e3e6;
}

3. 给父元素设置font-size为0
   
   ```
   
   ```
