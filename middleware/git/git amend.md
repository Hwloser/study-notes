# git amend

## <font color="green">preface</font>

- 当提交代码之后，发现有一个地方改错，且下一次提交的时候并不想保留上一次的提交记录。
- 上一次的commit message描述有误。

此时你可以使用`git commit --amend`。

## <font color="green">useage</font>

```bash
comment

# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# Date:      Wed Mar 17 10:34:19 2021 +0800
#
# On branch main
# Your branch is up to date with 'origin/main'.
#
# Changes to be committed:
# 。。。。。。。。。。。。
# Untracked files:
# 。。。。。。。。。。。。
```

## <font color="red">TIPS</font>

**该操作会改变你原来的commit id。**