### 计算属性

计算属性是继`data`,`methods`,`watch`之后的新成员

#### 书写位置

```html
<script>
  export default {    
    name: 'app',    
    // 计算属性
    computed: {},  
    };
</script>
```

#### 写法

属性内部是由一系列方法组成的键值对

比如

```html
<script>
  export default {      
        name: "app",      
        data:function(){          
            return {              
                message:"优课达--学的比别人好一点～"
                    } 
         }      // 计算属性
        computed:{          
            reverseMessage:function(){              
            return this.message.split('').reverse().join('') 
                 }     
         }  
    };
</script>
```

使用时不需要加小括号

#### 计算属性和方法的区别

使用methods也可以实现，如下

```html
<div id="app">
  <p>原字符串：{{ message }}</p>
  <p>反转后的字符串：{{ reverseMessage() }}</p>
</div>
```

```html
<script>
  export default {
    name: 'app',
    data: function () {
      return {
        message: '优课达--学的比别人好一点～',
      };
    },
    methods: {
      reverseMessage: function () {
        return this.message.split('').reverse().join('');
      },
    },
  };
</script>
```

要加括号

计算属性的两个性质：依赖和缓存

依赖：message就是计算属性的依赖

缓存：上一次计算得到的值

计算属性：

当message发生变化时，reverseMessage计算属性会重新计算，然后返回还会结果，

message不变计算属性会返回缓存的值，而不会重新计算

方法：

每次访问都会执行方法体的逻辑然后返回结果

计算属性性能更优

#### js filter()函数

filter函数是对数组进行过滤，它创建一个新数组。新数组中元素是通过检查指定数组中符合条件的所有元素，不会检测空数组，不会改变原数组

语法：

```js
array.filter(function(currentValue,index,arr),thisValue)
```

[前端开发之JS中filter()的使用 (baidu.com)](https://baijiahao.baidu.com/s?id=1719633936157373069&wfr=spider&for=pc)

```js
if (this.flag === 0) {
        return this.bookList;
      } else if (this.flag === 1) {
        return this.bookList.filter((item) => {
          return item.isRead === true;
        });
      } else if (this.flag === 2) {
        return this.bookList.filter((item) => {
          return item.isRead === false;
        });
      } else {
        return [];
      }
```

另外：checkbox的双向绑定v-model是对应的boolean值 

### 动态class

动态样式绑定就是根据条件去给某个标签添加样式

#### 动态样式绑定

语法：

```html
<div class="base" v-bind:class="{ active: isActive }"></div>
```

```css
.active {
  border: 1px solid green;
}
```

`active`是类名，对应css中的类名，`isActive`是`boolean`类型的值，决定是否应用该类名

##### 类名的书写

普通类名(没有-)就直接写

如果有连字符比如`base-active`就需要用引号双引号或单引号

```html
<div v-bind:class="{ 'base-active': isActive }"></div>
```

##### 引号规则

```html
<!-- 外双内单 -->
<div v-bind:class="{ 'base-active': isActive }"></div>
<!-- 外单内双 -->
<div v-bind:class='{ "base-active": isActive }'></div>
```

不可同时存在

##### 多类名写法

可以绑定多个类名

```html
<div v-bind:class='{ "base-active": isActive,"base":true}'></div>
```

添加对象就可以了

#### 动态样式绑定的条件类型

1. 变量形式
   
   在data中定义变量来表示false/true
   
   最后通过用户事件来改变布尔值

2. 方法形式
   
   ```js
   isActive: function(type) {
       if (type === 'NEW') {
           return true;
       }
       return false;
   }
   ```
   
   ```html
   <span :class="{ 'new-appear': isActive(item.type) }">新</span>
   ```

3. 表达式形式
   
   ```html
   <span :class="{ 'new-appear': item.type === 'NEW' }">新</span>
   ```

4. 计算属性形式
   
   ```js
   computed: {
     hoverObj: function () {
       return {
         hover: this.index === 1,
       };
     },
   },
   ```
   
   将样式对象以及判断条件放在计算属性内
   
   标签中有`class="hoverObj`"
   
   ![](C:\Users\ricardo\AppData\Roaming\marktext\images\2022-07-26-20-27-45-image.png)

#### 动态样式绑定的写法

两种：对象写法，数组写法

1. 对象写法
   
   可以写单个属性
   
   ```html
   <div v-bind:class="{ active: isActive }"></div>
   ```
   
   也可以写多个属性
   
   ```html
   <div v-bind:class="{ active: isActive ,hover:true,'after-hover':false}"></div>
   ```

2. 数组写法
   
   + 常规写法
     
     类名带引号
     
     ```html
     <div v-bind:class="['red-style', 'font-style']"></div>
     ```
   
   + 数组中使用三元表达式
     
     ```html
     <div v-bind:class="['red-style', 'font-style',isChoosed ? 'redbg' : '']"></div>
     ```
     
     也可以使用三元表达式实现两个类名的二选一
     
     ```html
     <div
       v-bind:class="['red-style', 'font-style',isChoosed ? 'redbg' : 'bluebg']"
     ></div>
     ```
   
   + 在数组中使用对象
     
     可以将三元表达式改写成数组中使用对象的写法
     
     ```html
     <div v-bind:class="['red-style', 'font-style',{'redbg':isChoosed}]"></div>
     ```

列表内多个元素添加独立hover可以结合index

### 动态style

行内样式`:style`动态style和动态class一样也分两种

对象语法和数组语法

1. 对象语法
   
   不同的是键值对是css样式的属性:值组成的一对键值对
   
   ```html
   <div :style="{background:'red','font-weight':700,'font-size':'20px'}"></div>
   ```
   
   + 引号的使用，属性如果是单个单词就不用引号，有连字符就需要引号；值除了数字其他都要引号，特殊的比如`1px solid black`就可以用模板字符串
   
   + font-weight写成fontWeight就不用引号
   
   + 改变动态样式的书写位置，写成
     
     ```html
     <div
       :style="{display:'flex',flexDirection:'column',justifyContent:'space-between',alignItems:'center',flexWrap:'no-wrap'}"
     ></div>
     ```
     
     不简洁，可以提取动态样式，再引入动态样式
     
     ```html
     <div :style="flexStyle"></div>
     ```
     
     ```js
     data:function(){
         return {
             flexStyle: {
                 display: 'flex',
                 flexDirection: 'column',
                 justifyContent: 'space-between',
                 alignItems: 'center',
                 flexWrap: 'no-wrap',
             },
         }
     }
     ```
     
     特殊情况不能提取(动态样式中有参数，比如可变的imgurl)，比如
     
     ```js
     data:function(){
         return {
             bg: {
                 background: `url(${imgUrls[currentIndex]}) no-repeat center / contain`,
             },
         }
     }
     ```
     
     就会报错(全局变量不能相互引用)，只能再标签上写动态样式

2. 数组语法
   
   :style数组里面是变量，不用引号
   
   常规写法：
   
   ```js
   data() {
     return {
       fontStyle: { color: 'red', fontSize: '33px' },
       boxStyle:{width: '200px', height: '200px', border: `1px solid black`}
     };
   },
   ```
   
   引入：
   
   ```html
   <div :style="[fontStyle,boxStyle]">这里是文字</div>
   ```
   
   在数组中使用三元表达式
   
   ```html
   <div :style="[fontStyle, isActive ? boxStyle : colorBox]">这里是文字</div>
   ```
