# 🍎 简介

![](images/Pasted%20image%2020230907120306.jpg)

JavaScript（简称“JS”）是一种具有函数优先的轻量级，解释型或即时编译型的编程语言。虽然它是作为开发Web页面的脚本语言而出名，但是它也被用到了很多非浏览器环境中，JavaScript基于原型编程、多范式的动态脚本语言，并且支持面向对象、命令式、声明式、函数式编程范式。 [1]
JavaScript在1995年由Netscape公司的Brendan Eich，在网景导航者浏览器上首次设计实现而成。因为Netscape与Sun合作，Netscape管理层希望它外观看起来像Java，因此取名为JavaScript。但实际上它的语法风格与Self及Scheme较为接近。
JavaScript的标准是ECMAScript。截至2012年，所有浏览器都完整的支持ECMAScript 5.1，旧版本的浏览器至少支持ECMAScript 3标准。2015年6月17日，ECMA国际组织发布了ECMAScript的第六版，该版本正式名称为ECMAScript 2015，但通常被称为ECMAScript 6或者ES2015。 [1]

# 🍎 js写在哪里

首先你要知道js是写在哪里的, 大多数情况下js都是写在html中, 使用`script`标签来括起来, 浏览器的js引擎就会对里面的代码进行编译运行, 就可以得到你想要的结果了

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title></title>
	</head>
	<body>
		<script type="text/javascript">
			let a = "hello world!";
			console.log(a);
		</script>
	</body>
