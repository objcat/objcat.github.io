# 🍎 简介

Flutter插件是Flutter应用程序与原生平台之间的桥梁，使得Flutter应用程序可以与原生代码进行交互，从而扩展Flutter应用程序的功能和能力。Flutter插件通常包括Dart和原生代码（例如Java、Kotlin或Objective-C、Swift等），并可以通过Flutter插件框架来注册、管理和调用。

在整个Flutter架构中，Flutter插件具有非常重要的作用和重要性。以下是Flutter插件在Flutter架构中的一些重要作用：

扩展Flutter应用程序的功能：Flutter插件可以提供许多原生平台的功能和能力，例如访问原生设备API、访问原生UI组件等。通过使用Flutter插件，Flutter应用程序可以获得更多的功能和能力，从而可以更好地满足用户需求。
提高Flutter应用程序的性能：通过使用Flutter插件，Flutter应用程序可以通过原生平台API来执行某些任务，从而可以提高应用程序的性能和响应速度。例如，使用原生平台的图像处理库来处理大量图像数据。
与原生代码进行交互：Flutter插件可以使Flutter应用程序与原生代码之间进行双向通信，从而可以让Flutter应用程序与原生平台进行无缝集成。这对于需要与现有原生应用程序集成的Flutter应用程序来说尤为重要。
促进Flutter生态系统的发展：Flutter插件可以提供许多不同类型的功能和能力，例如访问原生设备传感器、访问原生广告库等。通过将这些插件共享给其他Flutter开发者，Flutter插件可以促进Flutter生态系统的发展和壮大，使得更多的开发者能够使用Flutter来构建高质量的应用程序。

https://ducafecat.com/blog/flutter-plugin-channel

# 🍎 快速开始

## 🌲 创建插件项目

我们直接在`AndroidStudio`中可以快速创建插件模板

![](images/Pasted%20image%2020240119235740.png)

我们创建一个叫`test_flutter_plugin`的项目, 类型选择`Plugin`, 创建完成后目录是这样的

## 🌲 目录结构

![](images/Pasted%20image%2020240119223957.png)

我们可以看到目录接口还是比较清晰的, 我们主要注意的是`lib`和`example`

## 🌲 lib

lib就是我们插件的主工作区了, 我们可以看到`lib`中有一些`dart`文件

| 目录 | 文件名 | 说明 |
| -- | -- | -- |
| lib | test_flutter_plugin.dart | 接口调用类 |
| lib | test_flutter_plugin_channel.dart | 原生功能接口实现 |
| lib | test_flutter_plugin_platform_interface.dart | 功能接口定义(包含多平台) |
| lib | test_flutter_plugin_web.dart | Web功能接口实现 |

### 🌸 interface

我们先来看这个`test_flutter_plugin_platform_interface.dart`, 从字面上理解就是一个接口, 我们看一下代码

```dart
import 'package:plugin_platform_interface/plugin_platform_interface.dart';

import 'test_flutter_plugin_method_channel.dart';

abstract class TestFlutterPluginPlatform extends PlatformInterface {
  /// Constructs a TestFlutterPluginPlatform.
  TestFlutterPluginPlatform() : super(token: _token);

  static final Object _token = Object();

  static TestFlutterPluginPlatform _instance = MethodChannelTestFlutterPlugin();

  /// The default instance of [TestFlutterPluginPlatform] to use.
  ///
  /// Defaults to [MethodChannelTestFlutterPlugin].
  static TestFlutterPluginPlatform get instance => _instance;

  /// Platform-specific implementations should set this with their own
  /// platform-specific class that extends [TestFlutterPluginPlatform] when
  /// they register themselves.
  static set instance(TestFlutterPluginPlatform instance) {
    PlatformInterface.verifyToken(instance, _token);
    _instance = instance;
  }

  Future<String?> getPlatformVersion() {
    throw UnimplementedError('platformVersion() has not been implemented.');
  }
}
```

可以看到里面是写了一个抽象类, 然后声明了一些方法, 我们主要关注`getPlatformVersion`, 从字面意思理解就是获取平台版本号

### 🌸 channel

打开`test_flutter_plugin_channel.dart`文件我们可以看到`MethodChannelTestFlutterPlugin`继承了上面的`TestFlutterPluginPlatform`接口并重写了`getPlatformVersion`方法, 其中最重要的是`MethodChannel`这是我们flutter与原生交互的核心

