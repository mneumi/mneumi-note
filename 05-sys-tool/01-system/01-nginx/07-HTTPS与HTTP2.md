## 配置HTTP2

### 概述

配置HTTP2非常简单，只需要配置`server`块的`listen`中加上`http2`即可

### 示例配置

```nginx
worker_processes 1;

events {
    worker_connections 1024;
}

http {
    server {
        listen 80 http2; /* 这里加上http2 */
        server_name localhost;       
        
        location /image/ {
            root /data/;
            autoindex on;
        }
    }
}
```



## 配置HTTPS

### 概述

配置`HTTPS`之前，需要先获取证书，证书的获取不在此赘述

### 配置项

不同的证书对应的配置项可能不同，具体配置项需要查阅证书颁发机构提供的文档和资料

### 示例配置

这里以**阿里云**提供的免费证书为例子

```nginx
worker_processes 1;

events {
    worker_connections 1024;
}

http {
    server {
        listen 443 ssl http2; /* 标识 ssl */
        server_name demo.cn www.demo.cn;

        ssl_certificate /var/cert/demo.cn.pem; /* 证书pem的路径 */
        ssl_certificate_key /var/cert/demo.cn.key; /* 证书key的路径 */
        ssl_session_timeout 5m;
        ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
        ssl_prefer_server_ciphers on;

        location / {
            expires 24h;
            root /home/mneumi/note;
            index index.html;
        }
    }
}
```

