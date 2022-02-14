普通的背景颜色就是直接设置

#### 渐变色
![案例](http://document.youkeda.com/P3-1-HTML-CSS/1.10/1-gradient/1.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

这个按钮左边的颜色和右边的颜色就不一样、

左边的色值:   #95CA47
右边的色值为:  #4DC891

background的新的值liner-gradient(线性渐变)

![方法](http://document.youkeda.com/P3-1-HTML-CSS/1.10/1-gradient/3.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

渐变类型，渐变方向，开始颜色，结束颜色

+ 每个值的含义
  1. 渐变方向 
  to right/to left, 
  to top/to bottom,
  o right bottom/to right top/to left bottom/to left top
  xxxdeg  xxx的范围0-360  更精确的变化
  1. 渐变位置
  渐变不一定是开始到结束的，可以设置各种中间状态
  比如要在30%-70%的部分渐变
  ![渐变位置](http://document.youkeda.com/P3-1-HTML-CSS/1.10/1-gradient/4.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)
  我们可以在每个值的后面设置一个百分比，px来约定变色起止位置

+ 渐变有很多种
多颜色渐变，弧形渐变
[mdn渐变](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Images/Using_CSS_gradients)

#### 背景图片
![示例](https://document.youkeda.com/P3-1-HTML-CSS/1.10/2-background/4.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

hello world是字，优课达是背景图片

加入背景图片方法：
![背景图片](https://document.youkeda.com/P3-1-HTML-CSS/1.10/2-background/6.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

一个background-image标签，加一个url包括的图片地址
+ 背景图片出现了重复
如果背景图片长宽任意一项小于了容器的长宽，默认css会让图片重复，直至填满整个容器位置

可以用
```
background-repeat: no-repeat;
```
来设置图片不重复
+ background-repeat
repeat是默认值，如果小于容器宽高就会在x，y方向上重复

repeat-x只在水平方向上重复

repeat-y只在垂直方向上重复

no-repeat 只显示一次，不重复
+ 背景图片不居中
默认情况下，背景图片是从左上角开始布局，为了使容器垂直水平居中，我们可以用
```
background-position: center;
```

+ background-position
值：top left，top center，top right，center left， center center，center right，bottom left，bottom center，bottom right
描述： 值的两个元素一起出现，分别表示水平和垂直布局
比如 background-position: top right;
等同于 background-position-x: top;background-position-y: right;
如果只写一个关键词，那么另一个默认为center

值： x% y%
描述： 第一个是水平位置，第二个是垂直位置，左上角是0% 0%，右下角是100% 100%，如果只写一个值，另一个默认为50%

值： Xpx Ypx

+ 背景图片撑满整个容器
如下
![1](https://document.youkeda.com/P3-1-HTML-CSS/1.10/2-background/9.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

用到background-size属性
+ background-size的值
cover ：把背景图片扩展至足够大以使背景图片完全覆盖背景区域，但是背景图片的某些区域也许无法显示在背景定位区域中

contain： 把图像扩展至最大尺寸，以使宽度和高度都完全适应背景区域

Xpx Ypx： 手动设置宽高

x% y%： 设置相对于容器的宽高

+ background合并写法
![合并写法](https://document.youkeda.com/P3-1-HTML-CSS/1.10/2-background/12.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

比如：
```
background: url(https://style.youkeda.com/img/ykd-components/logo.png)
    no-repeat center / contain;
```

+ 其他属性
[background-attachment](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-attachment)

[background-clip](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-clip)




