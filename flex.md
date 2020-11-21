---
title: "Flex"
date: 2020-06-27T22:37:01+08:00
tags: ["flex"]
---

## flex布局

> 参考 [阮一峰flex](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)

> [b站地址](https://www.bilibili.com/video/BV19T4y1J7X2)

### 弹性布局

Flex 是 Flexible Box 的缩写，意为"弹性布局"，用来为盒状模型提供最大的灵活性。

任何一个容器都可以指定为 Flex 布局。

只需要给父级设置为`display: flex`

```
.box{
  display: flex;
}
```

### 以下是父元素设置中最常用的

> 以下是项目中最长用到的,详细的看原理

#### 一、子级元素排列方式

`flex-direction: row | column`

row 从左向右，column 从上向下

*flex-flow: <flex-direction> || <flex-wrap>;*

#### 二、水平居中

`justify-content: space-between | space-around | center`

* space-between 两端对齐，项目之间的间隔都相等。

* space-around 每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍。

* center 居中

#### 三、垂直居中

`align-items: center`

### 以下是子元素设置最常用的

#### 四、子元素自适应扩大

`flex-grow: <number>`

如果所有项目的flex-grow属性都为1，则它们将等分剩余空间（如果有的话）。如果一个项目的flex-grow属性为2，其他项目都为1，则前者占据的剩余空间将比其他项多一倍。

#### 五、子元素自适应填充

`flex-basis: <length> | auto;`

#### 六、子元素设置flex属性

` flex: none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]`

该属性有两个快捷值：auto (1 1 auto) 和 none (0 0 auto)。

建议优先使用这个属性，而不是单独写三个分离的属性，因为浏览器会推算相关值。