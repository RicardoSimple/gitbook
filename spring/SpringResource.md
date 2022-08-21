### Spring Resource

文件系统是编程不可避开的领域，因为我们总是有可能读文件，写文件

Spring Framework作为完整的java企业解决方案，文件处理系统就是Spring Resource

#### java工程中文件的几种情况

1. 文件在电脑某个位置，比如说`d:/mywork/a.doc`

2. 文件在工程目录下，比如说`mywork/toutiao.png`

3. 文件在工程中的`src/main/resources`中，这是Maven工程存放文件的位置

第一种和第二种情况都可以使用File对象读写，第三种情况比较特殊，因为Maven执行package时会把resources目录下的文件一起打包进jar文件中

显然在第三种情况中用File对象是读取不到的，因为文件已经在jar文件中了

#### `Classpath`

在java内部中，一般把文件路径称为classpath，所以读取内部的文件就是从classpath内读取，classpath指定的文件不能解析成File对象，但是可以解析成InputStream，借助JavaIO就可以读取出来了

classpath类似虚拟目录，它的根目录是从`/`开始代表的是`src/main/java`或者`src/main/resources`目录

### 读取文件借助commons-io库

依赖：

```java
<dependency>
  <groupId>commons-io</groupId>
  <artifactId>commons-io</artifactId>
  <version>2.6</version>
</dependency>
```

#### 测试代码

```java
public class Test {

  public static void main(String[] args) {
    // 读取 classpath 的内容
    InputStream in = Test.class.getClassLoader().getResourceAsStream("data.json");
    // 使用 commons-io 库读取文本
    try {
      String content = IOUtils.toString(in, "utf-8");
      System.out.println(content);
    } catch (IOException e) {
      // IOUtils.toString 有可能会抛出异常，需要我们捕获一下
      e.printStackTrace();
    }
  }

}
```

这行代码：

```java
InputStream in = Test.class.getClassLoader().getResourceAsStream("data.json");
```

意思是从java运行的类加载器(ClassLoader)实例中查找文件，`Test.class`指当前`Test.java`文件编译后生成的class文件

如果是在resources的子目录下，就要写子目录，比如`date/urls.txt`

### Spring Resource的作用

在spring当中定义了一个`org.springframework.core.io.Resource`类来封装文件，这个类的优势就在于可以支持普通的File也可以支持classpath文件

并且在`org.springframework.core.io.ResourceLoader`服务来提供任意文件的读写，可以在任意的spring bean中引入`ResourceLoader`

比如：

```java
@Autowired
private ResourceLoader loader;
```

#### 在spring当中如何读取文件

创建一个自己的`FileService`

```java
public interface FileSerice{
    String getContent(String name);
}
```

实现类：

```java
import fm.douban.service.FileService;
import org.apache.commons.io.IOUtils;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.core.io.Resource;
import org.springframework.core.io.ResourceLoader;
import org.springframework.stereotype.Service;

import java.io.IOException;
import java.io.InputStream;

@Service
public class FileServiceImpl implements FileService {

    @Autowired
    private ResourceLoader loader;

    @Override
    public String getContent(String name) {
        try {
            InputStream in = loader.getResource(name).getInputStream();
            return IOUtils.toString(in,"utf-8");
        } catch (IOException e) {
           return null;
        }
    }
}
```

调用：

```java
  FileService fileService = context.getBean(FileService.class);
  String content = fileService.getContent("classpath:data/urls.txt");
  System.out.println(content);
```

也可以读取本地文件，比如：参数为：`file:mywork/readme.md`代表本地文件

Resource还可以加载远程文件，比如：

```java
String content2 = fileService.getContent("https://www.zhihu.com/question/34786516/answer/822686390");
System.out.println(content2);
```

### Spring Bean的生命周期(LifeCircle)

<img title="" src="https://style.youkeda.com/img/ham/course/j4/beaninstance.svg" alt="bean生命周期" data-align="center">需要掌握init方法，init方法的名称可以是任意的，因为我们是通过注解来声明init的

比如`SubjectServiceImpl`,如下：

```java
import javax.annotation.PostConstruct;

@Service
public class SubjectServiceImpl implements SubjectService {

  @PostConstruct
  public void init(){
      System.out.println("启动啦");
  }

}  
```

只要在方法上添加`@PostConstruct`注解，就代表该方法在Spring Bean启动后会自动执行

PostConstruct完整类路径为`javax.annotation.PostConstruct`

有了init方法后，就可以把原来static代码块的代码放到init方法内

代码变为：

```java
@Service
public class SubjectServiceImpl implements SubjectService {

  @PostConstruct
  public void init(){
      Subject subject = new Subject();
      subject.setId("s001");
      subject.setName("成都");
      subject.setMusician("赵雷");

      subjectMap.put(subject.getId(), subject);
  }

}  
```
