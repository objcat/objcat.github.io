# 🍎 简介

flutter官方路由, 学起来

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

我们想要在主页中跳转到该页面, 只要在主页面中的点击方法中调用`Navigator.push`方法就可以了

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

路由传值也非常容易, 我们只需要在页面的构造函数中增加一个参数即可, 我们注意到了`title`我使用了`final`, 如果不使用`final`会有一个警告, `This class (or a class that this class inherits from) is marked as '@immutable', but one or more of its instance fields aren't final: SecondPage.title (Documentation)`, 意思是静态构造的页面中出现了可变的字段, 这样不安全

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

## 🌲 返回上一级路由

非常简单的一句代码

```dart
Navigator.pop(context);
```

## 🌲 替换路由

有时候我们希望新打开的路由可以替换掉之前的路由

```dart
Navigator.pushReplacementNamed(context, "/second", arguments: {"title": "第三个页面"});
```

## 🌲 返回根路由

```dart
Navigator.pushNamedAndRemoveUntil(context, "/", (route) => false);
```

# 🍎 命名路由

页面比较多的项目中, 我们为了方便管理都会配置路由表, 路由表中有路由的名字, 我们通过名字就可以实现路由的跳转

## 🌲 定义路由表

命名路由就是配置好路由表, 然后使用名称直接去跳转, 我们一起来看看吧, 命名路由要在`myapp`中配置

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

配置完路由表之后第一个路由就是主页, 我们也可以通过`initialRoute`来指定加载完成首页后自动跳转到某一个页面, 跳转的页面会保留返回箭头可以回到首页

```dart
@override
Widget build(BuildContext context) {
return MaterialApp(
  title: 'Flutter Demo',
  theme: ThemeData(
	useMaterial3: true,
  ),
  initialRoute: "/second",
  routes: {
	"/": (context) => const MyHomePage(title: "首页"),
	"/second": (context) => const SecondPage(title: "第二个页面"),
	"/third": (context) => const ThirdPage(title: "第三个页面"),
  }
);
}
```

## 🌲 路由跳转

使用起来也很简单, 直接把路由名放进去即可

```dart
void _click() {
  Navigator.pushNamed(context, "/second");
}
```

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

## 🌲 路由传值

命名路由的传值稍微复杂一些, 因为我们需要抽出路由表并重写`onGenerateRoute`方法, 首先我们新建`routes.dart`文件

```dart
import 'package:flutter/material.dart';
import 'package:test_flutter/main.dart';
import 'package:test_flutter/pages/second_page.dart';

final Map routes = {
  "/": (context) => const MyHomePage(title: "首页"),
  "/second": (context, {arguments}) => SecondPage(
        arguments: arguments,
      ),
};

Route? onGenerateRoute(RouteSettings settings) {
  final String? name = settings.name;
  final Function? pageContentBuilder = routes[name];
  if (pageContentBuilder != null) {
    if (settings.arguments != null) {
      final Route route = MaterialPageRoute(
          builder: (context) =>
              pageContentBuilder(context, arguments: settings.arguments));
      return route;
    } else {
      final Route route =
          MaterialPageRoute(builder: (context) => pageContentBuilder(context));
      return route;
    }
  }
  return null;
}
```

其中`onGenerateRoute`写法是固定的, 我们传递的参数叫做`arguments`是一个字典

然后我们在`MyApp`中配置路由, 主要就是`onGenerateRoute`

```dart
class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        useMaterial3: true,
      ),
      onGenerateRoute: onGenerateRoute,
    );
  }
}
```

然后我们在`SecondPage`中接受这个值, 注意`arguments`必须定义为可空, 因为很有可能为空的

```dart
import 'dart:developer';

import 'package:flutter/material.dart';

class SecondPage extends StatefulWidget {
  final Map? arguments;
  const SecondPage({super.key, required this.arguments});

  @override
  State<SecondPage> createState() => _SecondPageState();
}

class _SecondPageState extends State<SecondPage> {
  @override
  void initState() {
    super.initState();
    log(widget.arguments.toString());
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
        appBar: AppBar(
          backgroundColor: Theme.of(context).colorScheme.primary,
          title: const Text(
            "第二个页面",
          ),
        ),
        body: Column(
          children: [
            ElevatedButton(
                onPressed: () {
                  Navigator.pushNamed(context, "/second");
                },
                child: const Text("跳转"))
          ],
        ));
  }
}
```

我们在`initState`中打印参数, 然后我们在主页中`push`出`SecondPage`页面试一试吧

```dart
class _MyHomePageState extends State<MyHomePage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
        appBar: AppBar(
          backgroundColor: Theme.of(context).colorScheme.primary,
          title: Text(
            widget.title,
            style: const TextStyle(color: Colors.black),
          ),
        ),
        body: Column(
          children: [
            ElevatedButton(
                onPressed: () {
                  Navigator.pushNamed(context, "/second",
                      arguments: {"name": "张三"});
                },
                child: const Text("跳转"))
          ],
        ));
  }
}
```

我们发现路由是可以进行跳转并传值的, 但是这个方法还是相对复杂的, 除此之外我们还可以使用`GetX`来方便的做路由的传值






