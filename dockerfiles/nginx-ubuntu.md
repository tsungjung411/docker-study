
## 自建一個簡易 Ubuntu 版本的 nginx 映像檔

### Dockerfile (放在 mynginx 資料夾底下)
```dockerfile
FROM ubuntu:20.04

RUN apt-get update \
    && apt-get install -y nginx

COPY index.html /usr/share/nginx

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

### 建置 image 指令
```bash
docker build --tag my-nginx:v1 mynginx/
```

### 啟用 image 指令
```bash
docker run --name my-nginx -p 80:80 -d my-nginx:v1 
```
