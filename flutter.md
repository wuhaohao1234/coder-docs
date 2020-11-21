---
title: "Flutter"
date: 2020-09-10T22:36:07+08:00
---

## flutter

> 转自技术胖

### hello world

```
import 'package:flutter/material.dart';
//主函数（入口函数），下面我会简单说说Dart的函数
void main() =>runApp(MyApp());
// 声明MyApp类
class MyApp extends StatelessWidget{
  //重写build方法
  @override
  Widget build(BuildContext context){
    //返回一个Material风格的组件
   return MaterialApp(
      title:'Welcome to Flutteraa',
      home:Scaffold(
        //创建一个Bar，并添加文本
        appBar:AppBar(
          title:Text('学习flutter'),
        ),
        //在主体的中间区域，添加一个hello world 的文本
        body:Center(
          child:Text('Hello World'),
        ),
      ),
    );
  }
}
```

### 常用组件

#### Text组件

最基础的hello world

```
import 'package:flutter/material.dart';
void main () => runApp(MyApp());

class MyApp extends StatelessWidget{
  @override
  Widget build(BuildContext context ){
      return MaterialApp(
        title:'Text widget',
        home:Scaffold(
          body:Center(
            child:Text('Hello JSPang')
          ),
        ),
      );
  }
}
```

TextAlign属性就是文本的对齐方式，它的属性值有如下几个

center: 文本以居中形式对齐,这个也算比较常用的了。
left:左对齐，经常使用，让文本居左进行对齐，效果和start一样。
right :右对齐，使用频率也不算高。
start:以开始位置进行对齐，类似于左对齐。
end: 以为本结尾处进行对齐，不常用。有点类似右对齐.

```
child:Text(
  'Hello JSPang  ,非常喜欢前端，并且愿意为此奋斗一生。我希望可以出1000集免费教程。',
  textAlign:TextAlign.left,
)
```

设置最多显示的行数，比如我们现在只显示1行，类似一个新闻列表的题目。代码如下：
```
child:Text(
  'Hello JSPang  ,非常喜欢前端，并且愿意为此奋斗一生。我希望可以出1000集免费教程。',
  textAlign:TextAlign.left,
  maxLines: 1,
)
```

overflow属性是用来设置文本溢出时，如何处理,它有下面几个常用的值供我们选择。

clip：直接切断，剩下的文字就没有了，感觉不太友好，体验性不好。
ellipsis:在后边显示省略号，体验性较好，这个在工作中经常使用。
fade: 溢出的部分会进行一个渐变消失的效果，当然是上线的渐变，不是左右的哦。

style属性的内容比较多，具体的你可以查一下API，我这里带作一个效果，方便大家快速学会Style的用法。

我们下面要作的效果为，字体大小为25.0,颜色为粉红色,并且有一个下划线。

```
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Text widget',
      home: Scaffold(
        body: Center(
            child: Text(
          'Hello JSPang  ,非常喜欢前端，并且愿意为此奋斗一生。我希望可以出1000集免费教程。',
          textAlign: TextAlign.left,
          overflow: TextOverflow.ellipsis,
          maxLines: 1,
          style: TextStyle(
            fontSize: 25.0,
            color: Color.fromARGB(255, 255, 150, 150),
            decoration: TextDecoration.underline,
            decorationStyle: TextDecorationStyle.solid,
          ),
        )),
      ),
    );
  }
}

```
#### Container组件

Container（容器控件）在Flutter是经常使用的控件，它就相当于我们HTML里的<div>标签，每个页面或者说每个视图都离不开它。那这节课我们就来学习一下。

其实容器的作用就是方便我们进行布局的，Flutter这点也作的很好，我们先来看容器属性中的Alignment。

这个属性针对的是Container内child的对齐方式，也就是容器子内容的对齐方式，并不是容器本身的对齐方式。

先作一个效果：建立一个容器，然后容器内加入一段文字Hello JSPang, 并让它居中对齐。

```
import 'package:flutter/material.dart';
void main () => runApp(MyApp());

class MyApp extends StatelessWidget{
  @override
  Widget build(BuildContext context ){
      return MaterialApp(
        title:'Text widget',
        home:Scaffold(
          body:Center(
           child:Container(
             child:new Text('Hello JSPang',style: TextStyle(fontSize: 40.0),),
             alignment: Alignment.center,
           ),
          ),
        ),
      );
  }
}
```
这时候可以看见，我们的文本已经居中显示在手机屏幕上了。当然它的对齐方式还有如下几种：

