## Vue组件

组件是可复用的Vue实例，开发过程中可以把重复用到的功能封装成自定义的组件

### 组件的组织

通常一个应用会以一颗嵌套的组件树的形式来组织

![](https://document.youkeda.com/P3-5-Vue/5/4.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

为了能在模板`<template>`中使用，这些组件必须先注册以便识别

### 组件的注册

分全局注册和局部注册两种

1. 全局注册：用`Vue.component`来创建组件，注册之后可以在任何新创建的Vue根实例中使用

2. 局部注册：在单个Vue格式的文件中创建组件，在需要用到的地方进行注册

但通常我们都是基于Vue工程进行开发，会用到webpack这样的构建系统，而通过全局注册的组件在构建系统中即使没有被使用依然会存在于构建结果中，所以通常选择局部注册

### 组件的创建

1. 一个vue格式的文件，vue-cli创建的工程默认存在一个helloworld.vue
   
   可以指定组件名称
   
   ![](https://document.youkeda.com/P3-5-Vue/5/2.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)
   
   将其作为组件在App.vue中使用，组件可以重复使用
   
   ![](https://document.youkeda.com/P3-5-Vue/5/3.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

![](C:\Users\ricardo\AppData\Roaming\marktext\images\2022-07-30-14-57-42-image.png)

### 组件中的数据

自定义组件中的数据data必须是一个函数

```js
data: function () {
  return {
    count: 0
  }
}
```

组件间的data是相互独立的

### 组件单向数据流

实际开发同一个组件可能显示不同的内容

![](https://document.youkeda.com/P3-5-Vue/5/6.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

需要从父组件中传递不同的内容给子组件

#### prop的使用方法

基础使用

当父组件给子组件的prop传递一个值时这个值就变成了子组件实例的一个属性。比如给HelloVue组件传递一个标题

1. 父组件中，传递一个title给子组件
   
   ```html
   <template>
     <div id="app">
       <!-- 注意！title1 和 title2 是父组件的 data 中定义的数据，title 则是子组件中接收数据时的变量名 -->
       <HelloVue :title="title1"></HelloVue>
       <HelloVue :title="title2"></HelloVue>
     </div>
   </template>
   ```
   
   title1和title2是父组件的data中定义的数据而title则是子组件中接受数据时的变量名

2. 子组件中，用prop接受title
   
   ```html
   <template>
     <div class="hello">
       <!-- 第二步：在页面上显示 title 的值，写法和显示 data 里定义的数据一样 -->
       <h1>{{ title }}</h1>
     </div>
   </template>
   
   <script>
     export default {
       name: 'HelloVue',
       // 第一步：在 prop 属性中接收 title
       props: ['title']
     };
   </script>
   ```
   
   附带类型声明的props，类型首字母要大写
   
   ```html
   <script>
     export default {
       name: 'HelloVue',
       // 在 prop 属性中接收 title，其类型为 String
       props: {
         title: String
       }
     };
   </script>
   ```
   
   这里的prop是一个对象，当传入值有多个用逗号隔开，还可以给值设定一些要求
   
   ```js
   props: {
     title: String,
     // 多类型
     likes: [String, Number],
     // 带有默认值
     isPublished: {
       type: Boolean,
       default: true
     },
     // 必填
     commentIds: {
       type: Array,
       required: true
     },
     author: Object,
     callback: Function,
     contactsPromise: Promise
   }
   ```

不能用prop实现从子到父的数据传递

#### 单向数据流

父级prop更新向下流动

![](C:\Users\ricardo\AppData\Roaming\marktext\images\2022-07-30-15-17-58-image.png)

传到子组件的数据需要处理：

1. prop传入的数据需要处理
   
   使用计算属性对数据进行处理
   
   ```js
   props: ['initialTitle'],
   computed: {
     normalizedTitle: function () {
       // 对传入的 initialTitle 进行去空格、字母小写化处理
       return this.initialTitle.trim().toLowerCase()
     }
   }
   ```

2. prop传入的数据作为本地数据使用
   
   ```js
   props: ['initialTitle'],
   data: function () {
     return {
       // 要读取 prop 里的 initialTitle，要在前面加 “this.”
       // 将传入的 initialTitle 作为本地属性 title 的初始值
       title: this.initialTitle
     }
   }
   ```
   
   接下来就可以直接修改title，不改变父组件的initialTitle

ps：如果传递的是一个对象，在对其进行取数据操作时先判读是否有数据

#### 自定义组件绑定原生事件

##### 事件修饰符
