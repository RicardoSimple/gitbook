#### CORS

是一个W3C标准，全称是跨域资源共享

允许浏览器向跨源服务器发出请求，克服AJAX只能同源使用的限制

比如前端AJax遇到错误：

```
Cross-Origin Request Blocked: The Same Origin Policy disallows reading the remote resource at $somesite
```

就说明请求跨域了

CORS就相当于开信任通道，解决跨域

[CORS](https://www.ruanyifeng.com/blog/2016/04/cors.html)

SpringBoot支持CORS请求，推荐使用Filter来解决

```java
package com.youkeda.comment;

import org.springframework.boot.web.servlet.FilterRegistrationBean;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.cors.CorsConfiguration;
import org.springframework.web.cors.UrlBasedCorsConfigurationSource;
import org.springframework.web.filter.CorsFilter;

import java.time.Duration;

@Configuration
public class CorsConfig {

    @Bean
    public FilterRegistrationBean corsFilter() {
        UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
        CorsConfiguration config = new CorsConfiguration();
        config.setAllowCredentials(true);
        // 设置你要允许的网站域名，如果全允许则设为 *
        config.addAllowedOrigin("*");
        // 如果要限制 HEADER 或 METHOD 请自行更改
        config.addAllowedHeader("*");
        config.addAllowedMethod("*");
        config.setMaxAge(Duration.ofDays(5));
        source.registerCorsConfiguration("/**", config);
        FilterRegistrationBean bean = new FilterRegistrationBean(new CorsFilter(source));
        // 这个顺序很重要哦，为避免麻烦请设置在最前
        bean.setOrder(0);
        return bean;
    }
}
```

只要创建这个CorsConfig类就可以了
