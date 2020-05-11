
## 自建一個簡易 Ubuntu 版本的 nginx 映像檔

### Dockerfile (放在 mynginx 資料夾底下)
```dockerfile
FROM ubuntu:20.04

RUN apt-get update \
    && apt-get install -y nginx

COPY index.html /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

<br>

### 建置 image 指令
```bash
$ docker build --tag my-nginx:v1 mynginx/
```

<br>

### 瀏覽 image 指令
```bash
$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
my-nginx            v1                  f1cef06fbe7a        53 seconds ago      154MB
```

### 根據此 image，啟用一個 container
```bash
$ docker run --name my-nginx -p 80:80 -d my-nginx:v1 
```
測試網頁
- http://localhost:80/
- http://127.0.0.1:80/

### 查看啟用的 container
```
$ docker ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                NAMES
72cdb58c4a27        my-nginx:v1         "nginx -g 'daemon of…"   39 seconds ago      Up 37 seconds       0.0.0.0:80->80/tcp   my-nginx
```

### 連線到 container
```bash
$ docker exec -it my-nginx bash
```

### 結束該 container
```bash
$ docker rm -f my-nginx
```

### 移除該 image
```bash
$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
my-nginx            v1                  f1cef06fbe7a        53 seconds ago      154MB

$ docker rmi my-nginx:v1
```

