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

自定义组件的根元素监听一个原生事件和html原生标签监听一个原生事件是有区别的

App.vue中：

```html
<!-- 给自定义组件添加点击事件 print -->
<Article
  v-for="article in articleList"
  :key="article.title"
  :article="article"
  @click="print(article)"
></Article>
```

Article.vue中：

```html
<div class="article-title" @click="printTitle">{{ article && article.title }}</div>
```

method：

```js
// 在 `methods` 对象中定义方法
methods: {
  printTitle() {
    alert("cilcked a title");
  }
}
```

只有原生标签上的事件触发

让父组件中添加的print也执行，需要修饰符(之前有.prevent,.capture)

父组件中添加`.native`修饰符

```html
<Article
  v-for="article in articleList"
  :key="article.title"
  :article="article"
  @click.native="print(article)"
></Article>
```

这时两个事件都发生

***除事件修饰符，Vue还提供按键修饰符，监听键盘事件***

比如`@keyup.enter`监听回车，也可以用ASCII码比如

`@keyup.13`也是回车

#### 自定义事件

一般都在子组件Article.vue中写事件（比如子组件中的点赞按钮）

但是我们不能在子组件中直接修改父组件传来的prop数据，修改父组件中的原数据需要用自定义事件

比如在子组件中使用自定义事件"upVote"实现点赞

1. 给子组件绑定自定义事件
   
   App.vue中`v-on:upVote="handleLikes"`比如
   
   ```html
   <!-- 自定义事件 upVote，调用该事件时会执行 handleLikes 方法 -->
   <article
     v-for="article in articleList"
     :key="article.title"
     :article="article"
     v-on:upVote="handleLikes"
   ></article>
   ```
   
   `upVote`只是事件名，类似click

2. 在Article.vue中调用自定义事件upVote
   
   ```html
   <!-- 在 template 中直接调用自定义事件 upVote -->
   <button @click="$emit('upVote')">点赞</button>
   ```
   
   如果在click的同时还有其他事处理，可以写成
   
   ```html
   <button @click="childEvent">点赞</button>
   ```
   
   ```js
   methods: {
     childEvent: function() {
       // 调用自定义事件 upVote
       this.$emit('upVote');
       // do other things
     }
   }
   ```

通过自定义事件的参数实现数据从子组件传递到父组件

注意：在子组件调用时传参数方法：

```html
<button @click="childEvent">点赞</button>
```

```js
methods: {
  childEvent: function() {
    // 调用自定义事件 upVote，这里的第二个参数最后会传到父组件中的 handleLikes 方法里
    this.$emit('upVote', this.article);
    // do other things
  }
}
```

这里也可以有多个参数，会成为对应的参数

#### 自定义事件中的双向绑定

修饰符`.sync`

父组件中：

```html
<MyCount class="count" :count.sync="count"></MyCount>
```

count是已定义变量

子组件中用`update:count`模式触发事件

```html
<div class="my-count">
  <button @click="$emit('update:count', count+1)">加一</button>
  {{ count }}
</div>
```

```js
props: ['count'],
```

#### 组件函数调用

也可以将变量写在子组件然后父组件调用子组件方法来修改子组件中的visible

调用子组件中的方法就是访问子组件实例，调用实例中的方法

利用vue提供的ref属性来访问子组件实例并调用方法

##### 调用子组件中的方法

1. 给要访问的子组件添加ref属性
   
   ```html
   <template>
     <Modal ref="modal"></Modal>
   </template>
   ```
   
   现在就可以通过`this.$refs.modal`来访问自定义组件

2. 调用子组件中的方法
   
   调用show方法
   
   ```js
   <script>
   export default {
     methods: {
       showModal() {
         // 调用子组件中的 show 方法
         this.$refs.modal.show();
       }
     }
   };
   </script>
   ```

##### ref访问子元素

```html
<template>
  <div id="app">
    <input ref="input" type="text" />
    <button @click="focusInput">点击使输入框获取焦点</button>
  </div>
</template>
```

```js
<script>
export default {
  name: 'app',
  methods: {
    focusInput() {
      // this.$refs.input 访问输入框元素，并调用 focus() 方法使其获取焦点
      this.$refs.input.focus();
    }
  }
}
</script>
```

#### 组件slot入门

slot即插槽，相当于在子组件的dom中留一个位置，父组件如果需要就可以在插槽里添加内容

##### 插槽的基础使用

1. 在子组件Modal.vue中用slot标签预留一个位置，slot标签中的内容是后备内容(当父组件不在插槽中添加内容时显示的内容)，也可以为空
   
   ```html
   <div class="modal-content">
     <slot>这是个弹框</slot>
     <div class="footer">
       <button @click="close">close</button>
       <button @click="confirm">confirm</button>
     </div>
   </div>
   ```

2. 在父组件中使用子组件
   
   在父组件中使用子组件，但不向自定义组件的插槽slot中添加内容
   
   ```html
   <Modal :visible.sync="visible"></Modal>
   ```
   
   使用子组件插入并插入个性化内容
   
   - ```html
     <Modal :visible.sync="visible">个性化内容</Modal>
     ```

#### 组件slot进阶

需要多个插槽的时候，比如

```html
<div class="modal" v-if="visible">
  <div class="modal-content">
    <header>
      <!-- 我们希望把页头放这里 -->
    </header>
    <main>
      <!-- 我们希望把主要内容放这里 -->
    </main>
    <footer>
      <!-- 我们希望把页脚放这里 -->
    </footer>
  </div>
</div>
```

`<slot>`元素有属性：name可以定义额外的插槽

```html
<div class="modal" v-if="visible">
  <div class="modal-content">
    <header>
      <slot name="header"></slot>
    </header>
    <main>
      <slot></slot>
    </main>
    <footer>
      <slot name="footer"></slot>
    </footer>
  </div>
</div>
```

有name属性是具名插槽，没有name属性是匿名插槽，默认name为default

在向具名插槽提供内容时可以在一个`<template>`元素上使用v-slot指令，并以参数形式提供名称

```html
<Modal :visible.sync="visible">
  <template v-slot:header>
    <h1>Modal title</h1>
  </template>

  <div>main content</div>
  <div>main content</div>

  <template v-slot:footer>
    <p>Modal footer</p>
  </template>
</Modal>
```
