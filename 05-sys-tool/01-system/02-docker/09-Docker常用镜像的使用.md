## MySQL

搜索镜像

```shell
docker search mysql
```

拉取镜像

```shell
docker pull mysql:tag
```

根据镜像创建容器

```shell
docker run \
-d \ # 守护进程
--name mysql \ # 容器命名 
-p 3306:3306 \ # 端口映射
-e MYSQL_ROOT_PASSWORD=123456 \ # 通过环境变量设置root账户的密码
--restart always \ # 永远自动重启
-v /home/env/mysql/data:/var/lib/mysql \ # 数据卷挂载
mysql:tag # 镜像名
```

