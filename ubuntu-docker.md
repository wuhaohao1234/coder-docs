---
title: "Ubuntu Docker"
date: 2021-02-12T12:20:42+08:00
---

# ubuntu20.04-Docker

1. 选择ubuntu镜像为中国镜像

2. 安装需要的包

`sudo apt-get install apt-transport-https ca-certificates software-properties-common curl`

3. 添加GPG密钥,以Docker-ce 中国科技大学软件源

`curl -fsSL https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu/gpg | sudo apt-key add -`

```shell
sudo add-apt-repository "deb [arch=amd64] https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu \
$(lsb_release -cs) stable"
```

4. 安装docker

`sudo apt-get install docker-ce`

5. 添加用户组

```shell
sudo gpasswd -a ${USER} docker
newgrp - docker
sudo service docker restart
```

## 学习

docker暂时可以理解为一个虚拟机，通过编写dockerFile可以安装各种例如nginx,node,mysql,mongo环境

### 一条命令安装并启动nginx

`docker run -d --name nginx -p 8888:80 nginx:alpine`

-d 后台运行

--name nginx命名为nginx
8888:80 将docker内部nginx的80端口映射到本地的8888端口
nginx:alpine nginx版本

打开浏览器查看　localhost:8888

### 查看当前运行的容器

`docker ps -a`

### 进入容器环境

`docker exec -it nginx sh`

可以去etc/nginx中查看nginx中的配置

### 删除镜像

docker rm -f 镜像名

### 删除容器

docker rmi nginx:alpine

## dockerFile

https://shanyue.tech/op/docker.html#%E9%95%9C%E5%83%8F

## docker-compose

https://shanyue.tech/op/docker-compose-arch.html#traefik