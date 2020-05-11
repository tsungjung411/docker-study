## nginx

### 範例1
底下範例，取自 [9789572246559-Docker 這樣學才有趣：從入門，到玩直播、挖礦](https://www.tenlong.com.tw/products/9789572246559)

```dockerfile
FROM debian:jessie

MAINTAINER NGINX Docker Maintainers "docker-maint@ngninx.com"

ENV NGINX_VERSION 1.13.4-1~jessie

RUN apt-key adv --keyserver hkp://pgp.mit.edu:80 --recv-keys 573BFD6B3D8FBC641079A6ABABF5BD827BD9BF62
    && echo "deb http://nginx.org/packages/mainline/debian/ jessie ngnix" >> /etc/apt/sources.list \
    && apt-get update \
    && apt-get install --no-install-recommends --no-install-suggests -y ca-certificates \
       ngnix=${NGINX_VERSION} \
       nginx-module-xslt \
       nginx-module-geoip \
       nginx-module-image-filter \
       nginx-module-perl \
       nginx-module-njs \
       gettext-base \
    && rm -rf /var/lib/apt/lists/*

# forward request and error logs to docker log collector
RUN ln -sf /dev/stdout /var/log/nginx/access.log \
    && ln -sf /dev/stderr /var/log/nginx/error.log

EXPOSE 80 443

CMD ["nginx", "-g", "daemon off;"]
```

實際執行時，會遇到 error
```
W: Failed to fetch http://nginx.org/packages/mainline/debian/dists/jessie/InRelease  Unable to find expected entry 'ngnix/binary-amd64/Packages' in Release file (Wrong sources.list entry or malformed file)

E: Some index files failed to download. They have been ignored, or old ones used instead.
```


### 範例2
官方範例：https://github.com/nginxinc/docker-nginx/blob/f4d30145c60c433966df96f618d78513fee9d322/mainline/stretch/Dockerfile