</html>
```

当然它也不仅仅可以写在html中, 也可以写在`.js`文件中, 那就需要本地的`nodejs`环境了, 我们以后再来学习, 下面的练习默认是在`<script type="text/javascript"></script>`标签中进行书写的

# 🍎 开发者工具

开发者工具是我们常用的调试程序的工具, 几乎在每次开发中都会用到, 诸如看一些变量的值, 看打印输出, 打断点调试程序等

## 🌲 win

在windows系统下, 我们只需要按F12就可以打开开发者工具了

## 🌲 mac

在Mac系统下, 我们需要按`alt + cmd + i`来打开开发者工具

![](images/Pasted%20image%2020230112144513.png)

打开后我们可以看到是这个样子的, 现在的标签页是控制台, 可以看一些输出, 我们也可以切换到其他标签页去看看

# 🍎 变量

## 🌲 数据类型

JavaScript 有多种数据类型：数字，字符串，数组，对象等等

```js
var length = 16;                                  // Number 通过数字字面量赋值 
var lastName = "Johnson";                         // String 通过字符串字面量赋值
var cars = ["Saab", "Volvo", "BMW"];              // Array  通过数组字面量赋值
var person = {firstName:"John", lastName:"Doe"};  // Object 通过对象字面量赋值
```

或者使用es6语法let也可以

```js
let length = 16;                                  // Number 通过数字字面量赋值 
let lastName = "Johnson";                         // String 通过字符串字面量赋值
let cars = ["Saab", "Volvo", "BMW"];              // Array  通过数组字面量赋值
let person = {firstName:"John", lastName:"Doe"};  // Object 通过对象字面量赋值
```

## 🌲 区别

### 🌸 作用域

var是函数作用域, let是块作用域, 函数作用域要大于块作用域, 因为这个特性使用var的时候要特别小心否则容易出错

```js
if (1) {
	var a = 1;
	let b = 2;
}
console.log(a);
console.log(b);
/**
1
1.html:14 Uncaught ReferenceError: b is not defined
    at 1.html:14:16
*/
```

我们可以看到只有a可以打印出来, b不能打印出来, 在高级语言中比如java, 在块内声明的变量是不能在块外进行打印的, 但是因为var是函数作用域, 所以可以进行打印

我们再看另外一个

```js
for (var i = 0; i < 5; i++) {
	setTimeout(() => console.log(i), 100)
}
/**
5 5 5 5 5
*/
```

输出是5个5, 为什么呢? 没错就是作用域问题, `setTimeout`是异步延时, 是在同步代码之后执行的, 所以当它开始打印第一个时循环已经都执行完了, 那时候i已经变成了5所以打印的都是5

如果换成let

```js
for (let i = 0; i < 5; i++) {
	setTimeout(() => console.log(i), 0)
}
/**
0 1 2 3 4
*/
```

我们会发现它打印的是01234, 因为它是块作用域, 每个块里的变量都是唯一的相互隔离, 所以无论异步函数如何去打印, 它总能打印到当前块里的变量值

### 🌸 可重复声明

```js
var a = 1;
console.log(a);
var a = 2;
console.log(a);
```

我们学过高级语言的都知道, 重复声明是一种常规的错误, 但它在js中是被允许的, 如果使用let声明变量则不能重复声明

```js
let a = 1;
console.log(a);
let a = 2;
console.log(a);
// Uncaught SyntaxError: Identifier 'a' has already been declared (at 1.html:11:8)
```

我们发现会报出一个错误, 就是不能重复声明, 所以使用let会更严谨一些

# 🍎 函数

## 🌲 无参数无返回值

```js
function hello() {
	console.log("hello world!");
}
hello();
```

## 🌲 有参数无返回值

```js
function hello(text) {
	console.log(text);
}
hello("hello world!");
```

## 🌲 有参数有返回值

```js
function hello(text) {
	return text;
}
console.log(hello("hello world!"));
```

## 🌲 默认值

当text不传值的时候就会使用默认值, 能够打印出`hello world`

```js
function hello(text="hello world") {
	return text;
}
console.log(hello());
```

## 🌲 可选参数

有些时候我们可能想传递参数中其中一部分, 而不是全部或是我们想调整传递参数的顺序, 那么我们可能需要使用下面这种方式

```js
function hello({text, text2}) {
	return text + text2;
}
console.log(hello({text2: "世界", text: "你好"}));
```

# 🍎 字符串

## 🌲 声明

```js
let a = "hello";
```

## 🌲 获取

我们可以通过下标的方式轻松的获取字符串中的内容, 如果超出字符串的长度会返回`undefined`

```js
let a = "hello";
console.log(a[0]);
```

## 🌲 截取

我们使用`slice`进行截取, 如果长度超过字符串最大长度会截取完整的字符串, 不会报错

```js
let a = "hello";
a = a.slice(0, 1);
console.log(a);
// h
```

## 🌲 拼接

### 🌸 加号拼接

```js
let a = "hello";
let b = a + " world!";
console.log(b);
```

### 🌸 模板拼接

```js
let a = "hello";
let b = `${a} world!`
console.log(b);
```

### 🌸 函数拼接

```js
let a = "hello";
let b = a.concat(" world!")
console.log(b);
```

## 🌲 替换

我们使用`replace`函数来替换字符串

```js
let a = "hello";
a = a.replace("o", "o world!")
console.log(a);
// hello world!
```

## 🌲 查找

我们可以使用`indexOf`函数来进行查找, 如果查找不到会返回`-1`

```js
let a = "hello";
console.log(a.indexOf("h"));
// 0
```

## 🌲 遍历

```js
let a = "hello";
for (let s in a) {
	console.log(a[s]);
}
```

# 🍎 数组

## 🌲 声明

```js
let a = [1, 2, 3];
console.log(a);
```

## 🌲 获取元素

```js
let a = [1, 2, 3];
console.log(a[0]);
// 1
```

## 🌲 获取元素下标

```js
let a = [1, 2, 3];
console.log(a.indexOf(1));
```

## 🌲 添加元素

```js
let a = [1, 2, 3];
a.push(4);
console.log(a);
```

## 🌲 删除元素

```js
let a = [1, 2, 3];
a.pop(3);
console.log(a);
```

## 🌲 截取元素

使用`slice`切片来截取数组

```js
let a = [1, 2, 3];
a = a.slice(0, 1);
console.log(a);
// [1]
```

# 🍎 字典/对象

js中的字典和高级语言中还是有点差距的, 因为它本质上是js中的对象

## 🌲 定义

```js
let a = {"name": "张三"};
console.log(a);
/**
Object
name
: 
"张三"
*/
```

## 🌲 获取元素

```js
let a = {"name": "张三"};
console.log(a.name); // 或 a["name"]
```

## 🌲 添加元素

```js
let a = {"name": "张三"};
a.age = 18;
console.log(a);
/**
{name: '张三', age: 18}
*/
```

## 🌲 删除元素

```js
let a = {"name": "张三"};
delete a.name
console.log(a);
```

## 🌲 合并字典

我们使用`Object.assign`来合并字典或者使用es6延展操作符`...`

```js
var a = {"name": "张三"};
var b = {"age": 16};
console.log(Object.assign(a, b));
console.log({...a, ...b});
```

# 🍎 条件语句

## 🌲 if

```js
let a = 1;
if (a == 1) {
	console.log("a等于1");
}
```

## 🌲 if - else if

```js
let a = 1;
if (a == 1) {
	console.log("a等于1");
} else if (a == 2) {
	console.log("a等于2");
}
```

## 🌲 if - else if - else

```js
let a = 1;
if (a == 1) {
	console.log("a等于1");
} else if (a == 2) {
	console.log("a等于2");
} else {
	console.log("a既不等于1也不等于2");
}
```

# 🍎 循环语句

## 🌲 for

```js
for (let i = 0; i < 3; i++) {
 console.log(i);
}

