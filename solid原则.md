---
title: "Solid原则"
date: 2020-11-23T09:12:28+08:00
---

## solid原则

面向对象编程为软件开发带来了新的模式。这使开发人员能够将具有相同用途或功能的数据组合在一个类中，来处理单一的问题，而不用管整个应用程序。但是，这种面向对象的编程还是会让开发者写出混淆或不好维护的程序。

因此，罗伯特·C·马丁（Robert C. Martin）制定了五项准则。这五个准则/原则可以让开发人员轻松的写出可读性和可维护性高的程序。

这五个原则被称为 S.O.L.I.D 原则(首字母缩写是迈克尔·费瑟[Michael Feathers]提出的)。

S：单一职责原则

O：开闭原则原则

L：里氏替换原则

I：接口隔离原则

D：依赖反转原则

我们将在下面详细讨论它们。

注意：本文中的大多数示例，可能不足以说明实际情况或不适用于实际需求。这完全取决于您自己的设计和用例。最重要的是理解并知道如何应用/遵循这些原则。

Tip：SOLID 原则是为构建模块化、封装、可扩展和可组合的组件而设计的。Bit 是将这一原则付诸实践的有力工具：它帮助您轻松地隔离、共享和管理团队中不同项目中的此类组件。试试看！

单一职责原则
“……你只有一份工作” ——《雷神3:诸神黄昏》中洛基和斯克雷斯的合作 

一个类应该只负责一件事。如果一个类有多个职责，它就会耦合起来。对一个职责的更改会导致对另一个职责的更改。

提示: 这一原则不仅适用于类，而且也适用于软件组件和微服务架构。

例如，思考这样的设计：
```javascript
class Animal {  
    constructor(name: string){ }  
    getAnimalName() { }  
    saveAnimal(a: Animal) { }  
}
```
Animal类违反了单一职责原则（SRP）。

它怎么违反SRP的?

SRP 声明类应该只有一个职责，在这个类里，我们可以看到有两个职责：Animal 数据管理和动物属性管理。这个类的构造函数和 getAnimalName 方法管理 Animal 的属性，而saveAnimal 方法管理 Animal 的数据存储。

这个设计在以后会引起什么样的问题?

如果应用的更改影响到了数据库的操作，那么使用 Animal 属性的类必须做相应的修改。

这个系统缺乏弹性，就像多米诺骨牌效应，触摸一张牌就会影响到其他所有的牌。

为了符合 SRP，我们创建了另一个类，它的职责是存储 animal 到数据库:
```javascript
class Animal {  
    constructor(name: string){ }  
    getAnimalName() { }  
}

class AnimalDB {  
    getAnimal(a: Animal) { }  
    saveAnimal(a: Animal) { }  
}
```
当我们在设计类时，应该把特性相关的放在一起。同时，我们应该把特性不同的分开。 —— 史蒂夫芬顿

遵循这些原则会让我们的应用程序变得高内聚。

开闭原则
软件实体(类、模块、函数)应该是可以扩展的，而不是修改。

让我们继续我们的 Animal 类。
```javascript
class Animal {  
    constructor(name: string){ }  
    getAnimalName() { }  
}
我们想要遍历动物列表并设置它们的声音。

const animals: Array<Animal> = [  
    new Animal('lion'),  
    new Animal('mouse')  
];

function AnimalSound(a: Array<Animal>) {  
    for(int i = 0; i <= a.length; i++) {  
        if(a[i].name == 'lion')  
            log('roar');  
        if(a[i].name == 'mouse')  
            log('squeak');  
    }  
}  
AnimalSound(animals);
```
AnimalSound方法不符合开闭原则，因为有新动物出现时，需要修改 AnimalSound 方法。

如果我们添加一个新的动物，蛇：

```javascript
const animals: Array<Animal> = [  
    new Animal('lion'),  
    new Animal('mouse'),  
    new Animal('snake')  
]  
```
我们必须修改 AnimalSound 方法：

