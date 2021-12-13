# java第六章总结
## 登录界面
### vaddin登录框架
 LoginForm(对象)
### 上下左右居中布局
setJustifyContentMode(JustifyContentMode.CENTER);
        setAlignItems(Alignment.CENTER);
### 忘记密码按键修改
 +   loginForm.setForgotPasswordButtonVisible(false);
+ 没有忘记密码按键
### 添加大标题
 + H1 h1 = new H1("TODO");
 + 括号里面是标题内容，有时要导入类
### 注册提示
HorizontalLayout regLayout = new HorizontalLayout();
        Label tips = new Label("Don't have an account?");
        Button regbtn = new Button("sign up here.");
        regLayout.add(tips,regbtn);
        + 可以嵌套add
+ 对齐label和button text
+ regLayout.setAlignItems(Alignment.CENTER);
#### 按键样式修改
regbtn.setThemeName("tertiary");
+ 链接样式纯文本
## 跳转页面
+ 给button增加事件，
+ addButtonClickListener的对象是ClickEvent类型新建的方法要添加这个参数
+   regBtn.addClickListener(buttonclickevent -> {onsign(buttonclickevent );});
+   regBtn.addClickListener(this::onsign);
+ 跳转
layout中有getui的方法，是option对象，要先判断是否存在，用ifpresent()
+ getUI().ifPresent(ui ->{ui.navigate("/reg.html");});
## login判断
+ 给login增加事件
loginForm.addLoginListener(this::onlogin);
+ 得到用户名和密码
String userName = event.getUsername();
        String passWord = event.getPassword();
+ 遍历用户名和密码
 for (User user : Reg.users) {
            if (user.getUserName().equals(userName) && user.getPassword().equals(passWord) ){
                Notification notification = new Notification();
                notification.setDuration(3000);
                notification.setText("login success");
            }
        }
+ 发送错误（登录失败）
  loginForm.setError(true);
## 解决登录状态的持久处理
+ cookie创建
一个很短的数据存储，用它来判断登录持久化
Cookie cookie = new Cookie("Username",username);
    前面的参数是数据名称，后面是数据
    




