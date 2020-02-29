```
@Configuration
public class FeignConfig implements RequestInterceptor
{
    //这里是我自己的redis代理，用不上可以去掉
    @Autowired
    private IRedisProxy redisProxy;
 
    @Override
    public void apply(RequestTemplate requestTemplate)
    {
        // 生成远程调用认证token
        //String token = TokenUtil.TokenCreate("feign");
        // 放到redis,设置时长为10S，一般10S后还没有完成请求则token失效
        //redisProxy.setex(token, 10, token);
        //设置token，关键方法
        requestTemplate.header("Token", token);
    }
 
}
————————————————
版权声明：本文为CSDN博主「Gogym」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/KokJuis/article/details/80660273
```

```
@Bean
    public RequestInterceptor headerInterceptor() {
        return new RequestInterceptor() {
            @Override
            public void apply(RequestTemplate requestTemplate) {
                ServletRequestAttributes attributes = (ServletRequestAttributes) RequestContextHolder
                        .getRequestAttributes();
                HttpServletRequest request = attributes.getRequest();
                Enumeration<String> headerNames = request.getHeaderNames();
                if (headerNames != null) {
                    while (headerNames.hasMoreElements()) {
                        String name = headerNames.nextElement();
                        String values = request.getHeader(name);
                        requestTemplate.header(name, values);
                    }
                }
            }
        };
    }
————————————————
版权声明：本文为CSDN博主「junzibuqi124」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/u014519194/article/details/77160958
```





```
1、首先先看什么是Feign。

这里引用“大漠知秋”的博文https://blog.csdn.net/wo18237095579/article/details/83343915

2、若其他服务的接口未做权限处理，参照上文第1点的博文即可。

3、若其他服务的接口做了权限的处理（例如OAuth 2）时该如何访问?

a、有做权限处理的服务接口直接调用会造成调用时出现http 401未授权的错误，继而导致最终服务的http 500内部服务器错误

b、解决方式：最方便的就是往请求头里加上token，一起带过去；

b1、Feign有提供一个接口，RequestInterceptor；只要实现这个接口，简单做一些处理，比如说我们验证请求头的token叫Access-Token，我们就先取出当前请求的token，然后放到feign请求头上；

b2、新建配置类

@Configuration
public class FeignConfig implements RequestInterceptor {
    @Override
    public void apply(RequestTemplate requestTemplate) {
        ServletRequestAttributes attributes = (ServletRequestAttributes)RequestContextHolder.getRequestAttributes();
        HttpServletRequest request = attributes.getRequest();
        requestTemplate.header(HttpHeaders.AUTHORIZATION, request.getHeader(HttpHeaders.AUTHORIZATION));
    }
}
b3、在@FeignClient接口里添加configuration = {FeignConfig.class}

@FeignClient（value="被调用的服务名",configuration={FeignConfig.class}）
即可对做权限处理的服务接口进行调用
```





```
项目中经常涉及内部模块之间的相互调用，导致在用A模块调用B模块的方法时，请求头里面的token信息传递不到B模块，从而获取不到用户信息，导致保存数据时，保存的数据不全，根据网上一些博客写此篇博客以作记录，代码中如有错漏之处，请各位大神海涵!
（下面的操作步骤没关系，只要有就可以）

1.Configuration代码

package com.xxx.demo;

import feign.RequestInterceptor;
import feign.RequestTemplate;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.context.request.RequestContextHolder;
import org.springframework.web.context.request.ServletRequestAttributes;

import javax.servlet.http.HttpServletRequest;
import java.util.Enumeration;

/**
 * @description: Feign内部调用时带上请求头信息
 * @author: xxx
 * @create: 2019-08-09 10:02
 * 注意：要去yml里面改变hystrix Feign的隔离策为strategy: SEMAPHORE
 **/
@Configuration
public class FeignConfiguration implements RequestInterceptor {

    @Override
    public void apply(RequestTemplate template) {
        ServletRequestAttributes attributes = (ServletRequestAttributes) RequestContextHolder
                .getRequestAttributes();
        HttpServletRequest request = attributes.getRequest();
        Enumeration<String> headerNames = request.getHeaderNames();
        if (headerNames != null) {
            while (headerNames.hasMoreElements()) {
                String name = headerNames.nextElement();
                String values = request.getHeader(name);
                template.header(name, values);
            }
        }
        Enumeration<String> bodyNames = request.getParameterNames();
        StringBuffer body =new StringBuffer();
        if (bodyNames != null) {
            while (bodyNames.hasMoreElements()) {
                String name = bodyNames.nextElement();
                String values = request.getParameter(name);
                body.append(name).append("=").append(values).append("&");
            }
        }
        if(body.length()!=0) {
            body.deleteCharAt(body.length()-1);
            template.body(body.toString());
        }
    }
}
2.yml配置

feign:
  hystrix:
    enabled: true
  client:
    config:
      default:
        connectTimeout: 60000
        readTimeout: 60000

hystrix:
  command:
    default:
      execution:
        timeout:
          enabled: true
        isolation:
          strategy: SEMAPHORE
          thread:
            timeoutInMilliseconds: 60000
3.Feignclient调用的时候指定configuration

package com.xxx.service;

import org.springframework.cloud.openfeign.FeignClient;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

import java.util.Map;

@FeignClient(name = "device", configuration = FeignConfiguration.class)
public interface MonitorNetService {

    @RequestMapping(value = "api/monitorNetImport", method = RequestMethod.POST)
    public Map<String, Object> monitorNetImport(@RequestBody Map<String, String> params);
}
————————————————
版权声明：本文为CSDN博主「慕雪辰」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/Mu_Xue_Chen/article/details/98946207
```



