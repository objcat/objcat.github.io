# 🍎 简介

第三方状态管理插件, 但是它不仅仅可以做状态管理, 还可以做路由,  主题管理, 国际化多语言, obx局部更新, 网络请求, 数据验证功能, 所以是一个非常全面的框架

https://pub-web.flutter-io.cn/packages/get

# 🍎 快速开始

我们首先从学习它的状态管理开始

## 🌲 安装

```dart
flutter pub add get
```

## 🌲 接管App

我们想要在项目中使用Get的第一步就是让它去接管App

```dart
class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return GetMaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
        useMaterial3: true,
      ),
      home: const MyHomePage(title: 'demo'),
    );
  }
}
```

准备完毕, 现在你就可以使用Get了

# 🍎 状态管理

我们使用Get也可以轻松的实现我们的状态管理

## 🌲 响应式状态管理

### 🌸 小试牛刀

我们想要学习响应式状态管理就要先定义响应式的变量, 然后改变它来触发响应式的布局

我们通常使用`.obs`后缀定义响应式变量, 我们也可以使用`final RxInt _counter = RxInt(0);`来进行定义, 但是`.obs`更常用, 类型就是Rx开头的变量类型, 使用的时候在外面包裹一层`Obx`即可, 就能让这个控件变成响应式, 我们发现当我们想要改变某一个值的时候不需要再进行build了, 这样可以节省不少性能开销

```dart
class _MyHomePageState extends State<MyHomePage> {
  final RxInt _counter = 0.obs;

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
            Obx(() => Text("$_counter")),
            ElevatedButton(
              child: const Text("点击"),
              onPressed: () {
                _counter.value++;
              },
            )
          ],
        ));
  }
}
```

我们现在点击button就可以改变我们的计数器了

### 🌸 响应式列表

响应式列表也是非常简单的, 我们只需要定义一个响应式的数组即可

```dart
class _MyHomePageState extends State<MyHomePage> {
  final RxInt _counter = 0.obs;
  final RxList _list = ["张三", "李四"].obs;

  @override
  Widget build(BuildContext context) {
    log("build");
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
            Obx(() => Text("$_counter")),
            Obx(() => Column(children: _list.map((element) => ListTile(title: Text(element),)).toList(),)),
            ElevatedButton(
              child: const Text("点击"),
              onPressed: () {
                _counter.value++;
                _list.add("王五");
              },
            )
          ],
        ));
  }
}
```

然后我们来看一下效果

![](images/Pasted%20image%2020240122160540.png)

我们可以看到点击按钮可以添加王五进去, 通过这种方法我们也可以实现响应式的列表了

### 🌸 响应式对象

响应式对象和放在state中的其他变量一样使用

```dart
import 'package:get/state_manager.dart';

class Person {
  final RxString name = "默认名称".obs;
  final RxInt age = 0.obs;
}
```

然后我们在state中初始化这个person就可以了

```dart
class _MyHomePageState extends State<MyHomePage> {
  final RxList _list = ["张三", "李四"].obs;
  final Person _person = Person();

  @override
  Widget build(BuildContext context) {
    log("build");
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
            Obx(() => Text("${_person.name}")),
            Obx(() => Column(
                  children: _list
                      .map((element) => ListTile(
                            title: Text(element),
                          ))
                      .toList(),
                )),
            ElevatedButton(
              child: const Text("点击"),
              onPressed: () {
                _list.add("王五");
                _person.name.value = "你好";
              },
            )
          ],
        ));
  }
}
```

## 🌲 多页面数据共享

多页面数据共享, 顾名思义就是可以在多页面中读取同一个属性, 然后实现共享的一种策略, 当数据改变后多个页面的数据同时会进行刷新, 使用Get非常轻松就可以进行这部分操作

多页面共享要创建一个控制器继承于`GetxController`, 我们来创建一个

```dart
import 'package:get/state_manager.dart';

class CounterController extends GetxController {
  RxInt count = 0.obs;
}
```

然后我们在`State`页面进行使用

