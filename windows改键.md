---
title: "Windows改键"
date: 2020-10-06T19:45:07+08:00
---

## 场景

在使用vim的时候，以前是将esc键盘改为jj键盘，后来发现是错误的，于是将键盘改回去，并使用caps切换大小写健代替esc键

## 方法

新建.reg文件
编辑如下

```
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Keyboard Layout]
"Scancode Map"=hex:00,00,00,00,00,00,00,00,03,00,00,00,3a,00,01,00,01,00,3a,00,00,00,00,00
```

然后进行重启电脑

## 修改其它键位

https://blog.csdn.net/lhdalhd1996/article/details/90741092