# 🍎 简介

C# 是一种 **新式**、**创新**、**开放源代码**、**跨平台**，面向对象的编程语言，是 GitHub 上排在前列的 5 种编程语言之一。

是否拥有 JavaScript、Java 或 C++ 开发经验?你会立即发现 C# 用起来十分熟悉，并会乐于看到推出不断变化的功能，包括**类型安全**、**泛型**、**模式匹配**、**异步**、**记录**等。

我们希望你从按下第一个按键起，便爱上 C#

# 🍎 安装IDE

推荐选择微软的`Visual Studio`

[VisualStudio](../../7-software/IDE/VisualStudio/VisualStudio.md)

在VS中新建一个空项目我们就可以开始学习了

# 🍎 Hello World

```cs
using System;

namespace ConsoleApp1
{
    /* 类名为 HelloWorld */
    class HelloWorld
    {
        /* main函数 */
        static void Main(string[] args)
        {
            Console.WriteLine("hello world!");
        }
    }
}
```

如果你使用`.net6`以上也可以使用`顶级语句`, 在里面可以直接写代码, 不需要主类

```cs
// See https://aka.ms/new-console-template for more information
Console.WriteLine("Hello, World!");
```

# 🍎 变量

## 🌲 普通定义

```cs
// 整形
int testInt = 100;
// 浮点型
float testFloat = 1000f;
// 双精度浮点型
double testDouble = 1000.0; 
// 字符串
string testString = "objcat";    
// 数组
int[] testArr = [1, 2, 3];
// 列表
List<int> list = new List<int>() { 1, 2, 3 };
// 字典

```

## 🌲 推导类型

与普通定义类似, 我们只是把代表数据类型的符号替换成了`var`, 表示是一个推导类型, 它可以根据后面的类型自动推导

```cs
// 推导
var testInt = 100;
```

## 🌲 常量

当我们想让一个东西不可变的时候, 我们就可以使用常量, 与`typescript`不同的是, 这里的常量是不支持推导的, 所以只能标记类型, 比如

```cs
const int testInt = 100;
// 尝试修改就会报错 CS0131
testInt = 200;
```

# 🍎 函数

我们这里直接使用顶级语法来写, 因为使用起来比较方便

## 🌲 无参数无返回值

```cs
void hello()
{
    Console.WriteLine("hello world!");
}

hello();
```

## 🌲 有参数无返回值

```cs
void hello(string text)
{
    Console.WriteLine(text);
}

hello("hello world!");
```

## 🌲 有参数有返回值

```cs
string hello(string text)
{
    return text;
}

Console.WriteLine(hello("hello world!"));
```

## 🌲 默认参数

我们可以通过在参数上加`=`来设置默认参数, 当参数没传的时候程序会读取默认参数

```cs
void hello(string text = "这是默认参数")
{
    Console.WriteLine(text);
}

hello();
```

## 🌲 数组参数

### 🌸 数组

```cs
void hello(int[] arr)
{
    foreach (int i in arr)
    {
        Console.WriteLine(i);
    }
}

hello([1, 2, 3, 4, 5]);
```

### 🌸 列表

```cs
void hello(List<int> list)
{
    foreach (int i in list)
    {
        Console.WriteLine(i);
    }
}

var list = new List<int> { 1, 2, 3 };

hello(list);
```

## 🌲 字典参数

```cs
void hello(Dictionary<string, object> dic)
{
    foreach (var i in dic)
    {
        Console.WriteLine($"{i.Key} {i.Value}");
    }
}

var dic = new Dictionary<string, object>
{
    {"name", "张三" },
    {"age", 18 }
};

hello(dic);
```

## 🌲 静态函数

就是在函数前面加上`static`

```cs
static void hello()
{
    Console.WriteLine("hello world!");
}

hello();
```

在类中用法稍有不同, 下面会介绍

# 🍎 字符串

## 🌲 定义字符串

