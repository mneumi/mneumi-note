## rebase

### rebase作用

1. 用于对某一段线性提交历史进行编辑、删除、复制、粘贴
2. 用于合并分支

### rebase特点

合理使用rebase命令可以使提交历史干净、简洁

### rebase注意事项

不要通过rebase对任何已经提交到公共远程仓库中的commit进行修改

### 使用场景1：将多个提交记录整合为一个提交记录

#### 理论基础

```shell
git rebase -i [基点版本号]
```

`[基点版本号]`是起点，它本身不会进入编辑区域，而是它后面的节点会进入编辑区域

| 选项   | 说明                                                         |
| ------ | ------------------------------------------------------------ |
| pick   | 保留该commit（缩写:p）                                       |
| reword | 保留该commit，但我需要修改该commit的注释（缩写:r）           |
| edit   | 保留该commit, 但我要停下来修改该提交(不仅仅修改注释)（缩写:e） |
| squash | 将该commit和前一个commit合并（缩写:s）**【最常用】**         |
| fixup  | 将该commit和前一个commit合并，但我不要保留该提交的注释信息（缩写:f） |
| exec   | 执行shell命令（缩写:x）                                      |
| drop   | 我要丢弃该commit（缩写:d）                                   |

#### 使用实例

最初情况：分支情况

```shell
$ git branch
  learn
* master
```

最初情况：提交历史

```shell
$ git log --graph --all
* commit 87e6fc9cf8535bbf11ac3f35430a1de212f309b5 (HEAD -> master)
| Author: mneumi
| Date:   Thu Sep 24 20:44:47 2020 +0800
|
|     update master 1
|
| * commit 4962e296bf980d5573fd78b0931533efd544cbd0 (learn)
| | Author: mneumi
| | Date:   Thu Sep 24 20:43:24 2020 +0800
| |
| |     update index.js 3
| |
| * commit cc217f203da42fa496226b1a94c39b02c5b15bb8
| | Author: mneumi
| | Date:   Thu Sep 24 20:43:04 2020 +0800
| |
| |     update index.js 2
| |
| * commit 207bd6fcbc667c8b8c6478974bf1f97d18938ad4
| | Author: mneumi
| | Date:   Thu Sep 24 20:42:28 2020 +0800
| |
| |     update index.js 1
| |
| * commit c7c18e7f38c706c4cf1c6c39fd42fd2bfb275fde
|/  Author: mneumi
|   Date:   Thu Sep 24 20:42:06 2020 +0800
|
|       init index.js
|
* commit 97d24784596470e54c3b76f6d959c037a83e3a68
  Author: mneumi
  Date:   Thu Sep 24 20:40:34 2020 +0800

      init master
```

准备rebase，选取变基点

```shell
$ git rebase -i c7c18e7f
```

进入编辑界面

```shell
pick 207bd6f update index.js 1
pick cc217f2 update index.js 2
pick 4962e29 update index.js 3

# Rebase c7c18e7..4962e29 onto c7c18e7 (3 commands)
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
```

使用 squash

```shell
pick 207bd6f update index.js 1
squash cc217f2 update index.js 2
squash 4962e29 update index.js 3

# Rebase c7c18e7..4962e29 onto c7c18e7 (3 commands)
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
```

设置 commit 信息

```shell
# This is a combination of 3 commits.

combine 3 commit

# This is the 1st commit message:

update index.js 1

# This is the commit message #2:

update index.js 2

# This is the commit message #3:

update index.js 3

# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# Date:      Thu Sep 24 20:42:28 2020 +0800
#
# interactive rebase in progress; onto c7c18e7
# Last commands done (3 commands done):
#    squash cc217f2 update index.js 2
#    squash 4962e29 update index.js 3
# No commands remaining.
# You are currently rebasing branch 'learn' on 'c7c18e7'.
#
# Changes to be committed:
#       modified:   index.js
#
```

查看结果

```shell
$ git log
commit 176c0272bf375342ae6613289201ccff2f88dc55 (HEAD -> learn)
Author: mneumi 
Date:   Thu Sep 24 20:42:28 2020 +0800

    combine 3 commit

    update index.js 1

    update index.js 2

    update index.js 3

commit c7c18e7f38c706c4cf1c6c39fd42fd2bfb275fde
Author: mneumi 
Date:   Thu Sep 24 20:42:06 2020 +0800

    init index.js

commit 97d24784596470e54c3b76f6d959c037a83e3a68
Author: mneumi 
Date:   Thu Sep 24 20:40:34 2020 +0800

    init master
```

### 使用场景2：让分支A的某段提交粘贴到分支B

#### 理论基础

进行变基

```shell
# 位于处理分支
git rebase [根分支]
```

合并分支

```shell
git checkout [根分支]
git merge [处理分支]
```

删除分支

```shell
git branch -d [处理分支]
```

#### 使用实例

最初情况：分支情况

```shell
$ git branch
  learn
* master
```

