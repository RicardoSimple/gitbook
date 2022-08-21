继续完善书籍的添加逻辑，在实际的工作中对于数据的保存是离不开数据验证的，比如name必须要输入，isbn必须输入等，可以借助Spring Validation来处理表单数据的验证

#### JSR 380

JSR是java Specofication Requests的缩写，意思是Java规范提案。是指向JCP提出新增一个标准化技术规范的正式请求。任何人都可以提交JSR，以向Java平台增添新的API和服务

基本上我们用到的很多Java框架都是某个JSR的实现者

JSR 380其实就是Bean Validation2.0这个就是Bean验证的规范，这里的Bean就是实例化后的POJO类

可以添加依赖来引入工程

```xml
<dependency>
  <groupId>jakarta.validation</groupId>
  <artifactId>jakarta.validation-api</artifactId>
  <version>2.0.1</version>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-validation</artifactId>
</dependency>
```

Spring Validation也是JSR 380提案的一个实现方案。由于工程基于Spring boot，所依赖的包都自动加入，不需要额外配置

##### Validation注解

JSR 380定义了一些注解用于做数据校验，这些注解可以直接设置在Bean属性上

+ `@NotNull`不允许外null对象

+ `@AssertTrue`是否为true

+ `@Size`约定字符串长度

+ `@Min`字符串的最小长度

+ `@Max`字符串的最大长度

+ `@Email`是否为邮箱格式

+ `@NotEmpty`不允许为null或者空，可以用于判断字符串、集合，比如Map，数组，List

+ `@NotBlank`不允许为null和空格

例子：

```java
package com.bookstore.model;

import javax.validation.constraints.*;

public class User {

    @NotEmpty(message = "名称不能为 null")
    private String name;

    @Min(value = 18, message = "你的年龄必须大于等于18岁")
    @Max(value = 150, message = "你的年龄必须小于等于150岁")
    private int age;

    @NotEmpty(message = "邮箱必须输入")
    @Email(message = "邮箱不正确")
    private String email;

    // standard setters and getters 
}
```

大多数情况下建议使用NotEmpty替代NotNull，NotBlank

校验的注解是可以累加的，系统会按顺序执行，任何一条检验触发就会抛出校验错误到上下文中

创建一个表单页`user/addUser.html`

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">

<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <meta http-equiv="X-UA-Compatible" content="ie=edge" />
  <title>添加用户</title>
</head>

<body>
  <h2>添加用户</h2>
  <form action="/user/save" method="POST">
    <div>
      <label>用户名称:</label>
      <input type="text" name="name">
    </div>
    <div>
      <label>年龄:</label>
      <input type="text" name="age">
    </div>
    <div>
      <label>邮箱:</label>
      <input type="text" name="email">
    </div>
    <div>
      <button type="submit">保存</button>
    </div>
  </form>
</body>

</html>
```

用于添加用户

在control类里设置mapping，再在html文件的form表单添加action路径之后完成此控制类执行校验

```java
package com.bookstore.control;

import com.bookstore.model.User;
import org.springframework.stereotype.Controller;
import org.springframework.validation.BindingResult;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;

import javax.validation.Valid;

@Controller
public class UserControl {

    @GetMapping("/user/add.html")
    public String addUser() {
        return "user/addUser";
    }

    @PostMapping("/user/save")
    public String saveUser(@Valid User user, BindingResult errors) {
        if (errors.hasErrors()) {
            // 如果校验不通过，返回用户编辑页面
            return "user/addUser";
        }
        // 校验通过，返回成功页面
        return "user/addUserSuccess";
    }

}
```

方法参数中，user添加了注解`@Valid`，且新增了第二个参数errors，类型为BindingResult

`@Valid`完整包路径为`javax.validation.Valid;`

BindingResult类的路径是`org.springframework.validation.BindingResult`

BindingResult对象的hasErrors方法可以用于判断校验成功还是失败，俄国失败回到添加用户页面，成功显示成功页面

##### redirect跳转页面

`redirect:`这个用于跳转到某一个页面网址，如果是同一个域名，可以省略域名，直接写path

```java
@PostMapping("/user/save")
public String saveUser(@Valid User user, BindingResult errors) {
    if (errors.hasErrors()) {
        // 如果校验不通过，返回用户编辑页面
        return "user/addUser";
    }
    userService.saveUser(user);
    // 校验通过，返回成功页面
    return "redirect:/user/list.html";
}
```

跳转到的list.html也要写一个getMapping方法

也可以跳转到某个网站，比如：

```html
return "redirect:https://www.baidu.com";
```

当我们返回的是redirect关键字后，浏览器会自动打开这个页面内容

##### 数据输入错误返回原因

![](https://style.youkeda.com/img/ham/course/j4/error.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

Thymeleaf中只要把错误结果返回到模板中即可

+ 如何传递数据

+ 如何显示具体的字段错误

###### Control改造

把数据传递到页面上。如果想显示具体的字段的信息，就需要结合模型来传输，比如之前的User

改造`UserControl.addUser`

```java
@GetMapping("/user/add.html")
public String addUser(Model model) {
    User user = new User();
    model.addAttribute("user",user);
    return "user/addUser";
}
```

##### user/add.html改造

在`user/add.html`模板中，我们得去处理错误的状态，增加错误的样式和文案

+ th:object
  
  为了**让表单验证生效**，还需要再`form`标签里添加：`th:object=${user}`属性
  
  `th:object`用于替换对象，使用了就不需要每次都编写`user.xxx`直接操作`xxx`
  
  完整：
  
  ```html
  <form action="/user/save" th:object="${user}" method="POST">
    ...
  </form>
  ```

+ th:classappend
  
  关于错误提示这个信息，还需要再分解，如果想显示错误的状态，就要定义一个错误的css class比如
  
  ```css
  .error {
    color: red;
  }
  ```
  
  现在就需要动态管理表单的样式，如果有错误就在该

       标签添加这个class，可以使用`th:classappend`

```html
<div th:classappend="${#fields.hasErrors('name')} ? 'error' : ''">
</div
```

如果错误信息有name就会生成

```html
<div class="error">
</div>
```

`${#fields.hasErrors('key')}`语法是专门验证场景提供的，这里的kry就是对象的属性名称，比如name，age等

+ th:errors
  
  如果还想显示出错误信息，可以使用`th:errors="*{age}"`属性，会自动取出错误信息
  
  ```html
  <p th:if="${#fields.hasErrors('age')}" th:errors="*{age}"></p>
  这个*由于在form中使用了th:object="${user}"
  //所以就可以通过*{age}来获取具体属性值
  ```

+ th:field
  
  一般错误的时候，还希望显示上一次输入的内容，所以可以使用th:field
  
  ```html
  <div th:classappend="${#fields.hasErrors('age')} ? 'error' : ''">
    <label>年龄:</label>
    <input type="text" th:field="*{age}" />
    <p th:if="${#fields.hasErrors('age')}" th:errors="*{age}"></p>
  </div>
  ```

**验证错误一定要在参数加`@Valid`注解！！**
