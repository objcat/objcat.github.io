# 🍎 简介

路由

# 🍎 快速开始

## 🌲 路由跳转

我们的路由是通过`Navigator`类来实现跳转的, 首先新建一个页面, 当然为了方便我写成了无状态的

```dart
import 'package:flutter/material.dart';

class SecondPage extends StatelessWidget {
  const SecondPage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
        appBar: AppBar(
          backgroundColor: Theme.of(context).colorScheme.inversePrimary,
          title: const Text("second"),
        ),
        body: const Center(child: Text("我是second!")));
  }
}
```

我们想要在主页中跳转到该页面, 只要在主页面中的点击方法中调用`Navigator.of(context).push`方法就可以了

```dart
void _click() {
	Navigator.push(context, MaterialPageRoute(builder: (BuildContext context) {
	      return const SecondPage();
	}));
}

// 也可以使用of
Navigator.of(context).push(MaterialPageRoute(builder: (BuildContext build) {
	  return const SecondPage();
}));
```

或者你也可以使用箭头函数来写

```dart
Navigator.push(context, MaterialPageRoute(builder: (BuildContext context) => const SecondPage()));
```

## 🌲 路由传值

路由传值也非常容易, 我们只需要在页面的构造函数中增加一个参数即可, 我们注意到了`title`我使用了`final`, 如果不使用`final`会有一个警告, `This class (or a class that this class inherits from) is marked as '@immutable', but one or more of its instance fields aren't final: ThirdPage.title (Documentation)`, 意思是静态构造的页面中出现了可变的字段, 这样不安全

```dart
import 'package:flutter/material.dart';

class SecondPage extends StatelessWidget {
  final String title;
  const SecondPage({super.key, required this.title});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
        appBar: AppBar(
          backgroundColor: Theme.of(context).colorScheme.inversePrimary,
          title: Text(title),
        ),
        body: const Center(child: Text("我是second!")));
  }
}
```

然后我们直接使用路由在页面的初始化方法中进行传值

```dart
Navigator.push(context, MaterialPageRoute(builder: (BuildContext context) {
  return const SecondPage(title: "我是标题",);
}));
```

如果页面是有`状态的`也很好做, 我们来做跳转到一个有状态的页面

```dart
import 'package:flutter/material.dart';

class ThirdPage extends StatefulWidget {
  final String title;
  const ThirdPage({super.key, required this.title});

  @override
  State<StatefulWidget> createState() {
    // TODO: implement createState
    return _ThirdPageState();
  }
}

class _ThirdPageState extends State<ThirdPage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
        appBar: AppBar(
          backgroundColor: Theme.of(context).colorScheme.inversePrimary,
          title: Text(widget.title),
        ),
        body: const Center(child: Text("我是third!")));
  }
}
```

## 🌲 命名路由

命名路由就是配置好路由表, 然后使用名称直接去跳转, 我们一起来看看吧, 命名路由要在myapp中配置

```dart
class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
        useMaterial3: true,
      ),
      // home: const MyHomePage(title: 'demo'),
      routes: {
        "/": (context) => const MyHomePage(title: "demo"),
        "/second": (context) => const SecondPage(),
        "/third": (context) => const ThirdPage(),
      },
    );
  }
}
```

配置完路由表之后第一个路由就是主页

然后跳转使用名字就可以了

```dart
void _click() {
  Navigator.pushNamed(context, "/second");
}
```

## 🌲 初始页面

我们可以设置一个初始页面, 比如我应用进入就是`second`页面

```dart
@override
Widget build(BuildContext context) {
return MaterialApp(
  title: 'Flutter Demo',
  theme: ThemeData(
	colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
	useMaterial3: true,
  ),
  // home: const MyHomePage(title: 'demo'),
  routes: {
	"/": (context) => const MyHomePage(title: "demo"),
	"/second": (context) => const SecondPage(),
	"/third": (context) => const ThirdPage(),
  },
  initialRoute: "/second",
);
}
```


