## 建立私有 Ubuntu 版本：
```bash
# 測試版本：Ubuntu 20.04
docker pull ubuntu

# 建立一個 container
# -d: daemon, -t: tty
docker run --name ubuntu-base -dt ubuntu

# 連線到 container，並獲取套件清單來源
docker exec ubuntu-base apt update

# 安裝套件
## CMD: lsb_release -a
docker exec ubuntu-base apt install -y lsb-release
docker exec ubuntu-base lsb_release -a
## Distributor ID:	Ubuntu
## Description:	Ubuntu 20.04 LTS
## Release:	20.04
## Codename:	focal

## CMD: python3 (python 3.8 已內建在 Ubuntu 20.04)
docker exec ubuntu-base python3 -V
## Python 3.8.2

## CMD: python (額外安裝 python 2.7)
docker exec ubuntu-base apt install -y python
docker exec ubuntu-base python -V
## Python 2.7.18rc1

## CMD: virtualenv
docker exec ubuntu-base apt install virtualenv

## CMD: git
docker exec ubuntu-base apt install -y git
docker exec ubuntu-base git --help

## CMD: ping
docker exec ubuntu-base apt install -y iputils-ping
docker exec ubuntu-base ping -h

# 儲存 container
major=```lsb_release -r | egrep -o "[0-9.]+"```
minor=```date +"-%Y%m%d-%H%M%S"```
tag=ubuntu-$major$minor
echo tag: $tag
docker commit ubuntu-base ```echo $tag```
docker images  # list the images

# 停止執行中的 
docker stop ubuntu-base
```

## 生物資訊
```
docker exec -it ubuntu-base bash
cd /home
git clone https://github.com/galaxyproject/galaxy.git
cd galaxy

# 雖然 Galaxy 會安裝 virtualenv
# 但安裝 Galaxy 過程還是會出錯
virtualenv -p python2 env-python2
source env-python2/bin/activate

bash run.sh
```
