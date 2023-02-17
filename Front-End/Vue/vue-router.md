Vue Router是官方路由管理器

单页应用：spa单一页面应用程序，只有一个完整的页面；更新视图而不重新请求页面

路由：spa路径管理器，vue的单页面应用将路径和组件映射，路由用于设定访问路径，由路径之间的切换实现组件的切换

路由模式本质就是建立起url和页面之间的映射关系

#### 安装、配置vue-router

##### 安装路由

对于vue工程，我们通常用命令行：

```js
npm install vue-router
// 或者
yarn add vue-router
```

也可以先在package.json文件中预先写好再自动安装依赖

![](https://document.youkeda.com/P3-5-Vue/6/17.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

##### 配置路由

在main.js中进行路由配置

```js
// 0. 导入 Vue 和 VueRouter，要调用 Vue.use(VueRouter)
import Vue from 'vue'
import VueRouter from 'vue-router'

Vue.use(VueRouter)

// 1. 定义 (路由) 组件。
// 可以从其他文件 import 进来
import Foo from "./views/Foo.vue";
import Bar from "./views/Bar.vue";

// 2. 定义路由
// 每个路由应该映射一个组件。 其中"component" 可以是
// 通过 Vue.extend() 创建的组件构造器，
// 或者，只是一个组件配置对象。
// 我们晚点再讨论嵌套路由。
const routes = [
  { path: '/foo', component: Foo },
  { path: '/bar', component: Bar }
]

// 3. 创建 router 实例，然后传 `routes` 配置
// 你还可以传别的配置参数, 不过先这么简单着吧。
const router = new VueRouter({
  routes // (缩写) 相当于 routes: routes
})

// 4. 创建和挂载根实例。
// 记得要通过 router 配置参数注入路由，
// 从而让整个应用都有路由功能
const app = new Vue({
  router
}).$mount('#app')
```

这里的路径"/foo"就是对应Foo组件

##### 路由的使用

配置好路由，在vue文件中使用路由实现路径切换

先在App.vue中使用`<router-link>`组件来导航

```html
<template>
  <div id="app">
    <h1>Hello App!</h1>
    <p>
      <!-- 使用 router-link 组件来导航. -->
      <!-- 通过传入 `to` 属性指定链接. -->
      <!-- <router-link> 默认会被渲染成一个 `<a>` 标签 -->
      <router-link to="/foo">Go to Foo</router-link>
      <router-link to="/bar">Go to Bar</router-link>
    </p>
    <!-- 路由出口 -->
    <!-- 路由匹配到的组件将渲染在这里 -->
    <!-- App.vue 中的 <router-view></router-view> 是路由的最高级出口 -->
    <router-view></router-view>
  </div>
</template>
```

![](https://document.youkeda.com/P3-5-Vue/6/16.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

+ 导航标签`<router-link>`标签有一个`to`属性，指定路径；默认渲染成带有链接的a标签

+ 插槽标签`<router-view>`相当于一个插槽，所在位置将**渲染路由匹配到的组件**（包括留浏览器直接输入路径比如/foo）

Foo.vue和Bar.vue文件在`src/views`文件夹内

router配置过多可以将main.js中的部分抽出到`src/router/index.js`中

比如：

index.js:

```js
// 0. 使用模块化机制编程，导入Vue和VueRouter，要调用 Vue.use(VueRouter)
import Vue from 'vue';
import VueRouter from 'vue-router';
Vue.use(VueRouter);

// 1. 定义 (路由) 组件。
// 可以从其他文件 import 进来
import Foo from '../views/Foo.vue';
import Bar from '../views/Bar.vue';

// 2. 定义路由
// 每个路由应该映射一个组件。 其中"component" 可以是
// 通过 Vue.extend() 创建的组件构造器，
// 或者，只是一个组件配置对象。
const routes = [
  { path: '/foo', component: Foo },
  { path: '/bar', component: Bar }
];

// 3. 创建 router 实例，然后传 `routes` 配置
// 你还可以传别的配置参数, 不过先这么简单着吧。
export default new VueRouter({
  mode: 'history',
  routes // (缩写) 相当于 routes: routes
});

// 4. 在 main.js 中创建和挂载根实例
```

main.js:

```js
import Vue from "vue";
import App from "./App.vue";
import router from './router/index.js';

Vue.config.productionTip = false;

// 4. 创建和挂载根实例。
// 记得要通过 router 配置参数注入路由，
// 从而让整个应用都有路由功能
new Vue({
  router,
  render: h => h(App)
}).$mount("#app");
```

#### 添加路由

不仅可以普通import组件，比如

```js
// 1. 将要用到的组件从其他文件 import 进来
import Foo from './views/Foo.vue';
import Bar from './views/Bar.vue';
import User from './views/User.vue';

// 2. 定义路由，每个路由应该映射一个组件
// 添加路径即在 routes 数组中增加新的成员
const routes = [
  { path: '/foo', component: Foo },
  { path: '/bar', component: Bar },
  // 新增一项
  { path: '/user', component: User }
];

// 3. 创建 Router 实例，然后传 `routes` 配置
const router = new VueRouter({
  routes
});
```

也可以直接导入

```js
const routes = [
  { path: '/foo', component: () => import('./views/Foo.vue') },
  { path: '/bar', component: () => import('./views/Bar.vue') },
  { path: '/user', component: () => import('./views/User.vue') }
];
```

也可以命名同时直接导入，加上name属性即可

```js
// 0. 导入 Album、List、Add、Empty 三个组件

// 1. 定义路由
const routes = [
  { path: '/foo',
    name: 'fooName',
    component: () => import('./views/Foo.vue')
  }
];
```

通过命名跳转

```html
<!-- to 的值是一个对象而不是字符串，所以要在 to 前加 : -->
<router-link :to="{name: 'fooName'}">Go to Foo</router-link>
```

#### 路由布局管理

实际开发中会有组件多层嵌套而成，相应路由也会按某种结构对应嵌套

```
/album/list                           /album/add
+------------------+                  +-----------------+
| Album            |                  | Album           |
| +--------------+ |                  | +-------------+ |
| | List         | |  +------------>  | | Add         | |
| |              | |                  | |             | |
| +--------------+ |                  | +-------------+ |
+------------------+                  +-----------------+
```

1. 在组件中配合路由使用`<router-view>`
   
   App.vue:
   
   ```html
   <template>
     <div id="app">
       <h1>Hello App!</h1>
       <p>
         <router-link to="/album/list">Go to Album List</router-link>
         |
         <router-link to="/album/add">Go to Album Add</router-link>
       </p>
       <!-- 路由匹配到的组件将渲染在这里 -->
       <!-- 在本例中为 Album.vue 组件 -->
       <router-view></router-view>
     </div>
   </template>
   ```
   
   Album.vue:
   
   ```html
   <template>
     <div id="album">
       <div class="banner">banner</div>
       <!-- 路由匹配到的组件将渲染在这里 -->
       <!-- 在本例中为 List.vue 组件或 Add.vue 组件 -->
       <router-view></router-view>
     </div>
   </template>
   ```
   
   ![](https://document.youkeda.com/P3-5-Vue/6/5.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

2. 定义嵌套路由
   
   路由嵌套关系和组件一样
   
   ```js
   // 0. 导入 Album、List、Add 三个组件
   const routes = [
     {
       path: '/album',
       component: Album,
       // children 属性可以用来配置下一级路由（子路由）
       children: [
         { path: 'list', component: List },
         { path: 'add', component: Add }
       ]
     }
   ];
   ```
   
   App.vue中的`<router-view>`对应routes里的第一层即
   
   ```v
   {path:'/album',component:Album}
   ```
   
   Album.vue中的`<router-view>`对应第二层，即
   
   ```v
   {path:'list',component:List},
   {path:'add',component:Add}
   ```
   
   ##### 根路径/
   
   子路径path写法有两种：
   
   ```v
   path:'list'
   path:'/album/list'
   ```
   
   但是不能写成`path:'/list'`,以`/`开头的嵌套路径会被当成根路径，直接渲染到App.vue的`<router-view>`处

##### 空的子路由

如果在路径`/album`也能渲染的话可以添加空路由

```js
// 0. 导入 Album、List、Add、Empty 三个组件
const routes = [
  {
    path: '/album',
    component: Album,
    // children 属性可以用来配置下一级路由（子路由）
    children: [
      // 空的子路由
      { path: '', component: Empty },
      { path: 'list', component: List },
      { path: 'add', component: Add }
    ]
  }
];
```

#### 动态路由

会有多个路径对应一个组件的情况，比如`/user/123`,`/user/456`这里的123、456代表用户id，也就是路径添加的参数，但是渲染的页面是同一个组件

这种属于动态路由

写法：

```js
import User from "./views/User.vue";

const routes = [
  // id 就是路径参数
  { path: '/user/:id', component: User }
]
```

id为路径参数，一个`路径参数`前需要有冒号标记，当url匹配到路由中的一个路径时参数会被设置到`this.$route.params`里可以在组件内读取

比如`/user/123`匹配`/user/:id`，`this.$route.params.id`的值就是456，比如

```html
<template>
  <div>user id: {{ $route.params.id }}</div>
</template>
```

#### 捕获404页面

当用户输入的url不属于注册的任何路由，通常用404NotFound组件渲染，可以用通配符`*`来匹配任意路径：

```js
import NotFound from "./views/NotFound.vue";

const routes = [
  {
    // 会匹配所有路径
    path: '*',
    component: NotFound
  }
]
```

确保通配符的路由放在最后，因为路由匹配是根据注册顺序，如果这个放最前面那么所有页面都会NotFound

#### 读取匹配到的路径值

当使用一个通配符，`$routeparams`内会自动添加一个`pathMatch`参数，包含了URL通过通配符被匹配的部分，比如匹配`http://localhost:8081/non-existing/file`:

```js
this.$route.params.pathMatch // '/non-existing/file'
```

#### 页面跳转

##### 编程式导航

除使用`<router-link>`来创建a标签定义导航链接外还可以借助router的实例方法，通过编写代码比如`router.push()`

注：因为在Vue实例内部，我们可以通过$router访问router实例，所以可以用

`this.$router.push()`调用

##### 用`router.push`进行页面跳转及参数传递

1. router.push的参数为字符串路径
   
   ```js
   router.push('user')
   router.push('/user')
   ```
   
   两种写法跳转后的url变化不同
   
   ![](C:\Users\ricardo\AppData\Roaming\marktext\images\2022-08-30-14-32-27-image.png)
   
   因为`/`意味着匹配根路由，所以这样写不管原路径是什么都会跳转为
   
   `localhost:8080/user`

2. router.push的参数为描述地址的对象
   
   ```js
   // 对象
   // 这种写法和字符串类型的参数一样
   router.push({ path: 'home' })
   
   // 命名的路由
   router.push({ name: 'user', params: { userId: '123' }})
   
   // 带查询参数，变成 /register?plan=private
   router.push({ path: 'register', query: { plan: 'private' }})
   ```
   
   **后两种写法**：
   
   ```json
   {name:'user',params:{userId:'123'}}
   ```
   
   对应的**命名路由**为：
   
   ```json
   {path:'user/:userId',name:'user'}
   ```
   
   跳转后url:`localhost:8080/user/123`
   
   取参数`$route.params.userId`
   
   ```json
   {path:'register',query:{plan:'private'}}
   ```
   
   对应的路由为:
   
   ```json
   {path:'register'}
   ```
   
   跳转后url：`localhost:8080/register?plan=private`
   
   取参数:`$route.query.plan`

小结：

+ name对应params，路径形式user/123

+ path对应query，路径形式user?id=123

如果使用path进行页面跳转，写params传参会被忽略：

```js
// params 会被忽
router.push({ path: 'user', params: { userId: '123' }})
```

可以换成

```js
router.push({ path: 'user/123'})
```

也适用于router-link的to属性

#### 页面跳转如何获取参数

$route可以访问当前路由

对地址`http://localhost:8080/user/123?name=userName#abc`

```js
// $route
{
  // 路由名称
  name: "user",
  meta: {},
  // 路由path
  path: "/user/123",
  // 网页位置指定标识符
  hash: "#abc",
  // window.location.search
  query: {name: "userName"},
  // 路径参数 user/:userId
  params: {userId: "123"},
  fullPath: "/user/123?name=userName#abc"
}
```

就可以用`.`获取对应信息
