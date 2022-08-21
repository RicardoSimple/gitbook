### 报错NullPointerException

如果确定不是什么算法出错的问题，那么检查加了`@Autowired`注解的变量是不是有static，如果有就需要去掉

这个疏忽废了我巨多精力。。。才找出来

### 项目部署到服务器上

打成jar包，使用idea自带的maven install生成jar包

传到服务器上之后用对应的jdk环境运行

```shell
java -jar xxxx.jar
```

但是这样当窗口关闭时程序也会停止

3. 当窗口关闭时，程序也不会中止运行

```shell
#当用 nohup 命令执行作业时，缺省情况下该作业的所有输出被重定向到nohup.out的文件中，除非另外指定了输出文件
nohup java -jar XXX.jar &
```

4. 输出重定向到temp.file文件

```shell
#输出重定向到temp.file文件
nohup java -jar XXX.jar >temp.txt &

#即输出内容不打印到屏幕上，而是输出到temp.file文件中
```

jar包查看

```shell
ps -ef | grep java 查看当前运行的java进程
ps -ef | grep xxxx.jar查看当前运行的jar包
```

根据jar包占用的端口

```shell
netstat -nlp | grep :端口
```

jar包终止

`kill -9 pid`杀掉编号PID的进程，可以终止后台运行的jar包

### 跨域访问接口错误

添加此类

```java
import org.springframework.context.annotation.Configuration;
import javax.servlet.*;
import javax.servlet.annotation.WebFilter;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
@WebFilter(filterName = "CorsFilter ")
@Configuration
public class CorsFilter implements Filter {
    @Override
    public void doFilter(ServletRequest req, ServletResponse res, FilterChain chain) throws IOException, ServletException {
        HttpServletResponse response = (HttpServletResponse) res;
        response.setHeader("Access-Control-Allow-Origin","*");
        response.setHeader("Access-Control-Allow-Credentials", "true");
        response.setHeader("Access-Control-Allow-Methods", "POST, GET, PATCH, DELETE, PUT");
        response.setHeader("Access-Control-Max-Age", "3600");
        response.setHeader("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
        chain.doFilter(req, res);
    }
}
```

全局配置
