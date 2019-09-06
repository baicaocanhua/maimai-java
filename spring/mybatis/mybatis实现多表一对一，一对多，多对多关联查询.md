# mybatis实现多表一对一，一对多，多对多关联查询

https://blog.csdn.net/m0_37787069/article/details/79247321

# 学习Mybatis框架（五）—高级映射（多表关联查询）

https://blog.csdn.net/hefenglian/article/details/80699723

# [Mybatis学习记录（六）----Mybatis的高级映射](https://www.cnblogs.com/doctorJoe/p/5291023.html)

# [MyBatis学习总结（三）——多表关联查询与动态SQL](https://www.cnblogs.com/best/p/9723085.html)



```
<association property="card" javaType="com.gec.domain.Card">
  		<!-- 1、构造器注入
		<constructor>
  			<idArg column="id" javaType="int"/>
  			<arg column="code" javaType="string"/>
  		</constructor> 
		-->
		<!-- 2、setter注入 -->
  		<result column="id" property="cardId"/>
  		<result column="code" property="code"/>
  	</association>
————————————————
版权声明：本文为CSDN博主「Adamlawen」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/m0_37787069/article/details/79247321
```

