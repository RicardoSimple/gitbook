普通的背景颜色就是直接设置

+ 渐变色
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
