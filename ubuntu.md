## 常用套件
```bash
# 測試版本：Ubuntu 20.04
docker pull ubuntu

# 建立一個 container
# -t: tty ()
docker run --name ubuntu-base -t -d ubuntu

# 連線到 container
docker exec ubuntu-base apt update

# 安裝套件
# CMD: lsb_release -a
docker exec ubuntu-base apt install -y lsb-release
docker exec ubuntu-base lsb_release -a
# Distributor ID:	Ubuntu
# Description:	Ubuntu 20.04 LTS
# Release:	20.04
# Codename:	focal

# CMD: git
docker exec ubuntu-base apt install -y git
docker exec ubuntu-base git -h

# CMD: ping
docker exec ubuntu-base apt install -y iputil-ping
docker exec ubuntu-base ping -h


```

## 生物資訊
