# Java Date数据类型时差问题

https://www.jianshu.com/p/639251669e9d



## [SpringCloudFeign传输date类型参数，时间差14个小时](javascritp:void(0))





# 关于feign接口调用时的date类型的传参问题

https://blog.csdn.net/You_are_my_Mr_Right/article/details/102826619



# SpringCloud Feign 传参问题及传输Date类型参数的时差

https://blog.csdn.net/Zany540817349/article/details/79868257?utm_source=blogxgwz5





# springcloud feign 服务之间调用，date类型转换错误解决方案

https://blog.csdn.net/l448797381/article/details/101024418



# 关于Feign默认时区与实际相差14小时的问题

https://blog.csdn.net/qq_39547644/article/details/97132394



# SpringCloud Feign 传输Date类型参数的时差问题



https://www.jianshu.com/p/4b344319f908





# [Feign Date类型时间错误问题](https://www.cnblogs.com/ingxx/p/11286866.html)

# 解决方法

1. 使用String类型作为参数，在接收方进行类型转换
2. 使用JDK8中的LocalDate
3. 第三种方法增加配置类，使Feign使用自定义的规则转换