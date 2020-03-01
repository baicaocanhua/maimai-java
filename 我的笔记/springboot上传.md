## [SpringBoot文件上传异常之提示The temporary upload location xxx is not valid](https://www.cnblogs.com/yihuihui/p/10372887.html)





application.properties 配置文件中添加`spring.http.multipart.location=手动指定一个临时目录`属性，注意：目录需要手动创建



在tomcat中添加 basedir 给他提供文件上传临时路径

======================================== 
application.yml中加入 
tomcat: 
remote-ip-header: x-forward-for 
uri-encoding: UTF-8 
max-threads: 10 
basedir: ${user.home}/deployer/tomcat

因为我的项目的微服务的。所以配置加在zuul
--------------------- 
1 出错原因
临时文件夹无效

在springboot项目启动后，系统会在‘/tmp’目录下自动的创建几个目录：

   1，tomcat.************.8080,(结尾是项目的端后)

   2，tomcat-docbase.*********.8080。
1
2
3
Multipart（form-data）的方式处理请求时，默认就是在第二个目录下创建临时文件的。

2 解决办法
重启spring boot

修改tomcat启动配置
添加-Djava.io.tmpdir=

java -Djava.io.tmpdir=./temp -jar fdbserver-0.0.1-SNAPSHOT.jar -Dspring.config.location=file:./application.properties 
1
添加配置
/**
 * 文件上传临时路径
 */
 @Bean
 MultipartConfigElement multipartConfigElement() {
    MultipartConfigFactory factory = new MultipartConfigFactory();
    factory.setLocation("/app/tmp");
    return factory.createMultipartConfig();
    }
---------------------
