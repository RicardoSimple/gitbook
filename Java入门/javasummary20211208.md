# java第三章总结

## lambda语句

lambda表达式，也叫闭包，lambda允许把函数作为一个方法的参数
click->{代码}

## 逻辑语句

if/else语句，同C一样

## 自定义方法

和函数差不多

## 注解

@Route("/.....")
相当于页面

## vaadin框架button组件

### button完整类路径

com.vaadin.flow.component.button.Button

### 创建button

new Button().var
Button ButtonName = new Button();

### 按钮设置文字

ButtonName.setText("string")
或者 Button ButtonName = new Button("string");

## 提示框

### 先要创建提示框

new Notification().var

### 提示框内容

notificationName.setText("string");

### 提示框持续时间

notificationName.setDuration(3000);
这是持续**三秒**

### 提示框输出

notificationName.open();
