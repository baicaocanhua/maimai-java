reset --hard：重置stage区和工作目录

reset --hard 会在重置 HEAD 和branch的同时，重置stage区和工作目录里的内容
git reset --hard HEAD^


reset --soft：保留工作目录，并把重置 HEAD 所带来的新的差异放进暂存区
reset --soft 会在重置 HEAD 和 branch 时，保留工作目录和暂存区中的内容，并把重置 HEAD 所带来的新的差异放进暂存区。


dependencyManagement
《maven实战》 第8章 聚合与继承 了解一下

布隆过滤器(BloomFilter)或者压缩filter提前拦截

thradlocal

事务

把队列的异步返回改成同步

发送方
使用redis把队列的异步返回改成同步 - 队列使用
https://blog.csdn.net/xieye114/article/details/84915416

MQ 队列的异步转同步问题
http://blog.itpub.net/21634752/viewspace-670296/


如何保证消息队列的高可用和幂等性以及数据丢失，顺序一致性
https://www.jianshu.com/p/7a6deaba34d2

echo nameserver 8.8.8.8 > /etc/resolv.conf  
echo nameserver 8.8.4.4 > /etc/resolv.conf  

这两行命令直接将8.8.8.8与8.8.4.4写入Linux的DNS客户端解析文件resolv.conf里。