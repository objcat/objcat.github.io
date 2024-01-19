# 🍎 简介

表示一竖行的布局控件

# 🍎 快速开始

## 🌲 基础用法

```dart
@override
Widget build(BuildContext context) {
return Scaffold(
  backgroundColor: const Color.fromARGB(220, 43, 45, 48),
  appBar: AppBar(
	backgroundColor: Colors.black,
	title: Text(widget.title),
  ),
  body: const Column(children: [
	Text("123", style: TextStyle(backgroundColor: Colors.blue)),
	Text("456", style: TextStyle(backgroundColor: Colors.orange)),
	Text("789", style: TextStyle(backgroundColor: Colors.purple)),
  ]),
);
}
```

我们看一下效果

![](images/Pasted%20image%2020231111171745.png)

## 🌲 居中

可以看到是排成一竖排的, 那我们怎么让文字居中呢? 其实很简单, 我们需要让`Column`的宽度撑满屏幕, 然后使用`crossAxisAlignment`属性就能设置元素的位置了, 因为我们的`Column`是无法设置宽高的, 想要获得设置宽高的能力需要用到`Container`, 我们一步一步来先在外层嵌套`Container`

```dart
Container(
color: Colors.red,
child: const Column(children: [
  Text("123", style: TextStyle(backgroundColor: Colors.blue)),
  Text("456", style: TextStyle(backgroundColor: Colors.orange)),
  Text("789132", style: TextStyle(backgroundColor: Colors.purple)),
]),
),
```

嵌套完了, 我们可以看一下

![](images/Pasted%20image%2020231111172722.png)

我们可以看到`Column`的`初始宽度`是撑起子视图的宽度, 初始高度是`一整个屏幕`, 所以我们发现了无法居中的原因就是宽度太小, 所以我们有多种方法来让文字居中

### 🌸 方法1 aligment(错误方法)

我们在`Container`已经学习了它的特性, 如果设置了`aligment`那么`Container`默认就是父控件大小, 然后子控件会在它的内部排布

```dart
Container(
alignment: Alignment.center,
color: Colors.red,
child: const Column(children: [
  Text("123", style: TextStyle(backgroundColor: Colors.blue)),
  Text("456", style: TextStyle(backgroundColor: Colors.orange)),
  Text("789111", style: TextStyle(backgroundColor: Colors.purple)),
]),
)
```

![](images/Pasted%20image%2020231111173419.png)

效果是这样的

我们也可以再嵌套一个`Container`看看`Column`到底是什么大小的

```dart
Container(
alignment: Alignment.center,
color: Colors.red,
child: Container(color: Colors.cyan, child: const Column(children: [
  Text("123", style: TextStyle(backgroundColor: Colors.blue)),
  Text("456", style: TextStyle(backgroundColor: Colors.orange)),
  Text("789111", style: TextStyle(backgroundColor: Colors.purple)),
]),),
)
```

看一下

![](images/Pasted%20image%2020231111173531.png)

看到了吗, 其实`Column`一直是这么窄的, 只不过是`Container`让它居中了, 这样写就限制了我们之后的一系列属性, 所以这种写法是错误的

### 🌸 方法2 width

我们可以通过设置`Container`的宽度来设置`Column`的宽度从而让中间的文字居中

```dart
Container(
width: MediaQuery.of(context).size.width,
color: Colors.red,
child: Container(color: Colors.cyan, child: const Column(children: [
  Text("123", style: TextStyle(backgroundColor: Colors.blue)),
  Text("456", style: TextStyle(backgroundColor: Colors.orange)),
  Text("789111", style: TextStyle(backgroundColor: Colors.purple)),
]),),
)
```

我们看一下效果

![](images/Pasted%20image%2020231111173901.png)

我们可以看到这才是真正的居中了

## 🌲 间距

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
	  width: MediaQuery.of(context).size.width,
	  color: Colors.red,
	  child: Container(
		color: Colors.cyan,
		child: const Column(children: [
		  Text("123", style: TextStyle(backgroundColor: Colors.blue)),
		  SizedBox(
			height: 10,
		  ),
		  Text("456", style: TextStyle(backgroundColor: Colors.orange)),
		  SizedBox(
			height: 10,
		  ),
		  Text("789111", style: TextStyle(backgroundColor: Colors.purple)),
		]),
	  ),
	));
}
```

有时候我们希望块与块之间是有间距的, 很简单我们直接用`SizedBox`就可以了

![](images/Pasted%20image%2020231113151429.png)

## 🌲 mainAxisAlignment

主轴上面的布局方式, 有上中下

```dart
Container(
width: MediaQuery.of(context).size.width,
color: Colors.red,
child: Container(
  color: Colors.cyan,
  child: const Column(
	  mainAxisAlignment: MainAxisAlignment.center,
	  children: [
		Text("123", style: TextStyle(backgroundColor: Colors.blue)),
		Text("456", style: TextStyle(backgroundColor: Colors.orange)),
		Text("789111",
			style: TextStyle(backgroundColor: Colors.purple)),
	  ]),
),
)
```

看一下效果

![](images/Pasted%20image%2020231111174349.png)

## 🌲 crossAxisAlignment

元素的水平布局

```dart
Container(
width: MediaQuery.of(context).size.width,
color: Colors.red,
child: Container(
  color: Colors.cyan,
  child: const Column(
	  crossAxisAlignment: CrossAxisAlignment.start,
	  children: [
		Text("123", style: TextStyle(backgroundColor: Colors.blue)),
		Text("456", style: TextStyle(backgroundColor: Colors.orange)),
		Text("789111",
			style: TextStyle(backgroundColor: Colors.purple)),
	  ]),
),
)
```

自己看效果, 不想说了, 跟吃饭一样简单

![](images/Pasted%20image%2020231111174105.png)