```javascript
function AnimalSound(a: Array<Animal>) {  
    for(int i = 0; i <= a.length; i++) {  
        if(a[i].name == 'lion')  
            log('roar');  
        if(a[i].name == 'mouse')  
            log('squeak');  
        if(a[i].name == 'snake')  
            log('hiss');  
    }  
}

AnimalSound(animals);
```
对于每个新的 animal，就需要在 AnimalSound 方法中添加新的逻辑。这是一个相当简单的例子。应用复杂时，每当你新增一个动物时，AnimalSound 方法中if语句就会不停的重复。

我们如何使它（AnimalSound）符合 OCP?
```javascript
class Animal {  
        makeSound();  
        //...  
}

class Lion extends Animal {  
    makeSound() {  
        return 'roar';  
    }  
}

class Squirrel extends Animal {  
    makeSound() {  
        return 'squeak';  
    }  
}  
class Snake extends Animal {  
    makeSound() {  
        return 'hiss';  
    }  
}

function AnimalSound(a: Array<Animal>) {  
    for(int i = 0; i <= a.length; i++) {  
        log(a[i].makeSound());  
    }  
}  
AnimalSound(animals);
```
现在，Animal有一个 makeSound 方法。我们让每个 animal 都继承 Animal 类，并且实现 makeSound 方法。

让每个 animal 实现自己的 makeSound 方法。AnimalSound 方法遍历 animal 数组，然后调用下每个 animal 的 makeSound 方法即可。

现在，如果我们增加一个新的 animal，不需要去更改 AnimalSound 方法，只需要将新的 animal 添加到 animal 数组中。

AnimalSound 现在符合了 OCP 原则。

另外一个例子：

假设你有一家商店，给你喜欢的顾客打 20% 的折扣：
```javascript
class Discount {  
    giveDiscount() {  
        return this.price * 0.2  
    }  
}
当你决定为 VIP 顾客提供 40% 的折扣时。你可以这样修改类：

class Discount {  
    giveDiscount() {  
        if(this.customer == 'fav') {  
            return this.price * 0.2;  
        }  
        if(this.customer == 'vip') {  
            return this.price * 0.4;  
        }  
    }  
}
```
这违背了 OCP 原则。OCP 不允许这样做。如果我们想给不同类型的顾客不同的折扣，giveDiscount 方法中将会增加新的逻辑。

为了让其遵循 OCP 原则，我们将添加一个新的类，让它继承 Discount 类。让我们来一起实现它：

class VIPDiscount: Discount {  
    getDiscount() {  
        return super.getDiscount() * 2;  
    }  
}
如果你想给超级 VIP 客户 80% 的折扣，应该是这样的:

class SuperVIPDiscount: VIPDiscount {  
    getDiscount() {  
        return super.getDiscount() * 2;  
    }  
}
这就是拓展而不是修改。

里氏替换原则
子类必须可以替换它的超类

这个原则的目的是确保子类可以代替它的超类，且不产生错误。如果代码发现自己在检查类的类型，那么它一定违反了这个原则。

让我们以动物为例。

...  
function AnimalLegCount(a: Array<Animal>) {  
    for(int i = 0; i <= a.length; i++) {  
        if(typeof a[i] == Lion)  
            log(LionLegCount(a[i]));  
        if(typeof a[i] == Mouse)  
            log(MouseLegCount(a[i]));  
        if(typeof a[i] == Snake)  
            log(SnakeLegCount(a[i]));  
    }  
}  
AnimalLegCount(animals);
这违反了 LSP 原则(以及 OCP 原则)。它必须知道每种动物类型，并调用相关的 leg-counting 方法。

对于每一个新创建的 animal，必须修改函数以接受新 animal。

...  
class Pigeon extends Animal {  
          
}

const animals[]: Array<Animal> = [  
    //...,  
    new Pigeon();  
]

function AnimalLegCount(a: Array<Animal>) {  
    for(int i = 0; i <= a.length; i++) {  
        if(typeof a[i] == Lion)  
            log(LionLegCount(a[i]));  
        if(typeof a[i] == Mouse)  
            log(MouseLegCount(a[i]));  
         if(typeof a[i] == Snake)  
            log(SnakeLegCount(a[i]));  
        if(typeof a[i] == Pigeon)  
            log(PigeonLegCount(a[i]));  
    }  
}  
AnimalLegCount(animals);
为了使该函数遵循 LSP 原则，我们将遵循 Steve Fenton 假设的 LSP 要求:

