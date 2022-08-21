### Vue介绍

vue是用于构建用户界面的渐进式框架，主要特点是易用，灵活，性能优

vue是响应式的

#### 配置

##### 安装node.js

安装node.js是为了npm

[Node.js](https://nodejs.org/en/)

检查是否安装成功：`node -v`

检测npm：`npm -v`

##### 安装Vue CLI脚手架

Vue CLI简单说就是Vue工程版的升级版

```html
<div id="app"></div>
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
  new Vue({    el: '#app',    data: {},  });
</script>
```

这个代码也是Vue，但是不适合做大型项目

安装Vue CLI的命令：

```shell
npm install -g @vue/cli
// 如果是mac电脑，需要在命令前面加sudo
sudo npm install -g @vue/cli
```

如果很久都安装不好就切换镜像

```shell
sudo npm install -g cnpm --registry=https://registry.npm.taobao.org
```

验证脚手架是否安装成功：

```shell
vue --version
```

#### 创建Vue工程

执行命令创建一个vue工程

```shell
// vue 创建 工程名
vue create vue_first
```

出现选项后选最后一个自定义配置，键盘上下

![](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-5-Vue/1/img/%E5%88%9B%E5%BB%BAvue1.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

然后勾选Babel，Router即可（空格选）

![](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-5-Vue/1/img/%E5%88%9B%E5%BB%BAvue2.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

下一步询问是否使用历史模式的路由器，可根据自己的需要

![](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-5-Vue/1/img/%E5%88%9B%E5%BB%BAvue3.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

下一步询问你要将Babel等配置文件放在哪里，我们选择放在package.json里

![](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-5-Vue/1/img/%E5%88%9B%E5%BB%BAvue4.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

最后询问是否保存这次配置

![](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-5-Vue/1/img/%E5%88%9B%E5%BB%BAvue5.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

最后创建好之后脚手架的命令:

`cd vue_first`进入工程根目录

`npm run serve`启动vue工程

安装了yarn还会提示用yarn

![](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-5-Vue/1/img/%E5%88%9B%E5%BB%BAvue6.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

执行之后脚手架显示地址

- `http://localhost:8081/`或者自己的ip

打开之后就是vue的初始界面

### Vue工程目录

![](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-5-Vue/1/img/src-explain.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

assets存放项目需要用到的资源文件，css，js，imgs等

components存放vue开发的一些公共组件，例如项目初始的header.vue,footer.vue

router是vue路由的配置文件

views存放页面文件

app.vue是跟组件

main.js是项目的入口文件，定义了vue实例，并引入跟组件app.vue，将其挂载到index.html中id为app的节点上

### 声明式渲染

#### Vue代码结构

`app.vue`是工程的根组件

![](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-5-Vue/2/appvue.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

template是模板，每个vue文件都要有，在这里写html文件

```html
// template即模版的意思，每一个vue文件里必须要有一个，在这里写HTML代码
<template>
  <div id="app"></div>
</template>

// 在这里写js逻辑相关的代码
<script>
  export default {    name: "app"
  };
</script>

// 这里写样式代码
<style></style>
```

每个vue文对应三个部分：template，script，style分别对应html，js，css

template里面只允许有一个块状元素，通常情况下template里面写的都是div

#### 声明式渲染

差值表达式只用一个双层的大括号`{{}}`

##### 差值表达式渲染--字符串

```html
<template>
  <h2>{{title}}</h2>
</template>
```

script里：

```html
<script>
  // export default是固定格式，不需要纠结
  export default {    // 模块的名字
    name: "app",    // 页面中数据存放的地方
    data() {
      return {        
        title: "优课达--学的比别人好一点"
             };    
            }  
    };
</script>
```

##### 差值表达式--数组

```html
<h2 class="title">{{title}}</h2>
<ul class="list">
  <li>{{todoList[0]}}</li>
  <li>{{todoList[1]}}</li>
  <li>{{todoList[2]}}</li>
  <li>{{todoList[3]}}</li>
  <li>{{todoList[4]}}</li>
</ul>
```

（li标签可以用循环写）

script：

```js
<script>
export default {
  name: "app",
  data() {
    return {
      title: "今日待完成事项",
      todoList: [
        "完成HTML标签学习",
        "完成CSS文字样式学习",
        "完成CSS盒模型学习",
        "完成Flex布局学习",
        "完成JavaScript入门学习"
      ]
    };
  }
};
</script>
```

data中变量与变量之间用`,`隔开

style中sass语法需要加上`lang="scss"`

```html
<style lang="scss" scope>
  .title {    box-sizing: border-box;    width: 300px;    height: 30px;    margin: 0;    padding: 0 20px;    font-size: 18px;    font-weight: 700;    line-height: 30px;    color: white;    background: #fd6821;    border-radius: 6px;  }  .list {    list-style: none;    margin: 0;    padding: 0;    margin-top: 15px;    li {      box-sizing: border-box;      width: 300px;      height: 30px;      padding: 0 20px;      margin-bottom: 8px;      font-size: 14px;      line-height: 30px;      background: #8d999d;      color: white;      border-radius: 5px;    }  }
</style>
```

##### data,scope

data方法里用来存放数据或者全局变量

scope可以理解为锁，将css代码锁在本文件，只在本文件有用

##### 差值表达式--对象

```js
<script>
export default {
  name: "app",
  data(){
      return {
          list:[
              {
                  name:"张三",
                  grade:"三年级二班",
                  mark:290
              },
              {
                  name:"李四",
                  grade:"三年级二班",
                  mark:270
              },
              {
                  name:"王五",
                  grade:"三年级二班",
                  mark:270
              }
          ]
      }
  }
};
</script>
```
