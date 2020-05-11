## Galaxy
```bash
FROM: ubuntu:20.04

RUN apt update

RUN apt install -y lsb-release
    && lsb_release -a

WORKDIR /home
RUN apt install -y git
    && git clone https://github.com/galaxyproject/galaxy.git

WORKDIR galaxy
RUN bash run.sh
```

<br>

## 參考資料
- [生物資訊 / Galaxy 入門 / 安裝](https://hackmd.io/2uwnUsDkQ7uF9KfB8QTLQg#%E5%AE%89%E8%A3%9D-Galaxy)
