列出远程Git分支按作者排序的committerdate：

git for-each-ref --format='%(committerdate) %09 %(authorname) %09 %(refname)' | sort -k5n -k2M -k3n -k4n
然后比如你想查看某个分支branch_A, 那么就再后面加上|grep branch_A

git for-each-ref --format='%(committerdate) %09 %(authorname) %09 %(refname)' | sort -k5n -k2M -k3n -k4n|grep branch_A
————————————————
版权声明：本文为CSDN博主「怠惰的小小白」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/qq_35462323/article/details/103905412







https://stackoverflow.com/questions/2255416/how-to-determine-when-a-git-branch-was-created



git reflog --date=local | grep <branchname>