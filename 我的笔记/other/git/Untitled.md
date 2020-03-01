# [error: src refspec XXX matches more than one](https://www.cnblogs.com/softidea/p/6415343.html)



 

```
error: dst refspec v1.0 matches more than one.
error: failed to push some refs to ''
```

错误原因是 branch名和tag名有相同的，在执行git push origin :branchName时，就会报上面的错

删除branch：

```
git branch -r -d origin/branch-name  //只能使用这个命令来删除branch，下面的命令不可以。因为同样是因为有 matches more than one
git push origin :branch-name
```

 

删除tag:

```
git tag -d tagName
```