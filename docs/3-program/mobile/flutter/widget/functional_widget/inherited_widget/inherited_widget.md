# 🍎 简介

`flutter`官方组件, 是一个功能性组件, 作用是共享数据, 我把它叫做`继承视图`

由于Flutter采用节点树的方式组织页面，以致于一个普通页面的节点层级会很深。
此时，我们如果还是一层层传递数据，当需要修改数据时，就会比较麻烦。
《Flutter 实战》中讲到：InheritedWidget 是 Flutter 中非常重要的一个功能型组件，它提供了一种数据在 widget 树中从上到下传递、共享的方式
比如我们在应用的根 `widget `中通过 `InheritedWidget` 共享了一个数据，那么我们便可以在任意子 `widget` 中来获取该共享的数据！
这个特性在一些需要在 widget 树中共享数据的场景中非常方便！如Flutter SDK 中正是通过 `InheritedWidget` 来共享应用主题（Theme）和 Locale(当前语言环境)信息的。

![](images/Pasted%20image%2020240122101538.png)

# 🍎 快速开始

我们通过上面的简介了解到`InheritedWidget`的作用其实是共享数据, 那我们接下来就自己动手做一下吧

## 🌲 定义需要共享的数据

定义一个状态管理类

```dart
import 'package:flutter/material.dart';

class UserProfileState extends InheritedWidget {
  const UserProfileState({
    super.key,
    required this.userName,
    required this.changeUserName,
    required Widget child,
  }) : super(child: child);

  final String userName;
  final Function changeUserName;

  static UserProfileState? of(BuildContext context) {
    final userProfile =
        context.dependOnInheritedWidgetOfExactType<UserProfileState>();
    assert(userProfile != null, 'No UserProfileState found in context');
    return userProfile;
  }

  @override
  bool updateShouldNotify(covariant InheritedWidget oldWidget) {
    return true;
  }
}
```

这里唯一需要注意的就是`updateShouldNotify`, 这个方法是判断视图是否刷新, 可以自己实现一定的条件

## 🌲 在页面上使用

```dart
class _MyHomePageState extends State<MyHomePage> {
  String userName = "未登录";

  void _changeUserName() {
    setState(() {
      userName = "123";
    });
  }

  @override
  void didChangeDependencies() {
    super.didChangeDependencies();
  }

  @override
  Widget build(BuildContext context) {
    debugPrint("build");

    return Scaffold(
      appBar: AppBar(
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
        title: Text(widget.title),
      ),
      body: UserProfileState(
        userName: userName,
        changeUserName: _changeUserName,
        child: const UserWidget(),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: _changeUserName,
        tooltip: 'Increment',
        child: const Icon(Icons.add),
      ),
    );
  }
}
```

我们来看看`UserWidget`

```dart
import 'package:flutter/material.dart';
import 'package:test_flutter/states/user_profile_state.dart';

class UserWidget extends StatelessWidget {
  const UserWidget({super.key});

  @override
  Widget build(BuildContext context) {
    return Text("${UserProfileState.of(context)?.userName}");
  }
}
```

可以看到直接使用`UserProfileState.of`来读取即可

# 🍎 进阶

## 🌲 GetX

[跳转 getx](../../../flutter_plugin/getx/getx.md)

## 🌲 Provider

[跳转 provider](../../../flutter_plugin/provider/provider.md)

## 🌲 BLOC

待完善



