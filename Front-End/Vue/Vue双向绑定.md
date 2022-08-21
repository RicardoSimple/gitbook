### 处理用户输入

用户输入就是input，textarea及其他的选择框

#### v-model双向绑定--input

![](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-5-Vue/2/%E5%8F%8C%E5%90%91%E7%BB%91%E5%AE%9A.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

当改变下面input的输入框时，上面的框里的内容也会变

html：

```html
<template>
  <p class="page">{{message}}</p>
  <input type="text" v-model="message" placeholder="请输入你想输入的内容" />
</template>
```

变量：

```js
<script>
export default {
    name: "app",
    data() {
        return {
            message: ""
        };
    }
};
</script>
```

双向绑定就是改变message的值，input里面的值也要改变，改变input的值，message的值也要改变，只需加上v-model标签

textarea和input一样

#### v-model双向绑定--checkbox复选框

![](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-5-Vue/2/4.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

```html
<div class="food">
  <div class="check-box">
    <input type="checkbox" value="新奥尔良鸡腿堡" v-model="checkedGoods" />
  </div>
  <div class="food-name">新奥尔良鸡腿堡</div>
  <div class="food-price">￥24</div>
</div>
```

```js
<script>
export default {
    name: "app",
    data() {
        return {
            checkedGoods: []
        };
    }
};
</script>
```

#### 处理用户事件

js的dom中对事件的处理：

```js
document.getElementById("btn").addEventListener("click", function () {
  alert("Hello World");
});
```

在vue中：

##### v-on：（事件绑定）

第一步：给元素添加点击事件，vue中不叫添加事件，叫事件绑定

```html
<button v-on:click="add">按钮</button>
```

这样按钮就注册了一个点击事件，点击就会驱动add方法，再联系js

ps：vue为了方便用户开发，基本上每一个指令都有简写模式，比如`v-on`简写就是`@`符号

```html
<button @click="add">按钮</button>
```

##### methods(方法)

第二步：给点击事件添加方法

1. 抽离方法，只需将js中的匿名方法写在vue的规定的位置

```js
let alertFn = function () {
  alert("Hello World");
};

document.getElementById("btn").addEventListener("click", alertFn);
```

2. 书写位置

就是alerrFn的位置，在

```js
<script>
export default{
    name:"app",
    methods:{
        // 在这里存放方法
    }
}
</script>
```

3. 抽离的方法写在对应的位置`key:value`形式

```js
<script>
export default{
    name:"app",
    methods:{
        // 在这里存放方法
    }
}
</script>
```

##### 案例

![](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-5-Vue/2/5.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

```html
<template>
  <p>{{ counter }}</p>
  <button @click="add">点击</button>
</template>
```

```js
<script>
export default {
    name: "app",
    data() {
        return {
            // 初始次数
            counter:0
        };
    },
    methods:{
        add:function(){
            // 这里的this指向当前的vue实例
            this.counter++
        }
    }
};
</script>
```

注意：

1. this指向的是当前Vue实例，如果要用在data中定义的变量就要加this

2. `@click="add"`的完整写法是`@click="add()"`,在括号离可以传递参数
   
   ```html
   <button @click="add">点击</button>
   <!-- 等同于 -->
   <button @click="add()">点击</button>
   
   <!-- 当你的方法需要接收参数的时候，你可以将参数写在这个括号内 -->
   <button @click="add(number)">点击</button>
   ```

相应的method上也要添加参数

#### 事件修饰符

事件修饰符通常还要用到三种

1. 阻止冒泡事件
   
   ```html
   <div @click.stop="fn2"></div>
   ```

2. 捕获事件
   
   ```html
   <div class="div2" @click.capture="fn2"></div>
   ```

3. 阻止默认事件
   
   ```html
   <div class="div2" @click.prevent="fn2"></div>
   ```

### ![](C:\Users\ricardo\AppData\Roaming\marktext\images\2022-07-26-20-57-48-image.png)

### 监听数据变化

vue中是通过侦听器来实现

#### watch的基本用法

##### 书写位置

```html
<script>
  export default {    name: "app",    // 侦听器 key---watch value---{}
    watch: {}  };
</script>
```

顺序无所谓

##### 侦听对象

就是data中的变量，变量发生变化侦听器就开始运行

```html
<script>
  export default {    
    name: "app",    
    data: function () {      
        return {        
            count: 1
      };    
    },    
    watch: {      
        count() {        
            console.log("count发生了变化");      
                }    
        }  
    };
</script>
```

侦听器里面的方法一定要和被侦听的变量名一样

##### 侦听器运行时机

count要运行的话变量count就要变化

就需要添加改变count的方法

#### 侦听器进阶用法

获取前一次的值

侦听器可以给我们两个值，旧值和新值，添加一个参数即可获取旧值

```js
watch:{
    inputValue(value,oldValue) {
        // 第一个参数为新值，第二个参数为旧值，不能调换顺序
        console.log(`新值：${value}`);
        console.log(`旧值：${oldValue}`);
    }
}
```

#### handler方法和immediate属性

当我们侦听的值没有发生改变的时候是不会触发侦听器的，并且页面第一次渲染也不会触发侦听器

如果需要让页面第一次渲染的时候就触发侦听，就要immedidate属性

之前侦听器都是简写，实际侦听器是一个对象，里面包含handler方法和其他属性

```js
watch: {
      firstName: {
        handler: function (newName, oldName) {
          this.fullName = newName + " " + this.lastName;
        },
        immediate: true
      }
    }
```

当immediate属性为true时不论数据是否变化，页面刷新后handler方法都会运行