```cs
// 字符串变量
string myStr = "123";
```

## 🌲 截取字符串

```cs
string myStr = "123";
// 从第2个元素开始到结尾 
Console.WriteLine(myStr.Substring(1));
// 从第2个字符开始 截取1个字符
Console.WriteLine(myStr.Substring(1, 1));
```

## 🌲 拼接字符串

```cs
string myStr = "123";
// 加号拼接法
Console.WriteLine(myStr + "456");
// Format拼接法
Console.WriteLine(string.Format("{0}{1}", 1, 2));
// $拼接法 我们只需要在双引号前面加上$符号
var name = "张三";
Console.WriteLine($"Hello {name}!");
```

## 🌲 替换字符串

```cs
string myStr = "123";
Console.WriteLine(myStr.Replace("1", "0")); // 023
```

常量定义的字符串也可以使用这个方法替换, 原因是`Replace`方法是重新生成字符串而不是在原来的基础上替换

```cs
const string myStr = "123";
Console.WriteLine(myStr.Replace("1", "0")); // 023
```

## 🌲 查找字符串位置

返回字符串的索引位置, 从0开始, 可以看到字符串`2`的索引是`1`

```cs
string myStr = "123";
Console.WriteLine(myStr.IndexOf("2")); // 1
```

## 🌲 遍历字符串

```cs
// 方法1 for循环
string myStr = "123";
for (int i = 0; i < myStr.Length; i++)
{
	Console.WriteLine(myStr[i]);
}

// 方法2 foreach
foreach (var c in myStr)
{
	Console.WriteLine(c);   
}
```

# 🍎 数组

## 🌲 定长数组

在`C#`中数组有两种, 一种是可变的, 一种是不可变的, 不可变的数组又称定长数组, 我习惯把他们统称为数组

```cs
// 常规写法
int[] arr = { 1, 2, 3, 4, 5 };
// 集合简化写法, 必须指定类型 IDE0300 
int[] arr = [1, 2, 3];
// 推断 后面必须带类型, 否则无法推断
var arr = new int[] { 1, 2, 3, 4, 5 };
```

## 🌲 可变数组

可变数组就是创建后可以改动的, 又称为列表

```cs
// 带初始值
List<int> list = new List<int>() { 1, 2, 3 };
// 不带初始值
List<int> list = new List<int>();
```

## 🌲 获取元素

```cs
// 通过下标获取 下标从0开始
Console.WriteLine(list[0]); // 1
```

## 🌲 遍历数组

```cs
// 循环法
List<int> list = new List<int>() { 1, 2, 3 };
foreach (var item in list)
{
    Console.WriteLine(item); 
}
// 1
// 2
// 3

// 转字符串法
List<int> list = new List<int>() { 1, 2, 3 };
Console.WriteLine(string.Join(",", list)); // 1,2,3
```

## 🌲 添加元素

```cs
list.Add(4) // 1 2 3 4
```

## 🌲 删除元素

```cs
list.Remove(2);

foreach (var i in list)
{
	Console.WriteLine(i); // 1 3
}
```

## 🌲 截取数组

使用`GetRange`方法

```cs
List<int> list = new List<int>() { 1, 2, 3 };
List<int> list2 = list.GetRange(0, 1);
Console.WriteLine(string.Join(",", list2)); // 1
```

## 🌲 拼接数组

使用`AddRange`进行拼接, 值得注意的是这个方法会改变原数组

```cs
List<int> list = new List<int>() { 1, 2, 3 };
List<int> list2 = new List<int>() { 4, 5, 6 };
list.AddRange(list2);
Console.WriteLine(string.Join(",", list)); // 1,2,3,4,5,6
```

## 🌲 获取元素下标

使用`IndexOf`方法获取下标

```cs
List<int> list = new List<int>() { 1, 2, 3 };
Console.WriteLine(list.IndexOf(1)); // 0
```

# 🍎 字典

