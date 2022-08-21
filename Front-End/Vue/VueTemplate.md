### HTML属性渲染语法

让属性的值根据data中定义的变量的值的变化而变化，就要用属性绑定

#### 动态绑定 v-bind

比如img中的alt

```html
<template>
  <div id="app">
    <img src="#" v-bind:alt="imgText" />
  </div>
</template>
```

```js
<script>
export default {
    name: "app",
    // 数据
    data() {
        return {
            imgText:'周杰伦演唱会图片'
        };
    }
};
</script>
```

#### 动态渲染图片案例

```html
<template>
  <div id="app">
    <div class="album">
      <img :src="imgList[0]" />
      <img :src="imgList[1]" />
      <img :src="imgList[2]" />
      <img :src="imgList[3]" />
    </div>
  </div>
</template>
```

```html
<script>
  export default {      
    name: "app",      
    data() {          
        return {              
            imgList:[                  
                'http://pic2.zhimg.com/50/v2-4a06728efc99ba874a5d7422fd55aaed_hd.jpg',                  
                'http://img2.imgtn.bdimg.com/it/u=372764256,3394765004&fm=26&gp=0.jpg',                  
                'http://img1.imgtn.bdimg.com/it/u=1898582417,1582081952&fm=26&gp=0.jpg',                  
                'http://b-ssl.duitang.com/uploads/item/201707/10/20170710141316_vVFNh.thumb.700_0.jpeg'
                  ]          
                };      
        };  
      }
</script>
```

没有用`v-bind:src`而是用`:src`是因为这是简写模式

### 模板中使用表达式

模板中计算

差值表达式可以进行简单计算

```html
<template>
  <div id="app">
    <ul>
      <li>{{ goods[0].index + 1 }}---{{ goods[0].name }}</li>
      <li>{{ goods[1].index + 1 }}---{{ goods[1].name }}</li>
      <li>{{ goods[2].index + 1 }}---{{ goods[2].name }}</li>
    </ul>
  </div>
</template>
```

模板中使用三元表达式

```html
<template>
  <div id="app">
    <p>{{ flag?'你已经通过了考试':'你还没有通过考试' }}</p>
    <button @click="exchange">转换</button>
  </div>
</template>
```

模板中可以写方法，js内置的方法

```html
<template>
  <div id="app">
    <p>{{ message.split('').reverse().join('') }}</p>
  </div>
</template>
```

注意：不要在模板中滥用方法，不然会变得臃肿

### 条件渲染语句

##### v-if

当条件满足时，会显示标签内的内容，比如

```html
<p v-if="isShow">{{ message }}</p>
```

```js
<script>
export default {
    name: "app",
    // 数据
    data() {
        return {
            message:"当条件满足的时候，显示这里的内容"
        };
    },
    methods:{
        isShow(){
            if(!this.message) 
                return false;
            return true;
        }
    }
};
</script>
```

##### v-else

就是不符合if的时候

```html
<p v-if="isShow">{{ message }}</p>
<p v-else>{{ defaultMessage }}</p>
```

##### v-else-if

多个条件时使用

```html
<p v-if="questions[0].type==='PICK'">{{ questions[0].content }}</p>
<p v-else-if="questions[1].type==='MULT'">{{ questions[1].content }}</p>
<p v-else>题目还没有加载进来...</p>
```

##### v-show

用法和v-if一样，条件满足时会显示，但是区别：

+ v-show指令只是将标签的display属性设置为none

+ v-if不满足条件时此标签在dom中根本不存在

#### 列表渲染语句

##### 循环渲染数字

v-for

v-for原理和for循环相似

```html
<div id="app">
    <ul>
        <li v-for="item in 5" :key="item">{{ item }}</li>
    </ul>
</div>
```

会从1开始循环

##### 循环渲染数组

```html
<ul>
    <li v-for="(item,index) in nameList" :key="index">{{ item }}</li>
</ul>
```

```js
<script>
export default {
    name: "app",
    // 数据
    data() {
        return {
            nameList:["张淮森","周逸依","梁澄静","宁古薄","丘约靖"]
        };
    }
};
</script>
```

这里的index表示序号，从0开始

##### 循环渲染对象

会从对象里面循环取出里面的值

遍历对象时，括号内三个参数

```html
<!--     
value：对象中每一项的值    
key：对象中每一项的键    
index：索引 
-->
<li v-for="(value,key,index)" :key="index"></li>
```

大部分情况下只会用到value

##### 遍历数组中的对象

大多数情况下都是遍历数组中的对象

```html
<ul>
    <li v-for="(item,index) in books" :key="index">
        {{ index+1 }}----{{ item.title }}----{{ item.author }}----{{ item.publishedTime }}
    </li>
</ul>
```

```js
<script>
export default {
    name: "app",
    // 数据
    data() {
        return {
            books: [
                {
                    title: '《魔戒》',
                    author: '约翰·罗纳德·瑞尔·托尔金',
                    publishedTime: '1954'
                },{
                    title:'《哈利·波特》',
                    author:'J·K·罗琳',
                    publishedTime:'1997'
                },{
                    title:'《人性的弱点》',
                    author:'戴尔•卡内基',
                    publishedTime:'2008'
                }
            ]
        };
    }
};
</script>
```

直接使用`.`来访问对象属性

#### :key

在vue CLI项目中为了保证每一个item唯一所以需要一个唯一的key，否则会报错

key的值一般都是后台取到数据的id，主要是为了保证每一个节点都有唯一的key值

```html
<li v-for="(item,index) in books" :key="index">
```
