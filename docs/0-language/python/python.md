# 🍎 简介

![](images/Pasted%20image%2020230907120104.jpg)

Python由[荷兰国家数学与计算机科学研究中心](https://baike.baidu.com/item/%E8%8D%B7%E5%85%B0%E5%9B%BD%E5%AE%B6%E6%95%B0%E5%AD%A6%E4%B8%8E%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%A7%91%E5%AD%A6%E7%A0%94%E7%A9%B6%E4%B8%AD%E5%BF%83/53889845?fromModule=lemma_inlink)的[吉多·范罗苏姆](https://baike.baidu.com/item/%E5%90%89%E5%A4%9A%C2%B7%E8%8C%83%E7%BD%97%E8%8B%8F%E5%A7%86/328361?fromModule=lemma_inlink)于1990年代初设计，作为一门叫作[ABC语言](https://baike.baidu.com/item/ABC%E8%AF%AD%E8%A8%80/334996?fromModule=lemma_inlink)的替代品。 [1]  Python提供了高效的高级[数据结构](https://baike.baidu.com/item/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1450?fromModule=lemma_inlink)，还能简单有效地[面向对象](https://baike.baidu.com/item/%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1/2262089?fromModule=lemma_inlink)编程。Python语法和动态类型，以及[解释型语言](https://baike.baidu.com/item/%E8%A7%A3%E9%87%8A%E5%9E%8B%E8%AF%AD%E8%A8%80/8888952?fromModule=lemma_inlink)的本质，使它成为多数平台上写脚本和快速开发应用的[编程语言](https://baike.baidu.com/item/%E7%BC%96%E7%A8%8B%E8%AF%AD%E8%A8%80/9845131?fromModule=lemma_inlink)， [2]  随着版本的不断更新和语言新功能的添加，逐渐被用于独立的、大型项目的开发。 [3] 

Python在各个编程语言中比较适合新手学习，Python解释器易于扩展，可以使用[C](https://baike.baidu.com/item/C/7252092?fromModule=lemma_inlink)、[C++](https://baike.baidu.com/item/C%2B%2B/99272?fromModule=lemma_inlink)或其他可以通过C调用的语言扩展新的功能和[数据类型](https://baike.baidu.com/item/%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B/10997964?fromModule=lemma_inlink)。 [4]  Python也可用于可定制化软件中的扩展程序语言。Python丰富的标准库，提供了适用于各个主要系统平台的[源码](https://baike.baidu.com/item/%E6%BA%90%E7%A0%81/344212?fromModule=lemma_inlink)或[机器码](https://baike.baidu.com/item/%E6%9C%BA%E5%99%A8%E7%A0%81/86125?fromModule=lemma_inlink)。 [4]

# 🍎 官方网站

https://www.python.org/

# 🍎 环境搭建

我们可以直接在官方页面进行下载安装

https://www.python.org/downloads/

![](images/Pasted%20image%2020221213135129.png)

我们也可以去国内镜像源下载, 比如华为

https://mirrors.huaweicloud.com/python/

版本我推荐的是3.6, 3.8

使用3.6一般是用来做图像识别方面的, 因为版本的库可能存在版权问题

# 🍎 IDE

IDE下载

https://download.jetbrains.com.cn/python/pycharm-professional-2021.1.exe

# 🍎 Hello World

```python
print("Hello world!")
```

# 🍎 变量

```python
# 整型
a = 100
# 浮点型
b = 1000.0
# 字符串
c = "Objcat"
# 数组
d = [1, 2, 3]
# 字典
e = {"name": "objcat", "age": "18"}
# 元组
f = (1, 2, 3)
# 集合
g = {1, 2, 3}
```

# 🍎 函数

## 🌲 无参数无返回值

```python
def hello():
    print("hello world!")
```

## 🌲 有参数无返回值

```python
def hello(text):
    print(text)


hello("hello world!")

# 因为没有返回值 所以承接的返回值是None
result = hello("hello world!")
print(result) # None
```

## 🌲 有参数有返回值

```python
def hello(text):
    return text


result = hello("hello world!")
print(result) # hello world!

# 在定义的时候也可以指定返回值类型
def hello(text) -> str:
    return text
```

## 🌲 默认值
```python
# 可以给参数设置默认值, 设置后该参数可以不传
def hello(name="张三"):  
    print("hello", name)  


hello() # hello 张三
```

## 🌲 args和kwargs
```python
# python为开发者提供了两个可选参数 一个是*args(元组) 一个是**kwargs(字典)
def hello(name, *args, **kwargs):  
    print("hello", name,  args, kwargs)  
  
  
hello("张三") # hello 张三 () {}

# 传参
def hello(name, *args, **kwargs):  
    print("hello", name,  args, kwargs)  
  
  
hello("张三", 1, a=1) # hello 张三 (1,) {'a': 1}

# 也就是没有定义的单个参数就会默认装入 args 没有定义的键值对会默认装入 kwargs
# 而且传参顺序不能改变 不能交替传参
```

# 🍎 字符串

## 🌲 定义字符串

```python
my_str = "123"
```

## 🌲 获取字符串

```python
# 字符串可以通过下标来获取字字符串
print(my_str[0]) # 1 
print(my_str[1]) # 2 
```

## 🌲 拼接字符串

```python
# 加号拼接
my_str2 = my_str + "456" # 123456
# 数组拼接
str_arr = ["123", "456"]
my_str2 = "".join(str_arr) # 123456
# format
my_str2 = "%s%s" % ("123", "456") # 123456
# format2
a = 123
my_str2 = f"{a}456" # 123456
```

## 🌲 替换字符串

```python
my_str2 = my_str.replace("1", "12") # 456
```

## 🌲 查找字符串位置

```python
my_str.index("2") # 1
```

## 🌲 遍历字符串

```python
for s in my_str:
    print(s)
```

# 🍎 数组

## 🌲 定义数组

```python
my_arr = [1, 2, 3]
```

## 🌲 获取元素

```python
# 通过下标获取 下标从0开始
print(my_arr[0])
```

## 🌲 获取元素下标

```python
# 使用index方法来获取某一个元素的下标
print(my_arr.index(1)) 
```

如果不存在元素则抛出`ValueError`异常

## 🌲 添加元素

```python
my_arr.append(4) 
print(my_arr) # [1, 2, 3, 4]
```

## 🌲 删除元素

```python
my_arr.remove(4) 
print(my_arr) # [1, 2, 3]
```

## 🌲 截取元素

```python
my_arr2 = my_arr[0:2] # [1, 2]
my_arr2 = my_arr[:2]  # [1, 2]
my_arr2 = my_arr[1:2] # [2]
my_arr2 = my_arr[1:]  # [2, 3]
```

## 🌲 拼接数组

```python
my_arr.extend([4, 5, 6])
print(my_arr) # [1, 2, 3, 4, 5, 6]
```

## 🌲 遍历数组

```python
for i in arr:
	print(i) // 1 2 3
```

## 🌲 数组转元组

```python
tuple(arr)
```

# 🍎 字典

## 🌲 定义字典

```python
my_dic = {"name": "张三", "age": "18"}
```

## 🌲 获取元素

```python
str = my_dic["name"]
print(str) # 张三
```

## 🌲 添加元素

```python
my_dic["gender"] = "男"
print(my_dic) # {'name': '张三', 'age': '18', 'gender': '男'}
```

## 🌲 删除元素

```python
my_dic.pop("name")
print(my_dic) # dic = {"age": "18"}
```

## 🌲 截取字典

```python
无法截取
```

## 🌲 拼接字典

```python
my_dic.update({"gender": "男", "cat": "objcat"})
print(my_dic) # {'name': '张三', 'age': '18', 'gender': '男', 'cat': 'objcat'}
```

# 🍎 元组

## 🌲 定义元组

```python
my_tup = ('1', '2', '3')
```

## 🌲 获取元素

```python
# 通过下标获取 下标从0开始
print(my_tup[0]) # 1
```

## 🌲 添加元素

```python
元组不能修改, 所以无法添加
```

## 🌲 删除元素

```python
元组不能修改, 所以无法删除
```

## 🌲 截取元素

```python
my_tup2 = my_tup[0:2] # (1, 2)
my_tup2 = my_tup[:2]  # (1, 2)
my_tup2 = my_tup[1:2] # (2,)
my_tup2 = my_tup[1:]  # (2, 3)
```

## 🌲 拼接元组

```python
my_tup2 = my_tup + (4, 5, 6) # (1, 2, 3, 4, 5, 6)
```

# 🍎 集合

## 🌲 定义集合

```python
my_set = {1, 2, 3}
```

## 🌲 获取元素

```python
# 集合不能获取元素 但可以先转成数组再操作
my_set = {1, 2, 3}
arr = list(my_set)
print(arr[0]) # 1
```

## 🌲 添加元素

```python
my_set = {1, 2, 3}
my_set.add(4) # {1, 2, 3, 4}
print(my_set) 
```

## 🌲 删除元素

```python
my_set = {1, 2, 3}
my_set.remove(3) 
print(my_set) # {1, 2}
```

## 🌲 截取元素

```python
无法截取
```

## 🌲 拼接集合

```python
my_set = {1, 2, 3}
my_set2 = my_set | {4, 5, 6}
print(my_set2)
```

# 🍎 条件语句

## 🌲 if

```python
a = 1
# 如果a等于1 执行条件
if a == 1:
    print("a等于1")
```

## 🌲 if - else if

```python
a = 1
# 如果a等于1执行1 如果a等于2执行2
if a == 1:
    print("a等于1")
elif a == 2:
    print("a等于2")
```

## 🌲 if - else if - else

```python
a = 1
# 如果都不等 执行else
if a == 1:
    print("a等于1")
elif a == 2:
    print("a等于2") 
else:
    print("a既不等于1也不等于2")
```


# 🍎 循环语句

## 🌲 for

```python
for i in [1, 2, 3]:
    print(i) # 1 2 3
    
for i in range(3):
    print(i) # 0 1 2 - range是从0开始的
```

## 🌲 while

```python
i = 0
while (i < 3):
    print(i) # 0 1 2
    i += 1
```

# 🍎 类和对象

## 🌲 创建类

```
class Person:
    pass
```

## 🌲 定义成员变量

python的成员变量非常的神奇, 类和对象都可以使用它, 分为两种情况

### 🌸 带初始值的成员变量

带初始值的成员变量类是可以直接使用的, 而且类和对象都可以使用它

```python
class Person:
    name = "张三"
    age = 0
    pass

def main():
    # 打印类的成员变量
    print("类的成员变量", Person.name) # 张三
    per = Person()
    # 打印对象的成员变量
    print("对象的成员变量", per.name)  # 张三
    # 修改成员变量
    per.name = "李四"
    print("对象修改后的成员变量", per.name)  # 李四
    Person.name = "王五"
    print("类修改后的成员变量", Person.name)  # 王五

if __name__ == '__main__':
    main()
```

### 🌸 不带初始值的成员变量

不带初始值的成员变量定义需要添加类型

```python
class Person:
    name: str
    age: int
    pass
```

如果直接使用会抛出异常, 所以我们需要给它附上初始值再进行使用

```python
def main():
    Person.name

"""
Traceback (most recent call last):
  File "/Users/objcat/project/Python/test_python/src/playground/test_class/test_class.py", line 15, in <module>
    main()
  File "/Users/objcat/project/Python/test_python/src/playground/test_class/test_class.py", line 13, in main
    Person.name
AttributeError: type object 'Person' has no attribute 'name'
"""
```

虽然有初始值, 但我们会发现一个奇怪的问题

```python
class Person:
    name: str = "张三"
    age: int

def main():
    per = Person()
    print(per.name)
    print(per.__dict__)
if __name__ == '__main__':
    main()
```

就是name有初始值, 但是`per.__dict__`方法却打印不出对象的字典

```python
class Person:
    name: str = "张三"
    age: int


def main():
    per = Person()
    print(per.name)
    print(per.__dict__)
    

if __name__ == '__main__':
    main()

"""
张三
{}
"""
```

想要在对象字典里生效, 就需要在`初始化后`或`初始化时`进行赋值

```python

# 初始化后进行赋值
per = Person()
    per.name = "李四"

# 比如初始化时把初始值给per 我这么赋值只是举例子不要学我
def __init__(self) -> None:
	super().__init__()
	self.name = Person.name
```

## 🌲 定义初始化方法

```python
class Person:
    name = ""
    age = 0
    # 初始化方法
    def __init__(self, name, age):
	    super().__init__()
        self.name = name
        self.age = age
        
# 初始化

# 方法1
per = Person(name="张三", age="18")
# 方法2
per = Person("张三", "18")
print(per.__dict__) # {'name': '张三', 'age': '18'}
```

## 🌲 定义方法

```python
class Person:
    name = ""
    age = 0
    # 初始化方法
    def __init__(self, name, age):
        self.name = name
        self.age = age
        
	def say(self):
  		print("人说")

# 调用
per.say()
```

## 🌲 继承

```python
class Student(Person):
    pass


stu = Student(name=123, age=18)
print(stu.name)
```

## 🌲 重写方法

```python
class Student(Person):
    def say(self):
        print("学生说")
    pass
    
    
stu = Student(name=123, age=18)
stu.say()
```

## 🌲 getter/setter

使用`@property`来定义`getter`方法, 使用`@变量名.setter`定义`setter`方法

```python
class Person:
    _name: str = ""
    name: str

    @property
    def name(self):
        return self._name

    @name.setter
    def name(self, value):
        self._name = value

per = Person()
per.name = "456"
print(per.name)
```

## 🌲 判断类型

```python
def main():
    per = Person()
    print(isinstance(per, Person))
    print(isinstance("123", str))
```

## 🌲 获取方法名

```python
import sys

print(sys._getframe().f_code.co_name)
```

# 🍎 反射

通过反射, 我们可以轻松的对python中的成员变量进行操作

```python
class Person:
    name: str = None
    age: int = None
    
    def hello(self):
        print("hello")
```

## 🌲 赋值

我们使用`__setattr__`进行动态赋值, 为什么叫动态呢, 因为name和张三都可以是变量

```python
per = Person()
per.__setattr__("name", "张三")
```

不仅可以赋值变量也可以赋值方法

```python
def say():
    print("say")

def main():
    per = Person()
    per.__setattr__("say", say)
    per.say()
```

## 🌲 取值

我们使用`__getattribute__`进行动态取值

```python
print(per.__getattribute__("name"))
```

不仅可以取属性, 也可以取方法

```python
per.__getattribute__("hello")

我们可以使用()来调用方法

per.__getattribute__("hello")() # hello
```

## 🌲 获取对象所有属性和方法

```python
def main():
    per = Person()
    print(dir(per))
    # ['__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getstate__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__', 'age', 'name']
```

注意这种方法有一个问题, 就是你定义的时候一定要给一个初始值, 否则在这个列表中不会显示, 打个比方

```python
class Person:
    name: str
    age: int

def main():
    per = Person()
    print(dir(per))
    # ['__annotations__', '__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getstate__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__']
```

这样打印我们会发现什么也没有, 因为变量根本没赋初始值, 所以定义变量的时候一定要赋初始值, 哪怕都是None

# 🍎 获取局部变量名

```python
def main():
    hello = 123
    per = Person()
    loc = locals()
    print(loc)
# {'hello': 123, 'per': <__main__.Person object at 0x00000246BED04E90>}
```

# 🍎 多线程

## 🌲 Thread

```python
from threading import Thread  
  
def hello():  
	for i in range(100):  
    print("hello", i)  


if __name__ == '__main__':  
	t = Thread(target=hello)  
    t.start()  
    t2 = Thread(target=hello)  
    t2.start()

# t1和t2是同时执行的 所以打印100个数中间会有穿插 所以是多线程

# 传参也很简单
def hello(name):  
    for i in range(100):  
        print("hello", name, i)  


if __name__ == '__main__':  
    t = Thread(target=hello, args=("线程1",))  
    t.start()  
    t2 = Thread(target=hello, args=("线程2",))  
    t2.start()

# 复杂传参是这样 - 也很简单 - -
def hello(name, age, *args, **kwargs):  
    for i in range(100):  
        print("hello", name, age, args, kwargs, i)  


if __name__ == '__main__':  
    t = Thread(target=hello, args=("线程1","年龄18", "男"), kwargs={"id":"123"})  
    t.start()  
  
    t2 = Thread(target=hello, args=("线程2", "年龄20", "女"), kwargs={"id":"456"})  
    t2.start()

# 可以看出来args里的参数会按照顺序传入参数, 多出来的参数会进入到 *args, kwargs里面会直接进入到 **kwargs
```


## 🌲 Process
```python
from multiprocessing import Process  
  
  
def hello(name):  
    for i in range(1000):  
        print(name, i)  
  
# 创建进程也很简单, 语法与thread一致
if __name__ == '__main__':  
    p1 = Process(target=hello, args=("进程1",))  
    p1.start()  
  
    p2 = Process(target=hello, args=("---进程2",))  
    p2.start()  
    pass
```

# 🍎 re模块

## 🌲 findall

匹配全部

```python
s = re.findall(r"\d+", "我的电话是: 10086, 你的电话是: 10010")  
print(s) # ['10086', '10010']
```
## 🌲 finditer

finditer不同的是生成的不是列表而是迭代器, 迭代器比列表更节约资源

```python
iter = zre.finditer(r"\d+", "我的电话是: 10086, 你的电话是: 10010")
print(type(iter)) # <class 'callable_iterator'>
for i in iter:
	print(type(i)) # <class 're.Match'>
	print(i.group()) # 10086
```

## 🌲 search

匹配一个

```python
s = re.search(r"\d+", "我的电话是: 10086, 你的电话是: 10010")  
print(type(s)) # _sre.SRE_Match  
print(s.group()) # 10086
```

## 🌲 match

match是从头开始匹配 如果匹配不上就返回None 相当于`^\d+`

```python
# 匹配补上的情况
s = re.match(r"\d+", "我的电话是: 10086, 你的电话是: 10010")  
print(s) # None
# 匹配上的情况
s2 = re.match(r"\d+", "10086, 你的电话是: 10010")
print(s2.group()) # 10086
```

## 🌲 compile

compile是正则表达式预加载 可以提前把表达式准备好再进行正则匹配

```python
partten = re.compile(r"\d+")  
s = partten.findall("我的电话是: 10086, 你的电话是10010")  
print(s) # 10086
         # 10010
```

## 🌲 惰性匹配
```python

str1 = """  
<div class="1">张三</div>  
<div class="2">李四</div>  
<div class="3">王五</div>  
<div class="4">赵六</div>  
"""  

# 提取符合的文字
partten = re.compile(r'<div class=".*?">.*?</div>', re.S)  
s = partten.findall(str1)
print(s) # ['<div class="1">张三</div>', '<div class="2">李四</div>', '<div class="3">王五</div>', '<div class="4">赵六</div>']

# 只提取括号内部文字
partten = re.compile(r'<div class=".*?">(.*?)</div>', re.S)  
s = partten.findall(str1)  
print(s) # ['张三', '李四', '王五', '赵六']

# 如果括号有多个 会通过元组的形式进入数组
partten = re.compile(r'<div class="(.*?)">(.*?)</div>', re.S)  
s = partten.findall(str1)  
print(s) # [('1', '张三'), ('2', '李四'), ('3', '王五'), ('4', '赵六')]

# 使用 ?P<xxx> 来给字段分组
partten = re.compile(r'<div class=".*?">(?P<name>.*?)</div>', re.S)  
s = partten.finditer(str1)  
for i in s:  
    print(i.group("name")) # 张三
						   # 李四
						   # 王五
						   # 赵六

# 也可以使用多个分组
partten = re.compile(r'<div class="(?P<class>.*?)">(?P<name>.*?)</div>', re.S)  
s = partten.finditer(str1)  
for i in s:  
    print(i.group("name"))  
    print(i.group("class")) # 张三
							# 1
							# 李四
							# 2
                            # ...
```

## 🌲 flags

### 🌸 re.S

一般与`(.*?)`联合使用, 意思是开启跨行匹配, 我们来看例子

```python
arr = re.findall(r"我的电话是: (.*?),", """我的电话是: 10086, 你的电话是: 10010""")
for text in arr:
	print(text) # 10086
```

如果文字当中出现了换行是匹配不到的

```python
arr = re.findall(r"我的电话是: (.*?),", """我的电话是: 10086
, 你的电话是: 10010""")
for text in arr:
	print(text)
```

这个时候我们就要使用`re.S`

```python
arr = re.findall(r"我的电话是: (.*?),", """我的电话是: 10086
, 你的电话是: 10010""", re.S)
for text in arr:
	print(text) # 10086
```

但是需要注意的是, 如果你的匹配内容中间进行了换行, 那么出来的就是换行后的匹配结果

```python
arr = re.findall(r"我的电话是: (.*?),", """我的电话是: 100
86, 你的电话是: 10010""", re.S)
for text in arr:
	print(text)
"""
100
    86
"""
```

# 🍎 csv模块

## 🌲 写

```python
row = ["张三", "20", "男"]  
  
# 写入一行
with open("1.csv", "w") as f:  
    csv.writer(f).writerow(row)

rows = [  
    ["张三", "20", "男"],  
    ["李四", "18", "女"]  
]  
  
# 写入多行 
with open("1.csv", "w") as f:  
    csv.writer(f).writerows(rows)
```

## 🌲 读

```python
with open("1.csv") as f:  
    rows = csv.reader(f)  
    for row in rows:  
        print(row)
```

# 🍎 创建虚拟环境

```shell
python3 -m venv venv
```

# 🍎 测试

python中使用`unittest`来进行测试, 然后新建test为前缀的方法, 这样所有的方法都可以单独运行了

```python
class test_unittest(unittest.TestCase):
    def test(self):
        print("hello")
```

# 🍎 颜色输出

```python
for i in range(1, 1000):
	print(i, f"\033[1;{i}m你好\033[0m")
```