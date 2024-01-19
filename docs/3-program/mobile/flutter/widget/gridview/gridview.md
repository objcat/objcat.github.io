# 🍎 简介

网格布局

# 🍎 快速开始

## 🌲 GridView.count

我们可以使用`GridView.count`来方便的构造网格

```dart
@override
Widget build(BuildContext context) {
return Scaffold(
  backgroundColor: const Color.fromARGB(220, 43, 45, 48),
  appBar: AppBar(
	backgroundColor: Colors.black,
	title: Text(widget.title),
  ),
  body: GridView.count(crossAxisCount: 2, children: const [
	Icon(Icons.padding_outlined),
	Icon(Icons.abc),
	Icon(Icons.e_mobiledata),
	Icon(Icons.back_hand),
	Icon(Icons.bike_scooter),
  ],)
);
}
```

然后我们来看一下样式

![](images/Pasted%20image%2020231110213338.png)

可以看到网格已经显示出来了

## 🌲 GridView.extent

```dart
@override
Widget build(BuildContext context) {
return Scaffold(
  backgroundColor: const Color.fromARGB(220, 43, 45, 48),
  appBar: AppBar(
	backgroundColor: Colors.black,
	title: Text(widget.title),
  ),
  body: GridView.extent(maxCrossAxisExtent: 200, children: const [
	Icon(Icons.padding_outlined, color: Colors.white,),
	Icon(Icons.abc, color: Colors.yellow,),
	Icon(Icons.e_mobiledata, color: Colors.blue,),
	Icon(Icons.back_hand, color: Colors.green,),
	Icon(Icons.bike_scooter, color: Colors.pink,),
  ],)
);
}
```

我们可以根据宽度来创建表格, 我顺便给图标设置了一些颜色

![](images/Pasted%20image%2020231110213942.png)

## 🌲 动态生成表格

很简单, 我们只需要写一个函数, 里面用for循环来生成速度就可以了

```dart
List<Widget> _list() {
List<Widget> list = [];
for (int i = 0; i < 100; i++) {
  list.add(Container(
	color: Colors.primaries.length > i ? Colors.primaries[i] : Colors.red,
	alignment: Alignment.center,
	child: Text("$i"),
  ));
}
return list;
}

@override
Widget build(BuildContext context) {
return Scaffold(
	backgroundColor: const Color.fromARGB(220, 43, 45, 48),
	appBar: AppBar(
	  backgroundColor: Colors.black,
	  title: Text(widget.title),
	),
	body: GridView.extent(maxCrossAxisExtent: 200, children: _list()));
}
```

我们来看看效果

![](images/Pasted%20image%2020231110215902.png)

我用了`Colors.primaries`然后判断了一下, 否则会数组越界

## 🌲 设置间距

```dart
List<Widget> _list() {
List<Widget> list = [];
for (int i = 0; i < 100; i++) {
  list.add(Container(
	color: Colors.primaries.length > i ? Colors.primaries[i] : Colors.red,
	alignment: Alignment.center,
	child: Text("$i"),
  ));
}
return list;
}

@override
Widget build(BuildContext context) {
return Scaffold(
	backgroundColor: const Color.fromARGB(220, 43, 45, 48),
	appBar: AppBar(
	  backgroundColor: Colors.black,
	  title: Text(widget.title),
	),
	body: GridView.count(
		crossAxisCount: 3,
		mainAxisSpacing: 10,
		crossAxisSpacing: 10,
		children: _list()));
}
```

- mainAxisSpacing 主轴间距, 也就是纵轴间距
- crossAxisCount 副轴间距, 也就是横轴间距

设置完我们看一下效果

![](images/Pasted%20image%2020231110221931.png)

我们发现横轴和纵轴间距都设置好了, 但是我们发现边框还是有间距的, 所以我们需要在外面加一个`Container`然后加上内边距即可

```dart
@override
Widget build(BuildContext context) {
return Scaffold(
	backgroundColor: const Color.fromARGB(220, 43, 45, 48),
	appBar: AppBar(
	  backgroundColor: Colors.black,
	  title: Text(widget.title),
	),
	body: Container(
	  padding: const EdgeInsets.all(10),
	  child: GridView.count(
		  crossAxisCount: 3,
		  mainAxisSpacing: 10,
		  crossAxisSpacing: 10,
		  children: _list()),
	));
}
```

设置内边距, 我们看一下

![](images/Pasted%20image%2020231110222207.png)

我们会发现有了内边距好看了不少
