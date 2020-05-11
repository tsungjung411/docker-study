
## 一些安裝注意事項
### apt install lsb-release
```bash
# apt install -y lsb-release
...
The following additional packages will be installed:
  distro-info-data file libexpat1 libmagic-mgc libmagic1 libmpdec2 libpython3-stdlib libpython3.8-minimal libpython3.8-stdlib libreadline8 libsqlite3-0 libssl1.1 mime-support python3 python3-minimal
  python3.8 python3.8-minimal readline-common xz-utils
Suggested packages:
  python3-doc python3-tk python3-venv python3.8-venv python3.8-doc binutils binfmt-support readline-doc
The following NEW packages will be installed:
  distro-info-data file libexpat1 libmagic-mgc libmagic1 libmpdec2 libpython3-stdlib libpython3.8-minimal libpython3.8-stdlib libreadline8 libsqlite3-0 libssl1.1 lsb-release mime-support python3
  python3-minimal python3.8 python3.8-minimal readline-common xz-utils
...
```
- 安裝 lsb-release 套件，會間接安裝 python3
- ubuntu 18.04, 20.04 等，預設是無 python2/3

<br>

## build dockerfile 教學
- [Day5: 實作撰寫第一個 Dockerfile](https://ithelp.ithome.com.tw/articles/10191016)
