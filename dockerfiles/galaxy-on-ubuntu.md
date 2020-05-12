## Galaxy
```dockerfile
FROM: ubuntu:20.04

RUN apt update

RUN apt install -y lsb-release \  # will also install python3 (python 3.8)
    && lsb_release -a

RUN apt install -y virtualenv \
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

<br>

## 收集到的錯誤訊息
### Case1
```bash
$ bash run.sh
...
    Uninstalling requests-2.22.0:
      Successfully uninstalled requests-2.22.0
    Running setup.py install for numpy ... error
    ERROR: Command errored out with exit status 1:
     command: /home/galaxy/.venv/bin/python -u -c 'import sys, setuptools, tokenize; sys.argv[0] = '"'"'/tmp/pip-install-w5qan5ah/numpy/setup.py'"'"'; __file__='"'"'/tmp/pip-install-w5qan5ah/numpy/setup.py'"'"';f=getattr(tokenize, '"'"'open'"'"', open)(__file__);code=f.read().replace('"'"'\r\n'"'"', '"'"'\n'"'"');f.close();exec(compile(code, __file__, '"'"'exec'"'"'))' install --record /tmp/pip-record-37n2n3z3/install-record.txt --single-version-externally-managed --compile --install-headers /home/galaxy/.venv/include/site/python3.8/numpy
         cwd: /tmp/pip-install-w5qan5ah/numpy/
    Complete output (298 lines):
    Running from numpy source directory.
    
    Note: if you need reliable uninstall behavior, then install
    with pip instead of using `setup.py install`:
    
      - `pip install .`       (from a git repo or downloaded source
                               release)
      - `pip install numpy`   (last NumPy release on PyPi)
...
```
