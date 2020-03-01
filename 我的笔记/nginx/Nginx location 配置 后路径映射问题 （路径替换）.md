没有斜杠 追加

有斜杠     替换



nginx 配置 proxy_pass时可以实现URL路径的部分替换。

1.proxy_pass的目标地址，默认不带/，表示只代理域名，url和querystring部分不会变（把请求的path拼接到proxy_pass目标域名之后作为代理的URL）

2.如果在目标地址后增加/，则表示把path中location匹配成功的部分剪切掉之后再拼接到proxy_pass目标地址

例子：

server {
    access_log  /home/access.log;
    error_log   /home/error.log;
        server_name h5.xxx.com;
        location  /abc {
                proxy_pass http://server_url;
        }

       location  /abc {
                proxy_pass http://server_url/;
        }
 }

比如请求 /abc/b.html

如上两个匹配成功后，实际代理的目标url分别是

http://server_url/abc/b.html (把/abc/b.html拼接到http://server_url之后)

http://server_url/b.html (把/abc/b.html的/abc去掉之后，拼接到http://server_url/之后)
————————————————
版权声明：本文为CSDN博主「UTF杠8」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/qq_29298577/article/details/85050862