# git rebase

##  <font color="green">如何合并多次提交纪录？</font>

1. 我们来合并最近的 4 次提交纪录，执行：
```bash
git rebase -i HEAD~4
```

2. 这时候，会自动进入 vi 编辑模式：
```bash
pick 191b24c29d [SPARK-34672][BUILD][2.4] Fix docker file for creating release
pick 7985360cc2 [SPARK-34507][BUILD] Update scala.version in parent POM when changing Scala version for cross-build
pick 906df15f81 [SPARK-34703][PYSPARK][2.4] Fix pyspark test when using sort_values on Pandas
pick 6ec74e2c96 [SPARK-34696][SQL][TESTS] Fix CodegenInterpretedPlanTest to generate correct test cases

# Rebase eb4601e7e9..6ec74e2c96 onto eb4601e7e9 (4 commands)
#
# Commands:
# p, pick <commit> = use commit
# r, reword <commit> = use commit, but edit the commit message
# e, edit <commit> = use commit, but stop for amending
# s, squash <commit> = use commit, but meld into previous commit
# f, fixup <commit> = like "squash", but discard this commit's log message
# x, exec <command> = run command (the rest of the line) using shell
# b, break = stop here (continue rebase later with 'git rebase --continue')
# d, drop <commit> = remove commit
# l, label <label> = label current HEAD with a name
# t, reset <label> = reset HEAD to a label
# m, merge [-C <commit> | -c <commit>] <label> [# <oneline>]
# .       create a merge commit using the original merge commit's
# .       message (or the oneline, if no original merge commit was
# .       specified). Use -c <commit> to reword the commit message.
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
```

**有几个命令需要注意一下：**

- p, pick = use commit
- r, reword = use commit, but edit the commit message
- e, edit = use commit, but stop for amending
- s, squash = use commit, but meld into previous commit
- f, fixup = like “squash”, but discard this commit’s log message
- x, exec = run command (the rest of the line) using shell
- d, drop = remove commit

> 注意不要合并先前提交的东西，也就是已经提交远程分支的纪录。

3. 如果你异常退出了 vi 窗口，不要紧张：

```bash
git rebase --edit-todo
```

这时候会一直处在这个编辑的模式里，我们可以回去继续编辑，修改完保存一下：
```bash
git rebase --continue
```

4. 查看结果

```bash
git log
```

> 三次提交合并成了一次，减少了无用的提交信息。

## <font color="green">rebase场景二</font>

1. 我们先从 master 分支切出一个 dev 分支，进行开发：
```bash
git checkout -b feature1
```

![feature](./picture\git_rebase_1-1615452483331.png)

2. 这时候，你的同事完成了一次 hotfix，并合并入了 master 分支，此时 master 已经领先于你的 feature1 分支了：

![](./picture\git2.png)