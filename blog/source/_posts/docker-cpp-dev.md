---
title: 使用Docker构建cpp开发环境
date: 2023-11-12 10:11:08
---
## 特性
- 内置gnuc编译链
- ubuntu 22.04
- ssh服务
- 使用volumn持久化数据

## 配置步骤

1. 创建目录cpp_env
2. 将附录中的文件拷贝到该目录中
3. 创建docker image
```bash
docker build -t cppenv:latest ..
```
4. 启动docker容器
```bash
docker run -d -v volumn:/home -p 5000:22 cppenv
# 其中-d为后台运行
# -v volumn：/home将卷挂在到/home目录，volumn可以通过docker volumn命令生成
# -p 5000:22: 这个非常重要，将5000端口映射到22端口，host可以通过localhost：5000访问该容器
```
5. 使用ssh登入容器
```bash
ssh -p 5000 root@localhost
```
6. 创建自己的账号
```
adduser test
```
7. 使用vscode连接容器

## 附录
Dockerfile:
```dockerfile
FROM ubuntu:22.04

RUN apt-get update
RUN apt-get install apt-transport-https -y --fix-missing
RUN apt-get install ca-certificates -y
COPY sources.list /etc/apt/
RUN apt-get update && \
    apt-get -y install sudo

RUN apt-get install -y build-essential
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y cmake
RUN apt-get install -y gdb --fix-missing
RUN apt-get install -y net-tools gdb --fix-missing

RUN apt-get install -y openssh-server
RUN mkdir /var/run/sshd
RUN sed -ri 's/^#PermitRootLogin\s+.*/PermitRootLogin yes/' /etc/ssh/sshd_config
RUN sed -ri 's/UsePAM yes/#UsePAM yes/g' /etc/ssh/sshd_config

RUN echo 'root:root' |chpasswd
EXPOSE 22
COPY entrypoint.sh /sbin
RUN chmod +x /sbin/entrypoint.sh
ENTRYPOINT [ "/sbin/entrypoint.sh" ]
```
entrypoint.sh
```bash
#!/bin/bash

/usr/sbin/sshd -D
```
```sources.list
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ jammy main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ jammy main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ jammy-updates main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ jammy-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ jammy-backports main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ jammy-backports main restricted universe multiverse

# deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ jammy-security main restricted universe multiverse
# # deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ jammy-security main restricted universe multiverse

deb http://ports.ubuntu.com/ubuntu-ports/ jammy-security main restricted universe multiverse
# deb-src http://ports.ubuntu.com/ubuntu-ports/ jammy-security main restricted universe multiverse

# 预发布软件源，不建议启用
# deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ jammy-proposed main restricted universe multiverse
# # deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ jammy-proposed main restricted universe multiverse
```
