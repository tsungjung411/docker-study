## Galaxy
```dockerfile
FROM: ubuntu:20.04

RUN apt update

RUN apt install -y lsb-release \  # will also install python3 (python 3.8)
    && lsb_release -a

RUN apt install virtualenv \
    && apt install -y git

WORKDIR /home
RUN git clone https://github.com/galaxyproject/galaxy.git

WORKDIR galaxy
RUN virtualenv -p python3 env-python3 \
    && source env-python3/bin/activate
CMD ["bash", "run.sh"]
```

<br>

## 參考資料
- [生物資訊 / Galaxy 入門 / 安裝](https://hackmd.io/2uwnUsDkQ7uF9KfB8QTLQg#%E5%AE%89%E8%A3%9D-Galaxy)
