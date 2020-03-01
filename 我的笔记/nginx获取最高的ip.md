# nginx统计访问量最高的ip

nginx统计访问量最高的ip

命令统计apache或nginx日志中访问最多的100个ip及访问次数，这个在以前做日志统计的时候经常用到

awk '{print $1}' 日志地址 | sort | uniq -c | sort -n -k 1 -r | head -n 100

经常用到日志最后一列：awk分割信息后获取最后一列
linux，shell，awk，最后一列

查资料，终于找到，取最后一列使用$NF，示例如下：

cat $(ll /home/sdzw/tcf/20110914_001/|awk '{print $NF}')|grep "abc"
--------------------- 
