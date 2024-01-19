
# 🍎 简介

flutter的sqlite库

https://pub-web.flutter-io.cn/packages/sqflite

# 🍎 快速开始

## 🌲 安装

```
flutter pub add sqflite
```

## 🌲 构造数据库路径

```dart
import 'package:path/path.dart';

// 构造数据库路径
var path = join(await getDatabasesPath(), "test.db");
print(path);
```

## 🌲 打开数据库

```dart
// 打开数据库
Database database = await openDatabase(path, version: 1,
	onCreate: (Database db, int version) async {
  
});
```

## 🌲 创建表

```dart
// 打开数据库
Database database = await openDatabase(path, version: 1,
	onCreate: (Database db, int version) async {
  // 创建表
  await db.execute(
	  'CREATE TABLE Test (id INTEGER PRIMARY KEY, name TEXT, value INTEGER, num REAL)');
});
```

## 🌲 插入数据

```dart
int id1 = await txn.rawInsert(
      'INSERT INTO Test(name, value, num) VALUES("some name", 1234, 456.789)');
```

如果插入成功是1, 插入失败会抛出异常不会执行下面的代码

## 🌲 更新数据

更新完毕后会返回改变的数量

```dart
int count = await database.rawUpdate(
	'UPDATE Test SET name = ?, value = ? WHERE name = ?',
	['updated name', '9876', 'some name']);
print('updated: $count');
```

## 🌲 删除数据

```dart
count = await database
    .rawDelete('DELETE FROM Test WHERE name = ?', ['another name']);
assert(count == 1);
```

## 🌲 关闭数据库

使用完毕后就关闭数据库

```dart
// Close the database
await database.close();
```

## 🌲 事务

```dart
await database.transaction((txn) async {
  int id1 = await txn.rawInsert(
      'INSERT INTO Test(name, value, num) VALUES("some name", 1234, 456.789)');
  print('inserted1: $id1');
  int id2 = await txn.rawInsert(
      'INSERT INTO Test(name, value, num) VALUES(?, ?, ?)',
      ['another name', 12345678, 3.1416]);
  print('inserted2: $id2');
});
```