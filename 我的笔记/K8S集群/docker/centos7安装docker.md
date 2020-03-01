```
温馨提醒，docker内核版本必须是3.10+以上的版本

查看方式

uname -r

 

1. 卸载老版本的 docker 及其相关依赖

sudo yum remove docker docker-common container-selinux docker-selinux docker-engine

2，更新yum

yum update
1
​ 3. 安装 yum-utils，它提供了 yum-config-manager，可用来管理yum源

sudo yum install -y yum-utils
1
​ 4. 添加yum源

sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
1
​ 5. 更新索引

sudo yum makecache fast
1
​ 6. 安装 docker-ce

sudo yum install -y docker-ce
1
​ 7. 启动 docker

sudo systemctl start docker
1
​ 8. 验证是否安装成功

sudo docker info
————————————————
版权声明：本文为CSDN博主「qq_25760623」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/qq_25760623/article/details/88657491
```





```
最近公司使用了docker容器化部署项目，在安装完成后去搜索镜像资源出了这个问题，百度看了很多解决方案都不行 。

开始以为是镜像加速器没配好，就试着重新配置镜像加速器、重装了docker，折腾了一番依然没有解决。

具体错误信息：

1
2
[root@localhost ~]# docker search java
Error response from daemon: Get https://index.docker.io/v1/search?q=java&n=25: dial tcp: lookup index.docker.io on 192.168.2.1:53: read udp 192.168.2.189:35574->192.168.2.1:53: i/o timeout
　　

最后发现是机器网络配置出了问题，解决方案：

 

查看服务器DNS网络配置

vi /etc/resolv.conf
把里面的内容清除，并改为：

1
2
nameserver 8.8.8.8
nameserver 8.8.8.4
重启网络服务

1
systemctl restart network
 

按照上述步骤便可完美解决。
```

