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
