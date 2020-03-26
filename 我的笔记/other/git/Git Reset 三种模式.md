# Git Reset 三种模式

https://www.jianshu.com/p/c2ec5f06cf1a





# Git恢复之前版本的两种方法reset、revert（图文详解）

https://blog.csdn.net/yxlshk/article/details/79944535





**git reset --hard 目标版本号**



此时如果用“git push”会报错，因为我们本地库HEAD指向的版本比远程库的要旧：

所以我们要用“git push -f”强制推上去，就可以了：



**git revert -n 版本号”反做，并使用“git commit -m 版本名”提交：**