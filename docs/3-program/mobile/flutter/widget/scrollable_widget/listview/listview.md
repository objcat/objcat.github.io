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

## 🌲 横向列表

如果我们想让列表横向滚动要怎么做呢? 很简单, 我们只需要修改一个属性

```dart
ListView.builder(
  scrollDirection: Axis.horizontal,
```

然后我们来看看效果

![](images/Pasted%20image%2020240123090557.png)

效果似乎不是我们所期待的, 因为它太高了, 我们需要调整一下它的高度, 然而在实际操作的过程中我们发现调整`container`的高度是没有作用的, 我们只能控制宽度, 所以我们要在`listView`外面加一层

```dart
class _MyHomePageState extends State<MyHomePage> {
  final List<String> entries = <String>['A', 'B', 'C', 'D', 'E'];
  final List<int> colorCodes = <int>[600, 500, 100, 50, 800];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
        backgroundColor: const Color.fromARGB(220, 43, 45, 48),
        appBar: AppBar(
          titleTextStyle: const TextStyle(color: Colors.white, fontSize: 18),
          backgroundColor: Colors.black,
          title: Text(widget.title),
        ),
        body: SizedBox(height: 120, child: ListView.builder(
            scrollDirection: Axis.horizontal,
            padding: const EdgeInsets.all(8),
            itemCount: entries.length,
            itemBuilder: (BuildContext context, int index) {
              return Container(
                width: 120,
                color: Colors.cyan[colorCodes[index]],
                child: Center(child: Text('Entry ${entries[index]}')),
              );
            }),));
  }
}
```

我们来看一下效果

![](images/Pasted%20image%2020240123090935.png)

# 🍎 ListTile

顾名思义, 就是列表的瓦片, 一般我们去做列表都会用到它, 那我们就一起来学习它吧

## 🌲 常规使用

常规使用, 我们主使用上面的一些属性来描述瓦片

```dart
ListView(
  children: [
	ListTile(
	  title: const Text("更新App"),
	  leading: const Icon(Icons.system_security_update),
	  onTap: () {
		log("点击了瓦片");
	  },
	),
	ListTile(
	  title: const Text("收藏"),
	  leading: const Icon(Icons.favorite),
	  onTap: () {
		log("点击了瓦片");
	  },
	),
	ListTile(
	  title: const Text("添加书签"),
	  leading: const Icon(Icons.my_library_add),
	  onTap: () {
		log("点击了瓦片");
	  },
	)
  ],
)
```

我们来看一下效果

![](images/Pasted%20image%2020240122213034.png)

我们可以看到效果还是很棒的, 我们在个人中心经常会使用这样的列表

## 🌲 副标题

```dart
ListTile(
  title: const Text("添加书签"),
  subtitle: const Text("我是副标题"),
  leading: const Icon(Icons.my_library_add),
  onTap: () {
	log("点击了瓦片");
  },
)
```

![](images/Pasted%20image%2020240122213216.png)

## 🌲 右箭头

我们可以使用`trailing`在列表右边放一个元素

```dart
ListTile(
  title: const Text("添加书签"),
  subtitle: const Text("我是副标题"),
  leading: const Icon(Icons.my_library_add),
  trailing: const Icon(Icons.arrow_forward_ios),
  onTap: () {
	log("点击了瓦片");
  },
)
```

![](images/Pasted%20image%2020240122213638.png)

## 🌲 自定义列表

我们发现列表自带的属性是很方便的, 但是在实际开发中, 往往设计图都不是按照这个尺寸设计的, 所以我们需要去自定义列表来满足对UI方面的需求

### 🌸 三级标题

# 🍎 json表达列表

我们一起来看看`json`数据是如何一步一步转换成列表的吧

## 🌲 基础列表

我们先来一个最基础的

```dart
class _MyHomePageState extends State<MyHomePage> {
  List<ListModel> _list = [];

  @override
  void initState() {
    super.initState();
    // 网络请求
    request();
  }

  void request() {
    // 模拟网络请求
    List jsonList = [
      {"title": "更新", "icon": 1},
      {"title": "收藏", "icon": 2},
      {"title": "书签", "icon": 3},
    ];
    // 模拟json解析
    List<ListModel> list = jsonList.map((e) => ListModel.fromJson(e)).toList();
    setState(() {
      _list = list;
    });
  }

  Icon getIcon(int icon) {
    if (icon == 1) {
      return const Icon(Icons.update);
    } else if (icon == 2) {
      return const Icon(Icons.abc);
    }
    return const Icon(Icons.bookmark);
  }

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
      body: ListView.builder(
        itemCount: _list.length,
        itemBuilder: (BuildContext context, int index) {
          return ListTile(
            title: Text(_list[index].title ?? ""),
            leading: getIcon(_list[index].icon ?? 0),
          );
        },
      ),
    );
  }
}
```

## 🌲 自定义列表

待完善