如果超类（Animal）具有接受超类类型（Animal）参数的方法。它的子类（Pigeon）应该接受超类类型（Animal类型）或子类类型（Pigeon类型）作为参数。

如果超类返回一个超类的类型（Animal）。它的子类应该返回一个超类类型（Animal类型）或子类类型（Pigeon）；

现在，我们可以重新实现下 AnimalLegCount 方法：

function AnimalLegCount(a: Array<Animal>) {  
    for(let i = 0; i <= a.length; i++) {  
        a[i].LegCount();  
    }  
}  
AnimalLegCount(animals);
AnimalLegCount 方法并不关心参数 Animal 的类型，它只是去调用了下 LegCount 方法。它只要求参数必须为 Animal 类型，要么是 Animal 类，要么是它的子类。

现在，Animal 类必须实现或者定义一个 LegCount 方法：

class Animal {  
    //...  
    LegCount();  
}
而它的子类必须实现 LegCount 方法：

...  
class Lion extends Animal{  
    //...  
    LegCount() {  
        //...  
    }  
}  
...
当在 AnimalLegCount 方法中调用时，会返回 lion 的腿数。

AnimalLegCount 方法在不需要知道 Animal 类型的情况下，只调用每个 Animal 的 LegCount 方法，来获得 Animal 的腿数，因为根据约定，Animal 的子类实现了LegCount 方法。

接口隔离原则
创建特定于客户端的细粒度接口

不强制客户端依赖他们不使用的接口。

这个原则避免了大接口的缺陷。

让我们一起来看 IShape 接口：

interface IShape {  
    drawCircle();  
    drawSquare();  
    drawRectangle();  
}
这个接口绘制正方形、圆形和矩形。实现IShape接口的类必须定义 drawCircle, drawSquare, drawRectangle 方法。

class Circle implements IShape {  
    drawCircle(){  
        //...  
    }

    drawSquare(){  
        //...  
    }

    drawRectangle(){  
        //...  
    }      
}

class Square implements IShape {  
    drawCircle(){  
        //...  
    }

    drawSquare(){  
        //...  
    }

    drawRectangle(){  
        //...  
    }      
}

class Rectangle implements IShape {  
    drawCircle(){  
        //...  
    }

    drawSquare(){  
        //...  
    }

    drawRectangle(){  
        //...  
    }      
}
上面的代码非常有趣。Rectangle 类实现了它不使用的方法（drawCircle和drawSquare） ，同样，Square 类实现了 drawCircle 和 drawRectangle 方法，Square 类实现了 drawSquare 和 drawRectangle 方法。

如果我们需要在 IShape 接口中添加另外一个方法 drawTriangle，

interface IShape {  
    drawCircle();  
    drawSquare();  
    drawRectangle();  
    drawTriangle();  
}
类必须实现新的方法，否则会抛出错误。

我们可以看到，不可能实现一个形状类，它可以画圆，但是不能画矩形，正方形或三角形。我们可以实现一些方法来抛出一个错误，表明操作无法执行。

ISP 反对这种 IShape 接口的设计。客户端（这里指 Rectangle、Circle 和 Square 类）不应该被强制依赖他们不需要或不用的方法。并且，ISP 声明接口应该只负责一个任务（就像 SRP 原则），任何额外的行为都应该抽象到另一个接口上。

这里，我们的 IShape 接口中的多个操作，应该由别的接口独立负责。

为了让我们的 IShape 接口遵循 ISP 原则，我们将不同的操作隔离到不同的接口上：

interface IShape {  
    draw();  
}

interface ICircle {  
    drawCircle();  
}

interface ISquare {  
    drawSquare();  
}

interface IRectangle {  
    drawRectangle();  
}

interface ITriangle {  
    drawTriangle();  
}

class Circle implements ICircle {  
    drawCircle() {  
        ...  
    }  
}

