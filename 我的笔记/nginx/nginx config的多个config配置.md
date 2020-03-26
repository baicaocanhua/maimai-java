在我们的一台服务器上,一个nginx服务器下面可能跑着许多许多的项目;

那么就需要配置多个对应的配置 端口号 已经文件入库目录等等

那么项目多了以后,把这些项目都写到一个文件里 到后期难以查看与管理

我们只需要新建一个文件夹,下面全部存放 我们的子配置 然后在主配置中把这个子目录引入即可

![img](https://images2018.cnblogs.com/blog/1176981/201807/1176981-20180721120721301-662095376.png)

然后我们的主配置文件如下

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
user  www www;
worker_processes  8;

error_log  /www/logs/error.log info;
#access_log  /www/logs/nginx.access.log  main;
pid        /var/run/nginx.pid;


worker_rlimit_nofile 51200;

events {
    use epoll;
    worker_connections  51200;
}


http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;


log_format  main '"$time_local","$remote_addr","$http_x_forwarded_for","$http_host","$request","refer:$http_referer","$http_user_agent","$status","$request_time","$upstream_response_time","$body_bytes_sent","$upstream_addr","$upstream_status","$upstream_response_time","$http_cookie_pgv_pvi","$request_body","$uid_got"';


    server_names_hash_bucket_size 128;
    client_header_buffer_size 128k;
    large_client_header_buffers 4 128k;
    client_max_body_size 100m;
client_body_buffer_size 1024k;

    sendfile        on;
    tcp_nopush     on;

    keepalive_timeout  30;
    tcp_nodelay on;


  fastcgi_intercept_errors on;
  fastcgi_connect_timeout 300;
  fastcgi_send_timeout 300;
  fastcgi_read_timeout 300;
  fastcgi_buffer_size 128k;
user  www www;
worker_processes  8;

error_log  /www/logs/error.log info;
#access_log  /www/logs/nginx.access.log  main;
pid        /var/run/nginx.pid;


worker_rlimit_nofile 51200;

events {
    use epoll;
    worker_connections  51200;
}


http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;


log_format  main '"$time_local","$remote_addr","$http_x_forwarded_for","$http_host","$request","refer:$http_referer","$http_user_ag
ent","$status","$request_time","$upstream_response_time","$body_bytes_sent","$upstream_addr","$upstream_status","$upstream_response
_time","$http_cookie_pgv_pvi","$request_body","$uid_got"';


    server_names_hash_bucket_size 128;
    client_header_buffer_size 128k;
    large_client_header_buffers 4 128k;
    client_max_body_size 100m;
client_body_buffer_size 1024k;

    sendfile        on;
    tcp_nopush     on;

    keepalive_timeout  30;
    tcp_nodelay on;


  fastcgi_intercept_errors on;
  fastcgi_connect_timeout 300;
  fastcgi_send_timeout 300;
  fastcgi_read_timeout 300;
  fastcgi_buffer_size 128k;
  fastcgi_buffers 4 128k;
  fastcgi_busy_buffers_size 128k;
  fastcgi_temp_file_write_size 128k;

    gzip  on;
    gzip_min_length  1k;
    gzip_buffers     4 16k;
    gzip_http_version 1.0;
    gzip_comp_level 2;
    gzip_types       text/plain application/x-javascript text/css application/xml image/jpeg image/gif image/png;
    gzip_vary on;

    #server {
#       listen 80;
#       server_name  localhost;
 #       location / {
#           proxy_next_upstream http_502 http_504 http_404 error timeout invalid_header;
 #           proxy_pass http://78list.cn;
            #proxy_set_header Host www.yourdomain.com;
  #          proxy_set_header X-Forwarded-For $remote_addr;
   #     }

    #}
    #upstream 78list.cn {
    #    server 192.168.8.113:8080;
    #}  

    include /etc/nginx/default.conf;
    include /etc/nginx/upstream/*.conf;
    include /etc/nginx/conf.d/*.conf; // 这里就是引入的子配置文件夹
}
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

一个范例 子配置的conf

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
server
  {
    listen       832; // 端口号
    server_name localhost; // 域名
    index index.html index.htm index.php;
    root  /home/www/ai/crm/web/public; //项目的入口文件夹

        location ~ /.svn/ {
        deny all;
    }


    location / {
        rewrite ^/$ /index.php last;
        rewrite ^/(?!index\.php|index\.html|layui|css|js|bootstrap|robots\.txt)(.*)$ /index.php/$1 last;
    }

   location ~ \.php {
                fastcgi_pass 127.0.0.1:9002;
                fastcgi_index index.php;
                fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
                include fastcgi_params;
        }


    location ~/uploads/.*\.(php|php5)?$ {
        deny all;
    }
    location ~/public/.*\.(php|php5)?$ {
        deny all;
    }

   location ~ .*\.(gif|jpg|jpeg|png|bmp|swf)$
    {
      expires      30d;
    }

    location ~ .*\.(js|css)?$
   {
      expires      8d;
    }


    #access_log  /www/logs/access.log  main;

  }
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

 





标签: [ngin](https://www.cnblogs.com/djwhome/tag/nginx/)