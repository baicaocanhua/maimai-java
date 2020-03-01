# SpringBoot-SpringSession源码分析

<https://www.jianshu.com/p/ac3826ae1fca>

# 使用Spring-Session Redis实现Session共享

<https://blog.csdn.net/qq_33423418/article/details/83022736>



# Spring Session-使用Redis的HttpSession

<https://blog.csdn.net/zyhlwzy/article/details/78030739>

# [request.getSession(true)和request.getSession(false)的区别](https://www.cnblogs.com/tv151579/p/3870905.html)

当向Session中存取登录信息时，一般建议：HttpSession session =request.getSession();

当从Session中获取登录信息时，一般建议：HttpSession session =request.getSession(false);

# [Spring Session - 使用Redis存储HttpSession例子](https://www.cnblogs.com/chenpi/p/6347299.html)

![1560406182065](C:\Users\baica\AppData\Roaming\Typora\typora-user-images\1560406182065.png)



# [如果连铁将军都不再可靠--记一次排查使用分布式轮候锁+SESSION防订单重复仍然加锁失效问题经历](https://segmentfault.com/a/1190000014981796)

RedisFlushMode提供了ON_SAVE跟IMMEDIATE两种方式，根据这里的注释，这两个配置的作用分别是这样的：

> **ON_SAVE:** 只有当SessionRepository.save方法被调用的时候才将缓存的Session属性写入Redis，而在一般的Web项目中，上述方法会在Http Response被提交的时候才会被调用。
> **IMMEDIATE:** 尽可能地将数据写入Redis，例如创建Session、设置Session的Attribute都会将数据立即的写入Redis

再来看看API文档怎么描述的
![图片描述](https://segmentfault.com/img/bVba1Ev?w=1040&h=396)
看看这可爱的默认值！我们终于知道了当我们不做任何设置时，spring-session默认采用的是ON_SAVE方式！显而易见，使用ON_SAVE方式能最大限度的减少与Redis的IO交互，而在大多数场景下都是没有问题的。然而我们的代码就恰恰是在第一个请求还没提交，第二个请求已经进入到Action方法并获取Session，此时缓存中的TEMP_ORDER_ID并没有在Redis中被设置成空，因此导致了这个几乎不可能发生的“Session脏读”事件！







# session是什么时候创建的？

<https://blog.csdn.net/cxl0921/article/details/76854313/>





# java服务器何时创建Session

<https://blog.csdn.net/liang0000zai/article/details/51460005>



# [session什么时候被创建](https://www.cnblogs.com/quchengfeng/p/5007097.html)

# session是什么时候被创建

<https://www.jianshu.com/p/13a1647cc7bc>