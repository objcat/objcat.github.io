# 🍎 简介

flutter状态管理库, 底层是对`inheritedWidget`进行的封装

https://pub-web.flutter-io.cn/packages/provider/install

# 🍎 快速开始

## 🌲 安装

```
flutter pub add provider
```

## 🌲 创建数据模型

我们都知道这是一个共享数据的状态管理的框架, 那么使用它之前就需要一个共享数据的模型

```dart
class User {
  String name = "默认名字";
}
```

## 🌲 创建提供者

创建提供者是非常简单的, 我们可以看到`Provider`里面有两个参数, 一个是需要共享数据的`model`, 另外一个是`child`

```dart
class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return ChangeNotifierProvider<User>(
      create: (_) => User(),
      child: MaterialApp(
        title: 'Flutter Demo',
        theme: ThemeData(
          colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
          useMaterial3: true,
        ),
        home: const MyHomePage(title: 'demo'),
      ),
    );
  }
}
```

## 🌲 创建接收者

我们想要共享数据就要使用`Consumer`去消费

```dart
class _MyHomePageState extends State<MyHomePage> {
  @override
  Widget build(BuildContext context) {
    debugPrint("build");

    return Scaffold(
      appBar: AppBar(
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
        title: Text(widget.title),
      ),
      body: Consumer<User>(builder: (context, value, child) {
        return Text(value.name);
      }),
      floatingActionButton: Consumer<User>(builder: (context, value, child) {
        return FloatingActionButton(
          onPressed: () {
            value.name = "李四";
          },
          tooltip: 'Increment',
          child: const Icon(Icons.add),
        );
      }),
    );
  }
}
```

然后我们可以看到数据已经可以显示了

## 🌲 改变数据

当我们点击`floatingActionButton`后就可以去改变数据了, 我们点击后发现数据并没有改变, 不要急, 我们继续看

## 🌲 刷新数据

我们可以使用`notifyListeners`来让消费者去消费数据, 从而刷新我们的widget

```dart
onPressed: () {
  value.name = "李四";
  value.notifyListeners();
},
```

然后我们再次点击按钮发现可以刷新了, 但是我们发现`notifyListeners`有一个警告

```dart
The member 'notifyListeners' can only be used within instance members of subclasses of 'package:flutter/src/foundation/change_notifier.dart'.dart(invalid_use_of_protected_member)
The member 'notifyListeners' can only be used within 'package:flutter/src/foundation/change_notifier.dart' or a test.dartinvalid_use_of_visible_for_testing_member
```

似乎`flutter`只想让他用在内部, 所以我写了一个`relodata`来让它重新刷新数据

```dart
import 'package:flutter/material.dart';

class User extends ChangeNotifier {
  String name = "默认名字";

  void relodata() {
    notifyListeners();
  }
}
```

我们在使用的时候直接使用, 就不会出现警告了

```dart
onPressed: () {
  value.name = "李四";
  value.relodata();
},
```