for (let i in [0, 1, 2]) {
 console.log(i);
}
```

## 🌲 while

```js
let i = 0;
while (i < 3) {
	console.log(i);
	i += 1;
}
```

# 🍎 类和对象

在`ECMAScript6`规范之前, js中并没有类的概念, 但是我们可以使用原型来实现, 所谓原型就是`js`的`function`拥有的一个属性, 叫做`prototype`它可以存放属性和方法, 下面就一起来看看吧

## 🌲 ES5

虽然ES5中没有类, 但是我们可以使用`构造函数+原型`来代替, 因为它跟类的特性一样

### 🌸 构造函数

构造函数是我们创建对象和给对象赋初始值的方法, 我们上面定义的函数就是构造函数, 所以`Student`既可以看作是类又可以看作是构造方法, 下面的构造方法是在对象创建的时候传入一个 名字

```js
function Student(name) {
	this.name = name;
};
```

### 🌸 添加方法

学生是可以学习的, 下面我们就添加一个学习的方法, 原理是给类的原型对象上添加函数

```js
Student.prototype.study = function study() {
	console.log(this.name + "在学习");
}
```

### 🌸 创建对象

学习了构造函数, 那我们就使用它来创建对象吧, 我们使用`new`关键字

```js
// 创建李雷和韩梅梅对象
var lilei = new Student("李雷");
var hanmeimei = new Student("韩梅梅");

// 打印名字
console.log(lilei.name);
console.log(hanmeimei.name);

// 调用学习方法
lilei.study();
hanmeimei.study();
```

### 🌸 继承

继承是面向对象中非常重要的特性, 我们就写一个好学生的类去继承学生吧, 我们使用call关键字来继承

```js
function GreatStudent(name, grade) {
	Student.call(this, name);
	// 分数
	this.grade = grade;
}
// 绑定父类的prototype, 否则方法会继承不过来
GreatStudent.prototype = Object.create(Student.prototype);
// 创建好学生张三对象
var zhangsan = new GreatStudent("张三", 100);
// 打印名字和成绩
console.log(zhangsan.name);
console.log(zhangsan.grade);
// 调用学习方法
zhangsan.study();
```

## 🌲 ES6

刚才已经学习过es5中的构造函数和原型来实现类的对象了, 那我们再来看es6的, 在es6中终于引入了类和对象的概念, 我们可以使用`class`关键字轻松的创建类了, 但是原理还是构造函数和原型那一套, 这只是语法上面的改变, 称为语法糖

### 🌸 创建类

```js
class Student {

}
```

### 🌸 定义成员变量

比如我们想给学生定义了名字和年龄, 我们可以直接在大括号里面写需要定义的成员变量名称, 使用的时候也很简单, 直接new关键字创建对象

```js
class Student {
    name;
    age;
}

var stu = new Student();
stu.name = "张三";
stu.age = "18";
console.log(stu.name)
console.log(stu.age)
```

### 🌸 构造函数

构造函数是我们创建对象和给对象赋初始值的方法, 在上面的案例中, 我们是先创建对象后赋值, 我们在构造函数中也可以赋值

```js
class Student {
	// 构造方法
	constructor(name) {
		this.name = name;
	}
}

var stu = new Student("张三");
console.log(stu.name)
```

### 🌸 添加方法

学生是可以学习的, 下面我们就添加一个学习的方法

```js
Student {
	// 构造方法
	constructor(name) {
		this.name = name;
	}
	// 学习方法
	study() {
		console.log(this.name + "在学习");
	}
}
```

### 🌸 调用方法

学习了构造函数, 那我们就使用它来创建对象吧, 我们使用`new`关键字

```js
// 创建李雷和韩梅梅对象
var lilei = new Student("李雷");
var hanmeimei = new Student("韩梅梅");

// 调用学习方法
lilei.study(); // 李雷在学习
hanmeimei.study(); // 韩梅梅在学习
```

### 🌸 继承

在es6中我们使用`extends`关键字来继承, 我们发现比es5中方便了许多, 语义和更加清晰了

```js
class GreatStudent extends Student {
	// 构造方法
	constructor(name, grade) {
		super(name);
		this.grade = grade;
	}
}
// 创建好学生张三对象
var zhangsan = new GreatStudent("张三", 100);
// 打印名字和成绩
console.log(zhangsan.name);
console.log(zhangsan.grade);
// 调用学习方法
zhangsan.study();
```

# 🍎 dom

## 🌲 简介

HTML DOM (文档对象模型)
当网页被加载时，浏览器会创建页面的文档对象模型（Document Object Model）。
我们可以利用它使原本静态的网页具有动态改变的能力

## 🌲 添加元素

我们可以使用`write`函数来实现向`body`中添加元素

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		<script type="text/javascript">
			document.write("<h1>hello world!</h1>");
		</script>
	</body>
</html>
```

