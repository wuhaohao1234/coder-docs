---
title: "Spring Boot"
date: 2021-02-02T14:55:40+08:00
---

# springBoot

本篇文章会以最小且必备的知识点，带你入门后端java开发

spring 框架搭建: [spring](https://start.spring.io/)

![](https://my-blog-to-use.oss-cn-beijing.aliyuncs.com/2019-7/spring.start.io.png)

注意勾选上 Spring Web 这个模块，这是我们所必需的一个依赖。当所有选项都勾选完毕之后，点击下方的按钮 Generate 下载这个 Spring Boot 的项目。下载完成并解压之后，我们直接使用 IDEA 打开即可。

常用目录结构

项目初始

1. Application.java是项目的启动类
2. domain目录主要用于实体（Entity）与数据访问层（Repository）
3. service 层主要是业务类代码
4. controller 负责页面访问控制
5. config 目录主要放一些配置类

## hello world

快捷键 alt + insert 创建package 名称为controller

alt + insert 创建class 名称为HelloWorld

在spring中，一个类=一个文件，一个package=一个目录

```java
package com.example.demo.controller;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("test")
public class HelloWorldController {
    @GetMapping("hello")
    String sayHello() {
        return "hello world";
    }
}

```

使用shift + f10 启动项目，然后访问localhost:8080/test/hello可看到hello world

## resultful

创建entry包

然后创建Book实体类

```java
package com.example.demo.entity;

public class Book {
    private String name;
    private String desc;

    public void setName(String name) {
        this.name = name;
    }

    public String getDesc() {
        return desc;
    }

    public void setDesc(String desc) {
        this.desc = desc;
    }

    public String getName() {
        return name;
    }
}

```

在controller包中创建BookController类

```java
package com.example.demo.controller;

import com.example.demo.entity.Book;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

import java.util.ArrayList;
import java.util.List;
@RestController
@RequestMapping("/api")
public class BookController {
    private List<Book> books = new ArrayList<>();
    @PostMapping("/book")
    public ResponseEntity<List<Book>> addBook(@RequestBody Book book) {
        books.add(book);
        return ResponseEntity.ok(books);
    }
    @DeleteMapping("/book/{id}")
    public ResponseEntity<List<Book>> deleteBookById(@PathVariable("id") int id) {
        books.remove(id);
        return ResponseEntity.ok(books);
    }
    @GetMapping("/book")
    public ResponseEntity<List<Book>> getBookByName() {
        return ResponseEntity.ok(books);
    }
}

```

1. @RestController 将返回的对象数据直接以 JSON 或 XML 形式写入 HTTP 响应(Response)中。绝大部分情况下都是直接以 JSON 形式返回给客户端，很少的情况下才会以 XML 形式返回。转换成 XML 形式还需要额为的工作，上面代码中演示的直接就是将对象数据直接以 JSON 形式写入 HTTP 响应(Response)中。

2. @RequestMapping :上面的示例中没有指定 GET 与 PUT、POST 等，因为**@RequestMapping默认映射所有HTTP Action**，你可以使用@RequestMapping(method=ActionType)来缩小这个映射。

3. @PostMapping实际上就等价于 @RequestMapping(method = RequestMethod.POST)，同样的  @DeleteMapping ,@GetMapping也都一样，常用的 HTTP Action 都有一个这种形式的注解所对应。
@PathVariable :取url地址中的参数。@RequestParam  url的查询参数值。

4. @RequestBody:可以将 HttpRequest body 中的 JSON 类型数据反序列化为合适的 Java 类型。
ResponseEntity: 表示整个HTTP Response：状态码，标头和正文内容。我们可以使用它来自定义HTTP Response 的内容。

## 整合mybatis

在mysql中创建库test

在test库中创建user表

```sql
CREATE TABLE `user` (
  `id` int(13) NOT NULL AUTO_INCREMENT COMMENT '主键',
  `name` varchar(33) DEFAULT NULL COMMENT '姓名',
  `age` int(3) DEFAULT NULL COMMENT '年龄',
  `money` double DEFAULT NULL COMMENT '账户余额',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=5 DEFAULT CHARSET=utf8
```

pom.xml配置依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.4.2</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
	<groupId>com.example</groupId>
	<artifactId>demo</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<name>demo</name>
	<description>Demo project for Spring Boot</description>
	<properties>
		<java.version>11</java.version>
	</properties>
	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
		<dependency>
			<groupId>org.mybatis.spring.boot</groupId>
			<artifactId>mybatis-spring-boot-starter</artifactId>
			<version>1.3.2</version>
		</dependency>
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<scope>runtime</scope>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-devtools</artifactId>
			<scope>runtime</scope>
			<optional>true</optional>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
		<dependency>
			<groupId>org.projectlombok</groupId>
			<artifactId>lombok</artifactId>
			<version>1.18.10</version>
		</dependency>
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>

</project>

```

application.properties配置sql链接与端口号设置

```
server.port=8333
spring.datasource.url=jdbc:mysql://127.0.0.1:3306/test?useUnicode=true&characterEncoding=utf8&useSSL=false&serverTimezone=GMT%2B8
spring.datasource.username=root
spring.datasource.password=abu0418
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
```

创建用户类 Bean

bean/User.java

```java
package com.example.demo.bean;

public class User {
    private int id;
    private int age;
    public String name;
    private double money;

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public double getMoney() {
        return money;
    }

    public void setMoney(double money) {
        this.money = money;
    }
}

```
Dao 层开发

接口interface

```java
package com.example.demo.Dao;

import com.example.demo.bean.User;
import org.apache.ibatis.annotations.*;

import java.util.List;

@Mapper
public interface UserDao {
    @Select("SELECT * FROM user WHERE name = #{name}")
    User findUserByName(@Param("name") String name);
    @Select("SELECT * FROM user")
    List<User> findAllUser();
    @Insert("INSERT INTO user(name, age,money) VALUES(#{name}, #{age}, #{money})")
    void insertUser(@Param("name") String name, @Param("age") Integer age, @Param("money") Double money);
    @Update("UPDATE  user SET name = #{name},age = #{age},money= #{money} WHERE id = #{id}")
    void updateUser(@Param("name") String name, @Param("age") Integer age, @Param("money") Double money,
                    @Param("id") int id);
    @Delete("DELETE from user WHERE id = #{id}")
    void deleteUser(@Param("id") int id);
}

```

service层UserService

```java
package com.example.demo.service;

import com.example.demo.Dao.UserDao;
import com.example.demo.bean.User;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

import java.util.List;

@Service
public class UserService {
    @Autowired
    private UserDao userDao;
    public User selectUserByName(String name) {
        return userDao.findUserByName(name);
    }
    public List<User> selectAllUser() {
        return userDao.findAllUser();
    }
    public List<User> insertService() {
        userDao.insertUser("SnailClimb", 22, 33.1);
        userDao.insertUser("abu", 25, 33.1);
        return userDao.findAllUser();
    }
    public void deleteService(int id) {
        userDao.deleteUser(id);
    }
    /**
     * 模拟事务。由于加上了 @Transactional注解，如果转账中途出了意外 SnailClimb 和 Daisy 的钱都不会改变。
     */
    @Transactional
    public void changemoney() {
        userDao.updateUser("SnailClimb", 22, 2000.0, 3);
        // 模拟转账过程中可能遇到的意外状况
        int temp = 1 / 0;
        userDao.updateUser("Daisy", 19, 4000.0, 4);
    }
}

```

创建controller层UserController

```java
package com.example.demo.controller;

import com.example.demo.bean.User;
import com.example.demo.service.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.List;

@RestController
@RequestMapping("user")
public class UserController {
    @Autowired
    private UserService userService;
    @RequestMapping("/add")
    List<User> insertUser() {
        return userService.insertService();
    }
    @RequestMapping("/query")
    User testQuery() {
        return userService.selectUserByName("SnailClimb");
    }
    @RequestMapping("/queryAll")
    public List<User> selectAllUsers() {
        return userService.selectAllUser();
    }

    @RequestMapping("/changeover")
    public List<User> testChangeMoney() {
        userService.changemoney();
        return userService.selectAllUser();
    }

    @RequestMapping("/delete")
    public String testDelete() {
        userService.deleteService(3);
        return "OK";
    }
}

```

使用postman调用即可