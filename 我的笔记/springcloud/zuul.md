Path to install Zuul as a servlet (not part of Spring MVC). 

The servlet is more memory efficient for requests with large bodies,

e.g. file uploads.



路径加上zuul作为一个servlet，而不是spring MVC的一部分

servlet请求大的请求体是内存高效的

例如文件上传







# SpringCloud使用Zuul处理文件上传

https://blog.csdn.net/qq_26440803/article/details/82917766



遇到的问题
Q ： 对于上传中文文件会发生乱码
Zuul给出的方案是走Servlet而不是SpringMVC，也就是上传文件时，使用路径为
/zuul/customers/*，customers指你配置的路由zuul.routes.customers=/customers/**，所有path中包含customers前缀的都会分发到customers这个微服务中。但是这样一来太不灵活了，因为有些需要经过Servlet，有些却不需要，所以我们可以直接设置ZuulServlet的路径为root，即zuul.servlet-path=/。
Q：对于大文件的上传
Zuul针对大文件的上传会出现Requst Bad，但是在我实验的过程中没有走/zuul/customers/*却依然可以进行上传，只是当文件过大的时候会发生超时。但是通过设置超时时间就可以解决这个问题。但是这样带来后果无限的等待，调用任何的服务，只要服务出现错误就会一直等待，不能很好的进行熔断。
 ———————————————— 



# spring cloud zuul网关上传大文件



两步：
1.在请求路径上添加 /zuul 这样就可以越过zuul的springmvc
2.在资源服务器yml加个配置

spring:
	servlet:
      multipart:
        max-file-size: 10MB # 单个文件大小,默认是1MB
        max-request-size: 30MB # 请求总上传的数据大小
        enabled: true
 ———————————————— 
版权声明：本文为CSDN博主「田培融」的原创文章，遵循CC 4.0 by-sa版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/u011296165/article/details/90702271





# Zuul-Config

https://www.jianshu.com/p/0bc67ad41094

```
## 1 ###################### 路由指定:URL指定  #############################
## URL匹配关键字，如果包含关键字就跳转到指定的URL中 
#zuul.routes.book-product.path=/book-product/**
#zuul.routes.book-product.url=http://127.0.0.1:8083/


## 2 ###################### 路由指定:服务指定1  #############################
##将路径的/book-product/引到 eureka的e-book-product服务上
##规则：zuul.routes.路径名.path
##规则：zuul.routes.路径名.serviceId=eureka的服务名
##http://127.0.0.1:9010/book-product/product/list
##等同于
##http://127.0.0.1:9010/e-book-product/product/list
#zuul.routes.book-product.path=/book-product/**
#zuul.routes.book-product.serviceId=e-book-product


## 3 ###################### 路由指定:服务指定1   #############################
#zuul.routes后面跟着的是服务名，服务名后面跟着的是路径规则，这种配置方式更简单。
#zuul.routes.e-book-product.path=/book-product/**

## 4 ###################### 路由排除：排除某几个服务  #############################
##排除后，这个地址将为空 http://127.0.0.1:9010/e-book-product/product/list 
## 多个服务逗号隔开
#zuul.ignored-services=e-book-product

## 5 ###################### 路由排除：排除所有服务  #############################
#由于服务太多，不可能手工一个个加，故路由排除所有服务，然后针对要路由的服务进行手工加
#zuul.ignored-services=*
#zuul.routes.e-book-consumer-hystrix.path=/book-consumer/**

## 6 ###################### 路由排除：排除指定关键字的路径  #############################
# 排除所有包括/list/的路径
#zuul.ignored-patterns=/**/list/**
#zuul.routes.e-book-product.path=/book-product/**

## 7 ###################### 路由添加前缀：为所有路径添加前缀  #############################
##http://127.0.0.1:9010/book-product/product/list
##必须改成
##http://127.0.0.1:9010/api/book-product/product/list
#zuul.prefix=/api
#zuul.routes.e-book-product.path=/book-product/**
```