```dart
class _MyHomePageState extends State<MyHomePage> {
  final CounterController counterController = Get.put(CounterController());

  @override
  Widget build(BuildContext context) {
    log("build");
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
            Obx(() => Text("${counterController.count}")),
            ElevatedButton(
              child: const Text("点击"),
              onPressed: () {
                counterController.count++;
              },
            )
          ],
        ));
  }
}
```

发现计数器可以进行自增的, 我们再创建一个其他组件, 然后在里面也使用这个`CounterController`

```dart
import 'package:flutter/material.dart';
import 'package:get/get_state_manager/get_state_manager.dart';
import 'package:get/instance_manager.dart';
import 'package:test_flutter/states/counter_controller.dart';

class TestWidget extends StatelessWidget {
  TestWidget({super.key});

  final CounterController counterController = Get.find();

  @override
  Widget build(BuildContext context) {
    return Obx(() => Text('${counterController.count}'));
  }
}
```

然后把他放在主页面中点击按钮后能看到值的共享了

![](images/Pasted%20image%2020240122175345.png)

当然我们也可以创建多个`controller`, `Get.find`会根据类型自动找到对应的控制器, 比如我再创建一个`UserController`

```dart
import 'package:get/get.dart';

class UserController extends GetxController {
  final RxString name = "默认名字".obs;
}
```

然后直接在主页面上使用

```dart
class _MyHomePageState extends State<MyHomePage> {
  final CounterController counterController = Get.put(CounterController());
  final UserController userController = Get.put(UserController());

  @override
  Widget build(BuildContext context) {
    log("build");
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
            Obx(() => Text("${counterController.count}")),
            Obx(() => Text("${userController.name}")),
            ElevatedButton(
              child: const Text("点击"),
              onPressed: () {
                counterController.count++;
                userController.name.value = "李四";
              },
            ),
            TestWidget()
          ],
        ));
  }
}
```

## 🌲 binding

我们发现每次想使用`CounterController`的时候都要初始化, 这很不方便, 所以GetX也给我们提供了简便的方法来绑定数据

```dart
import 'package:get/instance_manager.dart';
import 'package:test_flutter/controllers/counter_controller.dart';
import 'package:test_flutter/controllers/user_controller.dart';

class AllControllerBindings extends Bindings {
  @override
  void dependencies() {
    Get.lazyPut(() => CounterController());
    Get.lazyPut(() => UserController());
  }
}
```

然后我们在`GetMaterialApp`上加载一下`binding`

```dart
class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return GetMaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        useMaterial3: true,
      ),
      routes: {
        "/": (context) => const MyHomePage(title: "首页"),
        "/second": (context) => const SecondPage(title: "第二个页面"),
        "/third": (context) => const ThirdPage(title: "第三个页面"),
      },
      initialBinding: AllControllerBindings(),
    );
  }
}
```

然后在所有页面我们就可以直接去`get`了, 而不需要去`初始化`

```dart
final CounterController counterController = Get.find();
final UserController userController = Get.find();
```

# 🍎 路由

Get对我们的路由进行了封装, 使用起来更加方便了

## 🌲 普通路由跳转

我们使用`Get.to`来进行普通路由的跳转

```dart
Get.to(const SecondPage(title: "第二个页面"));
```

## 🌲 命名路由跳转

我们可以使用`toNamed`来进行命名路由的跳转

```dart
Get.toNamed("/second");
```

传值也很简单

```dart
Get.toNamed("/second", arguments: {"title": "第二个页面222"});
```

不同的是, 我们可以直接在页面中使用`Get.arguments["title"]`来获取值

```dart
appBar: AppBar(
  backgroundColor: Theme.of(context).colorScheme.inversePrimary,
  title: Text(Get.arguments["title"]),
)
```

![](images/Pasted%20image%2020240122154052.png)

## 🌲 返回

```dart
Get.back();
```

## 🌲 返回到根

```dart
Get.offAll(const MyHomePage(title: "首页"));
```

## 🌲 跳到某一个页面并销毁之前页面

```dart
Get.offNamed("/third");
```

# 🍎 dialog