## 🌲 定义字典

```cs
// 创建空字典
Dictionary<string, object> dic = new Dictionary<string, object>();
// 写法2
Dictionary<string, object> dic = [];
// 带初始值的字典
Dictionary<string, object> dic = new Dictionary<string, object>
{
    {"name", "张三" },
    {"age", 18 }
};
// 带初始值的字典 写法2
Dictionary<string, object> dic = new Dictionary<string, object>
{
    ["name"] = "张三",
    ["age"] = 18
};
```

## 🌲 获取元素

```cs
Console.WriteLine(dic["name"]);
```

## 🌲 遍历字典

### 🌸 循环法

```cs
Dictionary<string, object> dic = new Dictionary<string, object>
{
    {"name", "张三" },
    {"age", 18 }
};

foreach (var item in dic)
{
    Console.WriteLine($"{item.Key} {item.Value}"); 
    // name 张三
	// age 18
}
```

### 🌸 函数法

```cs
Dictionary<string, object> dic = new Dictionary<string, object>
{
    {"name", "张三" },
    {"age", 18 }
};
Console.WriteLine(string.Join(",", dic.Select(x => $"{x.Key} {x.Value}"))); // name 张三,age 18
```

## 🌲 添加元素

```cs
Dictionary<string, object> dic = new Dictionary<string, object>
{
    {"name", "张三" },
    {"age", 18 }
};
dic["gender"] = "男";
Console.WriteLine(string.Join(",", dic.Select(x => $"{x.Key} {x.Value}"))); // name 张三,age 18,gender 男
```

## 🌲 删除元素

```cs
Dictionary<string, object> dic = new Dictionary<string, object>
{
    {"name", "张三" },
    {"age", 18 }
};
dic.Remove("name");
Console.WriteLine(string.Join(",", dic.Select(x => $"{x.Key} {x.Value}"))); // age 18
```

## 🌲 替换元素

```cs
dic["name"] = "李四"
Console.WriteLine(string.Join(",", dic.Select(x => $"{x.Key} {x.Value}"))); // age 18
```

# 🍎 条件语句

## 🌲 if

```cs
int a = 1;
if (a == 1)
{
    Console.WriteLine("a等于1");
}
```

## 🌲 if - else if

```cs
int a = 1;
if (a == 1)
{
    Console.WriteLine("a等于1");
}
else if (a == 2)
{
    Console.WriteLine("a等于2");
}
```

## 🌲 if - else if - else

```cs
int a = 1;
if (a == 1)
{
    Console.WriteLine("a等于1");
}
else if (a == 2)
{
    Console.WriteLine("a等于2");
}
else
{
    Console.WriteLine("a既不等于1也不等于2");
}
```

# 🍎 循环语句

## 🌲 for

通常配合数组使用

```cs
List<int> list = new List<int>() { 1, 2, 3 };

for (int i = 0; i < list.Count; i++)
{
    Console.WriteLine(i);
}

foreach (var item in list)
{
    Console.WriteLine(item);
}
```

## 🌲 while

```kotlin
int i = 0;
while (i < 3)
{
    Console.WriteLine(i);
    i++;
}
```

# 🍎 类和对象

## 🌲 类

我们发现`C#`和其他语言有一个不同的地方, 就是它的`顶级语法`中声明`Person`必须在下面, 否则会报错`顶级语句必须位于命名空间和类声明之前`, 如果使用传统的`Main`方法则随意

```cs
Person person = new Person();
Console.WriteLine(person);

class Person
{

}
```

## 🌲 定义成员变量

注意`C#`中的成员变量需要大写

```cs
Person person = new Person();
person.Name = "张三";
person.Age = 18;
Console.WriteLine(person.Name);

class Person
{
    public string Name;
    public int Age;
}
```

## 🌲 定义初始化方法

