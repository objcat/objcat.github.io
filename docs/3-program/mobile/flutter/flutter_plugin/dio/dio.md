# 🍎 简介

flutter网络请求库

https://pub-web.flutter-io.cn/packages/dio

# 🍎 快速开始

## 🌲 get

```dart
void _tap() {
print("开始进行网络请求");
const headers = {
  "User-Agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/119.0.0.0 Safari/537.36"
};
var dio = Dio();
dio.get("https://www.baidu.com", options: Options(headers: headers)).then((res) {
  print(res.data.toString());
});
}
```

我们用`flutter devtools`来打印结果

![](images/Pasted%20image%2020231114111743.png)

可以看到网页能请求下来, 但我们会发现网页卡死了

![](images/Pasted%20image%2020231114111922.png)

我们要容忍它的不完美, 不过它实在太差了...

我们用`logcat`试试

![](images/Pasted%20image%2020231114121427.png)

虽然会截断但好在不会卡顿

## 🌲 post

我自己写了个`POST`接口试试

```dart
var dio = Dio();
dio.post("http://127.0.0.1:4076/testPost").then((res) {
  print(res.data.toString());
});
```

会发现这样的问题

```shell
[ERROR:flutter/runtime/dart_vm_initializer.cc(41)] Unhandled Exception: DioException [connection error]: The connection errored: Connection refused
                                                                                                    Error: SocketException: Connection refused (OS Error: Connection refused, errno = 111), address = 127.0.0.1, port = 33792
```

后来上网查了一下, 发现高版本iOS和安卓默认都不支持HTTP, 要在配置文件中配置, 但是我配置好后, 仍然不行, 后来偶然查到需要使用内网ip进行访问(可能因为我是安卓模拟器吧)

```dart
var dio = Dio();
dio.post("http://10.104.4.174:4076/testPost").then((res) {
  print(res.data.toString());
});
```

我们看一下打印

```json
{name: 张三}
```

后来我又把安卓开启HTTP的方法删掉了, 发现也可以请求, 所以推断`flutter`某些情况下不需要配置HTTP支持

## 🌲 安卓开启HTTP支持

```xml
<application
        android:networkSecurityConfig="@xml/network_security_config"
        android:usesCleartextTraffic="true"
```

然后新建文件`network_security_config.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<network-security-config>
    <base-config cleartextTrafficPermitted="true">
        <trust-anchors>
            <certificates src="system" />
        </trust-anchors>
    </base-config>
</network-security-config>
```

即可