```dart
import 'package:flutter/foundation.dart';
import 'package:flutter/services.dart';

import 'test_flutter_plugin_platform_interface.dart';

/// An implementation of [TestFlutterPluginPlatform] that uses method channels.
class MethodChannelTestFlutterPlugin extends TestFlutterPluginPlatform {
  /// The method channel used to interact with the native platform.
  @visibleForTesting
  final methodChannel = const MethodChannel('test_flutter_plugin');

  @override
  Future<String?> getPlatformVersion() async {
    final version = await methodChannel.invokeMethod<String>('getPlatformVersion');
    return version;
  }
}
```

我们在上面的表格里说了, 这是实现原生端代码的类

### 🌸 web

我们打开`test_flutter_plugin_web.dart`可以看到`TestFlutterPluginWeb`也继承于`TestFlutterPluginPlatform`并重写了`getPlatformVersion`方法

```dart
// In order to *not* need this ignore, consider extracting the "web" version
// of your plugin as a separate package, instead of inlining it in the same
// package as the core of your plugin.
// ignore: avoid_web_libraries_in_flutter
import 'dart:html' as html show window;

import 'package:flutter_web_plugins/flutter_web_plugins.dart';

import 'test_flutter_plugin_platform_interface.dart';

/// A web implementation of the TestFlutterPluginPlatform of the TestFlutterPlugin plugin.
class TestFlutterPluginWeb extends TestFlutterPluginPlatform {
  /// Constructs a TestFlutterPluginWeb
  TestFlutterPluginWeb();

  static void registerWith(Registrar registrar) {
    TestFlutterPluginPlatform.instance = TestFlutterPluginWeb();
  }

  /// Returns a [String] containing the version of the platform.
  @override
  Future<String?> getPlatformVersion() async {
    final version = html.window.navigator.userAgent;
    return version;
  }
}
```

从上面的代码我们可以得出结论, flutter插件的开发首先定义一个抽象类来规范代码, 然后各个平台创建一个文件来实现平台独有的一些方法, 这就是flutter能区分各个平台的原理

## 🌲 平台源码区

所谓平台源码区就是指各个平台实现自己原生代码的地方, 比如我们安卓中获取版本是要从原生中进行获取的, 要去写java代码

我们使用安卓平台来举例子, 我们打开项目目录中的安卓, 可以看到系统给我们自动生成了一个叫`TestFlutterPlugin`的类, 这个任何人都能看出来就是实现我们安卓端插件代码的一个类

![](images/Pasted%20image%2020240119231857.png)

但是我们进来后发现项目是报红的, 这是为什么呢? 我们尝试使用小飞象同步一下发现还是不行, 不要慌我们来配置一下, 我们可以看到`FlutterPlugin, MethodCallHandler`都是报红的, 这些是我们flutterSDK中的代码, 所以我们需要在`gradle`中配置一下, 让IDE可以识别到这些类

```groovy
//获取local.properties配置文件
def localProperties = new Properties()
def localPropertiesFile = rootProject.file('local.properties')
if (localPropertiesFile.exists()) {
	localPropertiesFile.withReader('UTF-8') { reader ->
		localProperties.load(reader)
	}
}
//获取flutter的sdk路径
def flutterRoot = localProperties.getProperty('flutter.sdk')
if (flutterRoot == null) {
	throw new GradleException("Flutter SDK not found. Define location with flutter.sdk in the local.properties file.")
}

dependencies {
	compileOnly files("$flutterRoot/bin/cache/artifacts/engine/android-arm/flutter.jar")
}

testOptions {
	unitTests.all {
		testLogging {
		   events "passed", "skipped", "failed", "standardOut", "standardError"
		   outputs.upToDateWhen {false}
		   showStandardStreams = true
		}
	}
}
```

然后我们用小飞象同步一下发现`import androidx.annotation.NonNull;`还是报错, 我们导入下面的包

```groovy
implementation('androidx.annotation:annotation:1.7.1')
```

然后同步一下, 我们发现问题解决了

## 🌲 example

熟悉完`平台源码区`后我们再来看`example`, 顾名思义就是例子, 这里是让我们去测试插件的地方

![](images/Pasted%20image%2020240119230726.png)

我们打开文件夹发现各个平台的项目也都在里面, 而我们只需要去运行`flutter`端的代码即可进行测试

下面我们就来测试一下吧, 我们运行`example`项目, 使用安卓模拟器进行测试

![](images/Pasted%20image%2020240120144723.png)