```cs
Person person = new Person("李四", 18);
Console.WriteLine(person.Name);

class Person
{
    public string Name;
    public int Age;
    public Person(string name, int age)
    {
        this.Name = name;
        this.Age = age;
    }
}
```

## 🌲 定义方法

注意`C#`中的方法名也要大写

```cs
Person person = new Person("李四", 18);
person.Hello();
Console.WriteLine(person.Name);

class Person
{
    public string Name;
    public int Age;
    public Person(string name, int age)
    {
        this.Name = name;
        this.Age = age;
    }
    public void Hello()
    {
        Console.WriteLine("hello i am " + Name);
    }
}
```

## 🌲 继承

`C#`中使用`:`来实现继承

```cs
class Person
{
    public string Name;
    public int Age;
}

class Student : Person
{
}

namespace ConsoleApp1
{
    class HelloWorld
    {
        static void Main(string[] args)
        {
            Student student = new Student();
            student.Name = "李四";
            Console.WriteLine(student.Name);
        }
    }
}
```

值得注意的是, 当我们的`Person`中有构造函数且构造函数有参数时继承就会报错`Base class 'Person' doesn't contain parameterless constructor`

```cs
Base class 'Person' doesn't contain parameterless constructor
```

### 🌸 补空法

我们可以给`Person`补上一个构造空参数的构造函数

```cs
namespace ConsoleApp1
{
    class HelloWorld
    {
        static void Main(string[] args)
        {
            Student student = new Student();
            student.Name = "李四";
            Console.WriteLine(student.Name);
        }
    }
}

class Person
{
    public string Name;
    public int Age;

    public Person()
    {
    }

    public Person(string name, int age)
    {
        this.Name = name;
        this.Age = age;
    }
}

class Student : Person
{
}
```

### 🌸 增加构造方法

我们也可以在`Student`增加构造方法

```cs
namespace ConsoleApp1
{
    class HelloWorld
    {
        static void Main(string[] args)
        {
            Student student = new Student("李四", 20);
            Console.WriteLine(student.Name);
        }
    }
}
class Person
{
    public string Name;
    public int Age;
    public Person(string name, int age)
    {
        this.Name = name;
        this.Age = age;
    }
}

class Student : Person
{
    public Student(string name, int age) : base(name, age)
    {
    }
}
```

看了一会才理解这个意思`base(name, age)`是去父类`Person`中调用构造函数, 这段代码的意思是把调用子类初始化时传入的参数`name和age`作为参数调用父类初始化方法, 这样就能给`Name和Age`赋值了

## 🌲 getter/setter

### 🌸 控制读写

我们在定义变量的时候可以使用`{ get; set; }`进行修饰, 比如`Name`成员变量, `get`是取值的意思, `set`是赋值的意思, 所以连起来就是`可读可写`的意思, 我们可以看到`Age`只有一个`{ get; }`意思是只读, 所以代码中的`person.Age = 30;`报错了

```kotlin
var person = new Person();
person.Name = "张三";
person.Age = 30; ❌

class Person
{
    public string Name { get; set; }
    public string Age { get; }
}
```

### 🌸 展开

那你也许会问, `getter/setter`里面是如何实现的, 我们现在就展开, 为了简单, 我只写一个属性, 你要记住所有编程语言几乎都是这么实现的, 首先定义一个私有`private string name`, `get`就是取它的值, `set`就是给这个变量赋值, 有人会问为什么不直接用`Name`, 因为会递归, 在`set`中无限给自己赋值你想想 - -

```cs
var person = new Person();
person.Name = "张三";
Console.WriteLine(person.Name);
class Person
{
    private string name;
    public string Name
    {
        get
        {
            Console.WriteLine("调用get");
            return name;
        }

        set
        {
            Console.WriteLine("调用set");
            name = value;
        }
    }
}
```

可以看到执行`person.Name = "张三";`的时候调用`set`, 执行`Console.WriteLine(person.Name);`的时候调用`get`, 我们现在可以捕获变量赋值和取值时候的操作了, 从而可以实现我们根据条件去修改某个值