bottomCenter:下部居中对齐。
botomLeft: 下部左对齐。
bottomRight：下部右对齐。
center：纵横双向居中对齐。
centerLeft：纵向居中横向居左对齐。
centerRight：纵向居中横向居右对齐。
topLeft：顶部左侧对齐。
topCenter：顶部居中对齐。
topRight： 顶部居左对齐。

设置宽、高和颜色属性是相对容易的，只要在属性名称后面加入浮点型数字就可以了，比如要设置宽是500，高是400，颜色为亮蓝色。代码如下：

```
child:Container(
  child:new Text('Hello JSPang',style: TextStyle(fontSize: 40.0),),
  alignment: Alignment.center,
  width:500.0,
  height:400.0,
  color: Colors.lightBlue,
),
```

上节已经简单的学习了一下Container容器组件的用法，这节我们继续学习，主要讲解一下的padding,margin和decoration这几个属性。我们先来看看Padding属性。

padding的属性就是一个内边距，它和你使用的前端技术CSS里的padding表现形式一样，指的是Container边缘和child内容的距离。先来看一个内边距为10的例子。具体代码如下(我们还是接着上节课的代码来写)：

```
child:Container(
  child:new Text('Hello JSPang',style: TextStyle(fontSize: 40.0),),
  alignment: Alignment.topLeft,
  width:500.0,
  height:400.0,
  color: Colors.lightBlue,
  padding:const EdgeInsets.all(10.0),
),
```
上面主要的padding代码就一句。

`padding:const EdgeInsets.all(10.0),`

这句的意思是设置Container的内边距是10，左右上下全部为10，这看起来非常容易。那我们再加大一点难度。如果上边距为30，左边距为10，这时候EdgeInsets.all()就满足不了我们了。

** EdgeInsets.fromLTRB(value1,value2,value3,value4)**

```
padding:const EdgeInsets.fromLTRB(10.0,30.0,0.0,0.0),
```

会了padding属性的设置，margin就变的非常容易了，因为方法基本上一样。不过margin是外边距，只的是container和外部元素的距离。

现在要把container的外边距设置为10个单位，代码如下:
```
child:Container(
  child:new Text('Hello JSPang',style: TextStyle(fontSize: 40.0),),
  alignment: Alignment.topLeft,
  width:500.0,
  height:400.0,
  color: Colors.lightBlue,
  padding:const EdgeInsets.fromLTRB(10.0,30.0,0.0,0.0),
  margin: const EdgeInsets.all(10.0),
),
```
我们用EdgeInsets.fromLTRB(value1,value2,value3,value4) 可以满足我们的需求，LTRB分别代表左、上、右、下。

那我们设置上边距为30，左边距为10，就可以用下面的代码来编写。

decoration是 container 的修饰器，主要的功能是设置背景和边框。

比如你需要给背景加入一个渐变，这时候需要使用BoxDecoration这个类，代码如下（需要注意的是如果你设置了decoration，就不要再设置color属性了，因为这样会冲突）。

```
child:Container(
  child:new Text('Hello JSPang',style: TextStyle(fontSize: 40.0),),
  alignment: Alignment.topLeft,
  width:500.0,
  height:400.0,
  padding:const EdgeInsets.fromLTRB(10.0,30.0,0.0,0.0),
  margin: const EdgeInsets.all(10.0),
  decoration:new BoxDecoration(
    gradient:const LinearGradient(
      colors:[Colors.lightBlue,Colors.greenAccent,Colors.purple]
    )
  ),
),
```

设置边框可以在decoration里设置border属性，比如你现在要设置一个红色边框，宽度为2。代码如下：

```
child:Container(
  child:new Text('Hello JSPang',style: TextStyle(fontSize: 40.0),),
  alignment: Alignment.topLeft,
  width:500.0,
  height:400.0,
  padding:const EdgeInsets.fromLTRB(10.0,30.0,0.0,0.0),
  margin: const EdgeInsets.all(10.0),
  decoration:new BoxDecoration(
    gradient:const LinearGradient(
      colors:[Colors.lightBlue,Colors.greenAccent,Colors.purple]
    ),
    border:Border.all(width:2.0,color:Colors.red)
  ),
),
```

#### image图片

