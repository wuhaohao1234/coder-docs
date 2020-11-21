---
title: "Npm教学"
date: 2020-07-26T16:43:57+08:00
tags: ["npm"]
---

## npm

### 教学视频:

[科科智能cto帮主](https://www.bilibili.com/video/BV1Vb411L7se)

### 发布一个npm包

https://my.oschina.net/fenying/blog/1607571

### 相应的文档教学

https://www.ruanyifeng.com/blog/2016/01/npm-install.html

http://www.ruanyifeng.com/blog/2016/10/npm_scripts.html

### npm代理

> 尽量不要使用cnpm

1. 全局设置

`npm config set registry http://registry.npm.taobao.org`

2. 局部

编写.npmrc文件

```
registry=http://registry.npm.taobao.org
```

3. 使用nrm

➜ ~ nrm ls 
```
npm ---- https://registry.npmjs.org/ 
cnpm --- http://r.cnpmjs.org/ 
taobao - http://registry.npm.taobao.org/ 
eu ----- http://registry.npmjs.%
```
nrm use taobao


### 安装node-sass失败

`npm config set sass_binary_site https://npm.taobao.org/mirrors/node-sass/`

### electron安装失败

单独设置某个包的镜像，如electron，其镜像： https://npm.taobao.org/mirrors/electron/，命令如下：

```
npm config set electron_mirror https://npm.taobao.org/mirrors/electron/
```