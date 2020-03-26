问题根本应该还是本人常常寻找问题的根源：mysql数据库版本过高！我使用的是Mysql8.0


<jdbcConnection driverClass="com.mysql.cj.jdbc.Driver"
	connectionURL="jdbc:mysql://localhost:3306/mybatis?useSSL=false&amp;serverTimezone=UTC"
	userId="root"
	password="root">
    <property name="nullCatalogMeansCurrent" value="true"/>
</jdbcConnection>

首先数据库 要指定对！
其次带参数必然的
然后加上一行<property name="nullCatalogMeansCurrent" value="true"/>
<!-- 指定数据库表 -->
<table schema="mabatis"  tableName="tb_user" domainObjectName="User" />
指定一下数据库
数据库中的表
domain实体类的名字

本人已解决，你的呢？
--------------------- 
作者：敲啥呢？正在创造BUG 
来源：CSDN 
原文：https://blog.csdn.net/weixin_41809435/article/details/85207563 
版权声明：本文为博主原创文章，转载请附上博文链接！







# MyBatis Generator 生成器把其他数据库的同名表生成下来的问题



MyBatis Generator : Table Configuration scheme.table matched more than one table


在使用生成器生成代码的时候遇到了这个错误, 现象就是某个类中出来了数据库表里面没有的字段,非常奇怪.

解决方法是在生成器的配置文件里的数据库连接地址中添加下列参数:

nullCatalogMeansCurrent=true
大概就是这个样子:

<!--数据库连接的信息：驱动类、连接地址、用户名、密码 -->
<jdbcConnection driverClass="com.mysql.cj.jdbc.Driver"
				connectionURL="jdbc:mysql://localhost:3306/security"
				userId="root"
				password="root">
	<!--MySQL 8.x 需要指定服务器的时区-->
	<property name="serverTimezone" value="UTC"/>
	<!--MySQL 不支持 schema 或者 catalog 所以需要添加这个-->
	<!--参考 : http://www.mybatis.org/generator/usage/mysql.html-->
	<property name="nullCatalogMeansCurrent" value="true"/>
</jdbcConnection>
这个问题是在找了很久没找到然后去官网看文章看到的
--------------------- 
作者：莫弹弹 
来源：CSDN 
原文：https://blog.csdn.net/qq_40233736/article/details/83314596 
版权声明：本文为博主原创文章，转载请附上博文链接！







# [MyBatis Generator 生成器把其他数据库的同名表生成下来的问题](https://www.cnblogs.com/xqz0618/p/MybatisGeneratorError.html)