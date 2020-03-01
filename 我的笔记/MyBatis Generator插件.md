# [使用Mapper专用的MyBatis Generator插件](https://www.cnblogs.com/linjiaxin/p/6104984.html)

使用Maven插件的一个好处是可以将Maven中的属性使用`${property}`形式在[`generatorConfig.xml`](http://git.oschina.net/free/Mapper/blob/master/src/test/resources/generator/generatorConfig.xml)中引用。

mybatisgenerator 数据库连接 &amp

```xml
<jdbcConnection driverClass="com.mysql.jdbc.Driver"
            connectionURL="jdbc:mysql://localhost:3306/test?useUnicode=true&amp;characterEncoding=utf8&amp;serverTimezone=GMT"
            userId="root"
            password="123456">
</jdbcConnection>
```

# mybatis generator Unknown system variable 'query_cache_size' 的解决方法

出现这种错误，很显然是数据库驱动程序 与 数据库版本不对应；



如 mybatis使用 mysql-5.1.10的驱动程序，而mybatis配置的数据源连接的是 mysql-8.0.11 ，修改 pom文件即可，如下：

<dependency>
		    <groupId>mysql</groupId>
		    <artifactId>mysql-connector-java</artifactId>
		    <version>8.0.11</version>

		</dependency>
--------------------- 


```xml
<classPathEntry location="C:\Windows\System32\config\systemprofile\.m2\repository\mysql\mysql-connector-java\8.0.11\mysql-connector-java-8.0.11.jar" />
```







## [使用Mybatis-Generator自动生成Dao、Model、Mapping相关文件(转)](https://www.cnblogs.com/smileberry/p/4145872.html)

# IDEA集成MyBatis Generator 插件 详解

<https://blog.csdn.net/yangqinfeng1121/article/details/80183516>



# Intellij IDEA集成mybatis-generator插件自动生成数据库实体操作类

<https://blog.csdn.net/fishinhouse/article/details/82529338>



# 【项目管理】Mybatis-Generator之最完美配置详解



<https://blog.csdn.net/zsq520520/article/details/50952830>