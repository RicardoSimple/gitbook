### Apache Maven

Maven是一个项目管理和构建自动化工具，最关心的就是它的项目构建功能

Maven提供了一个命令行工具，可以把工程打包成java支持的格式(比如jar)，并且部署到中央仓库里，这样使用者只需要到通过工具就可以很快捷地运用别人的代码，只需要添加依赖就可以

![Maven](https://style.youkeda.com/img/ham/course/j4/mvn.svg)

从这个架构可以看到借助于中央仓库，我们可以把java代码任意共享给别人

+ Maven项目结构:

```
${basedir}:存放pom.xml和所有的子目录
${basedir}/pom.xml: Maven的项目配置文件
${basedir}/src/main/java: 项目的Java源代码
${basedir}/src/main/resourses: 项目的资源文件，比如说propety文件
${basedir}/src/test/java: 项目测试类，比如jUnit代码
${basedir}/src/test/resourses: 测试使用的资源
```

这里的`${basedir}`代表的是java工程的根路径

一个Maven项目在默认情况下会产生jar文件，另外，编译后的classes会放在`${basedir}/target/classes`下面，
jar文件会放在`${basedir}/target`下面

代码放错位置会导致程序无法完成编译

+ Maven安装
  需配置环境变量

+ Maven命令

```
mvn clean complie: 编译命令，maven会自动扫描src/main/java下的代码并自动完成编译工作，执行完会在根目录生成target/classes，存放所有的class文件

mvn clean package: 编译并打包命令，这个命令是complie和package的集合。也就是说会先执行compile，然后再执行jar打包命令，这个结果会把所有的java文件和资源打包成一个jar

mvn clean install: 执行安装命令，这个命令是compile,package,install的集合，也就是会先执行compile命令，再执行jar打包，然后执行install命令安装到本地的Maven仓库目录中，这个目录是${user_home}/.m2
这个${user_home}指的是你电脑登录用户名的个人目录

mvn compile exec:java -Dexec.mainClass=${main} ：这个命令的意思是在compile执行后，执行运行java的命令，具体执行哪个java类是由-Dexec.mainClass=${main}参数指定的，比如我们想执行com.youkeda.Test类，完整命令就是
mvn compile exec:java -Dexec.mainClass=com.youkeda.Test
```

#### maven核心概念

![maven](https://style.youkeda.com/img/ham/course/j4/Maven%E6%A0%B8%E5%BF%83%E6%A6%82%E5%BF%B5.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

Maven的配置文件是一个强约定的XML格式文件，文件名一定是`pom.xml`

一个java项目的所有配置都放在POM中，大概有如下的行为：

定义项目的类型，名字

管理依赖关系

定制插件的
![pom.xml](https://style.youkeda.com/img/ham/course/j4/pomxml.svg)

```
1. maven坐标
2. maven工程属性
3. maven依赖
4. maven插件
```

+ maven坐标

```
<groupId>com.youkeda.course</groupId>
<artifactId>app</artifactId>
<packaging>jar</packaging>
<version>1.0-SNAPSHOT</version>
```

这四个标签组成了maven坐标，所谓坐标就是一种位置信息Maven的坐标就决定了这个maven项目部署后存在maven仓库的文件位置

groupId：就像一个文件夹一样，它的命名和java的包类似，这里一般只用小写的英文字母和字符。

aritifactId:有点像文件名一样，在一个groupId内，它一定是唯一的，不能使用中文字符或者特殊字符

packaging： maven工程执行完后会把整个工程打包成packaging指定的文件格式，默认情况下是packaging的值是jar，如果pom.xml中没有声明这个标签，那就是jar；packaging的值有：jar，war，ear，pom

version：基本遵守了软件工程中对版本号的规定

```
在maven的世界里，会把一个工程分为两种状态，这也是软件工程中最常用的规范

SNAPSHOT 翻译过来就是快照的意思，实际上代表了这个程序还处于不稳定的阶段，随时可以再修改，所以在开发时，会在最后加上SNAPSHOT

RELEASE RELEASE和SNAPSHOT是对立面，代表的就是稳定，一般正式发布时会改成RELEASE
```

三位版本号也是有规则的：

```
第一位代表的是主版本号：主版本号一般是团队约定来的

第二位代表的是新增功能

第三位代表的是bugfix后的版本：bugfix是修复代码缺陷，bug的行为
```

有的时候，我们也可能用两位的版本号，那就是没有第一位的主版本号。

编程过程中约定大于一切

还有一个约定：mvn package,install命令生成的jar文件名是`[artifactId]-[version].jar`

+ maven属性配置

```
 <properties>
    <java.version>1.8</java.version>
    <maven.compiler.source>${java.version}</maven.compiler.source>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.target>${java.version}</maven.compiler.target>
</properties>
```

格式是在properties内，固定格式，properties内的标签可以自定义，但是一般来说只能是小写英文字母加.

默认也有一些公开参数可以调整，比如：

```
java.version
代表设置一个参数：java.version，值为1.8

maven.compiler.source
这个参数是指定Maven编译时候源代码的JDK版本，${java.version}这个值有点特殊，是个动态值${key}语法会找到key参数的值

project.build.sourceEncoding
这个参数指定的是工程代码源文件的文件编码格式，一般情况下都设置为UTF-8

maven.compiler.target
这个参数作用是按照这个值来进行编译源代码
```

### 依赖管理和插件体系

有了maven坐标就可以通过maven的依赖管理来运用其他人的代码

+ 依赖管理 dependencies

dependency就是用于指定当前工程依赖其他代码库的，maven会自动管理jar依赖

一旦在pom.xml里声明了依赖信息，会先去本地用户目录下的`.m2`文件夹内查找对应的文件，如果没有找到就会触发从中央仓库下载行为，下载完会保存在本地的`.m2`文件夹中

只需要在pom.xml中添加标签即可

首先声明父标签，然后在其中添加依赖

比如fastjson库

```
<dependencies>
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>fastjson</artifactId>
        <version>1.2.62</version>
    </dependency>
</dependencies>
```

一个pom.xml中只能有一个dependencies标签

一般会把别人写的代码称为三方库，自己、团队写的称为二方库

+ 中央仓库
  Maven会把所有的jar存放在中央仓库里，可以通过[中央仓库](https://search.maven.org/)访问，国内可以访问[阿里云的镜像服务器](https://maven.aliyun.com/mvn/search)

+ 间接依赖
  比如remote依赖okhttp3，locale依赖remote，那么locale也自动依赖okhttp3

+ 插件体系
  让Maven变得高度可定制

```
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.8.1</version>
        </plugin>
    </plugins>
</build>
```

声明了一个`maven-compiler-plugin`的插件用于执行mvn compile的，maven的插件也是放在中央仓库的，一切皆是jar
