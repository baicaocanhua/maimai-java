本来只是一个绝对url和相对url的简单问题，但实际使用中会碰到一些不常见的，比如双斜杠，经常不用竟然忘了，做一下总结。可以参考一下这篇文章

　　1.url前面是双斜杠（//mljr.com/car.html） 双斜杠是相对协议进行url转换的，如果当前页使用的是https协议，那么转换后的url就是https://mljr.com/car.html

　　2.url前面是单斜杠(/newcar.html)  单斜杠是相对服务器根目录进行url转换的

　　3.无斜杠和点+斜杠  对这两个目前认知是一样的，都是相对当前目录进行url转换的


/ 表示绝对路径，不管你这个文件在什么位置，比如文件在 根目录/aaa/cc/dd/cc/sample.html
/i/eg_tulip.jpg 表示图片在 根目录/i/eg_tulip.jpg
而如果不加/ 那就是 "i/eg_tulip.jpg " 表示图片在/aaa/cc/dd/cc/i/eg_tulip.jpg

---------------------
如在jsp页面引入js时候使用以下两个路径:

<script type="text/javascript" src="/js/jquery-3.2.1.js"></script>
<script type="text/javascript" src="js/jquery-3.2.1.js"></script>


比如当前jsp页面的路径是 http://localhost:8080/demo/page/test.jsp

加"/"代表 绝对路径,是从站点的根目录开始找 http://localhost:8080/js/jquery-3.2.1.js

不加"/"代表 相对路径,是从当前路径开始找 http://localhost:8080/demo/page/js/jquery-3.2.1.js
---------------------------

----------------
SpringMVC路径问题回顾，加斜杠和不加斜杠的问题(六)
https://www.cnblogs.com/suanshun/p/6699891.html

https://www.cnblogs.com/suanshun/p/6699891.html
http://www.360doc.com/content/19/0415/12/19913882_828920907.shtml