实际项目中会有大量的页面功能需要判断用户是否登录，每一个页面都判断是否登录，就太繁琐也不利于维护

需要一种统一处理相同逻辑的机制`HandlerInterceptor`拦截器

实现拦截器有三个步骤：

#### 创建拦截器

拦截器必须实现`HandlerInterceptor`接口。可以在三个点进行拦截：

1. controller方法执行之前，常用的拦截点，例如是否登录验证就要在`preHandle()`中处理

2. controller方法执行之后，例如记录日志，统计方法执行时间等就要在`postHandle()`方法中处理

3. 整个请求完成后，这是不常用的拦截点。例如统计整个请求的执行时间的时候用，在`afterCompletion`方法中处理

```java
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.springframework.web.servlet.HandlerInterceptor;
import org.springframework.web.servlet.ModelAndView;

public class InterceptorDemo implements HandlerInterceptor {

  // Controller方法执行之前
  @Override
  public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {

    // 只有返回true才会继续向下执行，返回false取消当前请求
    return true;
  }

  //Controller方法执行之后
  @Override
  public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler,      ModelAndView modelAndView) throws Exception {

  }

  // 整个请求完成后（包括Thymeleaf渲染完毕）
  @Override
  public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {

  }
}
```

`preHandle()`方法中有`HttpServletRequest`和`HttpServletResponse`可以像control中用于使用Session

#### 实现WebMvcConfigurer

创建一个类来实现`WebMvcConfigurer`并实现`addInterceptors()`方法，这个步骤用于管理拦截器

实现类要加上`@Configuration`注解，让框架能自动扫描处理

管理拦截器，比较重要的是为拦截器设置拦截范围。常用`addPathPatterns("/**")`表示拦截所有的`URL`

也可以调用`excludePathPatterns()`方法排除某些`URL`，例如登录也本身就不需要登录，需要排除

```java
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.InterceptorRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class WebAppConfigurerDemo implements WebMvcConfigurer {

  @Override
  public void addInterceptors(InterceptorRegistry registry) {
    // 多个拦截器组成一个拦截器链
    // 仅演示，设置所有 url 都拦截
    registry.addInterceptor(new UserInterceptor()).addPathPatterns("/**");
  }
}
```

通常拦截器会放在一个包里，例如`interceptor`。而用于管理拦截器的配置类会放在另一个包里，例如`config`里
