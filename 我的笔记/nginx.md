# nginx简单配置多个server

<https://blog.csdn.net/everljs/article/details/80818733>

1：安装nginx步骤就不说了 ，自行百度。



2：打开nginx的配置文件nginx.conf

![img](https://img-blog.csdn.net/20180626175621716?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0V2ZXJMSlM=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

这是项目1的配置，现在需要再开个同域名不同端口的项目，如下图：

![img](https://img-blog.csdn.net/20180626175816773?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0V2ZXJMSlM=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)



注意：LZ一直出现访问不了，折腾了许久，是因为服务器www.pigaudio.com或120.77.223.7只开了默认的80端口，而8088端口并未开，所以只需要登陆你的服务账号添加一个8088即可，比如你的服务器是阿里云购买的，则需要登陆阿里云加一个8088，还有问题就是，如果你服务器打开了网络防火墙也是访问也是不行的，关了即可。
--------------------- 
# [在Nginx上配置多个站点](https://www.cnblogs.com/Erick-L/p/7066564.html)

有时候你想在一台服务器上为不同的域名运行不同的站点。比如www.siteA.com作为博客，www.siteB.com作为论坛。你可以把两个域名的IP都解析到你的服务器上，但是没法在Nginx的根目录里同时运行两个不同的网站。这时候，你就需要使用虚拟目录了。假设你把博客放在”/home/user/www/blog”下，论坛放在”/home/user/www/forum”下。下面我们就开始配置了：

- 在Nginx配置目录下，创建一个”vhost”目录。本例假设Nginx是默认安装，配置目录在”/etc/nginx”

```
$ sudo mkdir /etc/nginx/vhost
```

- 创建siteA的配置文件

```
$ sudo vi /etc/nginx/vhost/vhost_siteA.conf
```

- 输入以下配置信息

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
server {
    listen       80;                        # 监听端口
    server_name www.siteA.com siteA.com;    # 站点域名
    root  /home/user/www/blog;              # 站点根目录
    index index.html index.htm index.php;   # 默认导航页
 
    location / {
        # WordPress固定链接URL重写
        if (!-e $request_filename) {
            rewrite (.*) /index.php;
        }
    }
 
    # PHP配置
    location ~ \.php$ {
        fastcgi_pass unix:/var/run/php5-fpm.sock;
        fastcgi_index index.php;
        include fastcgi_params;
    }
}
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

- 同siteA一样创建siteB的配置文件，两者仅有的不同是”server_name”和”root”目录

```
$ sudo vi /etc/nginx/vhost/vhost_siteB.conf
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
server {
    ...
    server_name www.siteB.com siteB.com;    # 站点域名
    root  /home/user/www/forum;             # 站点根目录
    ...
}
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

- 打开nginx.conf文件

```
sudo vi /etc/nginx/nginx.conf
```

- 将虚拟目录的配置文件加入到”http {}”部分的末尾

```
http {
    ...
    include /etc/nginx/vhost/*.conf;
}
```

- 重启Nginx服务

```
$ sudo service nginx restart
```

- 现在访问www.siteA.com和www.siteB.com，你将发现浏览器会打开不同的站点

#### 禁止访问小技巧

假如你的Nginx根目录设在”/home/user/www”，你想阻止别人通过”http://IP地址/blog”或”http://IP地址/forum”来访问你的站点，最简单的方法就是禁止IP地址访问。方法如下：

1. 打开Nginx网站默认配置文件，记得先备份

```
$ sudo cp /etc/nginx/sites-available/default /etc/nginx/sites-available/default_bak
$ sudo vi /etc/nginx/sites-available/default
```

1. 将所有内容删除，只留以下配置

```
server {
    listen 80 default_server;
    server_name _;
    return 404;
}
```

1. 重启Nginx后，别人将无法通过IP地址访问网站了

**如果你不想禁止IP地址访问整个目录，只是要防止别人通过IP访问你的博客和论坛。那就需要禁止”/blog”和”/forum”的目录访问。**

1. 打开Nginx网站默认配置文件，同上面一样，记得先备份

1. 在”server { }”部分加上以下配置

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
location ^~ /blog/ {
    deny all;
}
location ^~ /forum/ {
    deny all;
}
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

1. 重启Nginx即可