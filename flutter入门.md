---
title: "Flutter入门"
date: 2021-01-03T22:49:52+08:00
---

## 从TodoList入门Flutter开发

本文不会教你如何搭建开发环境，如果你想搭建，Flutter官网 讲的更加详细，我们可以直接访问 dartpad 这个网站直接开发Flutter

网站: https://www.dartpad.dev

### 基本结构

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Center(child: Text('Hello, World!')),
    );
  }
}
```
效果: 

![](https://mmbiz.qpic.cn/mmbiz_png/Y92zTxicekHWRRQK7hdiaiahy4RY1GX7V41sYJaGE3LY6icCnYvBAyKDTflTdVelFv9NnwI7SwicQfsMUIpnf96xLfA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)


代码解析：

* import 'package:flutter/material.dart'; 是使用Flutter组件必须要引入的，里面包含了 Text, Image, Center等基础组件等等。

* main 函数是Flutter 的入口函数，它使用 runApp 运行了 MyAPP 这个weiget。

* 接下来我们定义了 MyApp 这个 weiget，重写了它的 build 方法，使用了 MaterialApp 这个入口 widget，定义了首页的样子。

> MaterialApp还包含了title，routes等属性。

无状态StatelessWidget和有状态StatefulWidget

和React当中的有状态组件和无状态组件一样，有状态组件就是有单独的交互逻辑和业务数据，通过 setState 来更新，而无状态组件则没有这些，所有的数据都是由外部传进来。

### 开发todo页面

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: TodoPage(),
    );
  }
}

// 新增代码
class TodoPage extends StatefulWidget {
  TodoPage({Key key}) : super(key: key);

  @override
  _TodoPageState createState() => _TodoPageState();
}

class _TodoPageState extends State<TodoPage> {
  @override
  Widget build(BuildContext context) {
    return Center(child: Text('Hello, World!'));
  }
}
```


代码解析：

* 因为我们要有数据和交互逻辑，所以我们使用了 StatefulWidget 来创建一个有状态的 TodoPage widget。
* 首先我们使用 TodoPage 初始化一些入参以及继承父类的一些东西。
* 接着我们重写了 createState 这个方法，让它生成一个 _TodoPageState 的数据。
* _TodoPageState 继承了 state，在里面可以定义一些可响应的变量。

脚手架Scaffold

```dart
class _TodoPageState extends State<TodoPage> {
  @override
  Widget build(BuildContext context) {
    //新增代码，省略一些没变动代码。。。
    return Scaffold(
      appBar: AppBar(title: Text('todo list')),
      body: Center(child: Text('Hello, World!'))
    );
  }
}
```

> Scaffold有 appBar，drawer，bottomNavigationBar，body等属性，可以快速搭建一个页面结构

定义数据和Model

```dart
//新增代码
class TodoModel {
  TodoModel({
    this.id,
    this.title,
    this.complete = false,
  });

  int id;
  String title;
  bool complete;
}

class _TodoPageState extends State<TodoPage> {
  //新增代码，省略一些没变动代码。。。
  List<TodoModel> todos = [];
  int id = 0;
  //...
}
```
代码解析：

* 这里我们使用了 dart 中的 List 类型定义了 todos 变量，int 类型定义了个 id，更多dart类型看dart官方文档

* TodoModel 这个是什么？如果你写过 Typescript，那你应该清楚了，TodoModel定义了 todos内部的结构类型，在我们使用了就可以有很方便的提示，以及一些类型检测功能。

### 构建输入框和列表

一个Todo应用，最基本的结构就是一个可输入的表单和一个展示todo的列表，在Flutter中，我们使用 TextField 这个widget来创建输入框，使用 ListView 来构建列表：

```dart
class _TodoPageState extends State<TodoPage> {
  List<TodoModel> todos = [];
  int id = 0;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('todo list')),
      body: Column(children: [
      TextField(),
      Expanded(
          child: ListView.builder(
              itemBuilder: (context, index) {
                TodoModel item = todos[index];
                return ListTile(
                  title: Text(item.title),
                );
              },
              itemCount: todos.length)),
    ])
    );
  }
}
```
代码解析：

* Column 是纵向布局widget。

* ListView 两个重要属性， itemBuilder 和 itemCount ，一个是返回构建列表项的元素，一个是列表的数量，组合就构成了一个可滚动的列表。
ListTile 和 Text

* 注意：在使用Column时，经常会出现一些报错，那是因为Flutter容器的限制，在这个我们就使用 Expanded来包裹ListView，防止出现类似的错误。

添加Todo

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: TodoPage(),
    );
  }
}
class TodoPage extends StatefulWidget {
  TodoPage({Key key}) : super(key: key);

  @override
  _TodoPageState createState() => _TodoPageState();
}


class TodoModel {
  TodoModel({
    this.id,
    this.title,
    this.complete = false,
  });

  int id;
  String title;
  bool complete;
}


class _TodoPageState extends State<TodoPage> {
  List<TodoModel> todos = [];
  int id = 0;
  void addTodo(text) {
    TodoModel item = TodoModel(
      id: id++,
      title: text,
      complete: false,
    );

    setState(() {
      todos.add(item);
    });
  }
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('todo list')),
      body: Column(children: [
      TextField(onSubmitted: addTodo),
      Expanded(
          child: ListView.builder(
              itemBuilder: (context, index) {
                TodoModel item = todos[index];
                return ListTile(
                  title: Text(item.title),
                );
              },
              itemCount: todos.length)),
    ])
    );
  }
}
```

删除与更新todo

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: TodoPage(),
    );
  }
}

class TodoModel {
  TodoModel({
    this.id,
    this.title,
    this.complete = false,
  });

  int id;
  String title;
  bool complete;
}

class TodoPage extends StatefulWidget {
  TodoPage({Key key}) : super(key: key);

  @override
  _TodoPageState createState() => _TodoPageState();
}

class _TodoPageState extends State<TodoPage> {
  List<TodoModel> todos = [];
  int id = 0;

  void addTodo(text) {
    TodoModel item = TodoModel(
      id: id++,
      title: text,
      complete: false,
    );

    setState(() {
      todos.add(item);
    });
  }

  void deleteTodo(index) {
    List newTodos = todos;
    newTodos.remove(newTodos[index]);
    setState(() {
      todos = newTodos;
    });
  }

  void updateTodo(index) {
    List<TodoModel> newTodo = todos;
    TodoModel item = newTodo[index];
    item.complete = !item.complete;
    setState(() {
      todos = newTodo;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
        appBar: AppBar(title: Text('todo list')),
        body: Column(children: [
          TextField(onSubmitted: addTodo),
          Expanded(
              child: ListView.builder(
                  itemBuilder: (context, index) {
                    TodoModel item = todos[index];
                    return ListTile(
                      onTap: () {
                        updateTodo(index);
                      },
                      title: Text(
                        item.title,
                        style: TextStyle(
                            decoration: item.complete
                                ? TextDecoration.lineThrough
                                : TextDecoration.none),
                      ),
                      trailing: InkWell(
                          onTap: () {
                            deleteTodo(index);
                          },
                          child: Icon(Icons.close)),
                    );
                  },
                  itemCount: todos.length)),
        ]));
  }
}
```