## 🌲 查找元素

### 🌸 通过id

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		<h1 id="test-id">hello world!</h1>
		<script type="text/javascript">
			let h1 = document.getElementById("test-id");
			console.log(h1);
		</script>
	</body>
</html>
```

### 🌸 通过类名

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		<h1 class="test-class">hello world!</h1>
		<script type="text/javascript">
			let h1 = document.getElementsByClassName("test-class")[0];
			console.log(h1);
		</script>
	</body>
</html>
```

### 🌸 通过标签名

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		<h1>hello world!</h1>
		<script type="text/javascript">
			let h1 = document.getElementsByTagName("h1")[0];
			console.log(h1);
		</script>
	</body>
</html>
```

### 🌸 通过name

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		<h1 name="test-h1">hello world!</h1>
		<script type="text/javascript">
			let h1 = document.getElementsByName("test-h1")[0];
			console.log(h1);
		</script>
	</body>
</html>
```

# 🍎 随机数

## 🌲 随机整数

给定最大值和最小值获取随机的整数

```js
let randomInt = (min, max) => {
	let n = Math.random() * (max - min) + min;
	return Math.round(n);
}
```

# 🍎 fetch

https://developer.mozilla.org/zh-CN/docs/Web/API/Fetch_API/Using_Fetch

`fetch`是用来代替`XMLHttpRequest`的, 是新一代的网络请求框架, 绝大多数浏览器都支持, 除了IE, 所以可以直接粘贴到浏览器的控制台下使用, 或者使用`nodejs`运行也可以

## 🌲 请求HTML

```js
fetch("https://www.baidu.com").then(res => {
	return res.text();
}).then( html => {
	console.log(html);
}).catch (err => {
	console.log(err);
})
```

我们也可以使用`async/await`做请求

```js
async function test() {
	try {
		const res = await fetch("https://www.baidu.com");
		const html = await res.text();
		console.log(html);
	} catch(err) {
		console.log(err);
	}
}

test();
```

## 🌲 添加请求头

```js
async function test() {
	const res = await fetch("https://www.baidu.com", {
		"headers": {
			"User-Agent": "Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/86.0.4240.198 Safari/537.36"
		}
	});
	if(res.ok) {
		console.log(await res.text());
	}
}

test();
```

## 🌲 请求Json

```js
async function test() {
	try {
		const res = await fetch("https://mock.apifox.cn/m1/1337300-0-default/api/v1/getJson");
		const json = await res.json();
		console.log(json);
	} catch(err) {
		console.log(err);
	}
}

test();
```

# 🍎 sleep

会编程的都知道, sleep一般指的是阻塞的睡眠, 一旦开始执行页面就会卡住, 但有时候往往是需要这种操作的, js中虽然没有sleep但是我们可以使用循环来实现

```js
function sleep(numberMillis) {
	var now = new Date();
	var exitTime = now.getTime() + numberMillis;
	while (true) {
		now = new Date();
		if (now.getTime() > exitTime) {
			return;
		}
	}
}
```

# 🍎 Promise

promise是处理异步任务顺序执行非常好用的工具 下面我们就来看看吧

## 🌲 回调地狱问题

```js
function eat(food, callback) {
	setTimeout(() => {
		console.log(food + " 吃完了")
		callback()
	}, 1000)
}

eat("鸡", () => {
	eat("牛", () => {
		eat("猪", () => {
			eat("羊", () => {
				
			})
		})
	})
})
```

## 🌲 解决回调地狱-方法1

可以使用then来处理promise对象

```js
function eat(food) {
	return new Promise((resolve, reject) => {
		setTimeout(() => {
			console.log(food + " 吃完了")
			resolve()
		}, 1000)
	})
}

eat("鸡").then(() => {
	return eat("牛")
}).then(() => {
	return eat("猪")
}).then(() => {
	return eat("羊")
})
```

## 🌲 解决回调地狱-方法2

可以使用 await和async来处理promise对象

```js
function eat(food) {
	return new Promise((resolve, reject) => {
		setTimeout(() => {
			console.log(food + " 吃完了")
			resolve();
		}, 1000);
	});
}

async function async_eat() {
	await eat("鸡");
	await eat("牛");
	await eat("猪");
	await eat("羊");
}

async_eat();
```