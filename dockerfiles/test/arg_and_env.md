
## 測試 ARG 和 ENV 的差異
- ARG 就是 Dockerfile 裡面的變數(key=value)，並不會新增到 env 環境變數
- ENV 是將變數(key=value)新增到 env 環境變數中

### Dockerfile
```dockerfile
FROM nginx

# ============================================
# 測試1：設定到環境變數，有無雙引號皆可
# ============================================
ENV env_author_1_1=env_tj_tsai_1
ENV env_author_1_2="env_tj_tsai_1"

## 是字串，不是變數
RUN echo [1] >> env_author_1_1=env_author_1_1
## 是變數，不是字串
RUN echo [1] >> env_author_1_1=$env_author_1_1
## 是字串，不是變數
RUN echo [1] >> env_author_1_2=env_author_1_2
## 是變數，不是字串
RUN echo [1] >> env_author_1_2=$env_author_1_2


# ============================================
# 測試2：內建的 docerfile 變數
# ============================================
## 不會設定到 env
ARG arg_author_2=arg_author_2
RUN echo [2] >> arg_author_2=$arg_author_2


# ==============================================================
# 測試3：外部引入的 docerfile 變數 --build-arg arg_author_3=ooxx3
# ==============================================================
## 不會設定到 env
ARG arg_author_3=arg_author_3
RUN echo [3] >> arg_author_3=$arg_author_3


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

RUN echo [4] >> env_author_4_1=$env_author_4_1
RUN echo [4] >> env_author_4_2=$env_author_4_2


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

RUN echo [5] >> env_author_5_1=$env_author_5_1
RUN echo [5] >> env_author_5_2=$env_author_5_2


# CMD ["nginx", "-g", "daemon off;"]
```

## 建置映像檔
```bash
docker build \
--tag my-nginx mynginx2 \
--build-arg arg_author_3=ooxx3 \
--build-arg arg_author_5=ooxx5

```

## 載入映像檔
```bash
docker run --name my-nginx-container -d -p 8080:80 my-nginx

# 檢視執行狀態，查看 STATUS
docker ps -a
```
- 用瀏覽器檢視 http://localhost:8080/

## 檢視環境變數
```bash
docker exec my-nginx-container env
```
