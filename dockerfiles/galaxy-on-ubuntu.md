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
