#### 重定向和别名

之前的首页`/layout/home`，但是一般官网首页都是`/`

使用别名

##### 别名

```js
const routes: [
  // 定义 alias 属性
  { path: '/a', alias: '/b', component: A }
];
```

当访问`/a`渲染的是A，访问`/b`时就像访问`/a`一样渲染的也是A

这样我们先去掉layout

```js
const routes: [
  {
    path: '/layout',
    // 别名定为 /
    alias: '/',
    component: ()=> import('@/pages/nest/Layout.vue'),
    children: [
      { path: 'home', component: Home },
      { path: 'courseall', component: CourseAll },
      { path: 'coursedetail/:courseId', component: CourseDetail }
    ]
  }
];
```

这样`/layout/home`变成`/home`

接下来重定向把`/home`变成`/`

##### 路由重定向

重定向：

路由`/a`重定向到`/b`，即访问`/a`时，url自动跳转到`/b`然后匹配路由`/b`：

```js
const routes: [
  // 定义 redirect 属性，将 /a 重定向到 /b
  { path: '/a', redirect: '/b' }
]
```

现在将`/`重定向到`/home`这样访问`/`会自动跳转到`/home`

```js
const routes: [
  {
    path: '/layout',
    alias: '/',
    // 重定向到 /home
    redirect: '/home',
    component: ()=> import('@/pages/nest/Layout.vue'),
    children: [
      { path: 'home', component: Home },
      { path: 'courseall', component: CourseAll },
      { path: 'coursedetail/:courseId', component: CourseDetail }
    ]
  }
];
```

现在的路由：

![](C:\Users\ricardo\AppData\Roaming\marktext\images\2022-08-30-15-42-25-image.png)

#### 监听路由

有时需要对路由的变化进行监听，比如页面标签切换，路径发生变化，需要监听路由变化来改变标签的样式或者在页面中加载对应的内容

![](https://document.youkeda.com/P3-5-Vue/6/13.gif)

##### 监听路由`$route`的变化

需要vue中的watch和`$route`

写法：

```js
watch: {
  $route(to,from){
    console.log(to, from);
  }
}

// 或者
watch: {
  $route: {
    handler: function(to,from) {
      console.log(to,from);
    },
    // 深度观察监听
    deep: true
  }
},
```

to是变化后的路由，from是变化前的路由

##### 监听路由的使用

根据**路径参数**定位点击的标签

1. 基础样式
   
   tab的样式分为被点击和未被点击两种
   
   ![](https://document.youkeda.com/P3-5-Vue/6/14.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)
   
   被点击的类名会多个active

2. 定义tab点击事件
   
   ```js
   <script>
   export default {
     methods: {
       // 点击 tab 时会执行 changeTab 方法
       changeTab(type) {
         // 使用 Router 实例方法改变路径参数
         this.$router.push({ query: { type: type } });
       }
     }
   };
   </script>
   ```

3. 监听路由变化，更新tab样式
   
   ```js
   <script>
   export default {
     watch: {
       $route(to, from) {
         // 路由变化了就执行更新样式的方法
         this.updateTab();
         console.log(to, from);
       }
     },
     methods: {
       changeTab(type) {
         this.$router.push({ query: { type: type } });
       },
       // 更新样式的方法
       updateTab() {
         this.tabList.map(menu => {
           menu.active = menu.type === this.$route.query.type;
         });
       }
     }
   };
   </script>
   ```

4. 处理特殊情况
   
   如果页面一开始没有路径参数type，默认选第一个tab，如果一开始就有type那么需要马上更新tab样式
   
   ```js
   <script>
   export default {
     methods: {
       // ...
       updateTab() {
         if (!this.$route.query.type) {
           return;
         }
         this.tabList.map(tab => {
           tab.active = tab.type === this.$route.query.type;
         })
       }
     }
   };
   </script>
   ```

#### 网络请求async和await

js中有用fetch请求数据，比如

`https://www.fastmock.site/mock/b73a1b9229212a9a3749e046b1e70285/f4/f4-11-1-1`

有：

```js
fetch(
  'https://www.fastmock.site/mock/b73a1b9229212a9a3749e046b1e70285/f4/f4-11-1-1'
)
  .then(function(response) {
    return response.json();
  })
  .then(function(myJson) {
    console.log(myJson);
  });
```

但是由于fetch返回的是一个promise对象，特点无等待，所以相对返回的数据进行操作就必须在`then`里操作，`then`链就会很长

##### 异步async

async是异步的简写，用于申明一个异步function，而这个async函数返回的是一个promise对象

```js
async function asyncFn() {
  return {
    "company": "优课达",
    "slogan": "学的比别人好一点"
  };
}

const result = asyncFn();
console.log(result);
```

##### 等待异步await

用于等待一个异步方法执行的完成，会阻塞后面的代码，等着promise对象resolve，然后得到resolve的值，作为await表达式的运算结果

```js
async function getAsyncFn() {
  const result = await asyncFn();
  console.log(result);
}

getAsyncFn();
```

await只能出现在async函数中，普通函数中会报错

##### 多个请求并发执行Promise.all

```js
async function asyncFn1() {
  return "优课达";
}

async function asyncFn2() {
  return "学的比别人好一点";
}

async function getAsyncFn() {
  const result = await Promise.all([asyncFn1(), asyncFn2()]);
  console.log(result);
}

getAsyncFn();
```

 返回这样异步函数返回的值所组成的数组

##### vue中运用async和await

mock的数据返回格式：其中data是我们需要的数据

```json
{
  data: {...},
  isSuccess: true
}
```

比如获取全部课程

```js
<script>
export default {
  data: function() {
    return {
      courseList: []
    };
  },
  async mounted() {
    // 在生命周期 mounted 中调用获取课程信息的方法
    await this.queryAllCourse();
  },
  methods: {
    // 在 methods 对象中定义一个 async 异步函数
    async queryAllCourse() {
      // 在 fetch 中传入接口地址
      const res = await fetch('https://www.fastmock.site/mock/2c5613db3f13a5c02f552c9bb7e6620b/f5/api/queryallcourse');
      // 将文本体解析为 JSON 格式的promise对象
      const myJson = await res.json();
      // 获取返回数据中的 data 赋值给 courseList
      this.courseList = myJson.data;
    }
  }
}
</script>
```

##### 给api传参数

比如根据课程id获取课程信息，需要给api传课程id

```js
<script>
export default {
  data: function() {
    return {
      course: []
    };
  },
  async mounted() {
    await this.getCourse();
  },
  methods: {
    async getCourse() {
      // 从路径中获取课程 id
      const courseId = this.$route.params.courseId
      // 在接口地址后传入参数 id
      const res = await fetch('https://www.fastmock.site/mock/2c5613db3f13a5c02f552c9bb7e6620b/f5/api/getcourse?id=' + courseId);
      const myJson = await res.json();
      this.course = myJson.data;
    }
  }
}
</script>
```
