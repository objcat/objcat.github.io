# 🍎 简介

非常常用的控件, 跟html中的div很像, 我把它成为`壳`, 因为它经常去用来套别的控件

# 🍎 快速开始

## 🌲 设置背景颜色

最能体现一个组件的就是背景颜色了, 我们给它一个红色

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
	  color: Colors.red,
	));
}
```

我们可以看到默认就是全屏的

![](images/Pasted%20image%2020231111162349.png)

## 🌲 margin/padding

熟悉的都知道这个就是`外边距`和`内边距`

### 🌸 margin

比如我设置了一个`20`的外边距

```dart
Container(
  color: Colors.red,
  margin: const EdgeInsets.all(20),
)
```

![](images/Pasted%20image%2020231111163451.png)

外边距和可以单独设置某一面, 我们可以使用下面的方法, 四个参数分别对应的是`左上右下`

```dart
Container(
  color: Colors.red,
  margin: const EdgeInsets.fromLTRB(20, 20, 0, 0)
))
```

### 🌸 padding

这个是内边距, 我们想体现出来内边距需要在`Container`中嵌套一个其他视图

```dart
Container(
  color: Colors.red,
  child: Container(color: Colors.blue, width: 100, height: 100,),
)
```

![](images/Pasted%20image%2020231111163954.png)

嵌套之后我们发现, 外部的`Container`变成了跟随子视图大小了,  我们如果想维持全屏就要去设置父`Container`大小, 我使用了`MediaQuery`来获取屏幕的宽高

```dart
Container(
  width: MediaQuery.of(context).size.width,
  height: MediaQuery.of(context).size.height,
  color: Colors.red,
  child: Container(color: Colors.blue, width: 100, height: 100,),
)
```

设置完外部容器的大小发现了子组件又跟随了父元素大小

![](images/Pasted%20image%2020231111165015.png)

> 所以可以推算出来一个结论是`Container嵌套Container`父组件如果没设置宽高, 那么就是跟随子组件的大小, 如果设置了宽高, 那么就是跟随父组件大小

那我们如果想要实现下面的样式要怎么做呢?

![](images/Pasted%20image%2020231111165616.png)

其实很简单, 我们只需要设置一下父组件中的对齐方式即可

```dart
Container(
  width: MediaQuery.of(context).size.width,
  height: MediaQuery.of(context).size.height,
  alignment: Alignment.topLeft,
  color: Colors.red,
  child: Container(color: Colors.blue, width: 100, height: 100),
)
```

然后我们再设置`padding`后就能发现, 可以看到内边距了

```dart
Container(
  width: MediaQuery.of(context).size.width,
  height: MediaQuery.of(context).size.height,
  alignment: Alignment.topLeft,
  color: Colors.red,
  padding: const EdgeInsets.all(10),
  child: Container(color: Colors.blue, width: 100, height: 100),
)
```

![](images/Pasted%20image%2020231111165852.png)

## 🌲 嵌套Text

### 🌸 普通用法

```dart
Container(
  color: Colors.red,
  child: const Text("123", style: TextStyle(backgroundColor: Colors.blue),),
)
```

跟随子组件大小没啥好说的

![](images/Pasted%20image%2020231111170247.png)

不过我们可以发现文本的最右面有一丝丝红, 没事问题不大, 我们

### 🌸 居中

如果我们想把文字居中要怎么做呢?

```dart
Container(
  color: Colors.red,
  alignment: Alignment.center,
  child: const Text("123", style: TextStyle(backgroundColor: Colors.blue),),
)
```

我们设置`alignment`后发现它是按照屏幕来居中的

![](images/Pasted%20image%2020231111170605.png)


如果我们想一行居中要怎么做呢? 没错设置一下大小就行了

```dart
Container(
  width: MediaQuery.of(context).size.width,
  height: 20,
  color: Colors.red,
  alignment: Alignment.center,
  child: const Text("123", style: TextStyle(backgroundColor: Colors.blue),),
)
```

![](images/Pasted%20image%2020231111170811.png)

但是我们一般不会这么做





