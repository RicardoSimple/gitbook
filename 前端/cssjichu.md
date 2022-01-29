### css-美化文档
#### html内部添加样式
+ 在标签中添加声明
声明的关键字是style后接=再接""

```
<input type="text" placeholder="手机号码" style="">
```

声明位置不分先后
+ 在引号之间添加样式

```
<p style="font-size:14px;color:white"></p>
```

意思是设置p标签中的字体大小为14px，颜色为白色
#### 字体大小，字体粗细
css中，样式是由属性和值组成，冒号隔开分号收尾
![image-2.png](./image-2.png)
```
<!-- 设置字体的大小为12px -->
<p style="font-size: 12px;">
  一个轻量级和模块化的前端框架，用于开发快速和强大的web接口。
</p>
<!-- 设置字体的大小为24px -->
<p style="font-size: 24px;">
  一个轻量级和模块化的前端框架，用于开发快速和强大的web接口。
</p>
```

字体大小font-size

字体加粗font-weight
设置字体粗细时，其值可以是100,200,300,400,500,600,700,800,900中的任何一个，或者可以用英文代替，normal（正常粗细400），
lighter（细200），bold（粗700），bolder（更粗）

```
<p style="font-weight: 200;">优课达--学的比别人好一点～</p>
<p style="font-weight: lighter;">优课达--学的比别人好一点～</p>
<p style="font-weight: 400;">优课达--学的比别人好一点～</p>
<p style="font-weight: normal;">优课达--学的比别人好一点～</p>
<p style="font-weight: 700;">优课达--学的比别人好一点～</p>
<p style="font-weight: bold;">优课达--学的比别人好一点～</p>
```


