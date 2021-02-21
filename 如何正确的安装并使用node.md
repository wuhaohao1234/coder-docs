---
title: "如何正确的安装并使用node"
date: 2020-10-01T11:08:38+08:00
---

## 操作系统

不要windows, 最好是Linux或者mac

## 安装

首先安装git

 `sudo apt-get install git`
[git教程](https://juejin.im/post/6844903891067224078)

然后去github上克隆nvm，国内网速不好的可以使用gitee

[github nvm](https://github.com/nvm-sh/nvm)

[gitee nvm](https://gitee.com/mirrors/nvm)

## 具体步骤

``` shell
git clone git://github.com/creationix/nvm.git ~/nvm
git clone git://gitee.com/mirrors/nvm.git ~/nvm

command -v nvm

echo "source ~/nvm/nvm.sh" >> ~/.bashrc

source ~/.bashrc

这里可加入nvm中node源

node_mirror=https://npm.taobao.org/mirrors/node/


nvm install node

node --version # 检查node版本
npm --version # 检查npm 版本
npx --version # 检查npx版本
```

## 使用nrm为npm换源

安装

 `npm install -g nrm`
列出所有的npm源

``` shell
nrm ls

npm ---- https://registry.npmjs.org/
cnpm --- http://r.cnpmjs.org/
taobao - http://registry.npm.taobao.org/
eu ----- http://registry.npmjs.eu/
au ----- http://registry.npmjs.org.au/
sl ----- http://npm.strongloop.com/
nj ----- https://registry.nodejitsu.com/

```

使用taobao源

 `nrm use taobao`
查看当前的源

 `npm config get registry`

## 安装yarn

 `npm install -g yarn`
这里需要知道，一个项目中yarn.lock与package.json.lock是对应npm包的源地址

## 发布npm包

1. 到 https://www.npmjs.com 注册一个账号

    记得将npm中的taobao源切换为npm源

2. 进入你的项目根目录，运行 npm login

    会输入你的用户名、密码和邮箱

3. 登录成功后，执行 npm publish，就发布成功啦
