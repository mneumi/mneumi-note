## nginx安装

### 通过docker安装

```shell
# 查看nginx版本相关信息
docker search nginx

# 拉取nginx镜像
docker pull nginx:latest

# 查看本地镜像
docker images

# 启动nginx
docker run -d --name nginx -p 80:80 nginx:latest 
```



## nginx常用命令

| 功能            | 命令                   |
| --------------- | ---------------------- |
| 启动nginx       | servicectl nginx start |
| 重启nginx       | nginx -s reload        |
| 关闭nginx       | nginx -s stop          |
| 查看nginx版本号 | nginx -v               |
