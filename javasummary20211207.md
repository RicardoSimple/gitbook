# Java 总结
java编译为字节码，不直接编译为机器语言
## 包概念
（就是多重文件夹）
## String  
stringname = new String（"string"）;
## 语句结尾为;
与c一样
##  Double
实型
## Integer 
整型
## 输入框组件 
TextField  
### TextField组件完整类路径 
com.vaadin.flow.component.textfield.TextField
### 水平类路径 
com.vaadin.flow.component.orderedlayout.HorizontalLayout
### 垂直类路径 
com.vaadin.flow.component.orderedlayout.VerticalLayout
### 实例化TextField  
TextField field = new TextField();
### add方法 
添加任意个子组件 add(.........)
### 标题 
fieldname.setLabel   
### 提示
fieldname.setPlaceholder 
### 默认值
fieldname.setValue
