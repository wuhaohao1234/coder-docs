---
title: "Mysql"
date: 2020-07-10T19:45:34+08:00
tags: ["sql操作"]
---

### mysql

参考:

> https://juejin.im/post/5ae55861f265da0ba062ec71

> https://segmentfault.com/a/1190000006876419

> https://www.liaoxuefeng.com/wiki/1177760294764384

#### 前提

安装: 自行百度

1. 修改密码

```
如果密码忘记可用如下方法

先结束掉mysql服务net stop mysql

然后删除datadir目录

然后重新执行

mysqld --initialize --console

记住密码

修改密码方法

链接mysql然后修改密码

set password for 用户名@localhost = password('新密码');
```

2. 时区问题

mysql8时区问题

在MySQL命令行中执行如下代码：

```
set global time_zone = '+8:00';  # 修改mysql全局时区为北京时间，即我们所在的东8区
set time_zone = '+8:00';  ##修改当前会话时区
flush privileges;  #立即生效
```

#### 创造数据

1. 创建数据库test

```
CREATE DATABASE IF NOT EXISTS test;
```
2. 选择test数据库

```
USE test;
```

3. 删除classes表和students表（如果存在）：

```
DROP TABLE IF EXISTS classes;
DROP TABLE IF EXISTS students;
```

4. 创建classes表：

