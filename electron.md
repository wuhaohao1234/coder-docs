---
title: "Electron"
date: 2020-09-20T22:00:34+08:00
---

# electron

## 从零学习创建第一个Electron应用 

* 随着前端快速的发展，在任何开发领域都有前端的一席之地。那如何使用前端进行桌面端应用的开发呢？Electron是一个不错的选择。
* Electron是什么呢?
* Electron就是使用 JavaScript，HTML 和 CSS 构建跨平台的桌面应用程序.
*  Electron是由github开发的开源框架
* 它允许开发者使用web技术开发跨平台的桌面应用
* Electron = Chromium + Node.js + Native API
* Chromium：提供了强大的ui能力，利用强大的web生态来开发界面。
* Node.js：让 Electron有了底层才做能力，比如读写能力，并且可以使用大量的开源包来完成项目的开发。
* Native API：让Electron有了跨平台和桌面端的原生能力，比如有同意的原生界面、窗口、托盘。

## 创建第一个electron应用

设置全局淘宝变量

``` 
npm config set registry https://registry.npm.taobao.org
npm config set ELECTRON_MIRROR http://npm.taobao.org/mirrors/electron/
```

初始化项目

 `npm init -y`
创建main.js

``` 
const { app, BrowserWindow } = require('electron');
let mainWindow = null;

app.on('ready', () => {
    mainWindow = new BrowserWindow({    // 创建和控制浏览器窗口
        width: 800,                     // 窗口宽度
        height: 600,                    // 窗口高度
        webPreferences: {               // 网页功能设置
            nodeIntegration: true,      // 是否在node工作器中启用工作集成默认false
            enableRemoteModule: true,   // 是否启用remote模块默认false
        }
    });
    mainWindow.loadFile('index.html');  // 加载页面
    mainWindow.on('close', () => {      // 监听窗口关闭
        mainWindow = null
    })

})
```

创建index.html

``` 
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <button id="btn" >点击</button> 
</body>
</html>
```

修改package.json

```
{
  "name": "electron-demo1",
  "version": "1.0.0",
  "description": "",
  "main": "main.js",
  "scripts": {
    "start": "electron ."
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "electron": "^10.1.2"
  }
}

```

启动项目

`npm run start`

electron运行进程

1. 读取package.json文件中的入口文件，这里就是我们的main.js
2. main.js中我们引入electron 创建了渲染进程
3. index.html就是应用页面的布局和样式
4. 使用IPC在主进程执行任务并获取信息

## 试试node.js的读取

修改文件目录如下
```
package.json
package-lock.json
  - src
    main.js
    - render
      index.js
      index.html
      text.txt
```

main.js

```
const { app, BrowserWindow } = require("electron");
let mainWindow = null;

app.on("ready", () => {
  mainWindow = new BrowserWindow({
    width: 800,
    height: 600,
    webPreferences: {
      nodeIntegration: true,
      enableRemoteModule: true,
    },
  });
  mainWindow.loadFile(require('path').join(__dirname, './render/index.html'));
  mainWindow.on("close", () => {
    mainWindow = null;
  });
});
```

index.html

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <button id="btnRead" >读取文件</button>

    <button id="btnWrite" >写入文件</button>
    <div id="filesData"></div>
    <script src="./js/index.js"></script>
</body>
</html>
```

index.js

```
const fs = require('fs');
const path = require('path')

const btnRead = document.querySelector('#btnRead');
const filesDaata = document.querySelector('#filesData');
const btnWrite = document.querySelector('#btnWrite');


window.onload = ()=>{
    btnRead.onclick = () => {
        fs.readFile(path.join(__dirname, 'text.txt'),(err,data)=>{
            console.log(data);
            filesDaata.innerHTML = data
        })
    }
    btnWrite.onclick = () => {
        fs.writeFile(path.join(__dirname,'text.txt'),'添加1111', 'utf8',err=>{
            console.log(err)
        })
    }
}

```
text.txt

```
我是读取文件
```

## remote模块使用

说Remote之前我们要明确一点，当我们知道了Electron是分主进程、渲染进程后，还需要知道Electron的API方法和模块也是分主进程、渲染进程。

回到我们Remote的使用

Remote是渲染进程（web页面）和主进程通信（IPC）提供一种简单的方法。

在index.html中写上一个按钮，通过点击事件创建一个新的窗口

创建一个yellow.html用作新建窗口的渲染进程

在上一个项目基础上改进

main.js

```
const { app, BrowserWindow } = require("electron");
let mainWindow = null;

app.on("ready", () => {
  mainWindow = new BrowserWindow({
    width: 800,
    height: 600,
    webPreferences: {
      nodeIntegration: true,
      enableRemoteModule: true,
    },
  });
  mainWindow.loadFile(require('path').join(__dirname, './render/index.html'));
  mainWindow.on("close", () => {
    mainWindow = null;
  });
});
```

index.js

```
const fs = require('fs');
const path = require('path')

const btnRead = document.querySelector('#btnRead');
const filesDaata = document.querySelector('#filesData');
const btnWrite = document.querySelector('#btnWrite');
const BrowserWindow = require('electron').remote.BrowserWindow;
const btn = document.querySelector('#btn');

window.onload = () => {
    btnRead.onclick = () => {
        fs.readFile(path.join(__dirname,'text.txt'),(err,data) => {
            console.log(data);
            filesDaata.innerHTML = data
        })
    }
    btnWrite.onclick = () => {
        fs.writeFile(path.join(__dirname,'text.txt'),'添加1111','utf8',err => {
            console.log(err)
        })
    }
    btn.onclick = () => {
        const newWindow = new BrowserWindow({
            width: 300,
            height: 300,
        })
        newWindow.loadFile(require("path").join(__dirname,'yellow.html'));
        newWindow.on('close',() => {
            newWindow = null;
        })
    }
}
```

在render目录下创建yellow.html

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    我是新建的窗口
</body>
</html>
```

## 创建菜单

再上述代码根目录下新建main/menu.js

```
const { Menu, BrowserWindow } = require("electron");

const temlpate = [
  {
    label: "第一项",
    submenu: [
      {
        label: "1.1",
        accelerator:`crtl+n`,
        click: () => {
          let win = new BrowserWindow({
            width: 300,
            height: 300,
          });
          win.loadFile(require('path').join(__dirname,'../render/yellow.html'));
          win.on('close',()=>{
              win = null;
          })
        },
      },
      { label: "1.2" },
    ],
  },
  {
    label: "第二项",
    submenu: [{ label: "2.1" }, { label: "2.2" }],
  },
];

const menu = Menu.buildFromTemplate(temlpate);
Menu.setApplicationMenu(menu);
```

main.js

```
const { app, BrowserWindow } = require("electron");
let mainWindow = null;
require('./main/menu')

app.on("ready", () => {
  mainWindow = new BrowserWindow({
    width: 800,
    height: 600,
    webPreferences: {
      nodeIntegration: true,
      // enableRemoteModule: true,
    },
  });
  mainWindow.loadFile(require('path').join(__dirname, './render/index.html'));
  mainWindow.on("close", () => {
    mainWindow = null;
  });
});

```