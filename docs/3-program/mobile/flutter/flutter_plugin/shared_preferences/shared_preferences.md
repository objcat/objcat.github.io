# 🍎 简介

Flutter plugin for reading and writing simple key-value pairs. Wraps NSUserDefaults on iOS and SharedPreferences on Android.

https://pub-web.flutter-io.cn/packages?q=shared_preferences

# 🍎 快速开始

## 🌲 安装

```shell
flutter pub add shared_preferences
```

## 🌲 使用

```dart
SharedPreferences.getInstance().then((store) {
  // 存值
  store.setString("name", "张三");
  // 取值
  print(store.getString("name"));
});
```

我们发现直接就报错了

```shell
Launching lib/main.dart on Android SDK built for x86 in debug mode...
Running Gradle task 'assembleDebug'...

FAILURE: Build failed with an exception.

* What went wrong:
A problem occurred configuring project ':shared_preferences_android'.
> Could not resolve all files for configuration ':shared_preferences_android:classpath'.
   > Could not resolve com.android.tools.build:gradle:7.2.2.
     Required by:
         project :shared_preferences_android
      > Could not resolve com.android.tools.build:gradle:7.2.2.
         > Could not get resource 'https://dl.google.com/dl/android/maven2/com/android/tools/build/gradle/7.2.2/gradle-7.2.2.pom'.
            > Could not GET 'https://dl.google.com/dl/android/maven2/com/android/tools/build/gradle/7.2.2/gradle-7.2.2.pom'.
               > The server may not support the client's requested TLS protocol versions: (TLSv1.2, TLSv1.3). You may need to configure the client to allow other protocols to be used. See: https://docs.gradle.org/7.5/userguide/build_environment.html#gradle_system_properties
                  > Remote host terminated the handshake
> Failed to notify project evaluation listener.
   > Could not get unknown property 'android' for project ':shared_preferences_android' of type org.gradle.api.Project.
   > Could not get unknown property 'android' for project ':shared_preferences_android' of type org.gradle.api.Project.

* Try:
> Run with --stacktrace option to get the stack trace.
> Run with --info or --debug option to get more log output.
> Run with --scan to get full insights.

* Get more help at https://help.gradle.org
```

打开安卓项目更新一下就好了