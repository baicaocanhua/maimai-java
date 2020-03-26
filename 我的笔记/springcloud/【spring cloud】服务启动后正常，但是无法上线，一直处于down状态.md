# [【spring cloud】服务启动后正常，但是无法上线，一直处于down状态](https://www.cnblogs.com/lodor/p/7849967.html)

spring cloud eureka 如果出现某个应用实例 down(1),

​        说明 spring admin 健康检测没有通过导致 eureka 注册中心不会把这个实例从列表中删除掉。 这样所有使用这个实例的服务都会现404（前提是在应用中配置过spring admin）；

##              2：spring admin 健康检测会检测*.properties里的所有连能性的配置(mysql,redis,短信服务，邮件服务)，如果这些URL中有一个不通，则会导致eureka中出现， 这个实例down(1) 并且不会从列表中删除掉。

​                     例： 应用中不使用reides，但是在pom.xml中引用reides的配置（只限于spring-boot redis配置） 这样spring admin 健康检测发现*.properties没配置redis,但是spring-boot-starter-data-redis 有默认配置(是localhost), 会导致检测不通过，eureka 显示状态为 down(1).

 

​        处理这样问题可以使用：http://eureakIP:port/health 如果没有问题会返回：

​                 {"description":"Spring Cloud Eureka Discovery Client","status":"UP"} 如果有问题会返回那个实例的检测什么配置项没有通过，只要修改后重启应用实例，这样eureka应用会显示UP(1);

 

 

如：

```
{"description":"Remote status from Eureka server","status":"DOWN","discoveryComposite":{"description":"Remote status from Eureka server","status":"DOWN","discoveryClient":{"description":"Spring Cloud Eureka Discovery Client","status":"UP","services":     ["sail-coupon","member-inf","sail_message","sail-route","sail-member","sail-point","gift-card"]},   "eureka":{"description":"Remote status from Eureka server","status":"DOWN","applications":          {"SAIL-MEMBER":1,"SAIL-POINT":1,"SAIL-COUPON":1,"MEMBER-INF":1,"GIFT-CARD":1,"SAIL_MESSAGE":1,"SAIL-MERCHANT":0,"SAIL-ROUTE":1}}},     "diskSpace":{"status":"UP","total":42842714112,"free":25094348800,"threshold":10485760},     "rabbit":{"status":"DOWN","error":"org.springframework.amqp.AmqpConnectException: java.net.ConnectException: Connection refused: connect"},     "redis":{"status":"UP","version":"3.2.100"},"db":{"status":"UP","database":"MySQL","hello":1},"refreshScope":{"status":"UP"},"hystrix":{"status":"UP"}}
```