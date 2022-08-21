### 添加依赖

```xml
         <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-neo4j</artifactId>
        </dependency>
```

### 添加配置

不同版本的依赖配置可能不太一样，可以通过自动配置

```properties
spring.neo4j.uri=bolt://
spring.neo4j.authentication.username=
spring.neo4j.authentication.password=
```

### 操作数据库接口

`接口 extends Neo4jRepository<类型,Long>`

#### 创建类型时，需要有注解

`@NodeEntity(label="")`表示标签，类需要继承`implements Serializable`

id属性：

```java
@Id
@GeneratedValue
private Long id;
```