```
CREATE TABLE classes (
    id BIGINT NOT NULL AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    PRIMARY KEY (id)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

5. 创建students表：

```
CREATE TABLE students (
    id BIGINT NOT NULL AUTO_INCREMENT,
    class_id BIGINT NOT NULL,
    name VARCHAR(100) NOT NULL,
    gender VARCHAR(1) NOT NULL,
    score INT NOT NULL,
    PRIMARY KEY (id)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

6. 插入classes记录：

```
INSERT INTO classes(id, name) VALUES (1, '一班');
INSERT INTO classes(id, name) VALUES (2, '二班');
INSERT INTO classes(id, name) VALUES (3, '三班');
INSERT INTO classes(id, name) VALUES (4, '四班');
```

简写: insert into test.classes values (5, "五班");

7.  插入students记录

```
INSERT INTO students (id, class_id, name, gender, score) VALUES (1, 1, '小明', 'M', 90);
INSERT INTO students (id, class_id, name, gender, score) VALUES (2, 1, '小红', 'F', 95);
INSERT INTO students (id, class_id, name, gender, score) VALUES (3, 1, '小军', 'M', 88);
INSERT INTO students (id, class_id, name, gender, score) VALUES (4, 1, '小米', 'F', 73);
INSERT INTO students (id, class_id, name, gender, score) VALUES (5, 2, '小白', 'F', 81);
INSERT INTO students (id, class_id, name, gender, score) VALUES (6, 2, '小兵', 'M', 55);
INSERT INTO students (id, class_id, name, gender, score) VALUES (7, 2, '小林', 'M', 85);
INSERT INTO students (id, class_id, name, gender, score) VALUES (8, 3, '小新', 'F', 91);
INSERT INTO students (id, class_id, name, gender, score) VALUES (9, 3, '小王', 'M', 89);
INSERT INTO students (id, class_id, name, gender, score) VALUES (10, 3, '小丽', 'F', 85);
```

简写: INSERT INTO test.students VALUES (11, 2, '阿布', 'F', 85);

#### 使用数据

1. 查询所有学生

`SELECT * FROM students;`

2. 条件查询分数大于80的

`SELECT * FROM students WHERE score >= 80;`

3. 查找我想看到的字段

`SELECT id, name FROM students;`

4. 根据成绩由低到高查到我想看到的字段

`SELECT id, name, gender, score FROM students ORDER BY score;`

5. 分页查询

```
SELECT id, name, gender, score
FROM test.students
LIMIT 3 OFFSET 0;
```

6. 查询总数居有多少条

`SELECT COUNT(*) FROM students;`

7. 多表查询

`SELECT * FROM test.students, test.classes;`

8. 链接查询

内链接

```
SELECT student_item.id, student_item.name, student_item.class_id, student_item.name class_name, student_item.gender, student_item.score
FROM test.students student_item
INNER JOIN test.classes class_item
ON student_item.class_id = class_item.id;
```

外链接

```
SELECT student_item.id, student_item.name, student_item.class_id, student_item.name class_name, student_item.gender, student_item.score
FROM test.students student_item
RIGHT OUTER JOIN test.classes class_item
ON student_item.class_id = class_item.id;
```

### 事务

事务是指逻辑上的一组操作，组成这组操作的各个单元，要不全成功要不全失败。 
    - 支持连续SQL的集体成功或集体撤销。
    - 事务是数据库在数据晚自习方面的一个功能。
    - 需要利用 InnoDB 或 BDB 存储引擎，对自动提交的特性支持完成。
    - InnoDB被称为事务安全型引擎。

-- 事务开启
    START TRANSACTION; 或者 BEGIN;
    开启事务后，所有被执行的SQL语句均被认作当前事务内的SQL语句。
-- 事务提交
    COMMIT;
-- 事务回滚
    ROLLBACK;
    如果部分操作发生问题，映射到事务开启前。

-- 事务的特性
    1. 原子性（Atomicity）
        事务是一个不可分割的工作单位，事务中的操作要么都发生，要么都不发生。
    2. 一致性（Consistency）
        事务前后数据的完整性必须保持一致。
        - 事务开始和结束时，外部数据一致
        - 在整个事务过程中，操作是连续的
    3. 隔离性（Isolation）
        多个用户并发访问数据库时，一个用户的事务不能被其它用户的事物所干扰，多个并发事务之间的数据要相互隔离。
    4. 持久性（Durability）
        一个事务一旦被提交，它对数据库中的数据改变就是永久性的。

-- 事务的实现
    1. 要求是事务支持的表类型
    2. 执行一组相关的操作前开启事务
    3. 整组操作完成后，都成功，则提交；如果存在失败，选择回滚，则会回到事务开始的备份点。

-- 事务的原理
    利用InnoDB的自动提交(autocommit)特性完成。
    普通的MySQL执行语句后，当前的数据提交操作均可被其他客户端可见。
    而事务是暂时关闭“自动提交”机制，需要commit提交持久化数据操作。

-- 注意
    1. 数据定义语言（DDL）语句不能被回滚，比如创建或取消数据库的语句，和创建、取消或更改表或存储的子程序的语句。
    2. 事务不能被嵌套

-- 保存点
    SAVEPOINT 保存点名称 -- 设置一个事务保存点
    ROLLBACK TO SAVEPOINT 保存点名称 -- 回滚到保存点
    RELEASE SAVEPOINT 保存点名称 -- 删除保存点

-- InnoDB自动提交特性设置
    SET autocommit = 0|1;    0表示关闭自动提交，1表示开启自动提交。
    - 如果关闭了，那普通操作的结果对其他客户端也不可见，需要commit提交后才能持久化数据操作。
    - 也可以关闭自动提交来开启事务。但与START TRANSACTION不同的是，
        SET autocommit是永久改变服务器的设置，直到下次再次修改该设置。(针对当前连接)
        而START TRANSACTION记录开启前的状态，而一旦事务提交或回滚后就需要再次开启事务。(针对当前事务)


/* 锁表 */
表锁定只用于防止其它客户端进行不正当地读取和写入
MyISAM 支持表锁，InnoDB 支持行锁
-- 锁定
    LOCK TABLES tbl_name [AS alias]
-- 解锁
    UNLOCK TABLES


/* 触发器 */ ------------------
    触发程序是与表有关的命名数据库对象，当该表出现特定事件时，将激活该对象
    监听：记录的增加、修改、删除。

-- 创建触发器
CREATE TRIGGER trigger_name trigger_time trigger_event ON tbl_name FOR EACH ROW trigger_stmt
    参数：
    trigger_time是触发程序的动作时间。它可以是 before 或 after，以指明触发程序是在激活它的语句之前或之后触发。
    trigger_event指明了激活触发程序的语句的类型
        INSERT：将新行插入表时激活触发程序
        UPDATE：更改某一行时激活触发程序
        DELETE：从表中删除某一行时激活触发程序
    tbl_name：监听的表，必须是永久性的表，不能将触发程序与TEMPORARY表或视图关联起来。
    trigger_stmt：当触发程序激活时执行的语句。执行多个语句，可使用BEGIN...END复合语句结构

-- 删除
DROP TRIGGER [schema_name.]trigger_name

可以使用old和new代替旧的和新的数据
    更新操作，更新前是old，更新后是new.
    删除操作，只有old.
    增加操作，只有new.

-- 注意
    1. 对于具有相同触发程序动作时间和事件的给定表，不能有两个触发程序。

