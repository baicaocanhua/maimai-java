

# [GIT命令行统计代码提交行数](https://www.cnblogs.com/angryjj/p/11392053.html)

项目中遇到写报告的时候要反馈某个人或者某个功能的代码量，又没有集成CI这些插件，可以简单的用GIT命令统计下代码提交量:

**--统计某个人的提交代码**

```
git log --author=``"oldwang"` `--pretty=tformat: --numstat | ``gawk` `'{ add += $1 ; subs += $2 ; loc += $1 - $2 } END { printf "增加的行数:%s 删除的行数:%s 总行数: %s\n",add,subs,loc }'
```

**--统计某个人时间范围的提交代码**

```
git log --author=``"oldwang"` `--since=``'2019-04-01'` `--``until``=``'2019-04-07'` `--pretty=tformat: --numstat | ``gawk` `'{ add += $1 ; subs += $2 ; loc += $1 - $2 } END { printf "增加的行数:%s 删除的行数:%s 总行数: %s\n",add,subs,loc }'
```

**--统计每个人增删行数**

```
git log --``format``=``'%aN'` `| ``sort` `-u | ``while` `read` `name; ``do` `echo` `-en ``"$name\t"``; git log --author=``"$name"` `--pretty=tformat:
```