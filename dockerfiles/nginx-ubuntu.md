
## 自建一個簡易 Ubuntu 版本的 nginx 映像檔

### Dockerfile (放在 mynginx 資料夾底下)
```dockerfile
FROM ubuntu:20.04

# 安裝 nginx 軟體
RUN apt-get update \
    && apt-get install -y nginx

# 測試 index.html
COPY index.html /usr/share/nginx/html/my-index-1.html   # 404 Not Found
COPY index.html /var/www/html/my-index-2.html

# 將 container 的埠 80/81/8080，開方給主機連線
EXPOSE 80 81 8080

# 啟動 container 後要執行的指令
CMD ["nginx", "-g", "daemon off;"]
```

<br>

### 建置 image 指令
```bash
$ docker build --tag my-nginx:v1 mynginx/
```

### 瀏覽 image 指令
```bash
$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
my-nginx            v1                  f1cef06fbe7a        53 seconds ago      154MB
```

### 根據此 image，啟用一個 container
```bash
$ docker run --name my-nginx -p 81:80 -d my-nginx:v1 
```
測試網頁
- http://localhost:81/
- http://127.0.0.1:81/
- http://localhost:81/index.html  # 404 Not Found
- http://localhost:81/my-index-1.html  # 404 Not Found
- http://localhost:81/my-index-2.html  # file: /var/www/html/index.nginx-debian.html

### 查看啟用的 container
```
$ docker ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                NAMES
72cdb58c4a27        my-nginx:v1         "nginx -g 'daemon of…"   39 seconds ago      Up 37 seconds       0.0.0.0:80->80/tcp   my-nginx
```

### 連線到 container
```bash
$ docker exec -it my-nginx bash

# 檢查複製過來的檔案
$ ls /usr/share/nginx/html/
$ ls /var/www/html
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

