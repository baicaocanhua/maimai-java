# 你知道Forward和Redirect的原理和区别吗

https://cloud.tencent.com/developer/news/102662





# 重定向 return "redirect:/user/index";

https://blog.csdn.net/kevin_loving/article/details/80521197





# url中的jsessionid所引起的问题和解决

https://blog.csdn.net/zshake/article/details/37658147



# [url中的jsessionid解释](https://www.cnblogs.com/huzi007/p/4137045.html)  ************



# 对于url出现jsessionid问题

```
当时用<c:redirect>进行页面重定向时，浏览器地址栏会出现jsessionid，这是因为redirect会去检查是否有cookie，如果没有则url上添加jsessionid以便浏览器和服务器保持通话。

其实倒也没什么，但是毕竟地址栏出现这一串东西不太好看，其实是可以解决掉的。

方法一：<% response.sendRedirect("front/index.html"); %>

方法二：tomcat在上下文配置当中加入disableURLRewriting=true语句。
--------------------- 
作者：cuixuefeng1112 
来源：CSDN 
原文：https://blog.csdn.net/cuixuefeng1112/article/details/44754439 
版权声明：本文为博主原创文章，转载请附上博文链接！
```