加入图片的几种方式
Image.asset:加载资源图片，就是加载项目资源目录中的图片,加入图片后会增大打包的包体体积，用的是相对路径。
Image.network:网络资源图片，意思就是你需要加入一段http://xxxx.xxx的这样的网络路径地址。
Image.file:加载本地图片，就是加载本地文件中的图片，这个是一个绝对路径，跟包体无关。
Image.memory: 加载Uint8List资源图片,这个我目前用的不是很多，所以没什么发言权。
我们现在就以加入网络图片为例子，在Container里加入一张图片，代码如下：

```
import 'package:flutter/material.dart';
void main () => runApp(MyApp());

class MyApp extends StatelessWidget{
  @override
  Widget build(BuildContext context ){
      return MaterialApp(
        title:'Text widget',
        home:Scaffold(
          body:Center(
          child:Container(
            child:new Image.network(
              'http://newimg.jspang.com/Redux_index.png',
              scale:1.0,
            ),
            width:300.0,
            height:200.0,
            color: Colors.lightBlue,
          ),
          ),
        ),
      );
  }
}
```

fit属性的设置
fit属性可以控制图片的拉伸和挤压，这些都是根据图片的父级容器来的，我们先来看看这些属性（建议此部分组好看视频理解）

BoxFit.fill:全图显示，图片会被拉伸，并充满父容器。

BoxFit.contain:全图显示，显示原比例，可能会有空隙。

BoxFit.cover：显示可能拉伸，可能裁切，充满（图片要充满整个容器，还不变形）。

BoxFit.fitWidth：宽度充满（横向充满），显示可能拉伸，可能裁切。

BoxFit.fitHeight ：高度充满（竖向充满）,显示可能拉伸，可能裁切。

BoxFit.scaleDown：效果和contain差不多，但是此属性不允许显示超过源图片大小，可小不可大。

这部分我会在视频中充分演示，每一个效果都会作对应的演示，建议收看视频进行理解。

图片的混合模式
图片混合模式（colorBlendMode）和color属性配合使用，能让图片改变颜色，里边的模式非常的多，产生的效果也是非常丰富的。在这里作几个简单的例子，让大家看一下效果，剩下的留给小伙伴自己探索。

```
child:new Image.network(
  'http://jspang.com/static/myimg/blogtouxiang.jpg',
    color: Colors.greenAccent,
    colorBlendMode: BlendMode.darken,
),
```

color：是要混合的颜色，如果你只设置color是没有意义的。
colorBlendMode:是混合模式，相当于我们如何混合。
repeat图片重复
ImageRepeat.repeat : 横向和纵向都进行重复，直到铺满整个画布。

ImageRepeat.repeatX: 横向重复，纵向不重复。

ImageRepeat.repeatY：纵向重复，横向不重复。

```
child:new Image.network(
  'http://jspang.com/static/myimg/blogtouxiang.jpg',
   repeat: ImageRepeat.repeat,
),
```

#### ListView列表组件

列表组件的知识其实是很多的，也是一个经常使用的组件，我们这里先作一个简介，让大家有个直观的感受，先敲开大门，大家就好深入了。这节我们先学习最简单的列表组件如何制作。

学习不仅要学，还要不断的练习，这节我们重新熟悉一下一个Flutter页面的基本写法，因为前面已经学过，所以我相信很多小伙伴已经都会了，但是我么年主要是练习，代码如下：

```
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'JSPang Flutter Demo',
      home: Scaffold(
        appBar: new AppBar(title: new Text('ListView Widget')),
        body: new ListView(children: <Widget>[
          new ListTile(
              leading: new Icon(Icons.access_time),
              title: new Text('access_time')),
          new ListTile(
              leading: new Icon(Icons.access_time),
              title: new Text('access_time')),
        ]),
      ),
    );
  }
}

```
图片列表的使用
上节课学习了Image Widget，在这里我们就在列表中加入图片来试一下。我们插入4幅图片，然后看一下效果，代码如下：

```
body: new ListView(
  children:<Widget>[
   new Image.network(
      'http://jspang.com/static/upload/20181111/G-wj-ZQuocWlYOHM6MT2Hbh5.jpg'
    ),
     new Image.network(
      'http://jspang.com/static/upload/20181109/1bHNoNGpZjyriCNcvqdKo3s6.jpg'
    ),
     new Image.network(
      'http://jspang.com/static/myimg/typescript_banner.jpg'
    ),new Image.network(
      'http://jspang.com/static/myimg/smile-vue.jpg'
    )

  ]
),
```