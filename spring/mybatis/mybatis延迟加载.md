# [mybatis N+1问题解决](https://www.cnblogs.com/GodBug/p/7681249.html)



# MyBatis学习11】MyBatis中的延迟加载

https://blog.csdn.net/eson_15/article/details/51668523

### **延迟加载的配置**

<settings>
    <!-- 打开延迟加载的开关 -->
    <setting name="lazyLoadingEnabled" value="true"/>
    <!-- 将积极加载改为消极加载，即延迟加载 --将积极加载改为消极加载即按需加载-->
    <setting name="aggressiveLazyLoading" value="false"/>
</settings>
————————————————
版权声明：本文为CSDN博主「eson_15」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/eson_15/article/details/51668523





# MyBatis级联第五篇——树形加载规则和非全局性加载，延迟加载原理（推荐，继续上篇的重要原创）

https://blog.csdn.net/ykzhen2015/article/details/51364906