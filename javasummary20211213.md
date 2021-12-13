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
## 

