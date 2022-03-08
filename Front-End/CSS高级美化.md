#### 单行文本超出省略
在网页中我们经常看到文本内容溢出，用省略号代替剩余内容的情况

正常情况下文字内容超过所给的范围宽度就会自动换行，但是我们不希望它换行，就涉及三个内容：

强制不换行，元素内容溢出处理和文本溢出省略
+ 强制不换行
HTML5中推荐使用"white-space: nowrap"实现不换行

+ 元素内容溢出overflow
我们的目标是超出内容不显示，所以这一设置要隐藏元素，用到属性overflow

overflow有五个有效值：visible|hidden|inherit|scoll|auto;
```
visible(默认值): 从父元素继承overflow的值
hidden: 内容会被裁剪，并且超出的内容不可见
inherit: 内容不会被裁剪，会呈现在元素框之外
scoll: 内容会被裁剪，浏览器会显示滚动条以便查看超出的内容
auto: 由浏览器定夺，如果内容被裁剪，就会显示滚动条
```
+ 文本溢出省略 text-overflow
最后用省略号代替剩余内容用到text-overflow属性
有两个值 clip|ellipsis
```
clip(默认值): 表示在内容区域的极限出截断文本
ellipsis: 用省略号来表示被截断的文本
```

注： 设置这些属性的元素必须是块状元素，而且需要设置合适的宽度

#### 多行文本超出省略
+ 在webkit内核浏览器中
将之前的white-space: nowrap去掉再添加以下代码
```
/* 隐藏超出部分 */
overflow : hidden;
/* 文本超出就用省略号 */
text-overflow: ellipsis;
/* 把对象作为弹性伸缩盒子模型显示 */
display: -webkit-box;
/* WebKit内核的浏览器的私有属性，设置文本超出2行就用省略号 */
-webkit-line-clamp: 2;
/* WebKit内核的浏览器的私有属性，设置或检索伸缩盒对象的子元素的排列方式 */
-webkit-box-orient: vertical;
```
这些都是webkit内核私有的属性，其他内核浏览器不具备

+ 其他浏览器用js实现



