# Spring-Cloud整合Spring-Session的注意点

<https://www.jianshu.com/p/10429b5c22ce>



# SpringCloud微服务架构分布式组件如何共享session对象



<https://blog.csdn.net/justlpf/article/details/80350943>



# Feign Hystrix微服务调用Session传播

<https://blog.csdn.net/weihao_/article/details/83240099>

# 详解feign调用session丢失解决方案

<https://www.jb51.net/article/156492.htm>



# Spring Cloud系列（三十二）Feign丢失Cookie和Header信息的问题（Finchley.RC2版本）



<https://blog.csdn.net/WYA1993/article/details/84304243>





# [SpringCloud-Feign] Feign转发请求头，(防止session失效)

<https://blog.csdn.net/my_momo_csdn/article/details/80922737>





# feign调用session丢失解决方案

<https://blog.csdn.net/zl1zl2zl3/article/details/79084368>





您好！第七点中的cookie丢失的问题是由于springboot 默认的配置是讲request中的cookie信息给去掉了。需要配置zuul不去掉任何request中的东西。在application.properties文件中配置：（zuul.sensitive-headers=）可解决