## 🌲 默认dialog

```dart
class MyHomePage extends StatefulWidget {
  const MyHomePage({super.key, required this.title});

  void alertDialog(context) async {
    await showDialog(
        context: context,
        builder: (context) {
          return AlertDialog(
            title: const Text("我是标题"),
            content: const Text("我是提示内容"),
            actions: [
              TextButton(
                  onPressed: () {
                    log("ok");
                    Navigator.of(context).pop();
                  },
                  child: const Text("确定")),
              TextButton(
                  onPressed: () {
                    log("cancel");
                    Navigator.of(context).pop();
                  },
                  child: const Text("取消"))
            ],
          );
        });
  }

  final String title;

  // 创建状态
  @override
  State<MyHomePage> createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  @override
  Widget build(BuildContext context) {
    debugPrint("build");

    return Scaffold(
        appBar: AppBar(
          backgroundColor: Theme.of(context).colorScheme.inversePrimary,
          title: Text(widget.title),
        ),
        body: Column(
          children: [
            ElevatedButton(
              child: const Text("flutter原生dialog"),
              onPressed: () {
                widget.alertDialog(context);
              },
            ),
            ElevatedButton(
              child: const Text("getx default dialog"),
              onPressed: () {
                Get.defaultDialog(
                    title: "提示信息",
                    middleText: "我是提示内容",
                    confirm: TextButton(
                        onPressed: () {
                          log("确定");
                          // Navigator.of(context).pop();
                          // 使用back取代上面的代码, 关闭dialog
                          Get.back();
                        },
                        child: const Text("确定")),
                    cancel: TextButton(
                        onPressed: () {
                          log("取消");
                          Get.back();
                        },
                        child: const Text("取消")));
              },
            )
          ],
        ));
  }
}
```

我们可以看到Get使用dialog是非常方便的, 我们和原生的dialog进行对比后就会发现, Get帮助我们进行了封装, 我们还可以发现的一点就是它封装了context可以不进行传值, 使用起来更加方便了

![](images/Pasted%20image%2020240122142851.png)

## 🌲 snackbar

使用起来也非常简单

```dart
Get.snackbar("我是标题", "我是内容");
```

![](images/Pasted%20image%2020240122142744.png)

里面有很多参数可以设置, 比如把他调整到底部

```dart
Get.snackbar("我是标题", "我是内容", snackPosition: SnackPosition.BOTTOM);
```

## 🌲 bottomSheet

```dart
Get.bottomSheet(Container(
  color: Colors.white,
  height: 200,
  child: Column(children: [
	ListTile(
	  leading: const Icon(Icons.wb_sunny_outlined, color: Colors.black,),
	  title: const Text("白天模式", style: TextStyle(color: Colors.black),),
	  onTap: () {
		Get.changeTheme(ThemeData.light());
		Get.back();
	  },
	),
	ListTile(
	  leading: const Icon(Icons.settings, color: Colors.black,),
	  title: const Text("夜晚模式", style: TextStyle(color: Colors.black),),
	  onTap: () {
		Get.changeTheme(ThemeData.dark());
		Get.back();
	  },
	)
  ]),
)
```

我们来看一下效果

![](images/Pasted%20image%2020240122144619.png)

# 🍎 多语言

在国际化的App中多语言是非常常见的, 使用GetX就可以做到, 待补充

# 🍎 切换主题

我们使用`Get.changeTheme(ThemeData.dark());`来进行主题的切换

```
// 切换到暗黑模式
Get.changeTheme(ThemeData.dark());
// 切换到白天模式
Get.changeTheme(ThemeData.light());
```

我们还可以使用Get提供给我们的方法方便的判断当前主题, 我们可以看到`Get.isDarkMode`, 然后根据这个布尔值去设置不同的模式的一个颜色

```dart
appBar: AppBar(
          backgroundColor: Get.isDarkMode ? Theme.of(context).colorScheme.inversePrimary : Theme.of(context).colorScheme.primary,
          title: Text(widget.title, style: const TextStyle(color: Colors.black),),
        )
```
