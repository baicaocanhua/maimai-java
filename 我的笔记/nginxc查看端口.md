netstat -ntpl

启动nginx服务，无法正常启动，一查log日志，发现如题错误信息。

问题描述：地址已被使用。可能nginx服务卡死了，导致端口占用，出现此错误。

查看端口

netstat -ntpl

![img](https://img2018.cnblogs.com/blog/897885/201811/897885-20181113235057377-664795968.png)

杀掉进程   kill  -9  19588

 

重启ng

./sbin/nginx

 

服务正常。







敏感头信息Authorization,Cookie,Set-Cookie默认是不转发的，也就获取不到
在配置文件里设置zuul.sensitiveHeaders为空，就可以获取到了