重写隔离策略

# spring cloud 问题记录（十六） 使用Feign跨服调用时header请求头中的信息丢失

https://blog.csdn.net/zhuwei_clark/article/details/94718197





# Spring Cloud系列（三十二）Feign丢失Cookie和Header信息的问题（Finchley.RC2版本）

https://blog.csdn.net/WYA1993/article/details/84304243







# springcloud fegin获取request header解决方案



```
假设现在有A服务,B服务,外部使用RESTApi请求调用A服务，在请求头上有token字段，A服务使用完后，B服务也要使用，如何才能把token也转发到B服务呢？这里可以使用Feign的RequestInterceptor，但是直接使用一般情况下HttpServletRequest上下文对象是为空的，这里要怎么处理，请看下文。

作者：大猪大猪
链接：https://www.jianshu.com/p/3b34234c4623
来源：简书
简书著作权归作者所有，任何形式的转载都请联系作者获得授权并注明出处。
```

```
演示
A服务FeginInterceptor
@Configuration
public class FeginInterceptor implements RequestInterceptor {

    @Override
    public void apply(RequestTemplate requestTemplate) {
        Map<String,String> headers = getHeaders(getHttpServletRequest());
        for(String headerName : headers.keySet()){
            requestTemplate.header(headerName, getHeaders(getHttpServletRequest()).get(headerName));
        }
    }

    private HttpServletRequest getHttpServletRequest() {
        try {
            return ((ServletRequestAttributes) RequestContextHolder.getRequestAttributes()).getRequest();
        } catch (Exception e) {
            e.printStackTrace();
            return null;
        }
    }

    private Map<String, String> getHeaders(HttpServletRequest request) {
        Map<String, String> map = new LinkedHashMap<>();
        Enumeration<String> enumeration = request.getHeaderNames();
        while (enumeration.hasMoreElements()) {
            String key = enumeration.nextElement();
            String value = request.getHeader(key);
            map.put(key, value);
        }
        return map;
    }

}

重点配置
A服务配置bootstrap.yml
hystrix:
  command:
    default:
      execution:
        isolation:
          strategy: SEMAPHORE

A服务build.gradle
buildscript {
    ext{
        springBootVersion = '1.5.9.RELEASE'
    }

    repositories {
        mavenCentral()
    }
    dependencies {
        classpath("org.springframework.boot:spring-boot-gradle-plugin:${springBootVersion}")
        classpath "io.spring.gradle:dependency-management-plugin:0.5.6.RELEASE"
    }
}

apply plugin: 'java'
apply plugin: 'org.springframework.boot'
apply plugin: "io.spring.dependency-management"
version = '0.0.1-SNAPSHOT'
group 'com.dounine.test'

sourceCompatibility = 1.8

repositories {
    mavenLocal()
    mavenCentral()
}

ext {
    springCloudVersion = 'Dalston.SR2'
}

dependencies {
    compile('org.springframework.cloud:spring-cloud-starter-config')
    compile('org.springframework.cloud:spring-cloud-starter-eureka')
    compile('org.springframework.cloud:spring-cloud-starter-feign')
    compile group: 'org.aspectj', name: 'aspectjweaver', version: '1.8.13'
    compile('org.springframework.boot:spring-boot-starter-data-redis')
    testCompile('org.springframework.boot:spring-boot-starter-test')
}

dependencyManagement {
    imports {
        mavenBom "org.springframework.cloud:spring-cloud-dependencies:${springCloudVersion}"
    }
}


B服务Action
    @Autowired
    private HttpServletRequest servletRequest;

    public String test() {
        return servletRequest.getHeader("token");
    }

B服务问题
如若B服务也要将header请求转发到其它服务，将A服务的配置也应用到B服务上即可

作者：大猪大猪
链接：https://www.jianshu.com/p/3b34234c4623
来源：简书
简书著作权归作者所有，任何形式的转载都请联系作者获得授权并注明出处。
```

