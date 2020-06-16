# [git 修改远程分支名称](https://www.cnblogs.com/huting-front/p/12106578.html)

首先 git branch -m 旧分支名 新分支名

其次 git push --delete origin 旧分支名

将新分支名推上去 git push origin 新分支名

将新本地分支和远程相连 git branch --set-upsteam-to origin/新分支名



git fetch -p
git remote prune origin  清理远程分支，把本地不存在的远程分支删除

在从公用的仓库fetch代码：git fetch origin dev20181018:dev20181018



```
操作：
    1. git checkout 要改的本地分支名 (在本地切换成要重命名的分支）
    2. git branch -m 要改的本地分支名 修改后的分支名(修改本地分支)
    3. git push origin :远程修改前的分支名（删除远程分支）
    4. git push origin 修改后的分支名（push 到远程分支）
    5. git branch --set-upstream-to=origin/远程分支名 本地分支名(绑定分支)
1
2
3
4
5
举例：develop → stable
    git checkout develop
    git pull
    git branch -m develop stable
    git push origin :develop
    git push origin stable
    git branch --set-upstream-to=origin/stable stable
————————————————
版权声明：本文为CSDN博主「Mr小墨」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/wy0682109523/article/details/99451672
```



场景：将分支名称为 oldbranch 改为 newbranch 

步骤：

1、将本地分支oldbranch切一个分支到本地

   git branch -m oldbranch newbranch



2、删除远程分支

　  git push --delete origin oldbranch

3、将本地新分支推送到远程

　 git push origin newbranch