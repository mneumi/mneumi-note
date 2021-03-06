## 远程仓库概述

### 远程仓库介绍

远程仓库本质是一台计算机，该计算机上存储了版本数据

### 远程仓库分类

| 分类   | 例子                                |
| ------ | ----------------------------------- |
| 局域网 | 自建 git 服务器，如 gitlab          |
| 互联网 | github \| gitlab \| gitee \| coding |



## 远程仓库管理

### 查看本地库连接的远程仓库列表

```shell
git remote
git remote -v
```

### 将本地库连接远程仓库

```shell
git remote add [远程库名] [http协议地址|ssh协议地址]
```

### 删除本地库连接的远程仓库

```shell
git remote remove [远程库名]
```



## 远程库 => 本地库

### 克隆整个远程仓库

```shell
git clone [http协议地址] # 通过HTTP协议
git clone [ssh协议地址]  # 通过SSH协议
```

### 克隆远程仓库某个分支

```shell
git clone -b [远程库分支名] [http协议地址] # 通过HTTP协议
git clone -b [远程库分支名] [ssh协议地址]  # 通过SSH协议
```

### 拉取远程仓库分支到本地库分支

#### 完整写法

拉取后，本地库分支与远程库分支将建立**追踪关系**

```shell
git fetch [远程库名] [远程库分支名]:[本地库分支名]
```

#### 省略写法

如果省略本地库分支名，则会拉取到本地与远程库分支同名的分支上，如果不存在则创建，并建立**追踪关系**

```shell
git fetch [远程库名] [远程库分支名]
```

### 拉取远程仓库分支到本地库分支并合并到工作区

```shell
git pull [远程库名] [远程库分支名]:[本地库分支名]
```

`git push` = `git fetch` + `git merge`

```shell
git pull [远程库名] [远程库分支名]:[本地库分支名]
# 等价于下面两个命令
git fetch [远程库名] [远程库分支名]:[本地库分支名]
git merge [本地库分支名] # 注意分支是可以merge自己的，虽然没有效果，但是能不会报错
```

### 基于远程仓库分支，创建出本地一条本地分支

```shell
git checkout -b [本地库分支] [远程库分支]
```

### 限制克隆深度

使用 `git clone --depth=num` 能够限制克隆的深度，因此可以大大加速克隆速度

 `git clone --depth=1` 表示只克隆最近一次的commit，使用场景：只想clone最新版本来使用或学习，而不是要参与开发的操作



## 本地库 => 远程库

### 将本地库推送到远程库

#### 完整写法

一旦推送，本地库分支将与远程库分支建立起**追踪关系**

```shell
git push [远程库名] [本地库分支名]:[远程库分支名]
```

#### 省略写法1

如果省略**远程库分支名**，则表示将本地分支推送与之存在**追踪关系**的远程分支（通常两者同名），如果该远程分支不存在，则会被**自动新建并建立追踪关系**

```shell
git push [远程库名] [本地库分支名]
```

#### 省略写法2

如果当前分支与远程分支之间存在追踪关系，则本地分支和远程分支都可以省略

```shell
git push [远程库名]
```

#### 省略写法3

如果当前分支只有一个追踪分支，那么主机名都可以省略

```shell
git push
```

#### 省略写法4

如果当前分支与多个主机存在追踪关系，则可以使用-u选项指定一个默认主机

```shell
git push -u [远程库名]
```

配合上述的省略写法，最后可以实现这样的效果

```shell
# 首次推送，设置默认推送的远程仓库和分支
git push -u [远程库名] [本地分支名]:[远程库分支名]
# 以后如果要推送到该远程仓库和分支，则不用再再指明远程仓库和分支
git push # 等价于 git push -u [远程库名] [本地分支名]:[远程库分支名]
```

注意事项：在推送前，本地库必须拥有远程库的最新的修改，否则不能推送（需保证能够fast-forwards），即推送前一般需要使用`git pull`命令将远程库拉取到本地库

```shell
git pull [远程库名] [远程库分支名]:[本地分支名]
git push [远程库名] [本地分支名]
```

### 删除远程分支

```shell
git push [远程库名] --delete [远程库分支名]
# 等同于
git push [远程库名] :[远程库分支名] # 推送一个空的本地分支到远程分支
```

### 禁止执行的远程仓库命令

1. 禁止向远程仓库使用 `git push -f`命令

   该操作可能会造成远程仓库的版本丢失

2. 禁止向已提交到远程仓库执行rebase操作

   会造成版本混乱，非常危险，非常难解决



## 远程仓库免密登录

| 方式                                | 说明                                                     |
| ----------------------------------- | -------------------------------------------------------- |
| 添加远程库时在URL中加入用户名和密码 | 可以在配置文件更改remote地址                             |
| 使用操作系统管理凭据                | 使用HTTP协议，输入账号密码后，自动将账号密码存进系统中了 |
| 使用SSH协议连接                     | 需要在远程仓库中加入SSH公钥                              |

远程库时在URL中加入用户名和密码示例

```shell
git remote add origin https://用户名:密码@github.com/xxx/xxx.git
```