最初情况：提交历史

```shell
$ git log --graph --all
* commit d925b4f34f232001a0608f9ad8fcdcb1b7b63dcc (HEAD -> master)
| Author: mneumi 
| Date:   Thu Sep 24 21:28:16 2020 +0800
|
|     update master
|
| * commit 66b221c41392d479a41911ffae73c1cd4929fc6d (learn)
| | Author: mneumi 
| | Date:   Thu Sep 24 21:24:01 2020 +0800
| |
| |     update index.js 3
| |
| * commit 4f8add32f08c99bfdc831ad13bd69fcc73646f2c
| | Author: mneumi 
| | Date:   Thu Sep 24 21:21:45 2020 +0800
| |
| |     update index.js 2
| |
| * commit 8586b4c61d8dada46bc997cc63d167bd69a1386e
|/  Author: mneumi 
|   Date:   Thu Sep 24 21:21:30 2020 +0800
|
|       init index.js
|
* commit eb128c9e1daf5522cd1dad900b39bd3058efbc38
  Author: mneumi 
  Date:   Thu Sep 24 21:20:50 2020 +0800

      init master
```

切换到处理分支，并且rebase

```shell
$ git checkout learn
Switched to branch 'learn'

$ git branch
* learn
  master

$ git rebase master
Successfully rebased and updated refs/heads/learn.
```

查看日志

```shell
$ git log --graph --all
* commit 9e3918fd2f34ed7c299d8d341d189cbb5fa730a2 (HEAD -> learn)
| Author: mneumi 
| Date:   Thu Sep 24 21:24:01 2020 +0800
|
|     update index.js 3
|
* commit 12abb291deea3cba9318b5119e3d71d72fc66f14
| Author: mneumi 
| Date:   Thu Sep 24 21:21:45 2020 +0800
|
|     update index.js 2
|
* commit de5d30985cc0e95ad3b6fe8c8af66e056fa4fc0b
| Author: mneumi 
| Date:   Thu Sep 24 21:21:30 2020 +0800
|
|     init index.js
|
* commit d925b4f34f232001a0608f9ad8fcdcb1b7b63dcc (master)
| Author: mneumi 
| Date:   Thu Sep 24 21:28:16 2020 +0800
|
|     update master
|
* commit eb128c9e1daf5522cd1dad900b39bd3058efbc38
  Author: mneumi 
  Date:   Thu Sep 24 21:20:50 2020 +0800

      init master
```

合并分支

```shell
$ git checkout master
Switched to branch 'master'

$ git merge learn
Updating d925b4f..9e3918f
Fast-forward
 index.js | 3 +++
 1 file changed, 3 insertions(+)
 create mode 100644 index.js
```

查看日志

```shell
$ git log --graph --all
* commit 9e3918fd2f34ed7c299d8d341d189cbb5fa730a2 (HEAD -> master, learn)
| Author: mneumi 
| Date:   Thu Sep 24 21:24:01 2020 +0800
|
|     update index.js 3
|
* commit 12abb291deea3cba9318b5119e3d71d72fc66f14
| Author: mneumi 
| Date:   Thu Sep 24 21:21:45 2020 +0800
|
|     update index.js 2
|
* commit de5d30985cc0e95ad3b6fe8c8af66e056fa4fc0b
| Author: mneumi 
| Date:   Thu Sep 24 21:21:30 2020 +0800
|
|     init index.js
|
* commit d925b4f34f232001a0608f9ad8fcdcb1b7b63dcc
| Author: mneumi 
| Date:   Thu Sep 24 21:28:16 2020 +0800
|
|     update master
|
* commit eb128c9e1daf5522cd1dad900b39bd3058efbc38
  Author: mneumi 
  Date:   Thu Sep 24 21:20:50 2020 +0800

      init master
```

删除处理分支

```shell
$ git branch -d learn

$ git branch
* master
```

查看日志

```shell
$ git log --graph --all
* commit 9e3918fd2f34ed7c299d8d341d189cbb5fa730a2 (HEAD -> master)
| Author: mneumi 
| Date:   Thu Sep 24 21:24:01 2020 +0800
|
|     update index.js 3
|
* commit 12abb291deea3cba9318b5119e3d71d72fc66f14
| Author: mneumi 
| Date:   Thu Sep 24 21:21:45 2020 +0800
|
|     update index.js 2
|
* commit de5d30985cc0e95ad3b6fe8c8af66e056fa4fc0b
| Author: mneumi 
| Date:   Thu Sep 24 21:21:30 2020 +0800
|
|     init index.js
|
* commit d925b4f34f232001a0608f9ad8fcdcb1b7b63dcc
| Author: mneumi 
| Date:   Thu Sep 24 21:28:16 2020 +0800
|
|     update master
|
* commit eb128c9e1daf5522cd1dad900b39bd3058efbc38
  Author: mneumi 
  Date:   Thu Sep 24 21:20:50 2020 +0800

      init master
```