#### Thymeleaf表单

简易图书管理系统

图书模型

```java
public class Book{
  // 主键
  private long id;
  // 图书的名称
  private String name;
  // 图书的作者
  private String author;
  // 图书的描述
  private String desc;
  // 图书的编号
  private String isbn;
  // 图书的价格
  private double price;
  // 图书的封面图片
  private String pictureUrl;
  // 省略 getter、setter
}
```

Book类中把id主键的类型设置为long，这是因为long类型的id更易于搜索引擎。如果期望被搜索引擎能够关注到产品，那么就可以把id设置为long，否则还是用String，因为long很容易被机器猜到，所以很容易被爬取数据

页面开发

创建addBook.html的thymeleaf模板

```html
<form>
  <div>
    <label>书的名称:</label>
    <input type="text" />
  </div>
  <div>
    <label>书的作者:</label>
    <input type="text" />
  </div>
  <div>
    <label>书的描述:</label>
    <textarea></textarea>
  </div>
  <div>
    <label>书的编号:</label>
    <input type="text" />
  </div>
  <div>
    <label>书的价格:</label>
    <input type="text" />
  </div>
  <div>
    <label>书的封面:</label>
    <input type="text" />
  </div>
  <div>
    <button type="submit">注册</button>
  </div>
</form>
```

配置Spring Control

```java
package com.bookstore.control;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.*;

@Controller
public class BookControl {
  // 当页面访问 http://localhost:8080/book/add.html 时
  // 渲染 addBook.html 模板
  @GetMapping("/book/add.html")
  public String addBookHtml(Model model){
    return "addBook";
  }
}
```

保存书籍

新增一个control来处理书籍保存的逻辑

```java
package com.bookstore.control;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.*;

import java.util.*;

import com.bookstore.model.*;

@Controller
public class BookControl {
  //缓存所有书籍数据
  private static List<Book> books = new ArrayList<>();

  @GetMapping("/book/add.html")
  public String addBookHtml(Model model){
    return "addBook";
  }

  @PostMapping("/book/save")
  public String saveBook(Book book){
    books.add(book);
    return "saveBookSuccess";
  }

}
```

这里@PostMapping和@GetMapping不同点在于只接收http method为post请求的数据，包路径和GetMapping一样

新增templates/saveBookSuccess.html文件

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">

<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <meta http-equiv="X-UA-Compatible" content="ie=edge" />
  <title>添加书籍</title>
</head>

<body>
  <h2>添加书籍成功</h2>
</body>

</html>
```

form表单

还需要修改html form，**需要指定form的action属性值就是后端的请求路径**，由于我们写的是`/`开头，浏览器会自动把请求地址识别为`http://domain/user/reg`,如果本地开发这个domain可能就是`localhost:8080`

    一般情况我们会把html表单的method设置为post，这样可以保证数据传输安全，这样Spring MVC就需要接收Post请求

除了form属性调整，还需要修改input的name属性，属性和Book类的属性名要一致

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">

<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <meta http-equiv="X-UA-Compatible" content="ie=edge" />
  <title>添加书籍</title>
</head>

<body>
  <h2>添加书籍</h2>
//action的路径和PostMapping内的参数一样
  <form action="/book/save" method="POST">
    <div>
      <label>书的名称:</label>
      <input type="text" name="name">
    </div>
    <div>
      <label>书的作者:</label>
      <input type="text" name="author">
    </div>
    <div>
      <label>书的描述:</label>
      <textarea name="desc"></textarea>
    </div>
    <div>
      <label>书的编号:</label>
      <input type="text" name="isbn">
    </div>
    <div>
      <label>书的价格:</label>
      <input type="text" name="price">
    </div>
    <div>
      <label>书的封面:</label>
      <input type="text" name="pictureUrl">
    </div>
    <div>
      <button type="submit">注册</button>
    </div>
  </form>
</body>

</html>
```
