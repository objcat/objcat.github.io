# 🍎 简介

HTML的全称为超文本标记语言，是一种标记语言。它包括一系列标签．通过这些标签可以将网络上的文档格式统一，使分散的Internet资源连接为一个逻辑整体。HTML文本是由HTML命令组成的描述性文本，HTML命令可以说明文字，图形、动画、声音、表格、链接等。 [1] 
超文本是一种组织信息的方式，它通过超级链接方法将文本中的文字、图表与其他信息媒体相关联。这些相互关联的信息媒体可能在同一文本中，也可能是其他文件，或是地理位置相距遥远的某台计算机上的文件。这种组织信息方式将分布在不同位置的信息资源用随机方式进行连接，为人们查找，检索信息提供方便。 [1] 

# 🍎 新建项目

我们在`HbuilderX`里面点击新建

![](images/Pasted%20image%2020230109102828.png)

然后起个名字叫`test-html`点击创建就可以了

![](images/Pasted%20image%2020230109104023.png)

html的基本组成结构是标签, 而且大部分都是成对存在的, 也有单标签, 以后会讲, 最外层是`<html></html>`标签, 让浏览器识别这是一个网页, 里面的结构由`head`和`body`组成, 头里写我们的标题和引入一些js, css等, body中写我们的内容

# 🍎 标签

## 🌲 标题和段落

标题使用`<h1>~<h6>`六个标签, h1最大, h6最小, 段落我们下面就来写一个页面试试吧

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title></title>
	</head>
	<body>
		<h1>鹅鹅鹅</h1>
		<p>
			鹅鹅鹅，曲项向天歌。
			白毛浮绿水，红掌拨清波。
		</p>
	</body>
</html>
```

![](images/Pasted%20image%2020230109103652.png)

## 🌲 图像

比较常见的标签, 下面我们就给`鹅鹅鹅`添加上图片吧, 我们使用`<img>`标签来表示一个图片, 使用`src`属性来指定图片路径, 图片可以是本地或者网络上的, 我们先找一个鹅的图片, 把它放在img路径下

![](images/Pasted%20image%2020230109105618.png)

然后我们写一个img标签配上src图片路径就可以了, 这种路径叫做相对路径, 因为我们是在html中写的这个标签, 而img就在我们index.html的同级目录下, 所以是可见的, 我们直接使用`img/1.jpg`就可以引入这个图片了

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title></title>
	</head>
	<body>
		<h1>鹅鹅鹅</h1>
		<p>
			鹅鹅鹅，曲项向天歌。
			白毛浮绿水，红掌拨清波。
		</p>
		<img src="img/1.jpg" />
	</body>
</html>
```

![](images/Pasted%20image%2020230109105824.png)

我们可以看一下效果, 这样图片就引入进来了

## 🌲 链接

我们使用a标签来跳转到外部或内部链接, 平时跳转页面都要用到这个标签的

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title></title>
	</head>
	<body>
		<a href="https://www.baidu.com">百度一下</a>
	</body>
</html>
```

![](images/Pasted%20image%2020230111100511.png)

非常简单, 我们配置一下href属性来让它跳转到某一个页面, 比如我这里跳转到百度

## 🌲 布局盒子

div是html中最常用到的标签, 它就像一个容器一样, 几乎每个页面上都是用它装填元素的

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title></title>
	</head>
	<body>
		<div style="width: 100px; height: 100px; background-color: red;"></div>
	</body>
</html>
```

![](images/Pasted%20image%2020230111100434.png)

它默认是块级元素, 也就是每个会占用一行, 我们来看看这个特性, 我们可以再创建个div, 会发现第二个div在下面排列而不是横着排列

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title></title>
	</head>
	<body>
		<div style="width: 100px; height: 100px; background-color: red;"></div>
		<div style="width: 100px; height: 100px; background-color: blue;"></div>
	</body>
</html>
```

![](images/Pasted%20image%2020230111100350.png)

## 🌲 行内标签

span是常用的行内标签, 和块级标签不同的是, 它无法设置大小, 但它会根据内容物的大小自动放大和缩小, 常用于包裹一段文字, 给文字添加样式等

### 🌸 基本用法

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title></title>
	</head>
	<body>
		<span style="width: 100px; height: 100px; background-color: red;">123</span>
	</body>
</html>
```

![](images/Pasted%20image%2020230111101157.png)

我们再添加一个span试试

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title></title>
		<style>
			
		</style>
	</head>
	<body>
		<span>123</span>
		<span>456</span>
	</body>
</html>
```

![](images/Pasted%20image%2020230111101607.png)

我们会发现两个元素是横向排列的, 因为行内元素是不会占用一行

但是我们发现两个span中间有一个间隙, 这也是老生常谈的问题了

### 🌸 解决间隙问题

我们可以把两个span写在一行

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title></title>
		<style>
			
		</style>
	</head>
	<body>
		<span style="width: 100px; height: 100px; background-color: red;">123</span><span style="width: 100px; height: 100px; background-color: blue;">456</span>
	</body>
</html>
```

![](images/Pasted%20image%2020230111103306.png)

## 🌲 选择框

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title></title>
	</head>
	<body>
		<select>
			<option>张三</option>
			<option>李四</option>
			<option>王五</option>
		</select>
	</body>
</html>
```






