class Square implements ISquare {  
    drawSquare() {  
        ...  
    }  
}

class Rectangle implements IRectangle {  
    drawRectangle() {  
        ...  
    }      
}

class Triangle implements ITriangle {  
    drawTriangle() {  
        ...  
    }  
}  
class CustomShape implements IShape {  
   draw(){  
      ...  
   }  
}
ICircle 接口只绘制圆，IShape 接口绘制任意形状，ISquare 接口只绘制正方形，IRectangle 接口只绘制矩形。

或者类（Circle、Rectangle、Square、Triangle 等）只继承 IShape 接口，并且实现它们自己的绘画行为。

class Circle implements IShape {  
    draw(){  
        ...  
    }  
}  
  
class Triangle implements IShape {  
    draw(){  
        ...  
    }  
}  
  
class Square implements IShape {  
    draw(){  
        ...  
    }  
}  
  
class Rectangle implements IShape {  
    draw(){  
        ...  
    }  
}                                            
然后，我们可以使用I-接口创建特定的形状，如半圆、直角三角形、等边三角形、钝边矩形等。

依赖倒置原则
依赖关系应该是抽象的，而不是具体的。

A. 高级模块不应该依赖于低级模块。两者都应该依赖于抽象。

B. 抽象不应该依赖于细节。细节应该依赖于抽象。

在软件开发中，我们的应用程序最终主要由模块组成。当这种情况出现时，我们必须使用依赖注入来解决。高级组件依赖于低级组件发挥作用。

class XMLHttpService extends XMLHttpRequestService {}

class Http {  
    constructor(private xmlhttpService: XMLHttpService) { }  
    get(url: string , options: any) {  
        this.xmlhttpService.request(url,'GET');  
    }

    post() {  
        this.xmlhttpService.request(url,'POST');  
    }  
    ...  
}
这里，Http 是高级组件，而 HttpService 是低级组件。这种设计违反了 DIP：高级模块不应该依赖于低级模块。它应该依赖它的抽象。

这个 Http 类被强制依赖 XMLHttpService 类。如果我们要更改 Http 连接服务，可能需要通过 Nodejs 连接到互联网，或者模拟 http 服务。我们将必须修改每个 Http 实例，这违背了 OCP 原则。

Http 类应该更少的去关心你用的 Http 服务类型。让我们来实现一个 Connection 接口：

interface Connection {  
    request(url: string, opts:any);  
}
Connection 这个接口有一个 request 方法。有了这个接口，我们可以给我们的 Http 类传入一个 Connection 类型的参数：

class Http {  
    constructor(private httpConnection: Connection) { }

    get(url: string , options: any) {  
        this.httpConnection.request(url,'GET');  
    }

    post() {  
        this.httpConnection.request(url,'POST');  
    }  
    ...  
}
现在，不管给 Http 类传入什么类型的 Http 服务连接参数，在不用关心网络连接类型的情况下，连接到网络也是很容易的。

我们可以重新实现下我们的 XMLHttpService 类，来实现 Connection 接口：

class XMLHttpService implements Connection {  
    const xhr = new XMLHttpRequest();  
    ...  
    request(url: string, opts:any) {  
        xhr.open();  
        xhr.send();  
    }  
}
我们可以创建许多 Http Connection 类型，并将其传递给我们的 Http 类，而无需担心错误。

class NodeHttpService implements Connection {  
    request(url: string, opts:any) {  
        ...  
    }  
}

class MockHttpService implements Connection {  
    request(url: string, opts:any) {  
        ...  
    }      
}
现在，我们可以看到高级模块和低级模块都依赖于抽象。Http 类（高级模块）依赖于 Connection 接口（抽象），而 Http 服务类型（低级模块）也依赖 Connection 接口（抽象）。

此外，DIP 原则会强制我们遵循里氏替换原则：Connection 类型 Node-XML-MockHttpService 可以替换它们的父类型连接。

总结
我们在这里介绍了每个软件开发人员都必须遵守的五个原则。遵循所有这些原则一开始可能会令人畏惧，但是随着不断的实践和坚持，它将成为我们的一部分，并对应用程序的维护产生巨大的影响