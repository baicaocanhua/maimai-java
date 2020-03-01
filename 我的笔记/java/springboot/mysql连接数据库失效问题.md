# Spring Boot环境下出现No operations allowed after connection close错误

https://blog.csdn.net/qq_38023253/article/details/80815618







# SpringBoot2 hikari 关于 Failed to validate connection com.mysql.cj.jdbc.ConnectionImpl处理



问题很诡异，启动不报错，如果静默15分钟没有数据库操作就报上述错误

Failed to validate connection com.mysql.cj.jdbc.ConnectionImpl

分析是hikari 连接池对连接管理的问题？因此想方设法找SpringBoot连接池配置

后来发现SpringBoot2开始配置文件有所变化，特此记录

spring.datasource.hikari.minimum-idle=3
spring.datasource.hikari.maximum-pool-size=10
spring.datasource.hikari.max-lifetime =30000 // 不能小于30秒，否则默认回到1800秒
spring.datasource.hikari.connection-test-query=SELECT 1
--------------------- 
作者：darkread 
来源：CSDN 
原文：https://blog.csdn.net/darkread/article/details/89562148 
版权声明：本文为博主原创文章，转载请附上博文链接！