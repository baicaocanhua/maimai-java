# @Component和@Configuration作为配置类的差别

https://blog.csdn.net/long476964/article/details/80626930



从上面可以看到，虽然Component注解也会当做配置类，但是并不会为其生成CGLIB代理Class，所以在生成Driver对象时和生成Car对象时调用car()方法执行了两次new操作，所以是不同的对象。当时Configuration注解时，生成当前对象的子类Class，并对方法拦截，第二次调用car()方法时直接从BeanFactory之中获取对象，所以得到的是同一个对象。
--------------------- 
作者：一号搬砖手 
来源：CSDN 
原文：https://blog.csdn.net/long476964/article/details/80626930 
版权声明：本文为博主原创文章，转载请附上博文链接！



# Spring @Configuration 和 @Component 区别(精简汇总版)

https://blog.csdn.net/zzhuan_1/article/details/85091496





https://blog.csdn.net/isea533/article/details/78072133

# Spring @Configuration 和 @Component 区别



SpringBoot之@Component，@Bean与@Configuration配置

https://www.nonelonely.com/article/1546503121433