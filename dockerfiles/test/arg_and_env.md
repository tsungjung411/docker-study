
## 測試 ARG 和 ENV 的差異

### 三個原則
- ARG 就是 Dockerfile 裡面的變數(key=value)，並不會新增到 env 環境變數
- ENV 亦是 Dockerfile 裡面的變數(key=value)(同 ARG 功能)，並將該變數(key=value)新增到 env 環境變數中
- 當 ARG 與 ENV 有同名的變數時，ENV 優先權高於 ARG，詳見「**測試6**」

<br>

### 語法差異：
- ENV: ```key=value``` 或 ```key value```
- ARG: ```key=value``` (使用 ```key value``` 會失敗，Error: ARG requires exactly one argument)

<br>

### Dockerfile
```dockerfile
FROM nginx

# ============================================
# 測試1：設定到環境變數，有無雙引號皆可
# ============================================
ENV env_author_1_1=env_tj_tsai_1
ENV env_author_1_2="env_tj_tsai_1"
ENV env_author_1_3 env_tj_tsai_1
ENV env_author_1_4 "env_tj_tsai_1"

## 是字串，不是變數
RUN echo [1] env_author_1_1=env_author_1_1
RUN echo [1] env_author_1_2=env_author_1_2
RUN echo [1] env_author_1_3=env_author_1_3
RUN echo [1] env_author_1_4=env_author_1_4

## 是變數，不是字串
RUN echo [1] env_author_1_1=$env_author_1_1
RUN echo [1] env_author_1_2=$env_author_1_2
RUN echo [1] env_author_1_3=$env_author_1_3
RUN echo [1] env_author_1_4=$env_author_1_4

RUN echo [1] env_author_1_1=${env_author_1_1}
RUN echo [1] env_author_1_2=${env_author_1_2}
RUN echo [1] env_author_1_3=${env_author_1_3}
RUN echo [1] env_author_1_4=${env_author_1_4}


# ============================================
# 測試2：內建的 docerfile 變數
# ============================================
## 不會設定到 env
ARG arg_author_2_1=arg_tj_tsai_2
## ARG 需要 '=' , Error: ARG requires exactly one argument
#ARG arg_author_2_2 arg_tj_tsai_2
RUN echo [2] arg_author_2_1=$arg_author_2_1
RUN echo [2] arg_author_2_1=${arg_author_2_1}


# ==============================================================
# 測試3：外部引入的 docerfile 變數 --build-arg arg_author_3=ooxx3
# ==============================================================
## 不會設定到 env
ARG arg_author_3=arg_tj_tsai_3
RUN echo [3] arg_author_3=$arg_author_3
RUN echo [3] arg_author_3=${arg_author_3}


# =============================================================
# 測試4：ARG(內建) 和 ENV 同時並存的情況下，設定到環境變數的優先權
# =============================================================
ENV env_author_4=env_tj_tsai_4
## 不會新增到 env 環境變數
ARG env_author_4=arg_tj_tsai_4
## 是字串，不是變數
ENV env_author_4_1=env_author_4
## 是變數，不是字串
ENV env_author_4_2=$env_author_4

RUN echo [4] env_author_4_1=$env_author_4_1
RUN echo [4] env_author_4_2=$env_author_4_2


# =================================================================
# 測試5：ARG(外部引入) 和 ENV 同時並存的情況下，設定到環境變數的優先權
# --build-arg env_author_5=ooxx5
# =================================================================
ENV env_author_5=env_tj_tsai_5
## 不會新增到 env 環境變數
ARG env_author_5=arg_tj_tsai_5
## 是字串，不是變數
ENV env_author_5_1=env_author_5
## 是變數，不是字串
ENV env_author_5_2=$env_author_5

RUN echo [5] env_author_5_1=$env_author_5_1
RUN echo [5] env_author_5_2=$env_author_5_2


# =================================================================
# 測試6：ARG 和 ENV 交替指定
# =================================================================
ENV env_author_6_1=env_tj_tsai_6_1
ARG arg_author_6_2=env_author_6_1
ARG arg_author_6_3=$env_author_6_1
ENV env_author_6_4=arg_author_6_2
ENV env_author_6_5=$arg_author_6_3

## 當 ARG 與 ENV 有同名的變數時，ENV 優先權高於 ARG
## 亦即，先去 env 環境變數尋找，沒有再去 ARG 變數尋找
ENV env_author_6_6=ooxx1
ARG env_author_6_6=ooxx2
ENV env_author_6_7=$env_author_6_6

ARG env_author_6_8=ooxx3
ENV env_author_6_8=ooxx4
ENV env_author_6_9=$env_author_6_8

ENV env_author_6_A=$variable_not_exist


# CMD ["nginx", "-g", "daemon off;"]
```

<br>

## 建置映像檔
```bash
docker build \
--tag my-nginx mynginx2 \
--build-arg arg_author_3=ooxx3 \
--build-arg env_author_5=ooxx5
 
```

<br>

## 載入映像檔
```bash
docker run --name my-nginx-container -d -p 8080:80 my-nginx

# 檢視執行狀態，查看 STATUS
docker ps -a
```
- 用瀏覽器檢視 http://localhost:8080/

<br>

## 檢視環境變數
```bash
docker exec my-nginx-container env
```
執行結果
```bash
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
HOSTNAME=f8b464da20fe
NGINX_VERSION=1.17.10
NJS_VERSION=0.3.9
PKG_RELEASE=1~buster
env_author_1_1=env_tj_tsai_1
env_author_1_2=env_tj_tsai_1
env_author_1_3=env_tj_tsai_1
env_author_1_4=env_tj_tsai_1
env_author_4=env_tj_tsai_4
env_author_4_1=env_author_4
env_author_4_2=env_tj_tsai_4
env_author_5=env_tj_tsai_5
env_author_5_1=env_author_5
env_author_5_2=env_tj_tsai_5
env_author_6_1=env_tj_tsai_6_1
env_author_6_4=arg_author_6_2
env_author_6_5=env_tj_tsai_6_1
env_author_6_6=ooxx1
env_author_6_7=ooxx1
env_author_6_8=ooxx4
env_author_6_9=ooxx4
env_author_6_A=
HOME=/root
```

<br>

## 結束測試，清除暫存檔
```bash
docker rm -f my-nginx-container
docker rmi my-nginx:latest
docker images
 
```
