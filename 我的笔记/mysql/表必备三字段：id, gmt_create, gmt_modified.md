# 表必备三字段：id, gmt_create, gmt_modified

今天读了阿里巴巴JAVA开发手册 其中对于MYSQL的建表提出要求:

9. 【强制】表必备三字段：id, gmt_create, gmt_modified。
说明：其中 id 必为主键，类型为 unsigned bigint、单表时自增、步长为 1。gmt_create,
gmt_modified 的类型均为 date_time 类型，前者现在时表示主动创建，后者过去分词表示被
动更新。

于是就自己动手试了下 代码如下:

```sql
create table `test`(
        `id` bigint unsigned not null auto_increment,
		`gmt_create` datetime null default current_timestamp,
		`gmt_modified` datetime null default current_timestamp on update current_timestamp,
		primary key(`id`)
		);

```

但是一直失败 提示为:Invalid default value

百度得到的答案都认为是sql_mode的问题 但是没有一个能解决我的问题的

最后看到一个帖子 说5.5版本不支持datetime数据类型 于是我拿另一台电脑安装了5.7的mysql

第一台电脑:Server Version : 5.5.27
第二台电脑:Server Version : 5.7.26

第二台电脑顺利创建了该表

另外经常犯一个错误:把datetime写成datatime

希望这篇文章能帮到其他MYSQL初学者
--------------------- 
作者：Cowbell 
来源：CSDN 
原文：https://blog.csdn.net/Cowbell/article/details/89964544 
版权声明：本文为博主原创文章，转载请附上博文链接！





# mysql更新时设置ON UPDATE CURRENT_TIMESTAMP保存数据库的时间

https://blog.csdn.net/dongzhouzhou/article/details/80367551