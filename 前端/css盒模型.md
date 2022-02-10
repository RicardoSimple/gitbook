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

