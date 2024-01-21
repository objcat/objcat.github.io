# 🍎 简介

列表

https://api.flutter-io.cn/flutter/widgets/ListView-class.html

# 🍎 快速开始

## 🌲 创建列表

```dart
@override
Widget build(BuildContext context) {
return Scaffold(
	backgroundColor: const Color.fromARGB(220, 43, 45, 48),
	appBar: AppBar(
	  titleTextStyle: const TextStyle(color: Colors.white, fontSize: 18),
	  backgroundColor: Colors.black,
	  title: Text(widget.title),
	),
	body: ListView(
	  padding: const EdgeInsets.all(8),
	  children: <Widget>[
		Container(
		  height: 50,
		  color: Colors.amber[600],
		  child: const Center(child: Text('Entry A')),
		),
		Container(
		  height: 50,
		  color: Colors.amber[500],
		  child: const Center(child: Text('Entry B')),
		),
		Container(
		  height: 50,
		  color: Colors.amber[100],
		  child: const Center(child: Text('Entry C')),
		),
	  ],
	));
}
```

我们看一下效果

![](images/Pasted%20image%2020231113170226.png)

这不难理解吧

## 🌲 ListView.builder

动态创建列表的方式

```dart
class _MyHomePageState extends State<MyHomePage> {
  
  final List<String> entries = <String>['A', 'B', 'C'];
  final List<int> colorCodes = <int>[600, 500, 100];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
        backgroundColor: const Color.fromARGB(220, 43, 45, 48),
        appBar: AppBar(
          titleTextStyle: const TextStyle(color: Colors.white, fontSize: 18),
          backgroundColor: Colors.black,
          title: Text(widget.title),
        ),
        body: ListView.builder(
            padding: const EdgeInsets.all(8),
            itemCount: entries.length,
            itemBuilder: (BuildContext context, int index) {
              return Container(
                height: 50,
                color: Colors.cyan[colorCodes[index]],
                child: Center(child: Text('Entry ${entries[index]}')),
              );
            }));
  }
}
```

可以看到所谓`builder`的方式其实关键就俩方法

- itemCount 显示多少行
- itemBuilder 每行显示什么样, 通过index可以确定数据源

效果是这样的

![](images/Pasted%20image%2020231113170657.png)




