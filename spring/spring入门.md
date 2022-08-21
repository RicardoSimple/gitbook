### spring

#### spring5坐标

```
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-context</artifactId>
  <version>5.2.1.RELEASE</version>
</dependency>
```

spring强调的是面向接口编程，所以绝大多数情况下spring代码都会有接口和实现类

![差异](https://style.youkeda.com/img/ham/course/j4/springbeanvs.svg)

可以看到调用了MessageService实例的getMessage()方法

仔细对比可以发现我们调用MessageService可以直接从上下文获取，不需要关系实现类

所以最大的spring价值就是完全屏蔽了实现细节，也就意味这降低了工程的复杂度，因为只要开发双发约定好接口就可以一起工作，这都是对面对接口编程的理解
