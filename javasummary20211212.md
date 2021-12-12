# java第五章总结
## 异常
使用try/catch 环绕，后面的代码不受影响
## 文件读
### 配置Apache commons-io
在pom.xml 的 <dependencies>节点
<dependency>
  <groupId>commons-io</groupId>
  <artifactId>commons-io</artifactId>
  <version>2.10.0</version>
</dependency>
### 文件的对象 File (java.io) 实例化
File filename = new File("文件路径");文件路径为相对路径，.表示项目下为根目录
也可写为File filename = new File("父路径","子路径");
### FileUtils 是Apache commons-io提供的
FileUtils.readFileToString(文件名,"utf-8");
//要用try/catch环绕，但是里面的语句块是独立的，接受的变量是String类型，若定义在外部需要初始化为null
### 逐行读文件
