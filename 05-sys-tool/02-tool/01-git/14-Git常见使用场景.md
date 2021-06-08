## 个人本地操作

### 删除不需要的分支

```shell
git branch -a # 查看有什么分支
git branch -d [分支名|分支hash] # 删除分支
git branch -D [分支名|分支hash] # 强制删除未merge的分支
```

### 修改最新commit的message

```shell
git commit --amend
```

### 修改老旧commit的message

```shell
git rebase -i # 选择 reword r
```

### 把连续的commit合并为一个

```shell
git rebase -i # 选择 squash s
```

### 把间隔的commit合并为一个

```shell
git rebase -i # 选择 squash s，改变顺序，让其合并的commit变为连续
```

### 对比工作区和暂存区差异

```shell
git diff --cache
```

### 对比工作区与暂存区差异

```shell
git diff
```

### 正确删除文件的方法

```shell
git rm
```

### 正确重命名文件的方法

```shell
git mv
```

### 开发中临时加塞了紧急任务

```shell
git stash
git pop / apply
```

### 指定不需要Git管理的文件

设置`.gitignore`文件

### 查看不同提交的指定文件的差异

```shell
git diff [版本号|tag] [版本号|tag] --[fileName]
```

### 消除最近提交（删除最近的commit）

```shell
git reset --hard [版本]
```

### 在工作区中删除了文件，想从暂存区恢复

```shell
# 先查看状态
git status

# 使用 checkout
git checkout -- [文件名]
```

### 如果本地和远程库的同名分支具有不同的提交历史，默认情况下不能合并

比如在`github`上新建了一个仓库（里面有生成的 README.md），而本地又有一个`git`仓库，里面已经有提交了，现在本地库与远程库关联

这时，设置可以允许不相关历史提交

```shell
git pull origin master --allow-unrelated-histories
```



## 远程协作

### 不同人修改了不同文件

1. 不会冲突
2. 先 pull
3. 再 merge

### 不同人修改了相同文件的不同区域

1. Git会自动合并，会弹出一个对话框，告知用户已经自动合并

2. 如果不用修改commit 内容，则直接 `:wq` 保存退出即可

3. 最后push即可

### 不同人修改了相同文件的相同区域

1. 发生冲突，需要手动解决
2. Git会提示发生冲突的文件，编辑修改完成后
3. 使用 `git status` 观察一下
4. 进行选择
   1. 决定合并：`git add / commit / push`
   2. 放弃合并：`git merge --abort`：恢复之前两个分支各自的状态

### 同时修改了文件名和文件内容如何处理

1. Git能自动感应到文件名变更，会弹出一个对话框，告知用户已经自动合并
2. 如果不用修改commit 内容，则直接 `:wq` 保存退出即可

### 把同一文件改为了不同文件名

1. 发生冲突

2. 使用 `git rm` 删除原名文件

   ```shell
   git rm index.htm
   ```

3. 使用 `git add` 添加选择的文件

   ```shell
   git add index1.html
   ```

4. 最后 `git commit / push`

