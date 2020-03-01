![1563956203498](C:\Users\baica\AppData\Roaming\Typora\typora-user-images\1563956203498.png)





![1563956264583](C:\Users\baica\AppData\Roaming\Typora\typora-user-images\1563956264583.png)







# Whitelabel Error Page

This application has no explicit mapping for /error, so you are seeing this as a fallback.

Mon Jul 22 20:04:00 CST 2019

There was an unexpected error (type=Internal Server Error, status=500).

Ticket 'ST-58-6uPFcokzksjufK4HiARV-cas01.example.org' does not match supplied service. The original service was 'http://c1.cas.com:8088/user' and the supplied service was 'http://www.baidu.com:8088/user'.







# [nginx location指令详解](https://www.cnblogs.com/xiaoliangup/p/9175932.html)

**location区段**

1、没有修饰符 表示：必须以指定模式开始，如：

```
server {
　　server_name baidu.com;
　　location /abc {
　　　　……
　　}
}


那么，如下是对的：
http://baidu.com/abc
http://baidu.com/abc?p1
http://baidu.com/abc/
http://baidu.com/abcde
```

2、=表示：必须与指定的模式精确匹配

```
server {
server_name sish
　　location = /abc {
　　　　……
　　}
}
那么，如下是对的：
http://baidu.com/abc
http://baidu.com/abc?p1
如下是错的：
http://baidu.com/abc/
http://baidu.com/abcde
```

3、~ 表示：指定的正则表达式要区分大小写

```
server {
server_name baidu.com;
　　location ~ ^/abc$ {
　　　　……
　　}
}
那么，如下是对的：
http://baidu.com/abc
http://baidu.com/abc?p1=11&p2=22
如下是错的：
http://baidu.com/ABC
http://baidu.com/abc/
http://baidu.com/abcde
```

4、~* 表示：指定的正则表达式不区分大小写

```
server {
server_name baidu.com;
location ~* ^/abc$ {
　　　　……
　　}
}
那么，如下是对的：
http://baidu.com/abc
http://baidu..com/ABC
http://baidu..com/abc?p1=11&p2=22
如下是错的：
http://baidu..com/abc/
http://baidu..com/abcde
```