## 运算符重载

```cs
static void Main(string[] args)
{
	Person person = new Person();
	Person person2 = new Person();
	Console.WriteLine(person + person2); // 会打印两个人
}

class Person
{
	public static Person operator +(Person person1, Person person2)
	{
		Console.WriteLine("两个人");
		return new Person();
	}
	
	public static Person operator -(Person person1, Person person2)
	{
		Console.WriteLine("没有人");
		return new Person();
	}
	
	public static Person operator *(Person person1, Person person2)
	{
		Console.WriteLine("乘两人");
		return new Person();
	}
	
	public static Person operator /(Person person1, Person person2)
	{
		Console.WriteLine("除两人");
		return new Person();
	}
}
```

# 🍎 委托

接触的不深, 我觉得它可以看成一个匿名函数, 可以表示任何一个类型的函数

## 🌲 没有返回值的委托

```cs
using System;

delegate void TestDelegate();

namespace ConsoleApp1
{
    /* 类名为 HelloWorld */
    class HelloWorld
    {
        /* main函数 */
        static void Main(string[] args)
        {
            TestDelegate testDelegate = new TestDelegate(hello);
            testDelegate();
        }
        
        static void hello()
        {
            Console.WriteLine("hello world");
        }
    }
}
```

## 🌲 委托回调

```cs
static void Main(string[] args)
{
	hello(delegate
	{
		Console.WriteLine("触发回调");
	});
}

static void hello(TestDelegate testDelegate)
{
	Thread.Sleep(2000);
	testDelegate();
}


也可以写成括号语法
hello(() => {
	Console.WriteLine("触发回调");
});

```

# 🍎 异步

## 🌲 创建Task

c#中最基本的异步编程就是Task<>语法, 它可以创建出一个后台线程, 达到异步执行任务的目的

```cs
static Task<string> hello()
{
	return Task.Run(() => {
		// 模拟耗时任务
		Thread.Sleep(1000);
		return "hello world";
	});
}
```

虽然这可能没有什么意义, 但是存在即合理 请继续往下看

## 🌲 接收值的方式

这个非常非常的关键, 学会了价值百万, 我们来看

### 🌸 非阻塞等待

我们可以使用`GetAwaiter().OnCompleted`来异步等待Task完成, 使用`task.Result`来获取task任务中的返回值

```cs
static void Main(string[] args)
{
	var task = hello();
	task.GetAwaiter().OnCompleted(() =>
	{
		Console.WriteLine(task.Result);
	});
	Console.WriteLine("按任意键结束...");
	Console.ReadKey();
}
```

打印什么我问你, 很明显嘛, 异步非阻塞

```
end!
按任意键结束...
```

### 🌸 阻塞等待

```cs
static void Main(string[] args)
{
	var task = hello();
	Console.WriteLine(task.Result);
	Console.WriteLine("按任意键结束...");
	Console.ReadKey();
}
```

这个打印什么? 很明显嘛, 这个是阻塞的, 代码在执行任务的时候会卡住

```
hello world
按任意键结束...
```

## 🌲 async/await

上面的Task写法有什么问题呢, 答案是有点复杂, 那么怎么简化呢, 我们只需要使用`async/await`就能起到简化作用了

### 🌸 阻塞等待

我们直接使用await就可以拆包出来Task里面的值, 但是有一个大前提, 就是`Main`方法需要改成异步的, 我们可以在方法前加上`async`并把`void`改成`Task`就可以了

```cs
static async Task Main(string[] args)
{
	var task = hello();
	string str = await task;
	Console.WriteLine(str);
	Console.WriteLine("按任意键结束...");
	Console.ReadKey();
}
```

是不是嘎嘎简单呢? 被`async`修饰的方法叫做异步方法

### 🌸 非阻塞等待

这个目前还不能做到, 只能使用以前的旧方法