我们发现界面上显示除了安卓系统的版本, 那么我们插件这一块的测试也调通了, 这就是我们最简单的交互了, 功能是`flutter`获取原生设备的版本号

# 🍎 自定义交互(安卓)

学习完例子, 想必你对插件开发这一块已经有一点点的了解了, 那么我们去自定义一个方法, 来实现`flutter`与原生的交互, 我们优先使用安卓平台举例子

## 🌲 定义原生方法

如果想让`flutter`使用我们原生能力, 我们首先要在相对应的平台上去编写原生代码提供给`flutter`调用, 接下来我们来看一段核心代码

```dart
@Override
public void onMethodCall(@NonNull MethodCall call, @NonNull Result result) {
if (call.method.equals("getPlatformVersion")) {
  result.success("Android " + android.os.Build.VERSION.RELEASE);
} else {
  result.notImplemented();
}
}
```

可以看到当`flutter`调用`getPlatformVersion`方法时, 安卓原生就会在`onMethodCall`中收到`call.method`为`getPlatformVersion`的回调, 从而我们可以在原生中实现相对应的事情然后回传给`flutter`, 回传方法就是`result.success`

那我们接下来也写一个简单的, 比如我们要做一个加法, 那么就做一个加法功能, 把结果回传给`flutter`

```dart
@Override
public void onMethodCall(@NonNull MethodCall call, @NonNull Result result) {
if (call.method.equals("getPlatformVersion")) {
  result.success("Android " + android.os.Build.VERSION.RELEASE);
}

if (call.method.equals("add")) {
  int a  = call.argument("a");
  int b = call.argument("b");
  result.success(add(a, b));  
}

else {
  result.notImplemented();
}
}

int add(int a, int b) {
return a + b;
}
```

可以看到, 我的逻辑很简单, 就是当收到`add`这个回调的时候, 先获取`flutter`传给我们的参数, 然后调用原生的`add`方法将参数相加, 最后使用`result.success`返回对应的结果给到我们的`flutter`

## 🌲 定义flutter方法

我们需要定义一个flutter方法与原生的对应, 然后把这个方法提供给我们`example`来使用, 这就是自定义插件开发了

在flutter中我们需要写三个地方, 其中两个地方是接口和实现, 第三个地方就是把我们插件的能力向外暴露出去的另一个接口, 我们下面就开始编写吧

![](images/Pasted%20image%2020240120160521.png)

就是这三个文件

首先打开`test_flutter_plugin_platform_interface.dart`文件来写一个接口, 这没有什么好解释的, 我新加了一个方法叫`add`来做两个数相加

```dart
import 'dart:ffi';

import 'package:plugin_platform_interface/plugin_platform_interface.dart';

import 'test_flutter_plugin_method_channel.dart';

abstract class TestFlutterPluginPlatform extends PlatformInterface {
  /// Constructs a TestFlutterPluginPlatform.
  TestFlutterPluginPlatform() : super(token: _token);

  static final Object _token = Object();

  static TestFlutterPluginPlatform _instance = MethodChannelTestFlutterPlugin();

  /// The default instance of [TestFlutterPluginPlatform] to use.
  ///
  /// Defaults to [MethodChannelTestFlutterPlugin].
  static TestFlutterPluginPlatform get instance => _instance;

  /// Platform-specific implementations should set this with their own
  /// platform-specific class that extends [TestFlutterPluginPlatform] when
  /// they register themselves.
  static set instance(TestFlutterPluginPlatform instance) {
    PlatformInterface.verifyToken(instance, _token);
    _instance = instance;
  }

  Future<String?> getPlatformVersion() {
    throw UnimplementedError('platformVersion() has not been implemented.');
  }

  Future<int?> add(int a, int b) {
    throw UnimplementedError('add() has not been implemented.');
  }
}

```

接口写好之后我们打开`test_flutter_plugin_method_channel.dart`文件去实现该方法

```dart
import 'package:flutter/foundation.dart';
import 'package:flutter/services.dart';

import 'test_flutter_plugin_platform_interface.dart';

/// An implementation of [TestFlutterPluginPlatform] that uses method channels.
class MethodChannelTestFlutterPlugin extends TestFlutterPluginPlatform {
  /// The method channel used to interact with the native platform.
  @visibleForTesting
  final methodChannel = const MethodChannel('test_flutter_plugin');

  @override
  Future<String?> getPlatformVersion() async {
    final version =
        await methodChannel.invokeMethod<String>('getPlatformVersion');
    return version;
  }

  @override
  Future<int?> add(int a, int b) async {
    final sum = await methodChannel
        .invokeMethod<int>('add', <String, int>{'a': a, 'b': b});
    return sum;
  }
}
```

