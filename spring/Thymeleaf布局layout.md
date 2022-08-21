#### Themeleaf布局(Layout)

大多数的网站都有导航，底部等公共的东西，在一个网站里访问页面总会显示相同的导航、底部之类的内容

就需要布局知识，不是指css布局，而是说网站的页面架构

layout解决的是模板复用的问题，比如常见的网站：

![](https://style.youkeda.com/img/ham/course/j4/book/layout.svg)

按照这个布局，我们可以把导航和底部做成布局组件，每个页面套用就行

推荐使用`th:include + th:replace`方案完成布局的开发

##### layout.html

继续完善bookstore，创建一个`layout.html`

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
    <title>布局</title>
    <style>
        .header {background-color: #f5f5f5;padding: 20px;}        .header a {padding: 0 20px;}        .container {padding: 20px;margin:20px auto;}        .footer {height: 40px;background-color: #f5f5f5;border-top: 1px solid #ddd;padding: 20px;}    </style>
</head>
<body>
<header class="header">
    <div>
        <a href="/book/list.html">图书管理</a>
        <a href="/user/list.html">用户管理</a>
    </div>
</header>
<div class="container" th:include="::content">页面正文内容</div>
<footer class="footer">
    <div>
        <p style="float: left">© youkeda.com 2017</p>
        <p style="float: right">
            Powered by 优课达
        </p>
    </div>
</footer>

</body>
```

添加了`header,container,footer`三个节点。在container这个节点上使用了`th:include="::content"`语法

##### `th:include="::content"`

::content指的是选择器，这个选择器指的就是加载当前页面的`th:fragment`的值

当页面渲染时，布局会合并content这个fragment内容一起渲染

##### user/list.html

让这个页面支持布局

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org"
      th:replace="layout">
<div th:fragment="content">
    <h2>用户列表</h2>
    <div>
        <a href="/user/add.html">添加用户</a>
    </div>
    <table>
        <thead>
        <tr>
            <th>
                用户名称
            </th>
            <th>
                用户年龄
            </th>
            <th>
                用户邮箱
            </th>
        </tr>
        </thead>
        <tbody>
        <tr th:each="user : ${users}">
            <td th:text="${user.name}"></td>
            <td th:text="${user.age}"></td>
            <td th:text="${user.email}"></td>
        </tr>
        </tbody>
    </table>
</div>

</html>
```

th:replace="layout"这里指定了布局的名称，一旦声明后，页面会替换成layout的内容，这个`"layout"`指的是`templates/layout.html`

th:fragment="content"

```html
<div th:fragment="content">
</div>
```

fragment是片段的意思，当页面渲染时，可以通过选择器指定使用这个片段，之前的`th:include="::content"`指定的就是这个值

##### api

api服务如果要提供参数，需要给参数添加注解
