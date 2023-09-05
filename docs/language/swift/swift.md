# 🍎 简介

![swift](./images/swift.png)

Swift 是一种支持多编程范式和编译式的开源编程语言,苹果于2014年WWDC（苹果开发者大会）发布, 用于开发 iOS, OS X 和 watchOS 应用程序, Swift 结合了 C 和 Objective-C 的优点并且不受 C 兼容性的限制, Swift 在 Mac OS 和 iOS 平台可以和 Object-C 使用相同的运行环境, 2015年6月8日, 苹果于WWDC 2015上宣布, Swift将开放源代码, 包括编译器和标准库。

# 🍎 环境搭建

电脑: 

[MacOS](https://www.apple.com.cn/mac/)

官方网站

https://developer.apple.com

IDE

Xcode - 直接在AppStore里面搜索并下载


# 🍎 Hello World

```swift
print("Hello world!")
```

# 🍎 变量

```swift
// 整型
var a = 100
// 浮点型
var b = 1000.0
// 字符串
var c = "Objcat"
// 数组
var d = [1, 2, 3]
// 字典
let e = ["name": "objcat", "age": "18"]
// 元组
let f = (1, 2, 3)
let g = (name: "张三", age: "16")

var 表示变量 let 表示常量
```

# 🍎 函数

## 🌲 无参数无返回值

```swift
func hello() {
    print("hello world!")
}
```

## 🌲 有参数无返回值

```swift
func hello(text: String) {
    print(text)
}

hello(text: "hello world!")
```

## 🌲 有参数有返回值

```swift
func hello(text: String) -> String {
    return text
}

var result = hello(text: "hello world!")
print(result)
```

## 🌲 静态函数

```swift
static func hello() {
	print("hello")
}

SwiftViewController.hello()
```

## 🌲 数组参数

```swift
func hello(arr: [Int]) {
    print(arr)
}

func hello(arr: Array<Int>) {
    print(arr)
}

hello(arr: [1, 2, 3])
```

## 🌲 字典参数

```swift
func hello(dic: [String: String]) {
    print(dic)
}

func hello(dic: Dictionary<String, String>) {
    print(dic)
}

hello(dic: ["name": "张三"])
```

## 🌲 指针参数

```swift
// 使用 inout 关键字来传递一个可修改的值
func hello(dic: inout Dictionary<String, String>) {
    dic["name"] = "李四"
}
var dic = ["name": "张三"]
hello(dic: &dic)
print(dic)
```

## 🌲 + 和 += 方法

```swift
class Student {
    static func + (stu: Student, stu2: Student) {
        
    }
    
    static func += (stu: Student, stu2: Student) {
        
    }
}
```

# 🍎 字符串

## 🌲 定义字符串

```swift
// 定义单行
var my_str = "123"
// 定义多行
let my_str = """
hello world
hi siri
"""
```

## 🌲 获取字符串

```swift
// 从头截取两个字符
my_str[my_str.index(my_str.startIndex, offsetBy: 0) ... my_str.index(my_str.startIndex, offsetBy: 1)] // 12
// 从头截取两个字符
my_str[...my_str.index(my_str.startIndex, offsetBy: 1)] // 12
// 截取完整字符串
my_str[stmy_strr.index(my_str.startIndex, offsetBy: 0)...my_str.index(my_str.endIndex, offsetBy: -1)] // 123
// 或
my_str[my_str.index(my_str.startIndex, offsetBy: 0)...]
// 或
my_str[...]

// 经测试这种方法很慢反且效率很低, 我们也可以把字符串转化成数组进行截取
let my_arr = Array(my_str)
print(my_arr[0])
```

## 🌲 拼接字符串

```swift
// 加号拼接
let my_str2 = my_str + "456" // 123456
// 数组拼接
let my_str2 = ["123", "456"].joined() // 123456
// format
let my_str2 = my_str.appendingFormat("%@", "456")
print(my_str2) // 123456
// format
let my_str2 = "\(my_str)456" // 123456
```

## 🌲 替换字符串

```swift
// 将第一个字符替换成12
my_str.replaceSubrange(my_str.index(my_str.startIndex, offsetBy: 0)...my_str.index(my_str.startIndex, offsetBy: 0), with: "12") // 1223
```

## 🌲 查找字符串位置

```swift
print(my_str.range(of: "2"))
// Optional(Range(Swift.String.Index(_rawBits: 65536)..<Swift.String.Index(_rawBits: 131072)))

// 字符串位置转索引
my_str.distance(from: my_str.startIndex, to: range.lowerBound)
```

## 🌲 遍历字符串

```swift
for s in my_str {
    print(s)
}
```

# 🍎 数组

## 🌲 定义数组

```swift
// 推断类型
var my_arr = [1, 2, 3]
// 指定类型
let my_arr2: [Int] = [1, 2, 3]
// 指定类型
let my_arr3: Array<Int> = [1, 2, 3]
// 指定类型as
var my_arr4 = [1, "2"] as [Any]
// 空数组
var my_arr5 = []
```

## 🌲 获取元素

```swift
// 通过下标获取 下标从0开始
print(my_arr[0]) // [1, 2, 3]
```

## 🌲 添加元素

```swift
my_arr.append(4) 
print(my_arr) // [1, 2, 3, 4]
```

## 🌲 删除元素

```swift
// 移除第三个元素
my_arr.remove(at: 3)
print(my_arr) // [1, 2, 3]
```

## 🌲 截取元素

注意跟获取区分一下, 获取是传下标, 而截取是传区间, 数组中截取元素是使用区间来实现的

```swift
// 截取第一个元素
my_arr[0...0] // [1]
或
my_arr[0..<1] // [1]
// 某一个位置截取到最后
my_arr[0...] // [1, 2, 3]
my_arr[1...] // [2, 3]
// 从开始截取到某一个位置
my_arr[...0] // [1]
my_arr[...(my_arr.count - 1)] // [1, 2, 3]
// 截取所有元素
my_arr[...]
```

## 🌲 拼接数组

```swift
my_arr.append(contentsOf: [4, 5, 6]) // [1, 2, 3, 4, 5, 6]
```

## 🌲 获取元素下标

```swift
let arr = [1, 2, 1]
print(arr.firstIndex(of: 1) ?? "") // 0
print(arr.lastIndex(of: 1) ?? "") // 1
```

# 🍎 字典

## 🌲 定义字典

```swift
// 推断类型
var my_dic = ["name": "张三", "age": "18"]
// 指定类型
var my_dic2: [String: String] = ["name": "张三", "age": "18"]
// 指定类型
var my_dic3: Dictionary<String, String> = ["name": "张三", "age": "18"]
// 空字典
var my_dic4 = [:]
```

## 🌲 获取元素

```swift
var name = my_dic["name"]
print(name ?? "") // 张三
```

## 🌲 添加元素

```swift
my_dic["gender"] = "男"
print(my_dic) // ["gender": "男", "age": "18", "name": "张三"]
```

## 🌲 删除元素

```swift
my_dic.removeValue(forKey: "name")
print(my_dic) // ["age": "18"]
```

## 🌲 截取字典

```swift
无法截取或没找到截取方法
```

## 🌲 拼接字典

```swift
// 拼接字典
my_dic.merge(["no": "1"]) { current, new in
    return current
}
// 后面的block用途为当有重复键是保留原有值还是使用新值
my_dic.merge(["no": "1", "name": "5"]) { old, new in
    return new
}
```

## 🌲 字典转数组

```swift
let keys = my_dic.keys
let values = my_dic.values
print(keys) // ["name", "age"]
print(values) // ["张三", "18"]
```


# 🍎 元组

## 🌲 定义元组

```swift
// 数组类型元组
var my_tuple = (0, "66", 2)
// 字典类型元组
var my_tuple2 = (name: "张三", age: 18)
```

## 🌲 获取元素

```swift
# 通过下标获取 下标从0开始
print(my_tuple.0) // 0
print(my_tuple2.name) // 张三
```

## 🌲 修改元素

```swift
// 数组类型元组修改
var my_tuple = (0, "66", 2)
my_tuple.0 = 1
print(my_tuple)
// 字典类型元组修改
var my_tuple2 = (name: "张三", age: 18)
my_tuple2.name = "李四"
print(my_tuple2)
```

## 🌲 分解元组

```swift
let (a, b, c) = my_tuple
print(a) // 0
print(b) // 66
print(c) // 2
let (a, b) = my_tuple2
print(a) // 张三
print(b) // 18
```

## 🌲 添加元素

```swift
元组不能修改, 所以无法添加
```

## 🌲 删除元素

```swift
元组不能修改, 所以无法删除
```

## 🌲 截取元素

```swift
无法截取

my_tuple[0...1]
// Cannot access element using subscript for tuple type '(Int, String, Int)'; use '.' notation instead
```

## 🌲 拼接元组

```swift
无法拼接
```

# 🍎 集合

## 🌲 定义集合

```swift
var my_set: Set = [1, 2, 3, 1] // [3, 2, 1] 无序
var my_set2 = Set([1, 2, 3, 2]) // [2, 1, 3] 无序
```

## 🌲 获取元素

```python
# 集合不能获取元素 但可以先转成数组再操作
var my_arr = my_set.sorted()
print(my_arr[0])
```

## 🌲 添加元素

```swift
my_set.insert(4)
print(my_set) // [3, 1, 4, 2] 无序
```

## 🌲 删除元素

```swift
my_set.remove(1)
print(my_set) // [3, 4, 2] 无序
```

## 🌲 截取元素

```swift
let my_arr = Array(my_set)
print(my_arr)
print(type(of: my_arr))
print(my_arr[0...0]) // [1] 或 [2] 或 [3] 因为转化成数组并不是固定的顺序
```

## 🌲 拼接集合

```swift
var my_set: Set = [1, 2, 3]
var my_set2 = Set([1, 2, 3, 4])
// 交集
print(my_set.intersection(my_set2)) // [1, 2, 3] 无序
// 并集
my_set.formUnion(my_set2)
print(my_set) // [1, 2, 3, 4] 无序
// 补集 前者补给后者的值 - - 颠倒过来后返回是 [] 因为没有可补的
print(my_set2.subtracting(my_set)) // [4]
```

# 🍎 枚举

## 🌲 定义

我们使用`enum`来定义枚举

```swift
enum TestColor {
    case Red
    case Yellow
    case Blue
}
```

## 🌲 使用

### 🌸  条件判断

我们可以使用`if`来判断

```swift
let color = TestColor.Red
if color == .Red {
	print("是红色的")
}
```

也可以使用switch来判断

```swift
switch color {
case .Red:
	print("是红色的")
	break
case .Yellow:
	print("是黄色的")
	break
case .Blue:
	print("是蓝色的")
	break
}
```

### 🌸  传参

我们可以把枚举作为函数的参数传递进去

```swift
func hello(color: TestColor) {
	print(color)
}

hello(color: TestColor.Red)
或
hello(color: .Red)
```

## 🌲 原始值

有时候我们希望让枚举保存更多的值, 这就用到原始值了

```swift
enum TestColor: String, RawRepresentable {
    case Red = "红色"
    case Yellow = "黄色"
    case Blue = "蓝色"
}
```

使用`rawValue`来使用原始值

```swift
func hello(color: TestColor) {
	print(color.rawValue) // 红色
}

hello(color: .Red)
```


## 🌲 结构体枚举

有时候使用rawValue枚举存储的信息也有限, 那我们就要使用结构体枚举了

```swift
struct TestColor: Equatable, Hashable {
    public static let Red = TestColor(value1: "红色", value2: "abc")
    public static let Yellow = TestColor(value1: "黄色", value2: "def")
    public static let Blue = TestColor(value1: "蓝色", value2: "ghi")
    
    public let value1: String
    public let value2: String
    init(value1: String, value2: String) {
        self.value1 = value1
        self.value2 = value2
    }
}
```

利用结构体我们就可以让枚举储存更多的参数了

```swift
func hello(color: TestColor) {
	print(color.value1, color.value2)
}

hello(color: .Red)
```

# 🍎 条件语句

## 🌲 if

```swift
let a = 1
// 如果a等于1 执行条件
if a == 1 {
    print("a等于1")
}
```

## 🌲 if - else if

```swift
let a = 2
// 如果a等于1执行1 如果a等于2执行2
if a == 1 {
    print("a等于1")
} else if a == 2 {
    print("a等于2")
}
```

## 🌲 if - else if - else

```python
let a = 3
# 如果都不等 执行else
if a == 1 {
    print("a等于1")
} else if a == 2 {
    print("a等于2")
} else {
    print("a既不等于1也不等于2")
}
```

## 🌲 switch-case

和if写法类似

```swift
let a = 3

switch a {
case 1:
	print("a等于1")
	break
case 2:
	print("a等于2")
	break
default:
	print("a既不等于1也不等于2")
	break
}
```

# 🍎 循环语句

## 🌲 for

```swift
for i in [1, 2, 3] {
    print(i) // 1 2 3
} 
    
for i in 0...3 {
    print(i) // 0 1 2 3
}

[1, 2, 3].forEach({item in
    print(item)
}) // 1 2 3
```

## 🌲 while

```swift
var i = 0
while i < 3 {
    print(i) // 0 1 2
    i += 1
}
```

# 🍎 Map

## 🌲 基础用法

```swift
let arr = [1, 2, 3]
let result = arr.map({ $0 * 2 })
print(result)

// 或

let arr = [1, 2, 3]
let result = arr.map({ return $0 * 2 })
print(result)

// 或

let arr = [1, 2, 3]
let result = arr.map({ a in
	return a * 2
})
print(result)
```


# 🍎 闭包

闭包又名`Block`是一个匿名函数

## 🌲 无参数无返回值

```swift
let testBlock = { () -> Void in
    print("无参数无返回值")
}
// 或 如果没有返回值的时候 我们可以省略 '-> Void'
let testBlock = { () in
    print("无参数无返回值")
}
// 或 我们直接省略 '() -> Void in' 
let testBlock = {
    print("无参数无返回值")
}

// 执行
testBlock()

// 直接执行: 末位加括号 下文略
{
    print("无参数无返回值")
}()
```

## 🌲 有参数无返回值(单参数, 多参数)

```swift
// 指定类型
let testBlock = { (a: String) -> Void in
    print(a)
}
// 或 推导类型
let testBlock = { a in
    print(a)
}
// 或 隐式参数
let testBlock = {
    print($0)
}

// 执行
testBlock("有参数无返回值") // 有参数无返回值

// 多参数 两个或两个以上必须加括号 而且第二个参数类型不可推导 只能定义类型
let testBlock = { (a, b: String) in
    print(a)
}
testBlock(1, "hello") // 123


// 在swift4中可以推导了, 也不用加括号
let testBlock = { a, b in
	print(a, b)
}
testBlock(1, "hello")
```

## 🌲 有参数有返回值

```swift
let testBlock = { (a: String) -> String in
    return a
}
// 或
let testBlock = { a -> String in
    return a
}
print(testBlock("有参数有返回值"))
```

## 🌲 回调

一般回调运用在异步网络请求, 请求成功后返回结果, 我们可以看到醒目的`@escaping`, 这是用来标记block逃逸的, 听着挺唬人, 其实不难理解, 我们把`@escaping`去掉发现它报错了, 因为`asyncAfter`是非阻塞2秒后执行, 那个时候函数已经结束了, 所以swift发明了逃逸, 可以让block在函数结束后可以继续回调, 所以我们需要加上`@escaping`

```swift
func hello(a: String, block: @escaping () -> Void) {
    DispatchQueue.global().asyncAfter(deadline: .now() + 2, execute: {
        block()
    })
}

// 执行
hello(a: "123", block: {
    print("回调结束")
})
// 或
hello(a: "123") {
    print("回调结束")
}

print("执行完毕")

/**
执行完毕
回调结束
*/


// 两个Block参数
func hello(a: String, block: @escaping () -> Void, block2: @escaping () -> Void) {
    DispatchQueue.global().asyncAfter(deadline: .now() + 2, execute: {
        block()
        block2()
    })
}

// 执行 使用方法的时候 Xcode会自动提示这种写法 可以看到第一个block可以省略名字
hello(a: "123") {

} block2: {

}

// Block参数后面还有参数
func hello(a: String, block: @escaping () -> Void, block2: @escaping () -> Void, b: String) {
    DispatchQueue.global().asyncAfter(deadline: .now() + 2, execute: {
        block()
        block2()
    })
}

// 执行 同样Xcode会自动提示这种写法
hello(a: "123", block: {

}, block2: {

}, b: "145456")
```

## 🌲 函数尾随闭包

```swift
func calculate(num1: Int, num2: Int, block: (Int, Int) -> Int) -> Int {
	return block(num1, num2)
}

let value = calculate(num1: 1, num2: 2) {
	$0 + $1
}
print(value) // 3
```

## 🌲 排序

```swift
// 从小到大
let numbers = [1, 2, 5, 4, 3, 6, 8, 7]
let sortNumbers = numbers.sorted(by: { a, b in a < b })
print("numbers - " + "\(sortNumbers)") // [1, 2, 3, 4, 5, 6, 7, 8]
// 或
let numbers = [1, 2, 5, 4, 3, 6, 8, 7]
let sortNumbers = numbers.sorted(by: { return $0 < $1 })
print("numbers - " + "\(sortNumbers)") // [1, 2, 3, 4, 5, 6, 7, 8]
// 或
let numbers = [1, 2, 5, 4, 3, 6, 8, 7]
let sortNumbers = numbers.sorted(by: { $0 < $1 })
print("numbers - " + "\(sortNumbers)") // [1, 2, 3, 4, 5, 6, 7, 8]
// 或
let numbers = [1, 2, 5, 4, 3, 6, 8, 7]
let sortNumbers = numbers.sorted(by: < )
print("numbers - " + "\(sortNumbers)") // [1, 2, 3, 4, 5, 6, 7, 8]

//从大到小 - 修改符号即可
let numbers = [1, 2, 5, 4, 3, 6, 8, 7]
let sortNumbers = numbers.sorted(by: > )
print("numbers - " + "\(sortNumbers)") // // [1, 2, 3, 4, 5, 6, 7, 8]
```

## 🌲 筛选

```swift
let numbers = [1, 2, 5, 4, 3, 6, 8, 7]
let filterNumbers = numbers.filter({ $0 % 2 == 0 })
print(filterNumbers)
```

# 🍎 类和对象

## 🌲 类

```swift
class Person {

}
```

## 🌲 定义成员变量

我们可以通过`var`关键字定义成员变量

```swift
class Person {
    var name: String
    var age: Int
}
```

之后我们发现报错了

```
Class 'Person' has no initializers
```

它说我们的`Person`没有定义初始化方法, 在oc中这样写不会产生错误, 因为swift更加严格, 它默认变量必须是有值的并且不为nil, 我们接着往下看

## 🌲 初始化方法

我们在创建对象的时候调用的方法, 系统会自动给我们创建一个无参数的初始化方法, 同样的我们可以重写该方法, 在类中打`init`就可以自动生成带参数的方法了

```swift
class Person {
  var name: String
  var age: Int
  // 初始化方法
  init(name: String, age: Int) {
      self.name = name
      self.age = age
  }
}
        
// 初始化

// 方法1
var per = Persion(name: "张三", age: 16)
// 方法2
var per2 = Persion.init(name: "李四", age: 18)
```

## 🌲 定义方法

```swift
class Person {
    var name: String?
    var age: Int = 0

    init(name: String, age: Int) {
        self.name = name
        self.age = age
    }

    func say() {
        print("人说")
    }
  
  	static func say() {
        print("人类说")
    }
}

// 调用
var per = Person(name: "张三", age: 16)
per.say() // 人说
Persion.say() // 人类说
```

## 🌲 继承

继承后我们可以对方法进行改造, 这个操作叫做重写`override`

```swift
class Student: Person {
    override func say() {
        print("学生说")
    }
}

let stu = Student(name: "学生0", age: 18)
stu.say() // 学生说
```

但是要注意的是`static`方法不能重写, 我们重写后发现报错了

```swift
class Student: Person {
    
    override func say() {
        print("学生说")
    }
    
    override static func say() {
        print("学生'类'说")
    }
}
```

报错信息如下

```
Cannot override static method
```

有时候我们是希望可以重写这个类方法, 那我们可以使用`class`关键字来代替`static`, 我们就可以重写父类方法了

```swift
class Person {
    var name: String
    var age: Int
    
    init(name: String, age: Int) {
        self.name = name
        self.age = age
    }
    
    func say() {
        print("人说")
    }
    
    class func say() {
        print("人'类'说")
    }
}

class Student: Person {
    
    override func say() {
        print("学生说")
    }
    
    override class func say() {
        print("学生'类'说")
    }
}
```

可以看到我们把`static`替换成了`class`, 我们来看一下GPT的解释

```
除了重写之外，static 和 class 关键字在使用上还有一些区别：

重写行为：

static 关键字声明的类型方法不能被子类重写，而 class 关键字声明的类型方法可以被子类重写（前提是没有被标记为 final）。
动态调度：

static 关键字声明的类型方法是静态分派的，即在编译时就确定了调用的方法。
class 关键字声明的类型方法是动态分派的，即根据实际类型在运行时确定调用的方法，允许多态性。
继承链上的处理：

如果你在一个子类中使用 class 关键字重写了父类的类方法，而在另一个子类中又使用 static 关键字定义了相同的方法，那么在继承链上调用该方法时会根据实际类型选择合适的方法实现。
```

## 🌲 setter/getter

非常重要的概念哈, 贯穿全局, 所有编程语言几乎都有, 而且也很基础, 意思就是在属性赋值或取值的时候, 系统给你提供了方法可以对他们进行操作, 这会让编程更加灵活, 我们下面就来看一个简单的例子吧

```swift
import Foundation

class Student {
    var name: String
    var age: Int
}
```

### 🌸  默认实现逻辑

我们就用`name`的默认实现来举例子, 在`swift`中如果重写`name`的`setter/getter`方法需要定义一个新的内部成员变量`_name`它用于保存`name`的值,`newValue`是系统的关键字用户获取值, 在oc中是使用一个带参数的`setter`方法来实现了, 在`swift`中变成了隐式参数

```swift
class Person {
    var _name: String = ""
    var name: String {
        set {
            _name = newValue
        }
        get {
            return _name
        }
    }
    var age: Int
    
    init(name: String, age: Int) {
        self.name = name
        self.age = age
    }
}
```

然后编译我们发现出了一个错误

```
'self' used in property access 'name' before all stored properties are initialized
```

它说在所有的存储属性初始化之前调用了`name`, 这应该是swift中的一个规范吧, 那我们只能把`name`放到最后

```swift
init(name: String, age: Int) {
	self.age = age
	self.name = name
}
```

然后我们发现可以了

```swift
let person = Person(name: "张三", age: 18)
print(person.name) // 张三
```

在这里我们可以发现一个浅显的道理, 存储属性重写`setter/getter`方法后就变成非存储属性了, 我们必须定义一个内部变量才能存储它的值

### 🌸  赋值

还有另外一种使用情况, 一般是给model用的, 比如我们在tableViewCell中传入一个model希望它同时对cell进行赋值, swift也给我们提供了这种方式, 在oc中是要重写`setter`方法来赋值的, 在swift中进行了优化, 使用`willSet`来赋值

```swift
class Person {
    var name: String
    init(name: String) {
        self.name = name
    }
}

class CustomTableViewCell: UITableViewCell {
    var model: Person? {
        willSet {
            self.textLabel?.text = newValue?.name
            print(newValue?.name ?? "")
        }
    }
}

let person = Person(name: "张三")
let cell = CustomTableViewCell(style: .default, reuseIdentifier: "reuse")
cell.model = person
```

# 🍎 扩展(Extension)

扩展是编程中一个非常重要的特性, 它是指可以给已经创建好的类进行扩展

## 🌲 添加方法

```swift
class Person {
    var name: String?
}

extension Person {
    func hello() {
        print("hello \(name ?? "")")
    }
}


let person = Person()
person.name = "张三"
person.hello()
```

## 🌲 添加属性

我们尝试添加一个属性, 发现报错

```swift
extension Person {
    
    var age: String?
    
    func hello() {
        print("hello \(name ?? "")")
    }
}
```

意思是扩展不能包含存储属性

```
Extensions must not contain stored properties
```

所以我们只能使用计算属性, 计算属性是一个特殊的属性, 它类似一个没有参数的方法

```swift
extension Person {
    
    var age: String {
        return "18"
    }
    
    func hello() {
        print("hello \(name ?? "")")
    }
}

let person = Person()
person.name = "张三"
person.hello()
print(person.age)
```

## 🌲 重写

扩展中的方法是不能被重写的, 我们来看代码

```swift
class Person {
    var name: String?
}

extension Person {
    
    var age: String {
        return "18"
    }
    
    func hello() {
        print("hello \(name ?? "")")
    }
}

class Student: Person {
    override var age: String {
        return "20"
    }
    
    override func hello() {
        print("student hello \(name ?? "")")
    }
}
```

然后发现报错了

![](images/Pasted%20image%2020230516134118.png)

可以看到错误信息如下

```
Non-@objc property 'age' is declared in extension of 'Person' and cannot be overridden

Non-@objc instance method 'hello()' is declared in extension of 'Person' and cannot be overridden
```

这一点知道就行

# 🍎 协议(Protocol)

oc和swift中的协议类似于java中的接口, 是给对象扩展方法的一种技术

## 🌲 基本用法

我们使用`protocol`关键字来声明协议, 在里面定义一个叫`hello`的方法, 我们让`Person`签署协议, 并在类的内部进行实现

```swift
protocol TestDelegate {
    func hello();
}

class Person: TestDelegate {
    var name: String
    init(name: String) {
        self.name = name
    }
    func hello() {
        print("hello world! \(name)");
    }
}

let person = Person(name: "张三")
person.hello()
```

到这里有人会问了, 这不是脱裤子放屁吗, 我不签署协议也可以增加这个`hello`方法啊, 你说的有道理啊, 但是存在即合理, 我们还有其他的使用方式

## 🌲 协议接收对象

当我们对象签署了协议, 那就可以用协议来代表签署了协议的对象, 我们可以定义一个协议类型的变量`delegate`来接收`Person`对象, 然后使用`delegate`调用`hello`方法

```swift
let delegate: TestDelegate = Person(name: "张三")
delegate.hello()
```

有人会说, 这也没啥用啊, 我用`Person`对象就能调用为啥要变成协议调用, 那我们继续, 这个时候我们创建一个`Student`继承于`Person`, 这个时候也可以使用协议来接收`Student`对象

```swift
protocol TestDelegate {
    func hello();
}

class Person: TestDelegate {
    var name: String
    init(name: String) {
        self.name = name
    }
    func hello() {
        print("hello world! \(name)");
    }
}

class Student: Person {
    override func hello() {
        print("hello world! 学生 \(name)");
    }
}

let delegate: TestDelegate = Person(name: "张三")
let delegate2: TestDelegate = Student(name: "李四")
delegate.hello()
delegate2.hello()
// hello world! 张三
// hello world! 学生 李四
```

我们使用`delegate`分别调用`hello`方法, 发现两次打印是不同的, 我们使用协议运用了多态的原理, 来写代码, 到这里又有人说了, 我使用Person父类接收Person和Student的实例, 仍然能实现这个多态代码, 那协议到底有什么用

我认为有两个作用, 首先是`专门`, 我们使用`专门`的类型来处理`专门`的问题, 比如我们的代码中就是想说一个`hello`, 而如果是使用Person来接收对象, 如果Person里面的方法足够多, 那么就提现不出来这个专门了, 而是大杂烩, 到这里你应该有所感悟了

## 🌲 协议接收类对象

有时候我们想让协议接收类对象, 我们需要这么写

```swift
protocol TestDelegate {
    static func hello();
}

class Person: TestDelegate {
    var name: String
    init(name: String) {
        self.name = name
    }
    
    static func hello() {
        print("class hello world!");
    }
}

let delegate: TestDelegate.Type? = Person.self
delegate?.hello()
```

但是我们使用这个却不能实现多态, 因为`static`方法不允许在子类中重写, 而`Protocol`中又不能使用`class func`, 但是我们可以把思想转变一下, 我们可以把Person中的方法改造成`class`就可以实现我们想要的效果了

```swift
protocol TestDelegate {
    static func hello();
}

class Person: TestDelegate {
    var name: String
    init(name: String) {
        self.name = name
    }
    
    class func hello() {
        print("class hello world!");
    }
}

class Student: Person {
    override class func hello() {
        
    }
}

let delegate: TestDelegate.Type? = Person.self
delegate?.hello()
let delegate2: TestDelegate.Type? = Student.self
delegate2?.hello()
```


# 🍎 可选类型(Optional)

## 🌲 说明

可选类型是`swift`中非常重要的特性, 也是与其他语言对比的优势, oc程序员并不是很理解这个特性, 我们接下来就说一说, `swift中硬性规定`所有的变量都是有初始值的, 所以声明了成员变量就必须有初始化方法, 比如这么声明的类会报错

```swift
class Person {
	var name: String
}
/**
Class 'ViewController.Person' has no initializers
*/
```

我们需要给它加上初始化方法才能够运行

```swift
class Person {
	var name: String
	init(name: String) {
		self.name = name
	}
}
```

但往往某些情况下我们只想要创建一个空对象, 其他的值在创建完成后再慢慢去赋值, 这就导致了那些成员变量不能在初始化方法进行初始化,  所以`swift`引入了可选类型来解决这种问题

## 🌲 使用

我们可以通过在类型后面加上`?`来把属性定义成可选类型, 这样做的意思是, 这个属性的值可以为空

```swift
class Person {
	var name: String?
}
```

使用起来也很简单

```swift
let person = Person()
person.name = "张三"
print(person.name)
// Optional("张三")
```

但是我们发现会有一个警告`Expression implicitly coerced from 'String?' to 'Any'`

## 🌲 拆包

那是因为我们打印的`name`是一个`可选类型`, 实际上的实现原理使用一个`Optional`对象来包裹, 从打印里我们也可以看出来`Optional("张三")`, 我们想使用这个值是需要从`Optional`里取出来的, 这个过程叫做拆包, 也可以叫做解析

### 🌸  默认值拆包

我们可以使用`?? 默认值`的方式来取出一个变量的值, 意思是如果这个值不为空就拿出来, 如果为空就使用默认值代替原值

```swift
print(person.name ?? "我空了")
```

### 🌸 强制拆包

我们可以使用`!`来强制拆包, 但是这会出现一个问题, 就是如果值为空会报错, 所以一般这个操作是建立在你确定值不为nil的情况下的

```swift
let person = Person()
print(person.name!)
// Thread 1: Fatal error: Unexpectedly found nil while unwrapping an Optional value
```

### 🌸 if拆包

```swift
let person = Person()
person.name = "张三"
if let name = person.name {
	print(name)
}
```

### 🌸 调用方法

可选类型在调用方法的时候会与普通的属性不一样, 我们在使用`Optional`属性的时候也需要进行拆包, 正常书写的时候`Xcode`就会给我们带上, 如果`name`为空这个方法就不会进行调用

```swift
let person = Person()
person.name = "张三"
person.name?.append("123")
print(person.name ?? "")

我们可以看到可选类型的变量执行方法的时候都要加上? 意思是如果变量为空 那么就不继续执行后面的方法
```

### 🌸 自动拆包

所谓自动拆包就是我们在定义变量的时候把`?`替换为`!`, 当我们使用变量的时候编译器会自动帮我们把值取出来, 如果值为空会造成崩溃, 所以使用它的变量要确定不为空

```swift
class Person {
	var name: String!
}
let person = Person()
person.name = "张三"
person.name.append("123")
print(person.name ?? "")
```

我们可以看到, 使用`name.方法`的时候不需要使用`?`来进行拆包了



## 🌲 类型不符

```swift
{"name": "张三", "age": "aaa"}

let dic: Dictionary<String, Any>? = try JSONSerialization.jsonObject(with: data ?? Data(), options: .fragmentsAllowed) as? Dictionary

class Student {
    var name: String?
    var age: Int?
}

let stu = Student()
stu.age = dic?["age"] as? Int
print(stu.age) // nil

可以看到age是Int类型的, 但是json中age是String, 这里使用 as? 来进行转换, 如果发现类型不符合, 会自动将变量置为nil来保证变量不出错, 因为在可选类型中nil是安全的

如果使用 as! 进行转换 则必须保证类型正确 否则就会崩溃
stu.age = dic?["age"] as! Int // Thread 2: signal SIGABRT
```

# 🍎 类型转换

## 🌲 String & Int / Double/ Decimal

```swift
// String -> Int
let my_str = "123"
let my_int = Int(my_str)
print(my_int ?? "") // 123

// Int -> String
let my_int = 123
let my_str = String(my_int)
print(my_str) // 123

// String -> Double 
let my_str = "123"
let my_double = Double(my_str)
print(my_double ?? "") // 123.0

// Double -> String
let my_double = 123.0
let my_str = String(my_double)
print(my_str)

// String -> Decimal
let my_str = "123"
let my_decimal = Decimal(string: my_str)
print(my_decimal ?? "")

// Decimal -> String
let my_decimal: Decimal = 123
let my_str = String(format: "%.0lf", Double(truncating: my_decimal as NSNumber))
print(my_str)
```

## 🌲 String & Array

```swift
// String -> Array
let my_str = "123"
let my_arr = Array(my_str) // ["1", "2", "3"]

// Array -> String
let my_arr = ["1", "2", "3"]
// 无缝拼接
print(my_arr.joined()) // 123
// 分隔符拼接
print(my_arr.joined(separator: ",")) // 1,2,3
```

# 🍎 类型转换

`swift`中类型转换使用`as`, 这里有三种用法

## 🌲 as

顾名思义就是顺转, 比如所有类型都可以转为`Any`, 我们就可以使用as来进行转换

```swift
let str = "123"
let any = str as Any
print(any)
```

## 🌲as?

顾名思义就是`问转`, 意思是有可能转换不成功, 如果不成功就置为nil, 比如我们any转化为string就有可能不成功, 因为它的类型太广泛了, 有可能是字典, 你能用一个字典转化成字符串吗, 肯定不能, 所以这里编译器自动提示我们需要`问转`, 如果不成功就转化成nil

```swift
let str2 = any as? String
print(str2 ?? "") // "123"

let dic = any as? Dictionary<String, Any>
print(dic ?? "") // ""
```

## 🌲as!

顾名思义就是`强转`, 转换完毕会自动拆包, 如果any为nil或者类型不对会直接崩溃

```swift
let str2 = any as! String
print(str2) // "123"

let dic = any as? Dictionary<String, Any>
print(dic ?? "") // 崩溃
```

# 🍎 泛型

## 🌲 数组泛型

所谓泛型就是限定参数的类型, 从而在编译过程中就能由编译器检查出错误

```swift
// 一般定义数组是这样定义的
var arr = [1, 2, 3]
print(arr)
// 你可能没有注意, 这其中就包含了泛型, 我们把数组打印出来
print(type(of: a)) 
// Array<Int>
// 可以看到Array旁边是一个<Int>, 这表明了数组只能装Int类型, 这个类型是swift自动根据其内容推到出来的, 当我们往数组中添加不符合的类型的时候, 编译器就会报错, 如
a.append("4")
// No exact matches in call to instance method 'append'
// 很明显在泛型是Int的时候, 往数组里添加String是不被允许的

// 创建数组的时候也可以直接指定泛型
var a1 = Array<String>()
```

## 🌲 字典泛型

```swift
// 与数组泛型不同的是, 字典泛型需要传递两个参数
var d = Dictionary<String, String>()
d["name"] = "zhangsan"
// 我们尝试添加一个数字
d["name"] = 1
// Cannot assign value of type 'Int' to type 'String?'
```

## 🌲 函数泛型

```swift
func add<T>(a: T, b: T) {
    
}
```

# 🍎 反射

```swift
var mirror: Mirror? = Mirror(reflecting: stu);
repeat {
    mirror?.children.forEach({child in
        print(child.label ?? "")
        print(type(of: child.value))
    })
    mirror = mirror?.superclassMirror ?? nil
} while (mirror != nil)

/**
id
Optional<String>
name
Optional<String>
age
Int
*/
```

# 🍎 GCD

## 🌲 主队列 / 全局队列

```swift
// 主队列
DispatchQueue.main
// 全局队列
DispatchQueue.global()

执行任务是用抛的方式 把xx任务抛到某个队列中 队列会分配合适的线程去执行任务
- 抛到主队列中的任务由主线程执行
- 抛到全局队列中的任务 可能由主线程执行 也可能由子线程执行
```

## 🌲 线程阻塞

```swift
即主线程中执行`耗时任务`或`等待`

`耗时任务`: 例如从1循环到1亿
`等待`: 例如 sleep(5)
```

## 🌲 串行并行

```swift
串行: 按顺序执行, 排在前面的任务先执行
并行: 同时执行, 执行结束的顺序不确定

常见的串行队列 没错就是主队列
DispatchQueue.main 

常见的并行队列 全局队列
DispatchQueue.global()
```

## 🌲 同步和异步

```
sync: 同步
async: 异步

同步会暂停当前任务, 立即执行`新任务`
异步不会暂停当前任务, 会在其他同步任务执行之后执行
```

## 🌲 死锁理论

```
我自研了一套死锁理论来分析该问题, 想要分辨死锁, 需要符合2条
1.同步在串行队列中执行任务
2.该串行队列中存在其他没执行完的任务

这两点得出的结论就是 串行队列的机制是导致死锁的根本问题 串行队列中互相等待的任务会导致死锁
```

## 🌲 主队列抛入任务(同步, 死锁)

我们看下面的代码首先是使用`DispatchQueue.main`获取了主队列, 然后使用sync同步执行任务, 那么结果是什么?

```swift
override func viewDidLoad() {
	super.viewDidLoad()
	
	DispatchQueue.main.sync {
		print("主队列抛入任务(同步)")
		print(Thread.current)
	}
	print("执行完毕")
}

/**
结果是发生崩溃
原因: 死锁
Thread 1: EXC_BAD_INSTRUCTION (code=EXC_I386_INVOP, subcode=0x0)
*/

我们使用死锁理论来分析一下, 首先我们要知道`viewDidLoad`就是在串行队列main中的, 所以串行队列中存在没执行完的任务, 然后我们可以看到使用的是`sync`同步执行任务, 在它执行时我们的`viewDidLoad`并没有执行完, 它会暂停当前的程序优先执行`block`中的代码, 然而`block`要等待`viewDidLoad`执行完再执行, 而`viewDidLoad`也在等待`block`执行完, 相互等待, 所以会造成死锁
```

## 🌲 主队列抛入任务(异步)

同样的我们使用`DispatchQueue.main`获取主队列, 然后在里面异步执行任务, 因为异步任务要等待同步任务完成后再执行, 所以不会发生死锁, 这是一个正常的任务

```swift
DispatchQueue.main.async {
    print("主队列抛入任务(异步)")
  	print(Thread.current)
}

print("执行完毕")

/**
执行完毕
主队列抛入任务(异步)
<NSThread: 0x60000174c380>{number = 1, name = main}
*/

扩展, 如果这个时候向主队列同步抛入任务会不会死锁

DispatchQueue.main.async {
	print("主队列抛入任务(异步)")
	print(Thread.current)
	DispatchQueue.main.sync {
		print("主队列抛入任务(同步)")
	}
	print("主队列任务执行完毕")
}

print("执行完毕")

/**
执行完毕
主队列抛入任务(异步)
<_NSMainThread: 0x600002a48000>{number = 1, name = main}
(lldb) 
崩溃
*/


再次使用死锁理论来分析该问题, 我们要确认`viewDidLoad`执行完毕了, 但是当我们执行`async`任务的过程中, 突然插进来`sync`任务, 首先串行队列中确实存在没执行完的任务`async`, 然后又同步执行了`sync`任务, 导致了串行队列中这两个任务相互等待, 所以会死锁

扩展, 如果这个时候向主队列中异步抛入任务会不会死锁

DispatchQueue.main.async {
	print("主队列抛入任务(异步)")
	print(Thread.current)
	DispatchQueue.main.async {
		print("主队列抛入任务2(异步)")
	}
	print("主队列任务执行完毕")
}
print("执行完毕")

/**
执行完毕
主队列抛入任务(异步)
<_NSMainThread: 0x6000039b8400>{number = 1, name = main}
主队列任务执行完毕
主队列抛入任务2(异步)
*/

答案是不会, 异步任务不会暂停当前执行的任务, 所以不会死锁


扩展, 如果这个时候向该队列抛入耗时的`sleep`任务会不会阻塞线程

DispatchQueue.main.async {
	print("主队列抛入任务(异步)")
	print(Thread.current)
	sleep(10)
	print("睡眠完毕")
}

print("执行完毕")

/**
执行完毕
主队列抛入任务(异步)
<_NSMainThread: 0x6000030b0080>{number = 1, name = main}
睡眠完毕
*/

引入阻塞理论来分析该问题, 想要分辨阻塞, 需要符合一点
1.主线程执行耗时任务

我们可以看到打印中的线程确实是主线程, 并且`sleep(10)`是一个耗时任务, 所以会阻塞
```

## 🌲 全局队列抛入任务(同步)

注意全局队列是要用`global()`的, 因为main是`var`, 而global是`func`, 而且细心的你也许会发现, 把任务同步抛入全局队列实际上是由主线程来执行的, 主线程会切换到全局队列中同步执行任务, 又因为该同步任务不是在串行队列中执行的, 不会发生相互等待

```swift
DispatchQueue.global().sync {
    print("全局队列抛入任务(同步)")
    print(Thread.current)
}

print("执行完毕")

/**
全局队列抛入任务(同步)
<NSThread: 0x60000039c240>{number = 1, name = main}
执行完毕

我们会发现一个奇怪的现象就是全局队列中的同步任务是由主线程执行的, 不应该是子线程吗, 从而我们可以得出一个结论, 全局队列里某些任务是可以由主线程执行的
*/

扩展, 如果在这个基础上, 向主队里抛入同步任务会不会死锁呢

DispatchQueue.global().sync {
	print("全局队列抛入任务(同步)")
	print(Thread.current)
	DispatchQueue.main.sync {
		print("主队列抛入任务(同步)")
		print(Thread.current)
	}
	print("全局队列任务执行完毕")
}
print("执行完毕")

/**
全局队列抛入任务(同步)
<_NSMainThread: 0x600002fa4440>{number = 1, name = main}
*/

答案是会死锁, 虽然`sync`没有在串行队列中, 但是串行队列中存在没执行完的任务`viewDidLoad`, 我们又在主队列中执行了`sync`, 所以导致和`viewDidLoad`任务发生相互等待, 虽然这次情况比上次复杂, 但是仔细理解一下还是可以的

扩展, 如果在这个基础上, 向全局队列里抛入同步任务呢, 会死锁吗

DispatchQueue.global().sync {
	print("全局队列抛入任务(同步)")
	print(Thread.current)
	DispatchQueue.global().sync {
		print("全局队列抛入任务2(同步)")
		print(Thread.current)
	}
	print("全局队列任务执行完毕")
}
print("执行完毕")

/**
全局队列抛入任务(同步)
<_NSMainThread: 0x1007080a0>{number = 1, name = main}
全局队列抛入任务2(同步)
<_NSMainThread: 0x1007080a0>{number = 1, name = main}
全局队列任务执行完毕
执行完毕
*/

答案是不会死锁, 首先虽然串行队列中存在没执行完的任务`viewDidLoad`, 但是我们后续并没有在串行队列main中抛入同步任务, 而是抛入了`global`中, 所以不满足第二条, 不会死锁
```

## 🌲 全局队列抛入任务(异步)

```swift
DispatchQueue.global().async {
    print("全局队列抛入任务(异步)")
  	print(Thread.current)
}

print("执行完毕")

/**
执行完毕
全局队列抛入任务(异步)
<NSThread: 0x600000e57240>{number = 3, name = (null)}
*/

扩展, 如果在这个基础上, 向主队里抛入同步任务会不会死锁呢

DispatchQueue.global().async {
	print("全局队列抛入任务(异步)")
	print(Thread.current)
	DispatchQueue.main.sync {
		print("主队列抛入任务(同步)")
	}
	print("全局队列任务执行完毕")
}
print("执行完毕")

/**
执行完毕
全局队列抛入任务(异步)
<NSThread: 0x600000742f80>{number = 6, name = (null)}
主队列抛入任务(同步)
全局队列任务执行完毕
*/

我们当场就能看出来`async`是异步的, 打印出`执行完毕`证明`viewDidload`执行完了, 所以主队列中没有任务, 之后无论执行什么任务自然不会发生等待, 所以不会死锁

扩展, 如果在里面`sleep` 3秒, 这个会阻塞吗

DispatchQueue.global().async {
	print("全局队列抛入任务(异步)")
	print(Thread.current)
	sleep(3)
	print("睡眠结束")
}

print("执行完毕")

/**
执行完毕
全局队列抛入任务(异步)
<NSThread: 0x600002dbfcc0>{number = 7, name = (null)}
睡眠结束
*/


引入阻塞理论来分析该问题, 想要分辨阻塞, 需要符合一点
1.主线程执行耗时任务

答案很显然是不会阻塞的, 为什么, 首先看线程, 主线程执行耗时任务了吗, 没有! 本案例完全是由number=7的子线程执行的, 那么可以认为是永远都不会阻塞, 如果你调研出主线程以外的线程在任何情况下执行任务发生了阻塞, 请推翻我, 谢谢
```

## 🌲 连续全局队列任务(异步)

```swift
for i in 1...5 {
    DispatchQueue.global().async {
        print("子线程异步任务\(i)")
    }
}

print("执行完毕")

> 顺序不固定

/**
执行完毕
子线程异步任务2
子线程异步任务3
子线程异步任务4
子线程异步任务5
子线程异步任务1
/**

因为全局队列是个并行队列
```

## 🌲 连续全局队列任务(同步)

```swift
for i in 1...5 {
    DispatchQueue.global().sync {
      	// 全局队列的同步在主线程执行
        print("主线程同步任务\(i)")
    }
}

print("执行完毕")

> 顺序固定

// 主线程同步任务1
// 主线程同步任务2
// 主线程同步任务3
// 主线程同步任务4
// 主线程同步任务5
// 执行完毕
```

## 🌲 自定义并行队列

```swift
let queue = DispatchQueue(label: "my_concurrent_queue", qos: .default, attributes: .concurrent, autoreleaseFrequency: .inherit, target: nil)

queue.sync {
	print(Thread.current)
}

label 标签 用于给队列起名

qos 优先级
- .userInteractive 需要用户交互的, 优先级最高, 和主线程一样
- .userInitiated 即将需要, 用户期望优先级, 优先级高比较高
- .default 默认优先级
- .utility 需要执行一段时间后, 再通知用户, 优先级低
- *.background 后台执行的, 优先级比较低
- *.unspecified 不指定优先级, 最低

attributes 队列类型 只提供了两个选项
- .concurrent 并行
- .initiallyInactive 崩溃
我们会发现没有串行, 但是我们可以使用下面的代码代替串行
- DispatchQueue.Attributes() 串行

autoreleaseFrequency 自动释放池频率
- .inherit 表示不确定, 之前默认的行为也是现在的默认值
- .workItem 表示为每个执行的项目创建和排除自动释放池, 项目完成时清理临时对象
- .never 表示GCD不为您管理自动释放池

target 执行任务的目标队列
- 传入nil由自己执行
- 传入 DispatchQueue.main 则相当于在主队列执行任务
```

## 🌲 自定义串行队列

```swift
// 快速创建串行队列
let queue = DispatchQueue(label: "my_serial_queue");

for i in 0...100 {
    queue.async {
        print("\(i)");
    }
}

串行队列即使异步执行也是串行的
// 0
// 1
// ...
// 100

扩展, 如果在`viewDidLoad`中向串行队列同步抛入任务会怎样

let queue = DispatchQueue(label: "my_serial_queue");
queue.sync {
	print(Thread.current)
}

/**
<_NSMainThread: 0x600000c585c0>{number = 1, name = main}
*/

答案是会正常执行, 有人会问了, 这不会死锁吗? 那我们分析一下, 首先在串行队列中执行同步任务这个坐实了, 但是在同一个串行队列中并没有其他任务在执行, 所以不符合第二条, 所以不会死锁

那么再来一个, 这样会不会死锁?

let queue = DispatchQueue(label: "my_serial_queue");
queue.async {
	print(Thread.current)
	queue.sync {
		print(Thread.current)
	}
}

答案是会, 很显然这两个任务都是在串行队列里面执行的, 而且`queue.async`和`queue.sync`发生了相互等待
```

## 🌲 延时(After)

```swift
DispatchQueue.main.asyncAfter(deadline: .now() + 3, execute: {
    print(Thread.current)
    print("延迟3s")
})

/**
<NSThread: 0x600000ef83c0>{number = 1, name = main}
延迟3s
*/

DispatchQueue.global().asyncAfter(deadline: .now() + 3, execute: {
    print(Thread.current)
    print("延迟3s")
})

/**
<NSThread: 0x600000eb04c0>{number = 5, name = (null)}
延迟3s
*/
```

## 🌲 取消队列

```swift
class SwiftViewController: ZYKitBaseViewController {
    
    let dispatchQueue = DispatchQueue(label: "串行队列")
    
    override func viewDidLoad() {
        super.viewDidLoad()

        dispatchQueue.async {
            for i in 0...10 {
                sleep(1)
                print(i)
            }
        }
        
        dispatchQueue.async {
            for i in 0...10 {
                sleep(1)
                print(i)
            }
        }
        
        DispatchQueue.main.asyncAfter(deadline: .now() + 2) {
            self.dispatchQueue.suspend()
            print("暂停")
        }
    }
}

同样的, 你只能取消还没有开始执行的任务, 已经执行的无法取消

```


## 🌲 分组通知

> 并行同步: 任务同时执行 每个任务中执行的程序是同步按顺序执行的

```swift
let group = DispatchGroup()
let queue = DispatchQueue.main

queue.async(group: group, execute: {
    print(Thread.current)
    print("任务1")
})

queue.async(group: group, execute: {
    print(Thread.current)
    print("任务2")
})

group.notify(queue: queue, execute: {
    print(Thread.current)
    print("任务执行完毕")
})

/**
<NSThread: 0x600002304740>{number = 1, name = main}
任务1
<NSThread: 0x600002304740>{number = 1, name = main}
任务2
<NSThread: 0x600002304740>{number = 1, name = main}
任务执行完毕
*/

let group = DispatchGroup()
let queue = DispatchQueue.global()

queue.async(group: group, execute: {
    print(Thread.current)
    print("任务1")
})

queue.async(group: group, execute: {
    print(Thread.current)
    print("任务2")
})

group.notify(queue: queue, execute: {
    print(Thread.current)
    print("任务执行完毕")
})

/**
<NSThread: 0x600003318cc0>{number = 8, name = (null)}
<NSThread: 0x600003330780>{number = 9, name = (null)}
任务1
任务2
<NSThread: 0x600003330780>{number = 9, name = (null)}
任务执行完毕
*/
```

## 🌲 分组通知2

> 并行异步:  任务同时执行 每个任务中执行的程序有可能是异步的 比如网络请求 说不定什么时间执行完

```swift
let group = DispatchGroup()
let queue = DispatchQueue.global()

group.enter()
queue.async {
    print("简单任务1")
    group.leave()
}

group.enter()
queue.async {
    queue.asyncAfter(deadline: .now() + 3, execute: {
        print("耗时任务2")
        group.leave()
    })
}

group.notify(queue: DispatchQueue.main, execute: {
    print("完成")
})
```

> 并行同步的分组通知可以监测并行同步程序的同时结束 但是对于并行异步程序就不能准确收到执行结束的通知 如果要判断异步的结束 需要使用 enter() 和 leave() 当所有enter被leave之后 程序才会调用notify

## 🌲 信号量

> 信号量wait会阻塞线程 直到收到signal才放开阻塞 利用这一点可以来顺序执行代码 group.enter 底层就是信号量

```swift
let sem = DispatchSemaphore(value: 1)

sem.wait()
DispatchQueue.global().asyncAfter(deadline: .now() + 1, execute: {
    print("任务1")
    sem.signal()
})

sem.wait()
DispatchQueue.global().asyncAfter(deadline: .now() + 1, execute: {
    print("任务2")
    sem.signal()
})

sem.wait()
DispatchQueue.global().asyncAfter(deadline: .now() + 1, execute: {
    print("任务3")
    sem.signal()
})

print("执行完毕")

value: 每次执行几个任务 如果为2 会自动执行 2个wait 后面的代码

/**
任务1
任务2
执行完毕
任务3

注: 你可能会有一个疑问 为什么`执行完毕`会先于`任务3`执行 因为async是异步 同步要比异步执行的快 sem.wait执行后系统会阻塞线程并开始执行后面的代码直到收到sem.signal才放开阻塞
*/

但是上面的写法是在主线程中使用信号量 如果执行耗时任务会造成阻塞 导致界面卡顿 所以一般情况下不这么使用
所以还是在子线程中使用比较好 不会阻塞

// 异步
let sem = DispatchSemaphore(value: 1)

DispatchQueue.global().async {
    sem.wait()
    DispatchQueue.global().asyncAfter(deadline: .now() + 1, execute: {
        print("任务1")
        sem.signal()
    })

    sem.wait()
    DispatchQueue.global().asyncAfter(deadline: .now() + 3, execute: {
        print("任务2")
        sem.signal()
    })

    sem.wait()
    DispatchQueue.global().asyncAfter(deadline: .now() + 1, execute: {
        print("任务3")
        sem.signal()
    })

    sem.wait()
    DispatchQueue.global().asyncAfter(deadline: .now() + 1, execute: {
        print("任务4")
        sem.signal()
    })

    print("执行完毕")
}
```

## 🌲 信号量 - 让程序不结束

```swift
let semaphore = DispatchSemaphore(value: 0).wait(timeout: .now() + 100)
```

# 🍎 Thread

## 🌲 打印当前线程

```swift
print(Thread.current);
```

## 🌲 创建线程方法1

```swift
let thread = Thread(target: self, selector: #selector(hello(a:)), object: "thread1开始")
thread.start();
let thread2 = Thread(target: self, selector: #selector(hello(a:)), object: "thread2开始")
thread2.start();
let thread3 = Thread(target: self, selector: #selector(hello(a:)), object: "thread3开始")
thread3.start();

@objc func hello(a: String) {
	print(a)
	for i in 0...10 {
		print(i)
	}
}
/**
thread3开始
thread2开始
thread1开始
0
1
0
1
*/
```

## 🌲 创建线程方法2

与上述创建线程相似, 但是这个是不需要使用`start()`来启动的

```
Thread.detachNewThread {
	print(Thread.current)
}
```

## 🌲 创建线程方法3

```swift
Thread.detachNewThreadSelector(#selector(self.hello(a:)), toTarget: self, with: "123")
```

## 🌲 监控线程状态

```swift
override func viewDidLoad() {
	super.viewDidLoad()
	let thread = Thread(target: self, selector: #selector(hello(a:)), object: "thread1开始")
	thread.start()
	DispatchQueue.main.asyncAfter(deadline: .now() + 0.5) {
		print("是否正在执行", thread.isExecuting)
		print("是否完成", thread.isFinished)
		print("是否被取消", thread.isCancelled)
	}
}

@objc func hello(a: String) {
	sleep(3)
	print(Thread.current)
	print(a)
	for i in 0...10 {
		print(i)
	}
}
```

## 🌲 线程优先级

`threadPriority`取值范围0~1 默认0.5 越大越先执行

```swift
let thread = Thread(target: self, selector: #selector(hello(a:)), object: "thread1开始")
thread.threadPriority = 0.1;
thread.start()

let thread2 = Thread(target: self, selector: #selector(hello(a:)), object: "thread2开始")
thread2.threadPriority = 0.2;
thread2.start()

let thread3 = Thread(target: self, selector: #selector(hello(a:)), object: "thread3开始")
thread3.threadPriority = 0.3;
thread3.start()

@objc func hello(a: String) {
	// 虽然3的优先级最大 但是逻辑中写了当遇到3的时候就会睡眠一秒 所以1和2会先执行
	if a == "thread3开始" {
		sleep(1)
	}
	print(Thread.current)
	print(a)
	for i in 0...10 {
		print(i)
	}
}
```

## 🌲 取消

```swift
class SwiftViewController: ZYKitBaseViewController {
    var th: Thread? = nil
    override func viewDidLoad() {
        super.viewDidLoad()
        self.th = Thread(target: self, selector: #selector(self.hello), object: "thread1开始")
        self.th?.start()
        
    }
    
    @objc func hello() {
        for i in 0...10 {
            sleep(1)
            print(i)
        }
    }
    
    @IBAction func click(_ sender: Any) {
        self.th?.cancel()
    }
}

经过上述的操作下来 我们发现点击按钮并不能取消任务 原因就是`Thread`无法取消已经开始执行的任务

那是不是这个方法出bug了呢 要怎么解决呢 网上描述了另外一种解决方案

class SwiftViewController: ZYKitBaseViewController {
    var th: Thread? = nil
    override func viewDidLoad() {
        super.viewDidLoad()
        self.th = Thread(target: self, selector: #selector(self.hello), object: "thread1开始")
        self.th?.start()
        
    }
    
    @objc func hello() {
        for i in 0...10 {
            if (self.th?.isCancelled == true) {
                return
            }
            sleep(1)
            print(i)
        }
    }
    
    @IBAction func click(_ sender: Any) {
        self.th?.cancel()
    }
}
```

# 🍎 Operation

## 🌲 创建任务

一般是继承`Operation`重写main方法即可, 输出可见直接使用`start`方法运行由主线程执行的

```swift
class MyOperation : Operation {
	
	init(name: String) {
		super.init()
		self.name = name
	}
	
	override func main() {
		super.main()
		print(Thread.current)
		print(self.name ?? "")
	}
}

let op1 = MyOperation(name: "op1")
op1.start()

/**
<_NSMainThread: 0x600001a8c2c0>{number = 1, name = main}
op1
*/
```

## 🌲 并行队列

一般`Operation`都是跟队列一起使用的, `OperationQueue()`默认创建出的队列是并行队列

```swift
let queue = OperationQueue()
queue.addOperation(MyOperation(name: "op1"))
queue.addOperation(MyOperation(name: "op2"))
queue.addOperation(MyOperation(name: "op3"))

class MyOperation : Operation {
	
	init(name: String) {
		super.init()
		self.name = name
	}
	
	override func main() {
		super.main()
		print(Thread.current)
		print(self.name ?? "")
		for i in 0...10 {
			print(i)
		}
	}
}
    
/**
<NSThread: 0x600001c36380>{number = 4, name = (null)}
<NSThread: 0x600001c56140>{number = 7, name = (null)}
<NSThread: 0x600001c4d740>{number = 6, name = (null)}
op3
op2
op1
*/
```

## 🌲 并行队列2

把任务加入队列也可以使用`block`的方式

```swift
let queue = OperationQueue()

queue.addOperation {
	print(Thread.current)
}

queue.addOperation {
	print(Thread.current)
}

queue.addOperation {
	print(Thread.current)
}

/**
<NSThread: 0x600003f6c500>{number = 3, name = (null)}
<NSThread: 0x600003f42c80>{number = 6, name = (null)}
<NSThread: 0x600003f63900>{number = 5, name = (null)}
*/
```

## 🌲 串行队列

只需要设置`maxConcurrentOperationCount`最大并发数量即可

```swift
let queue = OperationQueue()
queue.maxConcurrentOperationCount = 1;

for i in 0...1000 {
	queue.addOperation {
		print(i)
	}
}
```


## 🌲 取消队列

```swift
let queue = OperationQueue()

queue.addOperation {
	sleep(1)
	print(Thread.current)
}

queue.cancelAllOperations();

与`Thread`一样, `cancelAllOperations`也无法取消已经开始执行的任务, 只能在任务没有执行之前进行取消操作
```

# 🍎 锁

## 🌲 atomic原子性

默认的情况下`swift`是没有原子性的, 多线程操作同一个资源有可能发生崩溃`Thread 27: EXC_BAD_ACCESS (code=1, address=0x7cd8273b6d80)`

```swift
class SwiftViewController: ZYKitBaseViewController {

    var str: String = ""
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        let q = OperationQueue()
        for i in 0...1000 {
            q.addOperation {
                self.str = String(format: "第%ld", i)
                print(self.str)
            }
        }
    }
}
```

解决方案是给`setter`和`getter`加上递归锁, 这也是`synchronized`的底层调用

```swift
class SwiftViewController: ZYKitBaseViewController {

    var _str: String?
    
    var str: String {
        set {
            objc_sync_enter(self)
            _str = newValue
            objc_sync_exit(self)
        }
        get {
            let result: String?
            objc_sync_enter(self)
            result = _str
            objc_sync_exit(self)
            return result ?? ""
        }
    }
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        let q = OperationQueue()
        for i in 0...1000 {
            q.addOperation {
                self.str = String(format: "第%ld", i)
                print(self.str)
            }
        }
    }
}
```

我们也可以使用`defer`来处理, 就不需要借助第三个变量了

```swift
class SwiftViewController: ZYKitBaseViewController {

    var _str: String?
    
    var str: String {
        set {
            objc_sync_enter(self)
            defer {
                objc_sync_exit(self)
            }
            _str = newValue
        }
        get {
            objc_sync_enter(self)
            defer {
                objc_sync_exit(self)
            }
            return _str ?? ""
        }
    }
    
    override func viewDidLoad() {
        super.viewDidLoad()
    }
    
    @IBAction func click(_ sender: Any) {
        let q = OperationQueue()
        for i in 0...1000 {
            q.addOperation {
                self.str = String(format: "第%ld", i)
                print(self.str)
            }
        }
    }
}
```

## 🌲 atomic属性包装器

我们也可以使用属性包装器来解决原子性问题

```swift
@propertyWrapper
class AtomicValue<T> {
    private let lock = NSLock()
    private var value: T
    
    init(default: T) {
        self.value = `default`
    }
    
    var wrappedValue: T {
        get {
            self.lock.lock()
            defer { self.lock.unlock() }
            return value
        }
        
        set {
            self.lock.lock()
            defer { self.lock.unlock() }
            value = newValue
        }
    }
}

class SwiftViewController: ZYKitBaseViewController {
    
    @AtomicValue(default: "")
    var str: String
    
    override func viewDidLoad() {
        super.viewDidLoad()
    }
    
    @IBAction func click(_ sender: Any) {
        let q = OperationQueue()
        for i in 1 ... 1000 {
            q.addOperation {
                self.str = String(format: "第%ld个", i)
                print(self.str)
            }
        }
    }
}
```

# 🍎 async/await

## 🌲 定义

我们可以在函数名称后面加上async来表示异步方法

```swift
func test() async {
	print("hello world!")
}
```

## 🌲 使用

异步方法在`swift`里需要写在`Task`的括号内

```swift
override func viewDidLoad() {
	super.viewDidLoad()
	Task {
		print(Thread.current)
		await self.test()
	}
	print("over")
}

func test() async {
	print(Thread.current)
	print("hello world!")
}

/**
over
<_NSMainThread: 0x600002b6c540>{number = 1, name = main}
<_NSMainThread: 0x600002b6c540>{number = 1, name = main}
hello world!
*/
```

我们可以在上面输出上看到一下两点

1.`Task`里面的任务是异步的, 因为`over`先打印出来了
2.`Task`里面的任务是由主线程执行的, 异步方法里的任务也是在主线程执行的

# 🍎 异常处理

## 🌲 throw

```swift
enum TestError: Error {
  // 异常1
  case one
  // 异常2
  case two
  // 异常3 带参数Int
  case three(Int)
  // 异常4 带参数String
  case four(String)
}

let a = 1
if a == 1 {
    throw TestError.one
}

// Swift/ErrorType.swift:200: Fatal error: Error raised at top level: playground_cmd_13.TestError.one
```

## 🌲 函数throw

```swift
// 函数后必须用throws
func hello() throws {
    let a = 1
    if a == 1 {
        throw TestError.one
    }
}

func hello() throws {
    let a = 1
    if a == 1 {
        throw TestError.three(1)
    }
}

func hello() throws {
    let a = 1
    if a == 1 {
        throw TestError.four("异常4")
    }
}

// 使用

// 有异常就崩溃
try hello()

// 有异常走catch不崩溃
do {
    try hello()
} catch let err {
    print(err)
}

// 单独catch某个异常
do {
    try hello()
} catch TestError.three(let err) {
    print(err)
}

// 注: 这种情况下未捕获的异常仍然会崩溃 除非在后面加上所有类型的catch
do {
    try hello()
} catch TestError.four(let err) {
    print(err)
} catch let err {
  	// 捕获所有异常
    print(err)
}

// 有异常不崩溃
try? hello()

// 强try 有异常崩溃
try! hello()
```

## 🌲 try

首先`try`是捕获异常常用的关键字, 跟我们上班的例子一样, 当我们要使用的方法结尾有`throw`关键字, 就说明方法会抛出异常, 编译器会自动提示我们加上try关键字, 写法如下

```swift
let data = try Data(contentsOf: URL(string: "https://www.baidu.com")!)
print(data) // 2443 bytes
```

我们发现代码可以顺利运行, 但是这只是正常情况下, 我们编程中会遇到很多异常情况, 如下

```swift
let data = try Data(contentsOf: URL(string: "12312321")!)
print(data)
```

当我们的URL不是一个有效的网址的时候它就会报错崩溃

```
Thread 1: Fatal error: Error raised at top level: Error Domain=NSCocoaErrorDomain Code=256 "The file “12312321” couldn’t be opened." UserInfo={NSURL=12312321}
```

所以我们需要使用捕获异常, 而swift中捕获异常的结构使用的是`do...catch`, 我并不能喜欢这种写法, 因为与主流`try...catch`公式不符合, 增加了记忆复杂性, 写法如下

```swift
do {
    let data = try Data(contentsOf: URL(string: "1231231")!)
    let str = String(data: data, encoding: .utf8)
    debugPrint(str ?? "")
} catch {
    debugPrint(error)
}
```

我们会发现程序不再崩溃, 取而代之的是控制台输出我们的错误, 这就是捕获异常, 捕获完成后就不会出现崩溃了

## 🌲 try?

上面的写法虽然全面, 但是有一个非常大的问题, 啰嗦!, 我们在开发的过程中有些异常并不关心, 不行就置空, 那么`try?`诞生了, 意思是捕获异常并使用optional可选类型包装值, 写法如下

```swift
let data = try? Data(contentsOf: URL(string: "1231231")!)
print(data ?? Data())
```

配合`grard`使用可以直接拆包里面的值, 如果为nil就return

```swift
guard let data = try? Data(contentsOf: URL.init(string: "https://www.baidu.com")!) else {
    return
}
let str = String(data: data, encoding: .utf8)
debugPrint(str ?? "")
```

## 🌲 try!

这个就比较强硬了, 出异常直接崩溃, 可以说在swift编程中用了`try!`的地方一般情况下, 开发者都有极大的自信心确认它不会有异常, 这样才会使用

```swift
// `强try` 异常即崩溃
let data = try! Data(contentsOf: URL.init(string: "https://www.baidu.com")!)
let str = String(data: data, encoding: .utf8)
debugPrint(str ?? "")
```

但凡事都有例外, 很多事情都是不可预见的, 所以在不清楚的时候使用`try?`是最保险的, 无非就是拆包比较啰嗦, 但可以保证程序比较安全

# 🍎 Json处理

## 🌲 字典转JSON字符串

我们使用`JSONSerialization.data`方法来把对象转换为`Data`, 再把`Data`转化为`Json字符串`, 由于里面包含了可选类型和异常捕获, 所以写起来会稍微麻烦一些, 但是习惯后就好些了

```swift
let dic: [String : Any]? = ["name": "张三", "age": 18]
do {
	let jsonData = try JSONSerialization.data(withJSONObject: dic ?? [], options: .sortedKeys)
	let jsonString = String(data: jsonData, encoding: .utf8)
	print(jsonString ?? "")
} catch let err {
	print(err)
}

或

let dic: [String : Any]? = ["name": "张三", "age": 18]
let jsonData = try? JSONSerialization.data(withJSONObject: dic ?? [], options: .sortedKeys)
let jsonString = String(data: jsonData ?? Data(), encoding: .utf8)
print(jsonString ?? "")
```

这里主要要讲一下`options`这个枚举

```swift
@available(iOS 5.0, *)
public struct WritingOptions : OptionSet {

	public init(rawValue: UInt)

	// 1.漂亮的印刷品, 意思就是给里面插入空格和换行, 打印出来的是一个有结构感的立体字符串
	public static var prettyPrinted: JSONSerialization.WritingOptions { get }

	
	/* Sorts dictionary keys for output using [NSLocale systemLocale]. Keys are compared using NSNumericSearch. The specific sorting method used is subject to change.
	 */
	@available(iOS 11.0, *)
	// 字典的key会进行排序
	public static var sortedKeys: JSONSerialization.WritingOptions { get }
	// 允许不是Json对象出现, 比如普通字符串 "123"
	public static var fragmentsAllowed: JSONSerialization.WritingOptions { get }
	// 内容里面包含斜杆"/"时不用会添加斜杆转义, 比如</html> 不会转义成 >\/html>
	@available(iOS 13.0, *)
	public static var withoutEscapingSlashes: JSONSerialization.WritingOptions { get }
}
```

### 🌸 prettyPrinted

```swift
let dic: [String : Any]? = ["name": "张三", "age": 18]
let jsonData = try? JSONSerialization.data(withJSONObject: dic ?? [], options: .prettyPrinted)
let jsonString = String(data: jsonData ?? Data(), encoding: .utf8)
print(jsonString ?? "")
/**
{
  "name" : "张三",
  "age" : 18
}
*/

如果使用别的就是这个样子
/**
{"name":"张三","age":18}
*/

```

### 🌸 sortedKeys

为了方便爷们就不写代码了 直接上结果 求别折磨, 我们可以看到"age"排到前面来了

```
{"age":18,"name":"张三"}
```

### 🌸 fragmentsAllowed

这个是需要好好认识一下的属性, 因为很常用, 翻译过来是`允许碎片`, 其实使用它可以解析不是`JSON`结构的东西, 否则遇到点啥字符串就会报错了

```swift
let str = "123"
let jsonData = try? JSONSerialization.data(withJSONObject: str, options: .prettyPrinted)
let jsonString = String(data: jsonData ?? Data(), encoding: .utf8)
print(jsonString ?? "")
/**
2022-10-14 15:39:34.080669+0800 playground_app_swift[20760:313820] *** Terminating app due to uncaught exception 'NSInvalidArgumentException', reason: '*** +[NSJSONSerialization dataWithJSONObject:options:error:]: Invalid top-level type in JSON write'
*/
```

这样写会报错, 因为`String 123`并不是一个`JSON`的标准结构, 标准结构一个是数组, 一个是字典, 我们改成`fragmentsAllowed`来看看

```swift
let str = "123"
let jsonData = try? JSONSerialization.data(withJSONObject: str, options: .fragmentsAllowed)
let jsonString = String(data: jsonData ?? Data(), encoding: .utf8)
print(jsonString ?? "")
/**
"123"
*/
```

那么怎么来看是否是`JSON`结构呢, 我们使用`isValidJSONObject`判断就可以了

```swift
print(JSONSerialization.isValidJSONObject("123"))
print(JSONSerialization.isValidJSONObject(1))
print(JSONSerialization.isValidJSONObject(["name"]))
print(JSONSerialization.isValidJSONObject(["name": "张三"]))
```

### 🌸 withoutEscapingSlashes

这是`iOS13`之后出的一个枚举, 可以防止斜杠被转义, 一般情况用不上, 但是当你使用JSON传递HTML的时候你可能需要用到

```swift
let dic: [String : Any]? = ["html": "<html></html>"];
let jsonData = try? JSONSerialization.data(withJSONObject: dic ?? [], options: .fragmentsAllowed)
let jsonString = String(data: jsonData ?? Data(), encoding: .utf8)
print(jsonString ?? "")
/**
{"html":"<html><\/html>"}
*/
```

当我们使用`.withoutEscapingSlashes`后会发现`/`不会被转义了

```swift
let dic: [String : Any]? = ["html": "<html></html>"];
let jsonData = try? JSONSerialization.data(withJSONObject: dic ?? [], options: .withoutEscapingSlashes)
let jsonString = String(data: jsonData ?? Data(), encoding: .utf8)
print(jsonString ?? "")
/**
{"html":"<html></html>"}
*/
```

那么到这里你可能会问, 这对数据传输有什么影响吗?

我在数据转换前后没有看出来什么影响, 但是这种东西如果要存到后台数据库里可能会十分不雅观, 我个人见解 hhh

### 🌸 传递多个值

啥意思呢, 比如我想既进行`prettyPrinted`又进行`withoutEscapingSlashes`, 我们就可以传过去一个数组

```swift
let dic: [String : Any]? = ["html": "<html></html>"];
let jsonData = try? JSONSerialization.data(withJSONObject: dic ?? [], options: [ .prettyPrinted,.withoutEscapingSlashes])
let jsonString = String(data: jsonData ?? Data(), encoding: .utf8)
print(jsonString ?? "")

let dic2 = try? JSONSerialization.jsonObject(with: jsonString?.data(using: .utf8) ?? Data(), options: .fragmentsAllowed)
print(dic2 ?? "")
/**
{
  "html" : "<html></html>"
}
*/
```

## 🌲 JSON字符串转字典

```swift
let dic2 = try? JSONSerialization.jsonObject(with: jsonString?.data(using: .utf8) ?? Data(), options: .fragmentsAllowed)
print(dic2 ?? "")
```

同样options也有很多

```swift
@available(iOS 5.0, *)
public struct ReadingOptions : OptionSet {

	public init(rawValue: UInt)

	// 解析出的容器可变
	public static var mutableContainers: JSONSerialization.ReadingOptions { get }
	// 解析出的字符串可变
	public static var mutableLeaves: JSONSerialization.ReadingOptions { get }
	// 允许碎片
	public static var fragmentsAllowed: JSONSerialization.ReadingOptions { get }

	// 允许json5标准
	@available(iOS 15.0, *)
	public static var json5Allowed: JSONSerialization.ReadingOptions { get }

	@available(iOS 15.0, *)
	public static var topLevelDictionaryAssumed: JSONSerialization.ReadingOptions { get }

	
	@available(iOS, introduced: 5.0, deprecated: 100000, renamed: "JSONSerialization.ReadingOptions.fragmentsAllowed")
	public static var allowFragments: JSONSerialization.ReadingOptions { get }
}
```

### 🌸 json5Allowed

```swift
let str = "{name: '张三', \"age\": 18}"
let dic2 = try? JSONSerialization.jsonObject(with: str.data(using: .utf8) ?? Data(), options: .json5Allowed)
print(dic2 ?? "")
/**
{
    name = "\U5f20\U4e09";
}
*/
```

## 🌲 JSON字符串转实体

### 🌸 基本使用

一般情况下我们会借助第三方框架去做JSON字符串和实体之间的转换, 但是在`swift4`新增了一个方法`Codable`, 我们只需要将自己的类或结构体遵循这个协议即可, 我们点进去这个协议可以看到`public typealias Codable = Decodable & Encodable`, 其实是两个协议, 可以编码和解码

```swift
class Person: Codable {
    let name: String?
    let age: Int?
    let address: String?
    
    enum CodingKeys: String, CodingKey {
        case name
        case age
        case address
    }
}

let jsonString = """
    {
        "name": "张三",
        "age": 20,
        "address": "Shanghai"
    }
"""

let person = try? JSONDecoder().decode(Person.self, from: jsonString.data(using: .utf8) ?? Data())
print(person?.name ?? "") // 输出 张三
print(person?.age ?? "") // 输出 20
print(person?.address ?? "") // 输出 Shanghai
```

值得注意的是, 一般情况下, swift中的模型都要使用可选类型, 因为这种类型可以允许空值, 否则有可能在转换过程中字符串出现null, 导致转换失败抛出异常

### 🌸 映射

在某些情况下, json中的字段名可能和我们的变量名不同, 这个时候我们就需要进行映射了, 比如我们的JSON是这样

```swift
let jsonString = """
	{
		"name": "张三",
		"age": 20,
		"homeAddress": "Shanghai"
	}
"""
```

这个时候我们的字段名又不想修改, 那么我们可以通过model中创建一个枚举进行映射

```swift
class Person: Codable {
	let name: String?
	let age: Int?
	let address: String?
	
	enum CodingKeys: String, CodingKey {
		case name
		case age
		case address = "homeAddress"
	}
}
```

值得注意的是, 这个枚举的名字必须是`CodingKeys`, 里面所有变量都需要写进来, 否则编译不通过

```swift
let person = try? JSONDecoder().decode(Person.self, from: jsonString.data(using: .utf8) ?? Data())
print(person?.name ?? "") // 输出 张三
print(person?.age ?? "") // 输出 20
print(person?.address ?? "") // 输出 Shanghai
```

使用上没有什么区别

### 🌸 重写decoder方法

如果我们想要更深入的控制转换, 我们就需要重写`decoder`方法

```swift
class Person: Codable {
	let name: String?
	let age: Int?
	let address: String?
	
	required init(from decoder: Decoder) throws {
		let container = try decoder.container(keyedBy: CodingKeys.self)
		name = try? container.decodeIfPresent(String.self, forKey: .name)
		age = try? container.decodeIfPresent(Int.self, forKey: .age)
		address = "苏家屯"
	}
	
	enum CodingKeys: String, CodingKey {
		case name
		case age
		case address = "homeAddress"
	}
}
```

假如我想地址永远是`苏家屯`, 那么就要在这个方法中去写

### 🌸 解析嵌套实体

这个也没什么难点, 我们一起来看看

```swift
class ClassRoom: Codable {
	let persons: [Person]?
}

class Person: Codable {
	let name: String?
	let age: Int?
	let address: String?
	
	required init(from decoder: Decoder) throws {
		let container = try decoder.container(keyedBy: CodingKeys.self)
		name = try? container.decodeIfPresent(String.self, forKey: .name)
		age = try? container.decodeIfPresent(Int.self, forKey: .age)
		address = "苏家屯"
	}
	
	enum CodingKeys: String, CodingKey {
		case name
		case age
		case address = "homeAddress"
	}
}

let jsonString = """
{
  "persons": [
			{
				"name": "张三",
				"age": 20,
				"homeAddress": "Shanghai"
			},
			{
				"name": "李四",
				"age": 20,
				"homeAddress": "Shanghai"
			},
			{
				"name": "王五",
				"age": 20,
				"homeAddress": "Shanghai"
			},
			{
				"name": "赵六",
				"age": 20,
				"homeAddress": "Shanghai"
			}
]
}
"""
let classRoom = try? JSONDecoder().decode(ClassRoom.self, from: jsonString.data(using: .utf8) ?? Data())

print(classRoom?.persons ?? "")
```

### 🌸 高级用法

我们的`JSONDecoder`其实还有很多属性可以使用, 我们下面就来看几个例子, 有时候后台给我们的数据千奇百怪, 加入是没有驼峰命名的, 而我们的代码中规范就是驼峰命名的变量, 这样要一个一个进行映射吗, 显然不是, 我们可以使用`keyDecodingStrategy`属性来允许驼峰命名的解码

```swift
class Person: Codable {
		let name: String?
		let age: Int?
		let address: String?
		let phoneNumber: String?
	}

	let jsonString = """
		{
			"name": "张三",
			"age": 20,
			"address": "Shanghai",
			"phone_number": "123456"
		}
	"""
	
	let decoder = JSONDecoder()
	decoder.keyDecodingStrategy = .convertFromSnakeCase
	let person = try? decoder.decode(Person.self, from: jsonString.data(using: .utf8) ?? Data())
	print(person?.name ?? "") // 输出 张三
	print(person?.age ?? "") // 输出 20
	print(person?.address ?? "") // 输出 Shanghai
	print(person?.phoneNumber ?? "") // 123456
}
```

这个时候如果定义一个叫`phone_number`的变量会怎么样呢? 

```swift
class Person: Codable {
	let name: String?
	let age: Int?
	let address: String?
	let phone_number: String?
	let phoneNumber: String?
}

let jsonString = """
	{
		"name": "张三",
		"age": 20,
		"address": "Shanghai",
		"phone_number": "123456",
	}
"""
```

答案是`phone_number`会解析为空, JSON中的`phone_number`会映射到`phoneNumber`上, 而`phone_number`会因为没有匹配的变量为nil

那如果我们在JSON里面再定义`phoneNumber`会怎样呢

```swift
let jsonString = """
	{
		"name": "张三",
		"age": 20,
		"address": "Shanghai",
		"phone_number": "123456",
		"phoneNumber": "456789"
	}
"""
```

答案是`phone_number`还是没有解析出来, 有此可以看出来, 驼峰解析法只可以将`abc_def`映射为`abcDef`, 但无法反向映射

那么又问了, 这个时候我删掉`phoneNumber`变量会怎么样

```swift
class Person: Codable {
	let name: String?
	let age: Int?
	let address: String?
	let phone_number: String?
}
```

答案是还是不能解析`phone_number`, 所以结论是如果设置了驼峰转换那么带下划线的这种变量将无法接收

### 🌸 缺陷

这个工具也是有缺陷的, 首先如果有不同名变量, 映射的时候要列出所有字段的枚举, 这个暂时还能够容忍, 还有另一个, 就是当JSON中和Model中同名字段类型不同的时候, 这个框架默认是解析不出来, 这会导致线上出现很多问题, 我们一起来看看

```swift
class ResponseObject2: Codable {
    var code: String?
    var message: Int?
    var data: [String: String]?
}

let jsonString = """
		{
			"message": "你好你好",
			"code": "200",
			"data": null
		}
	"""

let res = try? JSONDecoder().decode(ResponseObject2.self, from: jsonString.data(using: .utf8) ?? Data())
debugPrint(res?.message ?? "")
debugPrint(res?.code ?? "")
```

我们可以看到, JSON中的`message`是字符串类型的, 而我们定义的`message`是`Int`类型的, 这个时候`JSONDecoder`解析出来的对象就是一个nil, 按照正常逻辑来说, 其中一个值类型不同大不了就是这个值为空, 但是我们看到它影响到了全局, 这样容错率太低了, 是个棘手的问题, 想解决这个问题, 我暂时没找到简便方法, 但是我们以学习为目的还是看一看, 我们来改写一下, 捕获异常

```swift
class ResponseObject2: Codable {
    
    var code: String?
    var message: Int?
    var data: [String: String]?
    
    required init(from decoder: Decoder) throws {
        let container = try decoder.container(keyedBy: CodingKeys.self)
        code = try? container.decodeIfPresent(String.self, forKey: .code)
        do {
            message = try container.decodeIfPresent(Int.self, forKey: .message)
        } catch let e {
            debugPrint(e)
            message = nil
        }
        data = try? container.decodeIfPresent([String: String].self, forKey: .data)
    }
}
```

这样写我们就能打印出error了

```shell
Swift.DecodingError.typeMismatch(Swift.Int, Swift.DecodingError.Context(codingPath: [CodingKeys(stringValue: "message", intValue: nil)], debugDescription: "Expected to decode Int but found a string/data instead.", underlyingError: nil))
```

意思就是类型错误, 由于我们在上面捕获错误了, 所以现在的效果就是我们想要的, 我们看看打印

```
""
"200"
```

我们发现, 换成了手动处理, 即使类型错误我们也可以不影响其他的值了, 我们也可以换成try?的写法

```swift
required init(from decoder: Decoder) throws {
	let container = try decoder.container(keyedBy: CodingKeys.self)
	code = try? container.decodeIfPresent(String.self, forKey: .code)
	message = try? container.decodeIfPresent(Int.self, forKey: .message)
	data = try? container.decodeIfPresent([String: String].self, forKey: .data)
}
```

和上面的效果是一样的, 值得注意的是, 如果值很多的时候, 使用这个方法是非常不方便的, 所以这里推荐使用其他框架替代, 如kakajson


## 🌲 实体转JSON字符串

### 🌸 基本使用

```swift
let data = try? JSONEncoder().encode(person)
let json = String(data: data ?? Data(), encoding: .utf8)
print(json ?? "")
```

# 🍎 条件编译

有时候DEBUG和RELEASE会执行不同的代码, 所以我们就需要进行条件编译

```swift
#if DEBUG
// do
#else
// else do
#endif
```

# 🍎 画布

## 🌲 CAShapeLayer

CAShapeLayer继承于CALayer, 作用相当于一块画布, 我们可以很方便的在上面画图形, 通过下面的代码可以很方便的创建一个居中的画布

```swift
// 创建画布
let shapeLayer = CAShapeLayer()
// 设置大小
shapeLayer.frame = CGRect(x: 0, y: 0, width: 200, height: 200)
// 设置位置
shapeLayer.position = self.view.center
// 设置背景颜色
shapeLayer.backgroundColor = UIColor.black.cgColor
// 添加到self.view上
self.view.layer.addSublayer(shapeLayer)
```

## 🌲 UIBezierPath

贝塞尔曲线是表达简单或复杂路径的方式, 你可以理解为在画布上画图形

```swift
// 画布上画一个正方形

// 创建画布
let shapeLayer = CAShapeLayer()
// 设置大小
shapeLayer.frame = CGRect(x: 0, y: 0, width: 200, height: 200)
// 设置位置
shapeLayer.position = self.view.center
// 设置背景颜色
shapeLayer.backgroundColor = UIColor.black.cgColor
// 添加到self.view上
self.view.layer.addSublayer(shapeLayer)

// 创建一个四边形
let bezierPath = UIBezierPath(rect: CGRect(x: 10, y: 10, width: 50, height: 50))
// 把四边形放在画布上
shapeLayer.path = bezierPath.cgPath
// 设置填充颜色(四边形颜色)
shapeLayer.fillColor = UIColor.red.cgColor
// 设置线条宽度(边)
shapeLayer.lineWidth = 5
// 设置线条颜色(边)
shapeLayer.strokeColor = UIColor.blue.cgColor
```

很显然这种画图的方式使用两个view也能画出来, 而且比这些来的简单, 那么它存在的意义是什么呢? 它可以画复杂图形, 比如画一个圆

```swift
// 画圆
let bezierPath = UIBezierPath(ovalIn: CGRect(x: 10, y: 10, width: 100, height: 100))
```

可能贝塞尔自带的形状已经不能满足你的需求, 这时候你可以自己绘制图形

```swift
// 绘制三角形
let bezierPath = UIBezierPath()
bezierPath.move(to: CGPoint(x: 10, y: 10))
bezierPath.addLine(to: CGPoint(x: 10, y: 30))
bezierPath.addLine(to: CGPoint(x: 30, y: 30))
bezierPath.addLine(to: CGPoint(x: 10, y: 10))

// 绘制正方形
let bezierPath = UIBezierPath()
bezierPath.move(to: CGPoint(x: 10, y: 10))
bezierPath.addLine(to: CGPoint(x: 10, y: 30))
bezierPath.addLine(to: CGPoint(x: 30, y: 30))
bezierPath.addLine(to: CGPoint(x: 30, y: 10))
bezierPath.addLine(to: CGPoint(x: 10, y: 10))
```

# 🍎 动态成员查找

```swift
@dynamicMemberLookup
class Student {
    subscript(dynamicMember dynamicMember: String) -> String {
        return "默认值"
    }
    subscript(dynamicMember dynamicMember: String) -> Int {
        return 666
    }
    let name = "123"
}

let stu = Student()

print(stu.name as String) // 123
print(stu.age as String) // 默认值
print(stu.age as Int) // 666
```

# 🍎 Apple Swift Packages

苹果官方的`Swift`包管理工具

## 🌲 安装第三方库

`File -> Add Packages...`来添加

![](swift/images/spm_2.png)

## 🌲 卸载第三方库

![](swift/images/spm_3.png)

## 🌲 安装第三方库报错

只能尝试`搬梯子`了

![](swift/images/spm_4.png)

首先复制命令到终端

```
export https_proxy=http://127.0.0.1:7890 http_proxy=http://127.0.0.1:7890 all_proxy=socks5://127.0.0.1:7890
```

然后去项目根目录执行`xcodebuild -resolvePackageDependencies`添加依赖

```
xcodebuild -resolvePackageDependencies
```

之后会出现一个问题

```
xcodebuild: error: Could not resolve package dependencies:
  Dependencies could not be resolved because no versions of 'swift-algorithms' match the requirement 1.0.0..<2.0.0 and root depends on 'swift-algorithms' 1.0.0..<2.0.0.
```

我的解决方案是`卸载出问题的第三方库`然后重新添加, 方法参考上一节

之后就可以使用`xcodebuild -resolvePackageDependencies`正常安装了

```shell
objcat@yuanjun-2 Charts % xcodebuild -resolvePackageDependencies
Command line invocation:
    /Applications/Xcode13.app/Contents/Developer/usr/bin/xcodebuild -resolvePackageDependencies

User defaults from command line:
    IDEPackageSupportUseBuiltinSCM = YES

Resolve Package Graph

Resolved source packages:
  swift-algorithms: https://github.com/apple/swift-algorithms.git @ 1.0.0
  SnapshotTesting: https://github.com/pointfreeco/swift-snapshot-testing @ 1.9.0
  swift-numerics: https://github.com/apple/swift-numerics @ 1.0.2

resolved source packages: swift-algorithms, SnapshotTesting, swift-numerics
```

但是安装后打开项目仍然报错

```
Package.resolved file is corrupted or malformed; fix or delete the file to continue: unsupported schema version 2
```

网上的解决方案是删除`Package.resolved`, 然后从新`reset`, 路径如下

```
/Users/objcat/project/iOS/Charts/Charts.xcworkspace/xcshareddata/swiftpm
```

但是删完之后那个错误不见了 这次是找不到模块

然后我找到了第三方库的下载位置

```
/Users/objcat/Library/Developer/Xcode/DerivedData/Charts-cewjuxhpfptycmhfxacyugqlvizd/SourcePackages/checkouts/swift-algorithms
```

问题暂未解决... 后来直接用pod安装了

# 🍎 关键字

## 🌲 self

self关键字原本指方法所对应的对象, 非静态方法中, 指对象本身, 我们在`ViewDidLoad`中会经常调用, 比如`self.view`就是指视图控制器本身自带的view, 如果在静态方法中指的是当前类对象

在oc的基础上swift引入了大写的`Self`, 它使swift获得了可以快速使用类对象的关键字

## 🌲 inout

我们如果是学习c语言这个关键字可能没看到过, 但是学到指针的时候会讲到一个知识点, 就是函数内部是无法对变量本身做更改的, 需要把指针传入进去才能修改它本身

在swift中它把参数做了更严格的限制, 一旦改动参数本身就会报错, 我们来看一下

### 🌸 普通对象

我们下面就来创建一个`Student`对象来说明一下

```swift
class Student {
    var name: String?
}

var stu = Student()
stu.name = "张三"

func hello(s: Student) {
    s = Student()
}

hello(s: stu)

print(stu.name ?? "无")
```

我写一个函数hello, 把stu当参数传递进去, 然后在函数里面修改这个s, 我们发现爆红了

```
Cannot assign to value: 's' is a 'let' constant
```

swift为了不让我们修改煞费苦心, 它把形参全部改成了let这可以避免很多错误, 那如果我们如果真的想改这个对象呢, 其实也不是不可以, 我们可以使用`inout`关键字, 值得注意的是传递`stu`的时候需要使用取址符

```swift
class Student {
    var name: String?
}

var stu = Student()
stu.name = "张三"

func hello(s: inout Student) {
    s = Student()
    s.name = "李四"
}

hello(s: &stu)

print(stu.name ?? "无")
```

我们发现打印的名字就变成李四了, 说明对象修改成功

还有要说明的就是, 变成let的只是对象本身, 但是它身上的属性是可以随意修改的, 比如在函数里面修改张三的名字为王五, 这种修改是不需要inout的

```swift
class Student {
    var name: String?
}

var stu = Student()
stu.name = "张三"

func hello(s: Student) {
    s.name = "王五"
}

hello(s: stu)

print(stu.name ?? "无")
```

### 🌸 数组/字典

数组/字典如果当做参数传入函数的话与普通对象一样也会变成let本身不能修改, 但由于swift使用关键字来控制数组和字典不能修改这一特性的, 那么就会有一个影响, 就是数组里面的子元素也会跟着不能修改, 这是与对象不一样的, 因为对象可以修改里面的属性

所以我们想做一些操作比如删除数组中的元素还是要使用Inout关键字

```swift
var arr = [1, 2, 3]


func hello(arr: inout Array<Int>) {
    arr.removeAll()
}

hello(arr: &arr)

print(arr)
```


# 🍎 混编

有时候公司项目会用到swift和oc混编, 有些人比较懵, 其实还是比较简单的, 我们接下来就看看

[[iOS] Swift学习文档 框架版](../../../../jianshu/iOS/[iOS]%20Swift学习文档%20框架版/[iOS]%20Swift学习文档%20框架版.md)

# 🍎 构建文档

只要你形式有使用注释的习惯, 那么你就可以使用xcode自带的工具来构建文档, 我们使用`Product -> Build Documentation`来构建文档

构建完成后我们会进入到文档页面

![](images/Pasted%20image%2020230414130343.png)

可以发现, 多出了我们项目的一块区域, 但是我们可以在上面清楚的看到, 之后一些使用swift写的库, 而我们自己写的工具的文档并没有在里面, 我使用xcode13进行构建的, 下面我们再来试一下xcode14看看它对文档有没有优化

# 🍎 新特性

https://developer.apple.com/documentation/new-technologies-wwdc22/
https://developer.apple.com/documentation/new-technologies-wwdc21/