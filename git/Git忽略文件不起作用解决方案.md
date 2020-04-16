开发过程中由开发工具生成的文件一般不需要提交，但每次开发工具会自动去修改这些文件，每次都要去提交这些东西，不提交会有一系列问题，很烦人。
 可以通过配置.gitignore文件让Git不在跟踪记录这些文件。心血来潮去配置的时候，发现配置过的文件并没有生效，伤感万分，别担心有解决方案。先找原因，因为.gitignore只能忽略那些原来没有被track的文件，如果某些文件已经纳入版本管理中，则修改.gitignore不会生效。解决办法就是先把本地缓存删除（改成未track状态），然后再提交”：

1 到相应文件夹中右键"Git Bash Here"
 2 输入一下代码

```
git rm -r --cached .
---修改.gitignore文件，添加自己要忽略的规则
git add .
git commit -m "update .gitignore"
```





https://shiyousan.com/post/636470505667009340/

# 解决.gitignore文件忽略规则无效git依然跟踪修改的问题





[https://git-scm.com/book/zh/v2/Git-%E5%9F%BA%E7%A1%80-%E8%AE%B0%E5%BD%95%E6%AF%8F%E6%AC%A1%E6%9B%B4%E6%96%B0%E5%88%B0%E4%BB%93%E5%BA%93](https://git-scm.com/book/zh/v2/Git-基础-记录每次更新到仓库)





```
解决方法: git清除本地缓存（改变成未track状态），然后再提交:
[root@kevin ~]``# git rm -r --cached .
[root@kevin ~]``# git add .
[root@kevin ~]``# git commit -m 'update .gitignore'
[root@kevin ~]``# git push -u origin master
```



https://www.cnblogs.com/rainbowk/p/10932322.html

# [Git忽略规则(.gitignore配置）不生效原因和解决](https://www.cnblogs.com/rainbowk/p/10932322.html)



# git如何忽略已经提交的文件 (.gitignore文件无效)

https://www.jianshu.com/p/e5b13480479b