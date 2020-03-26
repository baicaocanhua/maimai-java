nginx 自动加端口





地址后面一定要加斜杠





# 如何解决nginx自动加端口的问题



路径后面要加斜杠 ******

#### 目前后面不加左斜杠的url，就有问题



前端nginx监听一个80端口，反向代理到一个后端服务器，后端服务器用nginx监听非80端口8001，这时候，如果访问如www.centos.bz/test  ，目前后面不加左斜杠的url，就会置身到www.centos.bz:8001/test/，这问题如何解决？





在配置文件server中加port_in_redirect off;



proxy_set_header Host $host;



# nginx的port_in_redirect配置

https://www.jianshu.com/p/8e262d10ce64













# nginx反向代理后，jsp页面request.getServerPort()获取得端口号总是80解决方案   *************

https://blog.csdn.net/wudinaniya/article/details/83108956