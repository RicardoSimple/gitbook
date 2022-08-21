##### 集合里删除数据需要使用迭代器

```java
Iterator<Book> itr = books.iterator();
while (itr.hasNext()) {
    Book book = itr.next();
    if (book.getId() == bookId) {
        itr.remove();
    }
}
```

a标签超链接有默认的打开链接行为，所以对于想要执行onclick时，一般设置为`href="javascript:;"`

```html
<table>
  <thead>
    <tr>
      ...
      <th>操作</th>
    </tr>
  </thead>
  <tbody>
    <tr th:each="book : ${books}">
      ...
      <td>
        <a href="javascript:;" th:onclick="delBook([[${book.id}]])">删除</a>
      </td>
    </tr>
  </tbody>
</table>
```

编译为：

```html
<table>
  <thead>
    <tr>
      ...
      <th>操作</th>
    </tr>
  </thead>
  <tbody>
    <tr th:each="book : ${books}">
      ...
      <td><a href="javascript:;" onclick="delBook(1)">删除</a></td>
    </tr>
  </tbody>
</table>
```

##### JS confirm函数

confirm();确认消息对话框。用于允许用户做选择的动作。弹出的对话框包括确认和取消按钮

confirm(str);参数说明

str:确认对话框中用于显示的文本内容。

返回值

当用户点击确认按钮，返回true

当用户点击取消按钮，返回false

![](https://img-blog.csdn.net/20160415103226404?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

##### tr标签

代表html表格中的一行

##### td标签

代表html表格中的一个单元格

##### th标签

是一个简单的html表格表头

定义表格内的表头单元格。

HTML 表单中有两种类型的单元格：

- 表头单元格 - 包含表头信息（由 th 元素创建）
- 标准单元格 - 包含数据（由 td 元素创建）

th 元素内部的文本通常会呈现为居中的粗体文本，而 td 元素内的文本通常是左对齐的普通文本。
