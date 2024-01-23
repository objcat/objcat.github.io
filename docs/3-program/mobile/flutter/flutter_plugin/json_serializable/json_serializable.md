# 🍎 简介

`谷歌`官方提供的`实体`和`json`相互转化的框架

https://pub-web.flutter-io.cn/packages/json_serializable

# 🍎 快速开始

## 🌲 导入依赖

```
dart pub add json_serializable build_runner --dev
dart pub add json_annotation
```

## 🌲 创建类

我们就用`Person`类来举例子, 首先创建`Person.dart`

```dart
import 'package:json_annotation/json_annotation.dart';

part 'Person.g.dart';

@JsonSerializable()
class Person {
  String name;
  int age;

  Person(this.name, this.age);

  factory Person.fromJson(Map<String, dynamic> json) => _$PersonFromJson(json);

  Map<String, dynamic> toJson() => _$PersonToJson(this);

  @override
  String toString() {
    return 'Person{name: $name, age: $age}';
  }
}
```

不要管报错就这么写

## 🌲 生成代码

我们使用下面的命令生成代码

```
dart run build_runner build
```

执行完毕后会自动生成`Person.g.dart`保存在和`Person`同级目录下

![](images/Pasted%20image%2020231115101817.png)

```dart
// GENERATED CODE - DO NOT MODIFY BY HAND

part of 'Person.dart';

// **************************************************************************
// JsonSerializableGenerator
// **************************************************************************

Person _$PersonFromJson(Map<String, dynamic> json) => Person(
      json['name'] as String,
      json['age'] as int,
    );

Map<String, dynamic> _$PersonToJson(Person instance) => <String, dynamic>{
      'name': instance.name,
      'age': instance.age,
    };
```

## 🌲 使用

然后我们就可以试试了

```dart
import 'package:test_dart/model/Person.dart';

main() {
  const map = {"name": "张三", "age": 18};
  final per = Person.fromJson(map);
  print(per);
  final jsonString = per.toJson();
  print(jsonString);
}
```

## 🌲 列表解析

我们也可以从一个列表中去解析

```dart
List<Person> list = jsonList.map((e) => Person.fromJson(e)).toList();
```

## 🌲 常见崩溃

这一章节的名字很奇怪, 但是很直白, 你只需要记住一点, 只要是不确定一定不是`null`的地方用`?`可空类型可以解决大多数问题, 比如我们的Person要这么设置, 否则一旦map中存在一个`null`就容易引发崩溃

```dart
class Person {
  String? name;
  int? age;
```