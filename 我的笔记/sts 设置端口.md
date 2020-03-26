# STS: Port 8080 required by Pivotal tc Server Developer is already in use

<https://blog.csdn.net/blackwoodcliff/article/details/50504002>

STS: Port 8080 required by Pivotal tc Server Developer is already in use
问题描述
使用 STS 开发 Spring MVC 程序，使用默认的 Pivotal tc Server 时，有时会出现 8080 端口被占用的问题。比如，我的机器上已经运行了一个 Tomcat，使用的是 8080 端口，此时再启动 Pivotal tc Server，就会出现此问题。截图如下： 

解决办法
打开 Servers 窗口 


在 Servers 窗口里，在 Pivotal tc Server Developer Edition 上点击鼠标右键，选择 Open 项 


在打开的窗口里，修改 bio.http.port 的值，比如改成 8081 


Ctrl+S 保存后，重新启动 Pivotal tc Server 就不报错了。
--------------------- 
作者：blackwood-cliff 
来源：CSDN 
原文：https://blog.csdn.net/blackwoodcliff/article/details/50504002 
版权声明：本文为博主原创文章，转载请附上博文链接！