  MySQL没有boolean类型。这也是比较奇怪的现象。例：

create table xs
(
id int primary key,
bl boolean
)

这样是可以创建成功，但查看一下建表后的语句，就会发现，mysql把它替换成tinyint(1)。也就是说mysql把boolean=tinyInt了。

boolean类型
MYSQL保存BOOLEAN值时用1代表TRUE,0代表FALSE，boolean在MySQL里的类型为tinyint(1),
MySQL里有四个常量：true,false,TRUE,FALSE,它们分别代表1,0,1,0  



# [关于 MySQL 的 boolean 和 tinyint(1)](https://www.cnblogs.com/xiaochaohuashengmi/archive/2011/08/25/2153011.html)

# mybaits中返回类型为boolean类型



  在Mybatis中，有时候需要返回布尔值 ，来确定某个记录行是否存在。 

例如： 
<select id="isExistCode" parameterType="string" resultType="boolean"> 
    <![CDATA[ select count(id) from table where code=#{code} ]]> 
</select> 

说明： 
Mybatis是根据查询到的记录数进行转换的(1=true,0=false) 
需要注意的地方：如果查询到多条记录(大于1)，返回的却是false, 这时就与我们的期望的刚好相反。这里，可以换其它方法，可以通过返回记录数，进行判断，也可以保证记录在数据库是唯一的。  





# mybaits中设置的返回值类型为boolean类型，当查询的结果大于1时返回True而不是false

https://blog.csdn.net/qq_27127145/article/details/83956484





# mark mybatis 返回boolean

https://blog.csdn.net/tt50335971/article/details/78687674