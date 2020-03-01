k 上
j 下
l 左
h 右


u 撤销
yy复制一行
dd 删除光标所在行
D删除到行末x删除当前字符
Y 复制到行末

gg 跳至文首
G 调至文尾
5gg/5G 调至第5行

p 粘贴粘贴板的内容到当前行的下面
P 粘贴粘贴板的内容到当前行的上面

J 将下一行和当前行连接为一行





```
三元表达式

  which cowsay>/dev/null && echo "exist" || echo "not exist" 



 which cowsay>/dev/nul，返回1 后面不执行还是1，执行 echo "not exist" 



查询上一条命令的执行结果【成功返回0，否则为1】

echo $?

&&

前面的命令 执行结果为0 ，则执行&&后面的

 ||

前面的命令 执行结果为1 ，则执行 ||后面的
```



cut 命令，打印每一行的某一字段

 打印`/etc/passwd`文件中以`:`为分隔符的第1个字段和第6个字段分别表示用户名和其家目录 

 cut /etc/passwd -d ':' -f 1,6 