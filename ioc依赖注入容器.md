---
title: "Ioc依赖注入容器"
date: 2020-07-17T16:02:25+08:00
tags: ["ioc"]
---

# 依赖注入

## IoC 概览

软件中的对象就像齿轮一样，协同工作，但是互相耦合，一个零件不能正常工作，整个系统就崩溃了。这是一个强耦合的系统。

现在，伴随着工业级应用的规模越来越庞大，对象之间的依赖关系也越来越复杂，经常会出现对象之间的多重依赖性关系，因此，架构师和设计师对于系统的分析和设计，将面临更大的挑战。

```
// 常见的依赖
import {A} from './a';
import {B} from './b';

class C {
  a: A
  b: B
  constructor() {
    this.a = new A();
    this.b = new B(this.a);
  }
  getA(): A {
    return this.a
  }
  getB(): B{
    return this.b
  }
}
const c =  new C()

console.log(c.getA())

```

这里的 A 被 B 和 C 所依赖，而且在构造器需要进行实例化操作，这样的依赖关系在测试中会非常麻烦。这个依赖，一般被叫做 "耦合"，而耦合度过高的系统，必然会出现牵一发而动全身的情形。

为了解决对象间耦合度过高的问题，软件专家 Michael Mattson 提出了 IoC 理论，用来实现对象之间的“解耦”。

控制反转（Inversion of Control）是一种是面向对象编程中的一种设计原则，用来减低计算机代码之间的耦合度。

### 使用 injection 解耦

```
// 常见的依赖
import { A } from './a';
import { B } from './b';

import { Container } from 'injection';

const container = new Container();
container.bind('a', A);
container.bind('b', B);

class C {
  a!: A
  b!: B;
  constructor() {

  }
  async setA() {
    this.a = await container.getAsync('a')
    return this.a
  }
  getA(): A {
    return this.a
  }
  getB(): B {
    return this.b
  }
}
const c = new C()

c.setA().then(res => {
  console.log(c.getA())
})
```

这里的 container 就是 IoC 容器，是依赖注入这种设计模式的一种实现，使得 C 和 A, B 没有了强耦合关系，甚至，我们可以把 C 也交给 IoC 容器，所以，IoC 容器成了整个系统的关键核心

```
IoC 容器就像是一个对象池，管理这每个对象实例的信息（Class Definition），所以用户无需关心什么时候创建，当用户希望拿到对象的实例 (Object Instance) 时，可以直接拿到实例，容器会 自动将所有依赖的对象都自动实例化。
```

## 使用装饰器注入

如果每次代码都需要手动绑定，然后通过 get/getAsync 方法拿到对应的对象，那将会非常繁琐，由于 在设计之初 injection 体系就基于 ts，参考了业界的 IoC 实现，完成了属于自己的依赖注入能力，主要是通过 @provide 和 @inject 两个装饰器来完成绑定定义和自动注入属性，大大简化了代码量。

### @provide

有了 @provide() 装饰器，就可以简化绑定，被 IoC 容器自动扫描，并绑定定义到容器上，对应的逻辑是 绑定对象定义。

### @inject

@inject() 的作用是将容器中的定义实例化成一个对象，并且绑定到属性中，这样，在调用的时候就可以访问到该属性。

```
import {Container, provide, inject} from 'injection';
// 提供userModel类
@provide('userModel')
class UserModel {

}

@provide('userService')
class UserService {
  // 注入userModel 
  @inject()
  private userModel!: UserModel;
  
  async getUser(uid: number) {
    console.log(this.userModel) // 打印出对应的UserModel
    // TODO
    return 'Alex';
  }
}


const container = new Container();
container.bind(UserService);
container.bind(UserModel);

async function getData() {
  const userService = await container.getAsync<UserService>('userService'); 
  const data = await userService.getUser(123);
  return data;
}

getData()
```