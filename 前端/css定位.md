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