我们注意到, 这里涉及到了flutter访问原生方法并传参, 使用的是`invokeMethod`方法, `<int>`是个泛型标记了返回值的类型

然后我们再打开`test_flutter_plugin.dart`文件把接口暴漏出来

```dart
import 'test_flutter_plugin_platform_interface.dart';

class TestFlutterPlugin {
  Future<String?> getPlatformVersion() {
    return TestFlutterPluginPlatform.instance.getPlatformVersion();
  }

  Future<int?> add(int a, int b) {
    return TestFlutterPluginPlatform.instance.add(a, b);
  }
}
```

## 🌲 example

然后我们在测试程序中使用这个插件来实现`add`功能吧

```dart
import 'package:flutter/material.dart';
import 'dart:async';

import 'package:flutter/services.dart';
import 'package:test_flutter_plugin/test_flutter_plugin.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatefulWidget {
  const MyApp({super.key});

  @override
  State<MyApp> createState() => _MyAppState();
}

class _MyAppState extends State<MyApp> {
  String _platformVersion = 'Unknown';
  final _testFlutterPlugin = TestFlutterPlugin();

  @override
  void initState() {
    super.initState();
    initPlatformState();
  }

  // Platform messages are asynchronous, so we initialize in an async method.
  Future<void> initPlatformState() async {
    String platformVersion;
    // Platform messages may fail, so we use a try/catch PlatformException.
    // We also handle the message potentially returning null.
    try {
      platformVersion = await _testFlutterPlugin.getPlatformVersion() ??
          'Unknown platform version';
    } on PlatformException {
      platformVersion = 'Failed to get platform version.';
    }

    // If the widget was removed from the tree while the asynchronous platform
    // message was in flight, we want to discard the reply rather than calling
    // setState to update our non-existent appearance.
    if (!mounted) return;

    setState(() {
      _platformVersion = platformVersion;
    });
  }

  int _result = 0;

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: const Text('Plugin example app'),
        ),
        body: Center(
          child: Column(children: [
            Text('Running on: $_platformVersion\n'),
            Text('$_result'),
            ElevatedButton(onPressed: _click, child: const Text('点击'))
          ]),
        ),
      ),
    );
  }

  void _click() async {
    int sum = await TestFlutterPlugin().add(_result, 1) ?? -1;
    setState(() {
      _result = sum;
    });
  }
}
```

![](images/Pasted%20image%2020240120161117.png)

可以看到上面就是实现的UI界面, 每次点击按钮的时候会调用`TestFlutterPlugin().add`方法来让`_result`加一, 我们发现上面的数字显示的是正确的, 说明交互成功了

# 🍎 自定义交互(苹果)

我们安卓已经写出来了, 那么我们苹果要怎么写呢? 照葫芦画瓢, 这根本不是什么难事!

## 🌲 定义原生方法

我们打开根目录下面的ios文件夹, 可以看到里面就几个文件甚至连项目都不是

![](images/Pasted%20image%2020240120165219.png)

这要怎么去写代码呢? 不要着急, 我们来打开sample中的工程

![](images/Pasted%20image%2020240120165314.png)

在里面的pods中可以找到我们插件的代码, 在这里就可以进行编写了

```objective-c
- (void)handleMethodCall:(FlutterMethodCall*)call result:(FlutterResult)result {
    if ([@"getPlatformVersion" isEqualToString:call.method]) {
        result([@"iOS " stringByAppendingString:[[UIDevice currentDevice] systemVersion]]);
    }
    
    if ([@"add" isEqualToString:call.method]) {
        int a = [call.arguments[@"a"] intValue];
        int b = [call.arguments[@"b"] intValue];
        result(@([self addWithA:a b:b]));
    }
    
    else {
        result(FlutterMethodNotImplemented);
    }
}

- (int)addWithA:(int)a
              b:(int)b {
    return a + b;
}
```

苹果中不同的是, `result`是一个`block`, 我们调用这个`block`和`flutter`进行通信

## 🌲 定义flutter方法

不需要定义, 安卓苹果共用一套

## 🌲 example

我们使用iOS模拟器运行项目, 发现是可以做交互的

![](images/Pasted%20image%2020240120171123.png)



























