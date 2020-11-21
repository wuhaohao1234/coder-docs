---
title: "Docker"
date: 2020-08-30T23:03:23+08:00
---

# docker

## 操作系统: deepin

安装

安装docker
`sudo apt-get install docker-ce`
curl

下载docker-compose(非必要操作)
```
sudo curl -L "https://github.com/docker/compose/releases/download/1.24.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```
加入权限
```
sudo chmod +x /usr/local/bin/docker-compose
```
加入软链接
```
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```
加入用户组
```
sudo su
sudo 用户
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