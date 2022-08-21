### Sping Bean

IoC(Inversion of Control,控制反转)容器是Spring框架最最核心的组件，没有IoC容器就没有Spring框架。是面向对象编程中的一种设计原则，可以用来减低计算机代码之间的耦合度

在Spring框架中，主要通过依赖注入(Dependency Injection，简称DI)来实现IoC

在Spring的世界中，所有的Java对象都会通过IoC容器转变为Bean(Spring对象的一种称呼，以后都有Bean来表示Java对象)，构成应用程序主干和由Spring IoC容器管理的对象称为beans，beans和它们之间的依赖关系反映在容器使用的配置元数据中。

基本上所有的Bean都是由接口+实现类完成的，用户相应获取Bean的实例直接从IoC容器获取就可以了，不需要关系实现类

![Bean](https://style.youkeda.com/img/ham/course/j4/ioc.svg)

#### Spring配置元数据的方式

Spring主要有两种配置元数据的方式，一种是基于XML，一种是基于Annotation方案的，目前主流的方案是基于Annotation的，所以我们这里也是以Annotation为基础方案来讲解

#### `org.springframework.context.AnnotationContext`

接口类定义容器的对外服务，通过这个接口，我们可以轻松的从IoC容器中得到Bean对象。我们在启动Java程序的时候必须要先启动IoC容器

Annotation类型的IoC容器对应的类是

`org.springframework.context.annotation.AnnotationConfigApplicationContext`

我们如果要启动IoC容器，可以运行下面的代码：

```java
ApplicationContext context = new AnnotationConfigApplicationContext("fm.douban");
```

这段代码的含义就是启动IoC容器，并且会自动加载包`fm.douban`下的Bean，哪些Bean会被加载呢？只要引用下Spring注解的类都可以被加载(前提是在这个包下)

 `AnnotationConfigApplicationContext`这个类的构造函数有两种：

+ `AnnotationConfigApplicationContext(String ...basePackages)`根据**包名**实例化

+ `AnnotationConfigApplicationConext(Class clazz)`根据自定义包扫描行为实例化

#### Spring官方声明为Spring Bean的注解有如下几种：

+ `org.springframework.stereotype.Service`

+ `org.springframework.stereotype.Component`

+ `org.springframework.stereotype.Controller`

+ `org.springframework.stereotype.Repository`

只要我们在类上引用这类注解，那么都可以被IoC容器加载

`@Component`注解是通用的Bean注解，其余三个注解都是扩展自`Component`

`@Service`正如这个名称一样，代表的是Service Bean

`@Controller`作用于Web Bean

`@Repository`作用于持久化相关Bean

实际上这四个注解都可以被IoC容器加载，一般情况下，我们使用`@Service`；如果是Web服务就使用`@Controller`

#### Ioc容器就像一个大型的工厂一样，我们不关心工厂如何生产，只需使用工厂生产的产品

依赖注入的第一步是完成容器的启动，第二步就是真正的完成依赖注入行为了

依赖注入这个词也是一种编程思想，简单来说就是一种获取其他实例的规范

### 依赖注入编程思想

新增`SubjectService和SubjectServiceImpl`

![XML图](https://style.youkeda.com/img/ham/course/j4/song-uml2.svg)

在`SubjectServiceImpl`中新增一个list方法用于查询专辑歌曲

```java
public class SubjectServiceImpl implements SubjectService {

    private SongService songService;

    //缓存所有专辑数据
    private static Map<String, Subject> subjectMap = new HashMap<>();

    static {
        Subject subject = new Subject();
        //... 省略初始化数据的过程
        subjectMap.put(subject.getId(), subject);
    }

    @Override
    public Subject get(String subjectId) {
        Subject subject = subjectMap.get(subjectId);
        //调用 SongService 获取专辑歌曲
        List<Song> songs = songService.list(subjectId);
        subject.setSongs(songs);
        return subject;
    }

    public void setSongService(SongService songService) {
        this.songService = songService;
    }
}
```

使用依赖注入后：

```java
import fm.douban.model.Song;
import fm.douban.model.Subject;
import fm.douban.service.SongService;
import fm.douban.service.SubjectService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.HashMap;
import java.util.List;
import java.util.Map;

@Service
public class SubjectServiceImpl implements SubjectService {

    @Autowired
    private SongService songService;

    //缓存所有专辑数据
    private static Map<String, Subject> subjectMap = new HashMap<>();

    static {
        Subject subject = new Subject();
        subject.setId("s001");
        //... 省略初始化数据的过程
        subjectMap.put(subject.getId(), subject);
    }

    @Override
    public Subject get(String subjectId) {
        Subject subject = subjectMap.get(subjectId);
        //调用 SongService 获取专辑歌曲
        List<Song> songs = songService.list(subjectId);
        subject.setSongs(songs);
        return subject;
    }
}
```

![区别](https://style.youkeda.com/img/ham/course/j4/autoware-change1.gif)

list方法为查询专辑歌曲

### 改动前和改动后的区别

改动前存在的问题

在任何需要使用`SubjectService`的地方都需要编码实例化对象

如果依赖的太多，就会需要实例化更多

如何解决问题(改动后)

加注解的作用就是让Spring系统自动管理各种实例

就是用`@Service`注解把`SubjectServiceImpl`和`SongServiceImpl`等所有服务实现，都标记成Spring Bean;然后，在任何需要使用服务类的地方，用`@Autowired`注解标记，告诉Spring这里需要注入实现类的实例

项目启动过程中，Spring会自动实例化服务实现类，然后自动注入到变量中，不需要大量写new代码

`@Service`和`@Autowired`是相辅相成的，如果服务实现类没有加`@Service`注解，那么就意味着没有标记成Spring Bean，即使加了`@Autowired`，也无法注入实例

`private SongService songService;`属性忘记加`@Autowired`，也无法自动注入

每个注解都有各自特定的功能，Spring检查到代码中有注解，就自动完成特定功能

### `@Autowired`的完整类路径：

`org.springframework.beans.factory.annotation.Autowired`
