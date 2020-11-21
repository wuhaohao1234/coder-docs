---
title: "Java Spring"
date: 2020-07-12T21:13:20+08:00

tags: ["springboot项目", "java"]
---

# spring boot入门

学习资料

> https://gitee.com/yidao620/springboot-bucket/tree/master
> https://mp.weixin.qq.com/s/-OpyJ7VvrXnKuuGsVTEXJQ
> https://www.liaoxuefeng.com/wiki/1252599548343744/1266265175882464


## 什么是 Spring Boot

Spring Boot 是由 Pivotal 团队提供的全新框架，其设计目的是用来简化新 Spring 应用的初始搭建以及开发过程。该框架使用了特定的方式来进行配置，从而使开发人员不再需要定义样板化的配置。用我的话来理解，就是 Spring Boot 其实不是什么新的框架，它默认配置了很多框架的使用方式，就像 Maven 整合了所有的 Jar 包，Spring Boot 整合了所有的框架。

## 使用 Spring Boot 有什么好处

其实就是简单、快速、方便！平时如果我们需要搭建一个 Spring Web 项目的时候需要怎么做呢？

* 配置 web.xml，加载 Spring 和 Spring mvc
* 配置数据库连接、配置 Spring 事务
* 配置加载配置文件的读取，开启注解
* 配置日志文件
* 配置完成之后部署 Tomcat 调试

现在非常流行微服务，如果我这个项目仅仅只是需要发送一个邮件，如果我的项目仅仅是生产一个积分；我都需要这样折腾一遍!

但是如果使用 Spring Boot 呢？
很简单，我仅仅只需要非常少的几个配置就可以迅速方便的搭建起来一套 Web 项目或者是构建一个微服务！

## 快速入门

### idea创建spring boot项目

> 前提: 需要maven与springboot Initializr

1. File->new->project；

2. 选择“Spring Initializr”，点击next；（jdk1.8默认即可）

3. 完善项目信息，组名可不做修改，项目名可做修改；最终建的项目名为：test，src->main->java下包名会是：com->example->test；点击next；

4. Web下勾选Spring Web Star

5. 选择项目路径，点击finish；打开新的窗口；

### Maven 构建项目

1. 访问 http://start.spring.io/
2. 选择构建工具 Maven Project、Java、Spring Boot 版本 2.1.3 以及一些工程基本信息
3. 点击 Generate Project 下载项目压缩包
4. 解压后，使用 Idea 导入项目，File -> New -> Model from Existing Source.. -> 选择解压后的文件夹 -> OK，选择 Maven 一路 Next，OK done!
5. 如果使用的是 Eclipse，Import -> Existing Maven Projects -> Next -> 选择解压后的文件夹 -> Finsh，OK done!

Spring Boot 的基础结构共三个文件:

```
src/main/java 程序开发以及主程序入口
src/main/resources 配置文件
src/test/java 测试程序
```

### 编写 Controller 内容：

新建controller包

```
@RestController
public class HelloWorldController {
    @RequestMapping("/hello")
    public String index() {
        return "Hello World";
    }
}
```

运行主程序

访问 `localhost:8080/hello` 查看返回值



