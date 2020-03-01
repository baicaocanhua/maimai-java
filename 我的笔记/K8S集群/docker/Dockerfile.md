```
#基于java镜像
FROM java:8
#维护人的信息

#基于java镜像
FROM java:8

#维护人的信息
MAINTAINER maimai <330557864@qq.com>

ADD docker-springboot-0.0.1-SNAPSHOT.jar app.jar

#开启80端口
EXPOSE 8090

ENTRYPOINT ["java","-jar","app.jar"]

```

****







```
Dockerfile是包含一系列命令的文本文件，这个文件包含6条命令

1、FROM是使用php官方镜像，左边是镜像名字，右边是标签名字，标签名字不写默认是latest

2、声明维护人员

3、RUN运行一条linux命令，我们把php代码重定向到/tmp/index.php

4、EXPOSE声明要开放的端口

5、WORKDIR启动容器后默认目录

6、CMD容器启动后，默认执行的命令，相当于应用的入口，用php自带的webserver监听8000
```

```
1、编写Dockerfile
2、构建镜像
docker build --tag php:local_server .
3、运行容器
docker run -p 8000:8000 --name php_local_server php:local_server
使用docker run命令运行镜像，-p将容器的8000端口映射到本机8000端口，—name给容器起个名字

```