```cs
var task = hello();
task.GetAwaiter().OnCompleted(() =>
{
	Console.WriteLine(task.Result);
});
```

### 🌸 异步方法hello world

异步方法中默认的返回值会被系统包装成`Task`所以不需要再`new`一个`task`了

```cs
static async Task<string> hello()
{
	 Thread.Sleep(1000);
	 return "hello world";
}
```

## 🌲 模拟网络请求

没使用Task前, 委托回调

```cs
delegate void CallBack(int i);

static void Main(string[] args)
{
	request((i =>
	{
		if (i == 1)
		{
			Console.WriteLine("网络请求成功");
		}
		else
		{
			Console.WriteLine("网络请求失败");
		}
	} ));
	
	Console.WriteLine("主线程执行完毕");
}

public static void request(CallBack callBack)
{
	new Thread(() =>
	{
		Thread.Sleep(2000);
		new Thread(() =>
		{
			Thread.Sleep(2000);
			callBack(2);
		}).Start();
	}).Start();
}
```

使用后 async/await 解决回调地狱问题

```cs
static void Main(string[] args)
{
	var req2 = request2();
	
	// 异步等待不阻塞主线程
	req2.GetAwaiter().OnCompleted(() =>
	{
		int i = req2.Result;
		if (i == 2)
		{
			Console.WriteLine("网络请求成功");
		}
		else
		{
			Console.WriteLine("网络请求失败");
		}
	});
	
	Console.WriteLine("主线程执行完毕");
	// 因为task属于后台线程系列, 所以会出现主线程结束停止执行的问题 所以这里面加上Console.ReadKey();
    Console.ReadKey();
}
            
public async static Task<int> request2()
{
	int i = await Task.Run(() =>
	{
		Thread.Sleep(1000);
		return 1;
	});

	int i2 = await Task.Run(() =>
	{
		Thread.Sleep(1000);
		return 1;
	});

	return i + i2;
}
```

# 🍎 线程

## 🌲 创建线程

```cs
new Thread(thread).Start();

// 匿名
new Thread(() =>
{
	Console.WriteLine(123);
}).Start();
```

# 🍎 扩展

## 🌲 扩展实例方法

```kotlin
fun main() {
    var str: String = "123"
    println(str.lenth2())
}

fun String.lenth2(): Int {
    return this.length;
}
```

## 🌲 扩展静态方法

```kotlin
fun main() {
    String.hello()
}

fun String.Companion.hello() {
    println("hello")
}
```

# 🍎 按键精灵

## 🌲 sendKey

```cs
// 异步点击
SendKeys.Send("{A}");
// 同步点击
SendKeys.SendWait("{A}");
```

常见用法是开一个线程

```cs
new Thread(() =>
{
	for (int i = 0; i < 10; i++)
	{
		Thread.Sleep(1000);
		SendKeys.SendWait("{A}");
	}
	MessageBox.Show("完毕");
}).Start();
```

## 🌲 keyboard_event

```cs
using System.Runtime.InteropServices;

[DllImport("user32.dll", EntryPoint = "keybd_event", SetLastError = true)]
public static extern void keybd_event(Keys bVk, byte bScan, uint dwFlags, uint dwExtraInfo);
```

使用方法

```cs
keybd_event(Keys.A, 0, 0, 0);
```

# 🍎 官方文档

## 🌲 `C#`

https://learn.microsoft.com/zh-cn/dotnet/csharp/?redirectedfrom=MSDN

## 🌲 .net

https://learn.microsoft.com/zh-cn/dotnet/fundamentals/

## 🌲 C#发展史

https://learn.microsoft.com/zh-cn/dotnet/csharp/whats-new/csharp-version-history

## 🌲 语言和不必要的规则

https://learn.microsoft.com/zh-cn/dotnet/fundamentals/code-analysis/style-rules/language-rules

## 🌲 .net发展史

![](images/Pasted%20image%2020230927094606.png)