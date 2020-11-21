---
title: "Package"
date: 2020-09-12T23:17:23+08:00
---

# package.json 的解读

---

# npm scripts
## 环境变量
npm 在执行指定 script 之前会把 `node_modules/.bin` 加到环境变量 `$PATH` 的前面，这意味着任何内含可执行文件的 npm 依赖都可以在 npm script 中直接调用，换句话说，你不需要在 npm script 中加上可执行文件的完整路径，比如 `./node_modules/.bin/eslint **.js`。<br />

```bash
npm run env | grep npm_package | sort # 拿到项目中 package.json 中显性的变量列表并排序
```


## 命令并行
让多个 npm script 并行,把连接多条命令的 `&&` 符号替换成 `&` 即可.但同时注意 npm 内置支持的多条命令并行跟 js 里面同时发起多个异步请求非常类似，它只负责触发多条命令，而不管结果的收集，因此需要注意增加 `& wait`<br />

```bash
npm run lint:js & npm run lint:css & npm run lint:json & wait
```

<br />或者通过第三方库：[**`npm-run-all `**](https://github.com/mysticatea/npm-run-all/blob/HEAD/docs/npm-run-all.md)<br />

```bash
npm-run-all lint:* --parallel # 其支持通配符，所以实现类似楼上。parallel即并行执行
```


## 自动完成


### 自己配置
```bash
npm completion >> ~/.npm-completion.bash
echo "[ -f ~/.npm-completion.bash ] && source ~/.npm-completion.bash;" >> ~/.zshrc
source ~/.zshrc
```


### 第三方库
[lukechilds/zsh-better-npm-completion: Better completion for npm](https://github.com/lukechilds/zsh-better-npm-completion)<br />[mklabs/yarn-completions: Completion handler for Yarn](https://github.com/mklabs/yarn-completions)<br />

## node 中使用


```bash
npm i shelljs chalk -D
```

<br />
<br />

<a name="z2lOI"></a>
# 官方字段


<a name="lpfreb"></a>
## json 和 JS 对象的区别

<br />package.json，顾名思义，它是一个json文件，而不能写入JS对象。<br />所以我们首先要搞懂的是JSON和JS对象的区别：

| 区别 | Json | Javascript对象 |
| --- | --- | --- |
| 含义 | 仅仅是一种数据格式 | 表示类的实例 |
| 传输 | 可以跨平台数据传输，速度快 | 不能传输 |
| 表现 | 1、键值对方式，键必须加双引号<br /> | 1、键值对方式，键不加引号<br />2、值可以是函数、对象、字符串、数字、boolean等<br /> |
| 相互转换 | Json转化为JS对象：<br />1、JSON.parse(jsonstring);<br />2、json=eval("("+jsonstring+")") | JS对象转换为JSON:<br /> |


<br />【注意】 在JSON中属性名一定要加上双引号<br />

<a name="57agkw"></a>
## name 字段

<br />**name字段的限制**<br />

- name字段必须小于214字符（这个没什么好记的～）

- name字段不能包含有“.”符号和下划线（这个要记一下哦～）

- name字段不能包含有大写字母（这个要记一下哦～）

- name字段不能含有非URL安全的字符，因为它将当发布的时候，它将作为你的包的相关信息被写入URL中<br />那么，有哪些算是非URL安全的字符呢？咱们看表说话：<br />




## version 字段


### npm对version定义的规则要求

<br />对于"version":"x.y.z"<br />1.修复bug,小改动，增加z<br />2.增加了新特性，但仍能向后兼容，增加y<br />3.有很大的改动，无法向后兼容,增加x<br />
<br />例如：我原本的项目是1.0.0版本的话<br />若是1中情况，变为1.0.1<br />若是2中情况，变为1.1.0<br />若是3中情况，变为2.0.0<br />

### npm 有自己的检验version的模块——node-semver

<br />npm有自己的一套检验version正确性的模块，它叫做 node-semver，是一开始就跟随着npm一起被打包安装的。当然了，你也可以通过自己安装去在自己的项目中使用它。<br />使用的例子像这样：<br />先npm install --save semver<br />然后:<br />

```javascript
const semver = require('semver')
semver.valid('1.2.3') // '1.2.3'
semver.valid('a.b.c') // null
```


## keywords 和 description 字段


### 字段要求：
description：字符串<br />keywords：字符串数组<br />
<br />简单地说，这两个东东是npm搜索系统中的搜索条件，所以。如果你试图发布的是一个开源插件，那么这两个字段你应该重视<br />

## license 字段

<br />这是你指定的项目的许可证，它告诉他人他们是否有权利使用你的包，以及，在使用你的包的时候他们应该受到怎样的限制<br />
<br />字段要求：<br />单个license：直接写入名称<br />

```json
{ "license" : "BSD-3-Clause" }
```

<br />多个license：在一对圆括号内写入license名称，且在多个license内用AND等连接<br />

```json
{ "license" : "(ISC AND GPL-3.0)" }
```

<br />[SPDX license表达式的语法规则 2.0版本](https://www.npmjs.com/package/spdx)<br />

## author 字段

<br />要求：一个字符串或是一个对象。<br />如果是一个对象，该对象包含三个属性：<br />name属性(必填)<br />email属性（选填）<br />URL属性（选填）<br />

```json
{ "name" : "Barney Rubble"
, "email" : "b@rubble.com"
, "url" : "http://barnyrubble.tumblr.com/"
}
```


## main 字段

<br />这个是你项目的入口文件。简而言之，当别人安装了你发布的模块时，require你的模块的时候取得的就是你main字段规定的入口文件的输出。<br />
<br />例如你写入了 { "main":"XXX.js"}，而他人通过npm install '你的模块名称' . 安装了你的模块后，他通过 var X = require('你的模块名称')取得的就是你在XXX.js的输出<br />

## script 字段

<br />写进scripts的命令(command),可以通过npm run 或者npm  运行对应的shell指令，例如：{"scripts": { "start": "node main.js"} } 可以让你在终端输入npm start的时候，等同于运行了node main.js<br />

### 什么时候要加“run”,什么时候可以不用加“run”呢？

<br />一个让我们可能有些困扰的问题是，通过`script`字段内的`npm命令`运行脚本时，有时候要加`“run”`，有时候又不要加`"run"`,即有时候是可以直接用`npm <command>`；而有时候又要用`npm run <command>` 才能运行脚本，这该如何区分呢？<br />
<br />首先要提一下的是，`run`的原名是`run-script`，是一段脚本，而`run`是它的一个别名（`alias`）<br />
<br />1.当`run[-script]`被 `test, start, restart, and stop`这四个自带的命令所使用时，它可以被省略（或者说不需要加`“run”`就可以直接调用），所以我们平时最常输入的`npm start`实际上相当于`npm run start`，只不过是为了方便省略了`run`而已<br />
<br />原文：`run[-script] is used by the test, start, restart, and stop commands, but can be called directly`<br />
<br />2.当你在`package.json`的`script`字段中定义的是除了1中的4个命令外的命令的时候，你就不能省略`“run”`了<br />例如你定义<br />

```json
"scripts": {
  "build": "XXX.js"
}
```
的时候，你运行XXX.js就只能通过npm run build去运行了<br />

### npm 为script字段中的脚本路径都加上了node_moudles/.bin前缀

<br />npm为script字段中的脚本路径都加上了node_moudles/.bin前缀，这意味着：你在试图运行本地安装的依赖在 node_modules/.bin 中的脚本的时候，可以省略node_modules/.bin这个前缀。例如：<br />我刚npm install webpack了，而在我的项目下的node_modules目录的.bin子目录下：就多了一个叫做webpack的脚本<br />
<br />本来运行这个脚本的命令应该是：node_modules/.bin webpack<br />但由于npm已经自动帮我们加了node_modules/.bin前缀了，所以我们可以直接写成：<br />

```json
"scripts": {"start": "webpack"}
```

<br />而不用写成：<br />

```json
"scripts": {"start": "node_modules/.bin webpack"}
```


> 原文：npm run adds node_modules/.bin to the PATH provided to scripts. Any binaries provided by locally-installed dependencies can be used without the node_modules/.bin prefix



> npm start是有默认值的，默认为：node server.js



## better-npm-run 的安装与betterScript字段的使用

<br />这个是package.json文档介绍里所没有的，但这里我想特别讲一下：<br />
<br />先通过npm install better-npm-run安装好包，然后你就可以在你的package.json里面使用一个新的字段—— "betterScripts"字段<br />
<br />故名思意，它和"scripts"字段很像，那么两者间有什么联系呢？咱还是用代码说话吧，它可以把<br />

```json
"scripts": {
   "test": "NODE_ENV=production karma start"
}
```

<br />变成：<br />

```json
"scripts": {
    "test": "better-npm-run test"
},
"betterScripts": {
    "test": {
        "command": "karma start",
        "env": {
            "NODE_ENV": "test"
          }
       }
}
```

<br />简单地说，就是当运行"scripts"字段中的命令的时候，它会进一步去运行 "betterScripts"中对应的命令，并通过"env"对象控制运行时的环境变量，如NODE_ENV。<br />
<br />好处是让你的代码的可读性更强一些<br />
<br />另外提一下NODE_ENV的作用：<br />用来设置环境变量（默认值为development）。<br />通过检查这个值可以分别对开发环境和生产环境下做不同的处理<br />
<br />例如在服务端代码中通过检查是否是开发环境（development）决定是否启动代码热重载功能<br />
<br />（热重载只是为了在开发环境【developmen】提高生产效率用，在生产环境【production】没用）<br />

```javascript
if (process.env.NODE_ENV === 'development') {
// 省略诸多内容
app.use(require('webpack-hot-middleware')(compiler, {
    path: '/__webpack_hmr'
}))
}
```


## dependencies 字段和 devDependencies 字段

<br />dependencies字段和devDependencies字段分别代表生产环境依赖和开发环境依赖<br />
<br />与两个字段相关的npm install的命令<br />npm install 模块 --save 安装好后写入package.json的dependencies中（生产环境依赖）<br />npm install 模块 --save-dev 安装好后写入package.json的devDepencies中（开发环境依赖）<br />
<br />**怎么区分到底安装包的时候放在dependencies中还是devDepencies中呢？**<br />
<br />很简单<br />1.一般你去github或者npm社区里面相关包的介绍后面都会带有--save 或者--save-dev 的参数的，这时候把命令直接复制过来运行就OK了，不用管那么多<br />
<br />2.如果没有1中的介绍，那么请思考，这个包到底是纯粹为了开发方便使用呢？还是要放到上线后APP的代码中呢？前者则为devDepencies，后者则为dependencies<br />
<br />【注意】：在团队协作中，一个常见的情景是他人从github上clone你的项目，然后通过npm install安装必要的依赖，（刚从github上clone下来是没有node_modules的，需要安装）那么根据什么信息安装依赖呢？就是你的package.json中的dependencies和devDepencies。所以，在本地安装的同时，将依赖包的信息（要求的名称和版本）写入package.json中是很重要的！

## prepublishOnly 字段


```json
"prepublishOnly": "npm run build"
```

<br />这样每次执行`npm publish`前都会先执行`npm run build`<br />

<a name="b7imam"></a>
## peerDependencies 字段
同版本依赖<br />npm2中，antd包中`peerDependencies`所指定的依赖会随着`npm install antd`一起被强制安装，所以不需要在宿主环境的package.json文件中指定对antd中`peerDependencies`内容的依赖。<br />但是在npm3中，`peerDependencies`的表现与npm2不同：
> npm3中不会再要求peerDependencies所指定的依赖包被强制安装，相反npm3会在安装结束后检查本次安装是否正确，如果不正确会给用户打印警告提示。

就拿上面的例子来说，如果我们npm install antd安装antd时，你会得到一个警告提示说：<br />
<br />`react是一个需要的依赖，但是没有被安装。`<br />这时，你需要手动的在MyProject项目的package.json文件指定antd的依赖。<br />另外，在npm3的项目中，可能存在一个问题就是你所依赖的一个package包更新了它peerDependencies的版本，那么你可能也需要在项目的package.json文件中手动更新到正确的版本。<br />

# 非官方字段
## yarn 相关字段

[yarn](https://github.com/yarnpkg/yarn) : 类似 npm 的依赖管理工具，但 yarn 缓存了每个下载过的包，所以再次使用时无需重复下载，同时利用并行下载以最大化资源利用率，因此安装速度更快。
<a name="urd0tl"></a>
### flat
```json
{
    "flat": true
}
```

如果你的包只允许给定依赖的一个版本，你想强制和命令行上 yarn install --flat 相同的行为，把这个值设为 true。<br />
<br />详细参考 [yarn - flat](https://yarnpkg.com/zh-Hans/docs/package-json#toc-flat)..<br />

### resolutions
```json
{
    "resolutions": {
        "transitive-package-1": "0.0.29",
        "transitive-package-2": "file:./local-forks/transitive-package-2",
        "dependencies-package-1/transitive-package-3": "^2.1.1"
    }    
}
```

<br />允许你覆盖特定嵌套依赖项的版本。有关完整规范，请参见[选择性版本解析 RFC](https://github.com/yarnpkg/rfcs/blob/master/implemented/0000-selective-versions-resolutions.md)。<br />
<br />详细参考 [yarn - resolutions](https://yarnpkg.com/zh-Hans/docs/package-json#toc-resolutions).<br />

## unpkg 相关字段

[unpkg](https://github.com/unpkg/unpkg.com): 让 npm 上所有的文件都开启 cdn 服务。<br />
<br />unpkg<br />

```json
# jquery
{
"unpkg": "dist/jquery.js"
}
```

正常情况下，访问 jquery 的发布文件通过 [https://unpkg.com/jquery@3.3.1/dist/jquery.js，当你使用省略的](https://unpkg.com/jquery@3.3.1/dist/jquery.js%EF%BC%8C%E5%BD%93%E4%BD%A0%E4%BD%BF%E7%94%A8%E7%9C%81%E7%95%A5%E7%9A%84) url [https://unpkg.com/jquery](https://unpkg.com/jquery) 时，便会按照如下的方式获取文件：<br />
<br />[latestVersion] 指最新版本号，pkg 指 package.json<br />

- 定义了 unpkg 属性时



<br />[https://unpkg.com/jquery@[latestVersion]/[pkg.unpkg]](https://unpkg.com/jquery@%5BlatestVersion%5D/%5Bpkg.unpkg%5D)<br />

- 未定义 unpkg 属性时，将回退到 main 属性



<br />[https://unpkg.com/jquery@[latestVersion]/[pkg.main]](https://unpkg.com/jquery@%5BlatestVersion%5D/%5Bpkg.main%5D)<br />详细参考 [https://unpkg.com](https://unpkg.com).<br />

### TypeScript 相关字段

TypeScript: JavaScript 的超集<br />types, typings
```json
{
    "main": "./lib/main.js",
    "types": "./lib/main.d.ts"
}
```

就像 main 字段一样，定义一个针对 TypeScript 的入口文件。<br />
<br />详细参考 TypeScript documentation.<br />

### browserslist 相关字段

browserslist: 设置项目的浏览器兼容情况。<br />browserslist<br />

```json
{
"browserslist": [
"> 1%",
"last 2 versions"
]
}
```

<br />
支持的工具：<br />
<br />Autoprefixer<br />Babel<br />postcss-preset-env<br />eslint-plugin-compat<br />stylelint-no-unsupported-browser-features<br />postcss-normalize<br />详细参考 browserslist.<br />

<a name="whz6lg"></a>
### 发行打包相关字段

点击 Setting up multi-platform npm packages 查看相关介绍。<br />

#### module
```json
{
"main": "./lib/main.js",
"module": "./lib/main.m.js"
}
```

<br />就像 main 字段一样，定义一个针对 es6 模块及语法的入口文件。<br />构建工具在构建项目的时候，如果发现了这个字段，会首先使用这个字段指向的文件，如果未定义，则回退到 main 字段指向的文件。<br />
<br />支持的工具：<br />
<br />rollup<br />webpack<br />详细参考 rollup - pkg.module.<br />

#### browser
```
{
    "main": "./lib/main.js",
    "browser": "./lib/main.b.js"
}
```
指定该模块供浏览器使用的入口文件。<br />如果这个字段未定义，则回退到 main 字段指向的文件。<br />
<br />支持的工具：<br />
<br />rollup<br />webpack<br />browserify<br />详细参考 babel-plugin-module-resolver.<br />

#### esnext
```
{
"main": "main.js",
"esnext": "main-esnext.js"
}
```


```json
{
    "main": "main.js",
    "esnext": {
    "main": "main-esnext.js",
    "browser": "browser-specific-main-esnext.js"
    }
}
```
使用 es 模块化规范，stage 4 特性的源代码。<br />详细参考 Transpiling dependencies with Babel, Delivering untranspiled source code via npm.<br />

#### es2015
```json
{
    "main": "main.js",
    "es2015": "main-es2015.js"
}
```
Angular 定义的未转码的 es6 源码。<br />详细参考 [https://docs.google.com/document/d/1CZC2rcpxffTDfRDs6p1cfbmKNLA6x5O-NtkJglDaBVs/edit#](https://docs.google.com/document/d/1CZC2rcpxffTDfRDs6p1cfbmKNLA6x5O-NtkJglDaBVs/edit#).<br />
<br />esm<br />详细参考 adjusted proposal: ES module "esm": true package.json flag.<br />

### react-native 相关字段

react-native: 使用 react 组件技术写原生APP。<br />

#### react-native
```json
{
    "main": "./lib/main.js",
    "react-native": "./lib/main.react-native.js"
}
```

<br />指定该模块供 react-native 使用的入口文件。<br />如果这个字段未定义，则回退到 main 字段指向的文件。<br />
<br />源代码查看.<br />

### webpack 相关字段
#### sideEffects
```json
{
    "sideEffects": true|false
}
```
声明该模块是否包含 sideEffects（副作用），从而可以为 tree-shaking 提供更大的优化空间。<br />详细参考 sideEffects example, proposal for marking functions as pure, eslint-plugin-tree-shaking.<br />

<a name="o47yaz"></a>
### microbundle 相关字段

microbundle: 基于 rollup 零配置快速打包工具。
<a name="fhklgb"></a>
#### source


```json
{
    "source"： "src/index.js"
}
```

源文件入口文件。<br />
<br />详细参考 Specifying builds in package.json.<br />

<a name="u7pxuy"></a>
#### umd:main
```json
{
    "umd:main"： "dist/main.umd.js"
}
```

<br />umd 模式 bundle 文件。<br />详细参考 Specifying builds in package.json.<br />

### parcel 相关字段

parcel: 零配置打包工具。<br />

#### source

查看 parcel-bundler/parcel#1652.<br />

### babel 相关字段

babel: es6 -> es5 转码器。<br />

#### babel

配置 babel。<br />

### eslint 相关字段

eslint: js 代码检查与优化。<br />

#### eslintConfig

配置 eslint。<br />

### jest 相关字段

jest: js 测试库。<br />

#### jest
```json
{
    "jest": {
        "verbose": true
    }
}
```
配置 jest。<br />详细参考 jest docs.<br />

### stylelint 相关字段

stylelint: style 代码检查与优化。<br />
<br />stylelint<br />配置 stylelint。<br />
<br />详细参考 New configuration loader.<br />

### ava 相关字段

ava: js 测试库。<br />

#### ava
```json
{
    "ava": {
        "require": [ "@std/esm" ]
    }
}
```
配置 ava。<br />详细参考 ava configuration.<br />

## nyc 相关字段

nyc: istanbul.js 命令行。<br />

#### nyc
```json
{
    "nyc": {
    "extension": [".js", ".mjs"],
    "require": ["@std/esm"]
    }
}
```

<br />配置 nyc。<br />详细参考 nyc docs.<br />

### CommonJS 保留字段

保留字段: build, default, email, external, files, imports, maintainer, paths, platform, require, summary, test, using, downloads, uid.<br />
<br />不可用字段: id, type, 以 _ 和 $ 开头的字段。<br />

### Standard JS 相关字段

Standard JS: js 代码检查与优化。<br />

#### standard
```json
{
    "standard": {
    "parser": "babel-eslint",
    "ignore": [
        "**/out/",
        "/lib/select2/",
        "/lib/ckeditor/",
        "tmp.js"
        ]
    }
}
```
配置 standard.<br />详细参考 [https://standardjs.com/](https://standardjs.com/).<br />

### 其他

style<br />声明当前模块包含 style 部分，并指定入口文件。<br />
<br />支持的工具：<br />
<br />parcelify<br />npm-less<br />rework-npm<br />npm-css<br />详细参考 Package.json "style" Attribute, istf-spec.<br />

#### less

与 style 一样，但是是 less 文件。<br />
<br />支持的工具：<br />
<br />npm-less