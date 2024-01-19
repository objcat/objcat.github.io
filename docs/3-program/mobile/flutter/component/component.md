# 🍎 快速开始

## 🌲 创建组件

我们首先在`lib`下面新建一个`components`文件夹, 然后在里面创建一个`.dart`文件, 叫`test_component`

```dart
import 'package:flutter/material.dart';

class TestComponent extends StatelessWidget {
  const TestComponent({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Container(
      color: Colors.cyan,
      child: const Text("hello world!"),
    );
  }
}
```

然后我们在主页面去使用就可以了

```dart
@override
Widget build(BuildContext context) {
return Scaffold(
	appBar: AppBar(
	  backgroundColor: Theme.of(context).colorScheme.inversePrimary,
	  title: Text(widget.title),
	),
	body: const TestComponent());
}
```

我们来看看效果吧, 这就是一个最简单的效果

![](images/Pasted%20image%2020231112111835.png)

## 🌲 组件加参数

加参数的方法就是通过我们的初始化方法指定参数

```dart
import 'package:flutter/material.dart';

class TestComponent extends StatelessWidget {
  final String title;
  final int number;

  const TestComponent({super.key, required this.title, required this.number});

  @override
  Widget build(BuildContext context) {
    return Container(
      color: Colors.cyan,
      child: Text("标题: $title 编号: $number"),
    );
  }
}
```

因为我们使用的是`StatelessWidget`不可变的组件, 所以`title`的改变只有初始化的时候, 我们使用`final`进行修饰, 并且初始化方法也要是`const`的

```
This class (or a class that this class inherits from) is marked as '@immutable', but one or more of its instance fields aren't final: TestComponent.title (Documentation)
```

## 🌲 使用组件

```dart
@override
Widget build(BuildContext context) {
return Scaffold(
	appBar: AppBar(
	  backgroundColor: Theme.of(context).colorScheme.inversePrimary,
	  title: Text(widget.title),
	),
	body: const TestComponent(
	  title: "你好我是标题",
	  number: 89757,
	));
}
```

我们来看一下效果

![](images/Pasted%20image%2020231112113806.png)

## 🌲 改变标题和编号

如果我们想改变组件的标题和编号要怎么做呢

```dart
class _MyHomePageState extends State<MyHomePage> {
  String title = "你好我是标题";
  int number = 89757;

  void _click() {
    setState(() {
      title = "开始增加编号";
      number += 1;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
        appBar: AppBar(
          backgroundColor: Theme.of(context).colorScheme.inversePrimary,
          title: Text(widget.title),
        ),
        body: TestComponent(
          title: title,
          number: number,
        ),
        floatingActionButton: FloatingActionButton(
          onPressed: _click,
          tooltip: 'test click',
          child: const Icon(Icons.add),
        ));
  }
}
```

很简单, 我们在`_MyHomePageState`中添加两个属性, 然后在`floatingActionButton`的点击方法中改变这两个属性, 就可以改变变量了, 我们会发现虽然`TestComponent`继承了`StatelessWidget`但是并没有影响外部改变它的值, 只是内部不能去改变而已

## 🌲 无状态拓展

如果你还是不懂无状态, 那我们来看另外一个案例, 我们在`TestComponent`中创建一个`title2`然后给他一个默认值, 然后发现`const TestComponent`报错了

```dart
import 'package:flutter/material.dart';

class TestComponent extends StatelessWidget {
  final String title;
  final int number;
  String title2 = "123";

  const TestComponent({super.key, required this.title, required this.number});

  void _click() {
    title2 = "456";
  }

  @override
  Widget build(BuildContext context) {
    return Column(children: [
      Text(
        "标题: $title 编号: $number 标题2: $title2",
        style: const TextStyle(backgroundColor: Colors.cyan),
      ),
      ElevatedButton(onPressed: _click, child: const Text("ElevatedButton"))
    ]);
  }
}
```

错误如下, 它说一个`const TestComponent`比如是`所有`字段都是`final`, 突出所有, 所以我们想增加额外变量只能把`const`去掉

```
Can't define a const constructor for a class with non-final fields. (Documentation)  Try making all of the fields final, or removing the keyword 'const' from the constructor.
```

然后我们增加一个`ElevatedButton`, 通过点击按钮来改变`title2`的值

![](images/Pasted%20image%2020231112115824.png)

我们发现是不能进行改变的, 因为根本没`setState`方法, 而且我们也知道只有`StatefulWidget`方法才能使用`setState`来刷新数据

## 🌲 无状态改有状态

如果我们确实需要在组件内部来控制组件中的值, 那我们需要把它做成有状态的

```dart
import 'package:flutter/material.dart';

class TestComponent extends StatefulWidget {
  final String title;
  final int number;

  const TestComponent({super.key, required this.title, required this.number});

  @override
  State<StatefulWidget> createState() {
    return _TestComponentState();
  }
}

class _TestComponentState extends State<TestComponent> {
  String title2 = "123";

  void _click() {
    setState(() {
      title2 = "456";
    });
  }

  @override
  Widget build(BuildContext context) {
    return Column(children: [
      Text(
        "标题: ${widget.title} 编号: ${widget.number} 标题2: $title2",
        style: const TextStyle(backgroundColor: Colors.cyan),
      ),
      ElevatedButton(onPressed: _click, child: const Text("ElevatedButton"))
    ]);
  }
}
```

这样我们发现点击按钮就能够改变变量了

## 🌲 改变所有变量(反向例子)

我们在上面的例子中已经可以改变`_TestComponentState`中的变量了, 那如果想改变`widget`中的变量要怎么做呢? 其实很简单, 把`final`去掉就可以了, 但是不建议这么做, 我们先看怎么实现

```dart
import 'package:flutter/material.dart';

class TestComponent extends StatefulWidget {
  String title;
  int number;

  TestComponent({super.key, required this.title, required this.number});

  @override
  State<StatefulWidget> createState() {
    return _TestComponentState();
  }
}

class _TestComponentState extends State<TestComponent> {
  String title2 = "123";

  void _click() {
    setState(() {
      widget.title = "123";
      widget.number = 77777;
      title2 = "456";
    });
  }

  @override
  Widget build(BuildContext context) {
    return Column(children: [
      Text(
        "标题: ${widget.title} 编号: ${widget.number} 标题2: $title2",
        style: const TextStyle(backgroundColor: Colors.cyan),
      ),
      ElevatedButton(onPressed: _click, child: const Text("ElevatedButton"))
    ]);
  }
}
```

然后会提示

```
This class (or a class that this class inherits from) is marked as '@immutable', but one or more of its instance fields aren't final: TestComponent.title, TestComponent.number (Documentation)
```

然后我们点击按钮就可以在内部改变`title`和`number`之类的, 但是这样会有一个什么问题呢? 就是内部改变`widget.title`不会改变外面的值, 当外面数据刷新了, 那么内部这两个数据都会被覆盖, 在`vue`中也存在这样的情况, 所以`vue`是严令禁止在内部修改外部传入的变量的, `vue`的解决方式是`emit`到外面改变变量

