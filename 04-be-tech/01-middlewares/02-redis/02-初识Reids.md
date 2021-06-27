## 初识Redis

### 概述

Redis 是一个开源（BSD许可）的，内存中的Key-Value存储系统，它可以用作数据库、缓存和消息中间件

官方网站：https://redis.io

中文网站：http://redis.cn

### 特点

| 特点             | 说明                                                         |
| ---------------- | ------------------------------------------------------------ |
| 开源免费         | 仓库地址：                                                   |
| 支持多种编程语言 | 支持如 Go，Java，C/C++，JS 等多种主流语言                    |
| 高性能           | 数据存储在内存中；用C语言编写； <br />单线程模型：一次只执行一条命令，避免线程切换和竞争带来的性能消耗 |
| 持久化           | 支持RDB和AOF两种持久化方式                                   |
| 多种数据结构     | string, list, hash, set, sorted set<br />bitmap, hyperloglog, geo |
| 功能丰富         | 慢查询；pipeline；发布订阅模式                               |
| 主从复制         | redis-sentinel                                               |
| 分布式部署       | redis-cluster                                                |
| 实现简单         | 仅仅用C语言，5w行；不依赖第三方库；单线程模型                |

### 使用场景

| 使用场景     | 说明                     |
| ------------ | ------------------------ |
| 缓存系统     | 使用 key-value 键值对    |
| 计数器       | 使用 inceby 等原子操作   |
| 排行榜       | 使用 sorted set 数据类型 |
| 消息队列系统 | 使用发布订阅模式         |



## Redis下载与安装

### 源码编译方式

| 操作 | 说明                             |
| ---- | -------------------------------- |
| 下载 | https://redis.io/download        |
| 解压 | tar -zxvf redis.tar.gz           |
| 安装 | cd redis && make && make install |

### docker方式

```shell
docker run --name myredis -d -p 6379:6379 redis
```



## Redis基础概念

### Redis端口与数据库

默认端口：**6379**，来自于Alessia  **Merz**

数据库：默认16个数据库，类似数组下标从0开始，初始默认使用0号库

### Redis目录文件

| 文件名          | 功能              |
| --------------- | ----------------- |
| redis-server    | redis服务器       |
| redis-cli       | redis客户端       |
| redis-benchmark | redis性能测试工具 |
| redis-check-aof | AOF文件修复工具   |
| redis-check-rdb | RDB文件修复工具   |
| redis-sentinel  | sentinel服务器    |

### 启动与关闭Redis服务端

| 操作                 | 说明                                       |
| -------------------- | ------------------------------------------ |
| 最简启动（前台启动） | ${redis}/src/redis-server                  |
| 配置文件启动         | ${redis}/src/redis-server config_file.conf |
| 动态参数启动         | ${redis}/src/redis-server --port 6380      |
| 检查是否启动         | ps -ef \| grep redis                       |
| 关闭服务端           | ${redis}/src/redis-cli -p 6380 shutdown    |

### 启动与关闭Redis客户端

| 操作 | 说明                                     |
| ---- | ---------------------------------------- |
| 启动 | ${redis}/src/redis-cli -h {ip} -p {port} |
| 关闭 | exit                                     |

