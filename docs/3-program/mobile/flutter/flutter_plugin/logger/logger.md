# 🍎 简介

日志框架

https://pub-web.flutter-io.cn/packages/logger

# 🍎 快速开始

## 🌲 安装

```shell
flutter pub add logger
```

## 🌲 查看

然后我们就可以在`pubspec.yaml`文件中查看到这个依赖库了

![](images/Pasted%20image%2020231110162215.png)

## 🌲 基础使用

```dart
// 初始化Logger
var logger = Logger();
// 在项目中使用
logger.t("Trace log");
logger.d("Debug log");
logger.i("Info log");
logger.w("Warning log");
logger.e("Error log", error: 'Test Error');
logger.f("What a fatal log", error: 'Test Error', stackTrace: null);
```

## 🌲 全局配置

我们不可能每次使用都重新创建一个`Logger`所以我们可以进行全局配置, 很简单, 新建一个`global.dart`

```
import 'package:logger/logger.dart';

var logger = Logger(level: Level.debug);
```

然后我们就可以全项目公用一个`logger`了

## 🌲 调试

### 🌸 logcat

调试有两种方法, 一种是logcat, 另外一种是flutter自带的

我们使用logcat来打印日志

![](images/Pasted%20image%2020231110170224.png)

可以看到样子并不好看, logcat不能打印颜色什么的

### 🌸 flutter devtools

我们点击`run`里面的`flutter devtools`

![](images/Pasted%20image%2020231110170402.png)

然后会打开一个网页, 在里面点击`debugger`就能够看到日志了

![](images/Pasted%20image%2020231110170540.png)

可以看到效果比`logcat`好很多







