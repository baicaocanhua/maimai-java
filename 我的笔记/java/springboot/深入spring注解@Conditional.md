# 深入spring注解@Conditional

https://blog.csdn.net/strive____/article/details/91403504

```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
@Conditional(MyCondition.class)
public @interface ConditionMy {

    boolean isOpen() default false;

}
```



# 浅谈spring中@Conditional（条件注解） 男人vs女人

https://blog.csdn.net/u010502101/article/details/76554564





# Spring之条件注解@Conditional，条件（系统）不同注入的对象也不同。

https://blog.csdn.net/yjc_1111/article/details/78807129



# Spring4中的@Profile和@Conditional注解的源码解析

https://blog.csdn.net/jia_costa/article/details/79213359





# Spring原理学习--扩展@Conditional注解

https://blog.csdn.net/wzl19870309/article/details/84940673