如果我们要做手机端上用的页面(H5),除了用媒体查询进行小屏幕设备的适配，还需要设置一些特别的内容，比如页面是否允许被用户缩放

这里我们要用到`<head>`中的`<meta>`元素

### meta元素
meta是一个自闭和元素，只有开始标签，没有结束标签

对meta元素进行设置指的是通过标签属性进行设置

meta元素表示那些不能由其他HTML元相关元素表示的任何元数据信息，比如页面是否允许用户缩放，比如针对搜索引擎和更新频率的描述和关键词等
优课达首页meta设置:
```
<!-- 声明文档使用的字符编码 -->
<meta charset="utf-8">
<!-- http-equiv相当于http的文件头作用，它可以向浏览器传回一些有用的信息，以帮助浏览器正确地显示网页内容 -->
<!-- 与http-equiv对应的属性值为content，这里IE=edge告诉IE使用最新的引擎渲染网页 -->
<meta http-equiv="X-UA-Compatible" content="IE=Edge">
<!-- name属性主要用于描述网页，与之对应的属性值为content，content中的内容主要是便于搜索引擎机器人查找信息和分类信息用的 -->
<meta name="title" content="优酷-中国领先视频网站,提供视频播放,视频发布,视频搜索 - 优酷视频" />
<meta name="keywords" content="视频,视频分享,视频搜索,视频播放,优酷视频" />
<meta name="description" content="视频服务平台,提供视频播放,视频发布,视频搜索,视频分享" />
```

+ charset 属性
utf-8:HTML的默认字符集，UTF-8(unicode)涵盖了世界上几乎所有的字符和符号

+ 移动web前端meta的基础设置
在开发移动端页面时需要设置meta如下:
```
<meta name="viewport" content="width=device-width,initial-scale=1.0,minimum-scale=1.0,maximum-scale=1.0,user-scalable=no" />
```
+ viewport的属性

 width: viewport的宽度
 height: viewport的宽度(很少使用)
 initial-scale: 初始的缩放比例
 minimum-scale: 允许用户缩放到的最小比例
 maximum-scale: 允许用户缩放到的最大比例
 user-scalable: 用户是否可以手动缩放

上面代码区块的设置表示：H5页面窗口自动调整到设备宽度，并禁止用户缩放页面

### rem/em
在开发响应式布局时，不同的浏览器宽度会有不同的尺寸

但是如果新增一个断点，就要重修写一遍@media

#### rem单位
css3中的rem单位可以简化这个工作
+ rem与px的转换
使用rem做单位时，rem和px的转换比取决于页根元素`<html></html>`的字体大小

1em = 根元素字体大小

一般浏览器的默认根元素字体大小为16px，但有时候是其他值。最小显示字体一般为10px，所以设置小于10px的值是无效的，会按10px计算，除非使用transform

所以设置标题字体和logo大小时就可以用rem为单位，这样设置`@media`就只设置html的字体就行

#### em单位
rem是根据根元素字体大小确定的，而em是根据父元素字体大小确定的，与父元素的字体大小相同
但是
```
.parent {
  // 父元素的字体大小为 20px
  font-size: 20px;

  > .child {
    // 子元素的字体大小为 0.7 * 20px = 14px
    font-size: 0.7em; 
    // 特别注意：子元素的宽度为 10 * 14px = 140px
    width: 10em;
  }
}
```
这两种em的值不一样，因为子元素中只有font-size是根据父元素确定em，其他都是根据子元素字体大小确定em


#### 新的单位vw/vh
vw,vh也是相对长度单位
 1vw = 1/100视窗宽度；
 1vh = 1/100视窗高度

视窗(viewport)就是文档的可见部分

用到最多的地方是模态框

将模态框的大小设置为视窗大小：
```
// 将蒙层的宽高设置为视口的宽度和高度
width: 100vw;
height: 100vh;
```
再将模态框定位到左上角
```
// 用定位把蒙层定位到浏览器的左上角
position: fixed;
top: 0;
left: 0;
```
