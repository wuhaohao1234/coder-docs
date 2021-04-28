---
title: "Honeycomb Server安装"
date: 2021-04-10T16:51:50+08:00
---

# honeycomb server安装

操作系统: win10子系统ubuntu

当前用户为abu0418

## 国内安装node

1. clone nvm

`git clone http://gitee.com/mirrors/nvm.git ~/nvm`

2. 添加软连接

`command -v nvm`

3. 写入bashrc

`echo "source ~/nvm/nvm.sh" >> ~/.bashrc`

4. 更新命令

`source ~/.bashrc`

5. 设置node淘宝源下载

`node_mirror=https://npm.taobao.org/mirrors/node/`

6. 安装node

`nvm install node`

7. 验证node

`npm --version && npx --version && node --version`

8. 切换源

`npm config set registry http://registry.npm.taobao.org`

安装nrm

`npm install -g nrm`

切换nrm源(淘宝源)

`nrm use taobao`

## 国内安装zsh

`sudo apt install zsh`

`wget https://gitee.com/mirrors/oh-my-zsh/raw/master/tools/install.sh`

修改gitee 地址

vim install.sh

```shell
# Default settings
ZSH=${ZSH:-~/.oh-my-zsh}
REPO=${REPO:-ohmyzsh/ohmyzsh}
REMOTE=${REMOTE:-https://github.com/${REPO}.git}
BRANCH=${BRANCH:-master}

替换为
# Default settings
ZSH=${ZSH:-~/.oh-my-zsh}
REPO=${REPO:-shrekuu/ohmyzsh}
REMOTE=${REMOTE:-https://gitee.com/${REPO}.git}
BRANCH=${BRANCH:-master}
```

安装

sh install.sh

这里下次进入的时候默认是zsh，但是需要再输入bash进入bash，然后再进入zsh，不然没有node命令

## honeyserver安装

原文是需要创建admin用户，本文采用修改代码方式改为创建自己的用户

全局安装honeycomb

`npm i -g honeycomb-cli`

从gitee clone honeycomb-server

`git clone https://gitee.com/wuhaohao1234/honeycomb-server`

`honeycomb package`

进入out目录，进入honeycomb-server_x.y.z ，默认已经解压

修改honeycomb_install.sh

将admin改为自己当前账户,当前电脑账户为abu0418

执行honeycomb_install.sh

进入当前账户下的/home/abu0418/honeycomb

修改/home/abu0418/honeycomb/conf/config_default.js

里面admin修改为当前账户abu0418

执行../bin/server_ctl start 启动服务
