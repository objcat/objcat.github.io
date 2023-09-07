# 🍎 简介

![](images/Pasted%20image%2020230907111727.png)

Java是一门[面向对象](https://baike.baidu.com/item/%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1?fromModule=lemma_inlink)的编程语言，不仅吸收了[C++](https://baike.baidu.com/item/C%2B%2B?fromModule=lemma_inlink)语言的各种优点，还摒弃了C++里难以理解的多继承、[指针](https://baike.baidu.com/item/%E6%8C%87%E9%92%88/2878304?fromModule=lemma_inlink)等概念，因此Java语言具有功能强大和简单易用两个特征。Java语言作为静态面向对象编程语言的代表，极好地实现了面向对象理论，允许程序员以优雅的思维方式进行复杂的编程 [1] 。

Java具有简单性、面向对象、[分布式](https://baike.baidu.com/item/%E5%88%86%E5%B8%83%E5%BC%8F/19276232?fromModule=lemma_inlink)、[健壮性](https://baike.baidu.com/item/%E5%81%A5%E5%A3%AE%E6%80%A7/4430133?fromModule=lemma_inlink)、[安全性](https://baike.baidu.com/item/%E5%AE%89%E5%85%A8%E6%80%A7/7664678?fromModule=lemma_inlink)、平台独立与可移植性、[多线程](https://baike.baidu.com/item/%E5%A4%9A%E7%BA%BF%E7%A8%8B/1190404?fromModule=lemma_inlink)、动态性等特点 [2] 。Java可以编写[桌面应用程序](https://baike.baidu.com/item/%E6%A1%8C%E9%9D%A2%E5%BA%94%E7%94%A8%E7%A8%8B%E5%BA%8F/2331979?fromModule=lemma_inlink)、Web应用程序、[分布式系统](https://baike.baidu.com/item/%E5%88%86%E5%B8%83%E5%BC%8F%E7%B3%BB%E7%BB%9F/4905336?fromModule=lemma_inlink)和[嵌入式系统](https://baike.baidu.com/item/%E5%B5%8C%E5%85%A5%E5%BC%8F%E7%B3%BB%E7%BB%9F/186978?fromModule=lemma_inlink)应用程序等 [3] 。

# 🍎 Hello World

```java
System.out.println("hello world!");
```

# 🍎 变量

```java
// 整形
int testInt = 100;
Integer testInteger = 123;
// 浮点型
float testFloat = 1000.0F;
Float testFloat2 = 1000.0F;
// 双精度浮点型
double testDouble = 1000.0;
Double testDouble2 = 1000.0;
// 数字
Number test_number = 123.0;
// 字符串
String c = "Objcat";
// 数组
String[] testArray = {"1", "2", "3"};
// 容器
ArrayList<Integer> testArrayList = new ArrayList<Integer>();
// 字典
HashMap<String, String> testHashMap = new HashMap<>();
```

# 🍎 函数

## 🌲 无参数无返回值

```java
public class Test {
    public void hello() {
        System.out.println("hello world!");
    }

    public static void main(String[] args) {
        Test test = new Test();
        test.hello();
    }
}
```

## 🌲 有参数无返回值

```java
public class Test {
    public void hello(String text) {
        System.out.println(text);
    }

    public static void main(String[] args) {
        Test test = new Test();
        test.hello("hello world!");
    }
}
```

## 🌲 有参数有返回值

```java
public class Test {
    public String hello(String text) {
        return text;
    }
    
    public static void main(String[] args) {
        Test test = new Test();
        System.out.println(test.hello("hello world!"));
    }
}
```

## 🌲 静态函数

```java
public class Test {
    public static void hello() {
        System.out.println("hello world!");
    }

    public static void main(String[] args) {
        Test.hello();
    }
}
```

## 🌲 数组参数

```java
void hello(String[] arr) {
	
}

void hello(ArrayList<Integer> arr) {

}
```

## 🌲 字典参数

```java
void hello(HashMap<String, String> hashMap) {

}
```

# 🍎 字符串

## 🌲 定义字符串

```java
// 定义字符串
String myStr = "123";
```

## 🌲 截取字符串

```java
String myStr = "123";
// 从1开始到结尾
System.out.println(myStr.substring(1));
// 从第n个索引开始截取n个元素
System.out.println(myStr.substring(0, 1));
```

## 🌲 拼接字符串

```java
String myStr = "123";
System.out.println(myStr + "4");
```

## 🌲 替换字符串

```java
String myStr = "1231";
System.out.println(myStr.replace("1", "0")); // 0230
```

## 🌲 查找字符串位置

```java
String myStr = "1231";
// 查询首次出现的位置
System.out.println(myStr.indexOf("1")); // 0
// 从第几个位置开始查找
System.out.println(myStr.indexOf("1", 1)); // 3
// 查询最后出现的位置
System.out.println(myStr.lastIndexOf("1")); // 3
```

## 🌲 遍历字符串

```java
String my_str = "123";
for (char c : myStr.toCharArray()) {
	System.out.println(new Character(c).toString()); // 123
	System.out.println(c + ""); // 123
}
```


# 🍎 数组

## 🌲 定义数组

```java
// 定义数组
ArrayList<Integer> my_arr = new ArrayList<>();
my_arr.add(1);
my_arr.add(2);
my_arr.add(3);

System.out.println(Arrays.toString(my_arr));
```

## 🌲 获取元素

```java
// 通过下标获取 下标从0开始
System.out.println(my_arr.get(0));
```

## 🌲 添加元素

```java
my_arr.add(1);
```

## 🌲 删除元素

```java
// 删除第0个元素
my_arr.remove(0);
```

## 🌲 截取元素

```java
System.out.println(my_arr.subList(0, 1));
```

## 🌲 拼接数组

```java
ArrayList<Integer> my_arr = new ArrayList<Integer>();
my_arr.add(1);
my_arr.add(2);
my_arr.add(3);

ArrayList<Integer> my_arr2 = new ArrayList<Integer>();
my_arr2.add(1);
my_arr2.add(2);
my_arr2.add(3);

my_arr.addAll(my_arr2);

System.out.println(my_arr);
```

## 🌲 获取元素下标

```java
// 获取数组总元素下标 没有就是0
System.out.println(my_arr.indexOf(0)); // -1
System.out.println(my_arr.indexOf(1)); // 0
System.out.println(my_arr.lastIndexOf(1)) // 0;
```

# 🍎 字典

## 🌲 定义字典

```java
HashMap<String, String> my_map = new HashMap<>();
my_map.put("name", "张三");
my_map.put("age", "16");
```

## 🌲 获取元素

```java
System.out.println(my_map.get("name"));
```

## 🌲 添加元素

```java
my_map.put("gender", "男");
```

## 🌲 删除元素

```java
my_map.remove("name");
```

## 🌲 替换元素

```java
my_map.replace("name", "李四");
System.out.println(my_map);
```

## 🌲 遍历字典

### 🌸 for循环遍历

```java
for (Object key : map.keySet()) {
	System.out.println(key);
}
```

### 🌸 迭代器遍历

```java
Iterator iterator = map.keySet().iterator();
while (iterator.hasNext()) {
	System.out.println(iterator.next());
}
```

### 🌸 entry遍历

```java
for (Object o : map.entrySet()) {
	Map.Entry entry = (Map.Entry) o;
	System.out.println(entry.getKey());
	System.out.println(entry.getValue());
}
```

### 🌸 entry迭代器

```java
Iterator iterator = map.entrySet().iterator();
while (iterator.hasNext()) {
	Map.Entry entry = (Map.Entry) iterator.next();
	System.out.println(entry.getKey());
	System.out.println(entry.getValue());
}
```

## 🌲 快速创建

在我们的语法中有很多快速创建字典的方法, 我们一起来看看吧

### 🌸 双括号法

```java
Map<String, Integer> map = new HashMap<String, Integer>() {{
    put("A", 1);
    put("B", 2);
    put("C", 3);
}};
```

### 🌸 mapof方法(java 9)

```java
Map<String, Integer> map = Map.of("A", 1, "B", 2, "C", 3);
```

### 🌸 stream方法

```java
Map<String, Integer> map = Stream.of(new Object[][] {
    { "A", 1 },
    { "B", 2 },
    { "C", 3 }
}).collect(Collectors.toMap(e -> (String)e[0], e -> (Integer)e[1]));
```

# 🍎 集合

## 🌲 定义集合

```java
HashSet<Integer> my_set = new HashSet<>();
my_set.add(1)
my_set.add(2)
```

## 🌲 获取元素

```java
# 集合不能获取元素 但可以先转成数组再操作
HashSet<Integer> my_set = new HashSet<>();
my_set.add(1);
Object[] list = my_set.toArray();
System.out.println(list[0]);
```

## 🌲 添加元素

```java
// 集合中不能存在相同的元素, 所以向集合中添加元素成功返回true 不成功返回false
my_set.add(1) // true
my_set.add(1) // false
```

## 🌲 删除元素

```java
my_set.remove(1)
print(my_set) // [3, 4, 2] 无序
```

## 🌲 截取元素

```java
无法截取 - 可以转化成数组后截取
List<Object> list = Arrays.asList(my_set.toArray());
System.out.println(list);
```

## 🌲 拼接集合

```java
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

# 🍎 条件语句

## 🌲 if

```java
int a = 1;
if (a == 1) {
	System.out.println("a等于1");
}
```

## 🌲 if - else if

```java
int a = 2;
if (a == 1) {
	System.out.println("a等于1");
} else if(a == 2) {
	System.out.println("a等于2");
}
```

## 🌲 if - else if - else

```java
int a = 3;
if (a == 1) {
	System.out.println("a等于1");
} else if(a == 2) {
	System.out.println("a等于2");
} else {
	System.out.println("a既不等于1也不等于2");
}
```


# 🍎 循环语句

## 🌲 for

```java
int[] list = {1, 2, 3};
for (int i : list) {
	System.out.println(i);
}

for (int i = 1; i <= 3; i++) {
	System.out.println(i);
}


List<Integer> list = new ArrayList();
list.add(1);
list.add(2);
list.add(3);
list.forEach((i) -> {
	System.out.println(i);
});

List<Integer> list = new ArrayList();
list.add(1);
list.add(2);
list.add(3);
list.forEach(System.out::println);
```

## 🌲 while

```java
int i = 0;
while (i < 3) {
   System.out.println(i);
   i++;
}
```

# 🍎 类和对象

## 🌲 类

```java
public class Person {
    
}
```

## 🌲 定义成员变量

```java
public class Person {
    String name;
    Integer age;
}
```

## 🌲 定义构造方法

```java
public class Person {
	// 创建成员变量
    String name;
    Integer age;

	// 空构造方法
    public Person() {

    }
    
	// 带参数构造方法
    public Person(String name, Integer age) {
        this.name = name;
        this.age = age;
    }

    public static void main(String[] args) {
	    // 使用构造方法初始化对象
        Person per = new Person("张三", 18);
        System.out.println(per.name);
        System.out.println(per.age);
    }
}
```

## 🌲 定义方法

```java
public class Person {
    String name;
    Integer age;

    public Person() {

    }

    public Person(String name, Integer age) {
        this.name = name;
        this.age = age;
    }

    // 实例方法 - 对象调用
    void say() {
        System.out.println("人说");
    }

    // 静态方法 - 类调用
    static void say2() {
        System.out.println("静态人说");
    }

    public static void main(String[] args) {
        Person per = new Person("张三", 18);
        per.say();
        Person.say2();
    }
}
```

## 🌲 继承

```java
class Student extends Person {
    @Override
    void say() {
        System.out.println("学生说");
    }
}


Student stu = new Student();
stu.say();
```

# 🍎 Optional

Java Optional是Java 8引入的一个新特性，它是一个可以为null的容器对象, 利用它我们不用显式进行空值检测, 可以避免很多空指针的问题

## 🌲 问题

我们在编程中会经常遇到空指针的问题, 比如下面的代码

```java
String string = "123";
System.out.println(string.length());
```

当我们的字符串为null, 的时候则会抛出异常

```java
java.lang.NullPointerException
```

因此我们会经常写if判断

```java
if (string != null) {
	System.out.println(string.length());
}
```

但是这样写会很麻烦, 如何简化这种代码呢

## 🌲 使用

我们可以使用`Optional`来简化判断, 提高安全性

```java
Optional<String> s = Optional.ofNullable("123");
System.out.println(s.map(String::length).orElse(0));
```

这样即使字符串为null, 也不会报错

# 🍎 BigDecimal

我们一般在使用浮点数时都会采用诸如`Double`和`Float`这样的类型, 但是他们在做运算的时候会产生精度问题, 我们一起来看看吧

```java
public class TestDecimal {
    public static void main(String[] args) {
        Float f = 0f;
        for (int i = 0; i < 10; i++) {
            f += 0.1f;
            System.out.println(f);
        }
    }
}
/**
0.1
0.2
0.3
0.4
0.5
0.6
0.70000005
0.8000001
0.9000001
1.0000001
*/
```

我们可以看到, 从0.7之后就会出现乱糟糟的数字, 这就是我们老生常谈的精度问题, 一般在对数字要求严格的地方都会采用`BigDecimal`来计算, 我们一起来看吧

## 🌲 创建

有两种方法可以创建一个`bigDecimal`类, 而这两种方法是有一定区别的

```java
BigDecimal bigDecimal = new BigDecimal(0.01);
System.out.println(bigDecimal);

BigDecimal bigDecimal2 = BigDecimal.valueOf(0.01);
System.out.println(bigDecimal2);

/**
0.01000000000000000020816681711721685132943093776702880859375
0.01
*/
```

我们可以看到使用`valueOf`来创建的数字更干净, 因为`valueOf`是做了优化的, 所以我们以后就使用`valueOf`来创建`BigDecimal`

## 🌲 加法

```java
BigDecimal bigDecimal = BigDecimal.valueOf(0);
bigDecimal = bigDecimal.add(BigDecimal.valueOf(0.1));
System.out.println(bigDecimal);
```

## 🌲 减法

```java
BigDecimal bigDecimal = BigDecimal.valueOf(1);
bigDecimal = bigDecimal.subtract(BigDecimal.valueOf(0.1));
System.out.println(bigDecimal);
```

## 🌲 乘法

```java
BigDecimal bigDecimal = BigDecimal.valueOf(1);
bigDecimal = bigDecimal.multiply(BigDecimal.valueOf(0.1));
System.out.println(bigDecimal);
```

## 🌲 除法

我们会发现除法与我们期望的数值不同

```java
BigDecimal bigDecimal = BigDecimal.valueOf(1);
bigDecimal = bigDecimal.divide(BigDecimal.valueOf(0.1));
System.out.println(bigDecimal);
// 1E+1
```

我们需要使用更高级的方法来进行处理, 三个参数分别是数字, 保留几位和舍入策略, RoundingMode.UNNECESSARY表示不舍入

```java
BigDecimal bigDecimal = BigDecimal.valueOf(1);
bigDecimal = bigDecimal.divide(BigDecimal.valueOf(0.1), 1, RoundingMode.UNNECESSARY);
System.out.println(bigDecimal);
// 10.0
```

需要注意的是保留几位如果填写0, 一旦结果是小数就会出现`Rounding necessary`异常, 如

```java
BigDecimal bigDecimal = BigDecimal.valueOf(1);
bigDecimal = bigDecimal.divide(BigDecimal.valueOf(10), 0, RoundingMode.UNNECESSARY);
System.out.println(bigDecimal);
```

所以我们一般会保留一位到两位小数

## 🌲 舍入规则

舍入规则最常见的就是四舍五入, 我们下面就一起来看看吧

### 🌸 RoundingMode.UNNECESSARY

不进行舍入

### 🌸 RoundingMode.UP

永远进位

```
5.5 6
2.5 3
1.6 2
1.1 2
1.0 1
-1.0 -1
-1.1 -2
-1.6 -2
-2.5 -3
-5.5 -6
```

### 🌸 RoundingMode.DOWN

永远退位

```
5.5 5
2.5 2
1.6 1
1.1 1
1.0 1
-1.0 -1
-1.1 -1
-1.6 -1
-2.5 -2
-5.5 -5
```

### 🌸 RoundingMode.CEILING



```
5.5 6
2.5 3
1.6 2
1.1 2
1.0 1
-1.0 -1
-1.1 -1
-1.6 -1
-2.5 -2
-5.5 -5
```

### 🌸 RoundingMode.ROUND_FLOOR

```
5.5 5
2.5 2
1.6 1
1.1 1
1.0 1
-1.0 -1
-1.1 -2
-1.6 -2
-2.5 -3
-5.5 -6
```

### 🌸 RoundingMode.HALF_UP

```
5.5 6
2.5 3
1.6 2
1.1 1
1.0 1
-1.0 -1
-1.1 -1
-1.6 -2
-2.5 -3
-5.5 -6
```

### 🌸 RoundingMode.HALF_DOWN

```
5.5 5
2.5 2
1.6 2
1.1 1
1.0 1
-1.0 -1
-1.1 -1
-1.6 -2
-2.5 -2
-5.5 -5
```

### 🌸 RoundingMode.HALF_EVEN

```
5.5 6
2.5 2
1.6 2
1.1 1
1.0 1
-1.0 -1
-1.1 -1
-1.6 -2
-2.5 -2
-5.5 -6
```

# 🍎 泛型

所谓泛型就是编程中为了限定或指定参数类型的一个方法, 常见的泛型有容器泛型, 对象泛型, 方法泛型

## 🌲 容器泛型

顾名思义, 该泛型作用在对象上, 举个例子, 比如我们的List, 我们使用`List<String>`来指定数组里只能存字符串, 如果存其他类型的值就会报错, 这就达到了编译中检测我们值的目的, 字典数组都是如此

```java
List<String> list = new ArrayList<>();
list.add("张三");
list.add("李四");
list.forEach(System.out::println);
```

## 🌲 对象泛型

加入我有这样一个需求, 有一个Student学生类, 学生有一个名字, 同时他还拥有一只宠物, 如果没有泛型我们要怎么定义呢

```java
public class TestGeneric {

    static class Cat {
        String name;
    }

    static class Dog {
        String name;
    }

    static class Student {
        String name;
        Object pet;
        
        public Student(String name, Object pet) {
            this.name = name;
            this.pet = pet;
        }
    }

    public static void main(String[] args) {

    }
}
```

然后使用的时候就是

```java
public static void main(String[] args) {
	Cat cat = new Cat();
	cat.name = "miao";
	Student student = new Student("张三", cat);
	Cat cat2 = (Cat) student.pet;
	System.out.println(cat.name);
}
```

我们发现使用cat的时候很不方便, 因为需要把object转换成cat

如果使用泛型我们就不用这么麻烦了, 声明类的时候需要写成`Student<T>`来表示这是带泛型的类

```java
public class TestGeneric {

    static class Cat {
        String name;
    }

    static class Dog {
        String name;
    }

    static class Student<T> {
        String name;
        T pet;
        
        public Student(String name, T pet) {
            this.name = name;
            this.pet = pet;
        }
    }

    public static void main(String[] args) {
        Cat pet = new Cat();
        cat.name = "miao";
        Student<Cat> student = new Student("张三", pet);
        System.out.println(student.pet.name);
    }
}
```

使用的时候需要使用`Student<Cat>`来指定类中泛型的类型, 这样我们使用student.pet就能自动识别到是我们的Cat了, 同样的dog也是一样的

```java
Dog pet = new Dog();
pet.name = "wang";
Student<Dog> student = new Student("张三", pet);
System.out.println(student.pet.name);
```

## 🌲 方法泛型

对象泛型我们已经见识过了, 在声明的时候指定里面某个值的类型可以很方便的对泛型的对象进行操作, 但是它有一个缺点, 就是类型单一, 假如我指定了Pet是狗, 后来我想养猫了, 就无法把猫赋值到pet上, 因为会报错, 所以我们这个时候就要用到方法泛型了

我们用交通方式来举例子, 张三的家离公司很远, 它需要先做公交, 再坐地铁, 这种情况下交通工具可能会换, 所以不能使用泛型来指定, 那我们就可以用方法泛型来解决此类问题了, 我们先看看没有泛型的时候要怎么做

```java
public class TestGeneric {

    static class Cat {
        String name;
    }

    static class Dog {
        String name;
    }

    static class Bus {
        public void run() {
            System.out.println("公交开了");
        }
    }

    static class Subway {
        public void run() {
            System.out.println("地铁开了");
        }
    }

    static class Student<T> {
        String name;
        T pet;
        Object vehicle;

        public Student(String name, T pet) {
            this.name = name;
            this.pet = pet;
        }

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }
    }

    public static void main(String[] args) {
        Dog pet = new Dog();
        pet.name = "wang";
        Student<Dog> student = new Student("张三", pet);
        // 坐公交
        student.vehicle = new Bus();
        Bus bus = (Bus) student.vehicle;
        bus.run();
        // 做地铁
        student.vehicle = new Subway();
        Subway subway = (Subway) student.vehicle;
        subway.run();
    }
}
```

可以看到我们增加了一个`vehicle`交通工具, 它是Object类型可以接受任意对象, 我们定义了Bug和Subway来当做公交和地铁, 学生每次乘坐公交和地铁的时候都要去强转, 然后调用run开启路程

我们来看一下泛型方法优化后的代码

```java
public class TestGeneric {

    static class Cat {
        String name;
    }

    static class Dog {
        String name;
    }

    static class Bus {
        public void run() {
            System.out.println("公交开了");
        }
    }

    static class Subway {
        public void run() {
            System.out.println("地铁开了");
        }
    }

    static class Student<T> {
        String name;
        T pet;
        Object vehicle;

        public Student(String name, T pet) {
            this.name = name;
            this.pet = pet;
        }

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }

        public <V> V getVehicle(Class<V> cls) {
            return (V) this.vehicle;
        }
    }

    public static void main(String[] args) {
        Dog pet = new Dog();
        pet.name = "wang";
        Student<Dog> student = new Student("张三", pet);
        // 坐公交
        student.vehicle = new Bus();
        student.getVehicle(Bus.class).run();
        // 做地铁
        student.vehicle = new Subway();
        student.getVehicle(Subway.class).run();
    }
}
```

我们写一个方法`getVehicle`, 然后我们在乘坐公交时指定公交的类型, 这样就直接能拿到公交, 我们在乘坐地铁的时候也传递一个类型地铁, 这样就直接能拿到地铁而不需要强转了

# 🍎 反射

https://blog.csdn.net/qq_51515673/article/details/124830558

Reflection(反射) 是 Java 程序开发语言的特征之一，它允许运行中的 Java 程序对自身进行检查。被private封装的资源只能类内部访问，外部是不行的，但反射能直接操作类私有属性。反射可以在运行时获取一个类的所有信息，（包括成员变量，成员方法，构造器等），并且可以操纵类的字段、方法、构造器等部分。

## 🌲 Class类

Class 对象是在加载类时由 Java 虚拟机以及通过调用类加载器中的defineClass 方法自动构造的。也就是这不需要我们自己去处理创建，JVM已经帮我们创建好了。

我们知道Spring框架可以帮我们创建和管理对象。需要对象时，我们无需自己手动new对象，直接从Spring提供的容器中的Beans获取即可。Beans底层其实就是一个Map<String,Object>，最终通过getBean(“user”)来获取。而这其中最核心的实现就是利用反射技术。   

![](images/Pasted%20image%2020230903091203.png)

### 🌸 获取类对象的三种方式

```java
class Person {
    String name;
    Integer age;
}

// 调用对象的`getClass`方法
Person person = new Person();
Class cls = person.getClass();

// 调用类的.class属性
Class cls = Person.class;

// 使用类的全路径来获取(最安全，性能最好)
try {
	System.out.println(Class.forName(person.getClass().getName()));
} catch (ClassNotFoundException e) {
	e.printStackTrace();
}


打印如下:

如果person是个静态类
// class com.objcat.playground.test_copyproperty.TestCopyProperty$Person
如果person不是静态类
// class com.objcat.playground.test_copyproperty.Person
我们可以看到静态类是依附于`TestCopyProperty`本身的, 而非静态类是算在包下的
```

### 🌸 获取类的全路径

我们使用`getName`来获取类的全路径

```java
Class cls = Person.class;
System.out.println(cls.getName());
```

所以到这里相互转换可以走通了, 我们既可以通过类的全路径来获取类, 也可以使用类来获取全路径的值

### 🌸 获取包名

包由文件夹组成, 所以就是获取类所在的文件夹的名称, 也就是包名

```java
System.out.println(Person.class.getPackage());
// package com.objcat.playground.test_copyproperty
```

### 🌸 获取类中的字段

getFields():获取某个类的所有的公共(public)的字段，包括父类中的字段

getDeclaredFields()：获取某个类的所有声明的字段，即包括public 、private和protected，但是不包括父类中的声明字段

```java
Object person = new Person();
Class cls = person.getClass();
Field[] declaredFields = cls.getDeclaredFields();
Field[] fields = cls.getFields();
System.out.println(Arrays.toString(declaredFields));
System.out.println(Arrays.toString(fields));

我们可以看到, `getDeclaredFields`打印出了所有字段, 而`getFields`打印出了一个空数组, 这是因为`getFields`值打印公共的成员变量

/**
[java.lang.String com.objcat.playground.test_copyproperty.Person.name, java.lang.Integer com.objcat.playground.test_copyproperty.Person.age]
[]
*/
```

#### 🌵 getName

我们使用`getName`来获取成员变量的名字

```java
for (Field declaredField : declaredFields) {
	System.out.println(declaredField.getName());
}
/**
name
age
*/
```

#### 🌵 get

我们使用`get`方法传入对象就可以获取对象字段下面的值

```java
for (Field declaredField : declaredFields) {
	declaredField.setAccessible(true);
	try {
		System.out.println(declaredField.get(person));
	} catch (IllegalAccessException e) {
		e.printStackTrace();
	}
}
/**
张三
18
*/
```

#### 🌵 setAccessible

这是一个神奇的字段, 表示为`Java安全检查`, 网上的解释一般是为, 默认为`false`, 如果获取字段的值不设置为`ture`, 那么就无法访问私有变量的值, 会抛出异常`IllegalAccessException`, 但是经过我的测试, 与网上的见解不同

| 条件 | 现象 |
| -- | -- |
| 同包名下 | 不能访问private, 其他都能访问 |
| 不同包名下 | 只能访问public, 其他会抛出异常 |

综上所述, 想要摆脱这样的现象就要把该项设置为`true`关闭安全检查即可

```
java.lang.IllegalAccessException: Class com.objcat.playground.test_copyproperty.TestCopyProperty can not access a member of class com.objcat.playground.test_callback.Person with modifiers ""
	at sun.reflect.Reflection.ensureMemberAccess(Reflection.java:102)
	at java.lang.reflect.AccessibleObject.slowCheckMemberAccess(AccessibleObject.java:296)
	at java.lang.reflect.AccessibleObject.checkAccess(AccessibleObject.java:288)
	at java.lang.reflect.Field.get(Field.java:390)
	at com.objcat.playground.test_copyproperty.TestCopyProperty.main(TestCopyProperty.java:27)
```

## 🌲 常用方法

```java
public class Test {

    public static void main(String[] args) {
        Class cls = Student.class;
        // 获取包名
        System.out.println(cls.getPackage().getName());
        // 获取类名
        System.out.println(cls.getSimpleName());
        // 获取完整类名
        System.out.println(cls.getName());
        // 获取所有公开的变量(不包含受保护和私有的)
        for (Field field : cls.getFields()) {
            System.out.println(field.getName());
        }
        // 获取当前类中所有变量(不包含父类)
        for (Field declaredField : cls.getDeclaredFields()) {
            System.out.println(declaredField.getName());
        }
        // 获取当前类的父类中的变量(不包含爷类)
        for (Field declaredField : cls.getSuperclass().getDeclaredFields()) {
            System.out.println(declaredField.getName());
        }
    }

    class Person {
        public String name;
        protected String age;
        private String gender;
    }

    class Student extends Person {
        public String name1;
        protected String age1;
        private String gender1;
    }
}
```

```java
// 获取包名、类名
clazz.getPackage().getName()// 包名
clazz.getSimpleName()// 类名
clazz.getName()// 完整类名
 
// 获取成员变量定义信息
getFields() // 获取所有公开的成员变量,包括继承变量
getDeclaredFields() // 获取本类定义的成员变量,包括私有,但不包括继承的变量
getField(变量名)
getDeclaredField(变量名)
 
//获取构造方法定义信息
getConstructor(参数类型列表)//获取公开的构造方法
getConstructors()//获取所有的公开的构造方法
getDeclaredConstructors()//获取所有的构造方法,包括私有
getDeclaredConstructor(int.class,String.class)
 
//获取方法定义信息
getMethods()//获取所有可见的方法,包括继承的方法
getMethod(方法名,参数类型列表)
getDeclaredMethods()//获取本类定义的的方法,包括私有,不包括继承的方法
getDeclaredMethod(方法名,int.class,String.class)
 
//反射新建实例
clazz.newInstance();//执行无参构造创建对象
clazz.newInstance(222,"韦小宝");//执行有参构造创建对象
clazz.getConstructor(int.class,String.class)//获取构造方法
 
//反射调用成员变量
clazz.getDeclaredField(变量名);//获取变量
clazz.setAccessible(true);//使私有成员允许访问
f.set(实例,值);//为指定实例的变量赋值,静态变量,第一参数给null
f.get(实例);//访问指定实例变量的值,静态变量,第一参数给null
 
//反射调用成员方法
Method m = Clazz.getDeclaredMethod(方法名,参数类型列表);
m.setAccessible(true);//使私有方法允许被调用
m.invoke(实例,参数数据);//让指定实例来执行该方法
```

# 🍎 日期类

我们常用的时间类有三个分别是`Date,LocalDate,LocalDateTime`, 其中后面两个是Java8新增的

为了更方便的区分它们, 我们来打印一下

```java
Date date = new Date();
LocalDateTime localDateTime = LocalDateTime.now();
LocalDate localDate = LocalDate.now();
System.out.println(date);
System.out.println(localDate);
System.out.println(localDateTime);
/**
Mon Oct 10 21:40:30 CST 2022
2022-10-10
2022-10-10T21:40:30.541
*/
```

我们可以看到`date和localDateTime`都是表示`日期+时间`而`localDate`只表示日期

## 🌲 Date

### 🌸 创建对象

```
Date date = new Date();
```

### 🌸 格式化

#### 🌵 字符串 -> Date

我们可以使用`SimpleDateFormat`来进行转换, 但是大家都知道`SimpleDateFormat`是非线程安全的, 所以我们使用的时候一般会加锁, 或者每次都使用`new SimpleDateFormat`

```java
String str = "2022-01-01 00:00:00";
SimpleDateFormat simpleFormatter = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
try {
	Date date1 = simpleFormatter.parse(str);
	System.out.println(date1);
} catch (ParseException e) {
	e.printStackTrace();
}
// Sat Jan 01 00:00:00 CST 2022
```

#### 🌵 Date -> 字符串

```java
SimpleDateFormat simpleFormatter = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
String dateString = simpleFormatter.format(date);
System.out.println(dateString);
// 2022-10-10 22:03:10
```

## 🌲 LocalDate

### 🌸 创建对象

```java
LocalDate localDate = LocalDate.now();
```

### 🌸 格式化

#### 🌵 字符串 -> LocalDate

`DateTimeFormatter`是线程安全的, 推荐使用

```java
String ymd = "2022-01-01 00:00:00";
DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
LocalDate localDate1 = LocalDate.parse(ymd, formatter);
System.out.println(localDate1);
// 2022-01-01
```

#### 🌵 LocalDate -> 字符串

我们可以看到`ofPattern`里面的规则只有年月日, 因为如果加入时分秒的格式化会报错, 也变相印证了`LocalDate`只做日期的决心

```java
DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd");
String dateString = formatter.format(localDate);
System.out.println(dateString);
// 2022-10-10
```

## 🌲 LocalDateTime

### 🌸 创建对象

```java
LocalDateTime localDateTime = LocalDateTime.now();
```

### 🌸 格式化

#### 🌵 字符串 -> LocalDateTime

`DateTimeFormatter`是线程安全的, 推荐使用

```java
String str = "2022-01-01 00:00:00";
DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
LocalDateTime localDateTime1 = LocalDateTime.parse(str, formatter);
System.out.println(localDateTime1);
// 2022-01-01T00:00
```

#### 🌵 LocalDateTime -> 字符串

```java
DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
String dateString = formatter.format(localDateTime);
System.out.println(dateString);
// 2022-10-10 22:06:22
```

# 🍎 enum

## 🌲 简单枚举

```java
enum Color {
    RED,
    YELLOW,
    BLUE
}
```

## 🌲 复杂枚举

```java
enum Color {
    RED(0, "我是红色"),
    YELLOW(0, "我是黄色"),
    BLUE(0, "我是蓝色");

    int index;
    String value;

    Color(int index, String value) {
        this.index = index;
        this.value = value;
    }
}
```

# 🍎 Thread

## 🌲 创建

```java
// 方法1
Thread thread = new Thread(new Runnable() {
	@Override
	public void run() {
		System.out.println(Thread.currentThread());
	}
});

// 方法2
Thread thread = new Thread() {
	@Override
	public void run() {
		System.out.println(Thread.currentThread());
	}
}; 

// 方法3
Thread thread = new Thread(() -> {
	System.out.println(Thread.currentThread());
});
```

## 🌲 启动

```java
thread.start();

// 简便写法 直接启动
Thread thread = new Thread(() -> {
	System.out.println(Thread.currentThread());
}).start();
```

## 🌲 封装

有时候里面逻辑非常的多, 们就可以实现Runnable接口把东西封装进去

```java

public class Test {
    public static void main(String[] args) {
        System.out.println("开始");
        StringBuilder stringBuilder = new StringBuilder();

        Thread thread1 = new Thread(new Run1(stringBuilder));
        Thread thread2 = new Thread(new Run2(stringBuilder));
        thread1.start();
        thread2.start();
    }
}

class Run1 implements Runnable {

    StringBuilder stringBuilder;

    Run1(StringBuilder stringBuilder) {
        this.stringBuilder = stringBuilder;
    }

    @Override
    public void run() {
        for (int i = 0; i < 10; i++) {
            this.stringBuilder.append(i);
        }
    }
}

class Run2 implements Runnable {

    StringBuilder stringBuilder;

    Run2(StringBuilder stringBuilder) {
        this.stringBuilder = stringBuilder;
    }

    @Override
    public void run() {
        for (int i = 0; i < 10; i++) {
            System.out.println(this.stringBuilder.toString());
        }
    }
}
```

# 🍎 回调

回调从本质上就是一个类

```java
public class Test {
    public static void main(String[] args) throws InterruptedException {

        Test test = new Test();
        test.test(new CallBack() {
            @Override
            public void complete(String text) {
                System.out.println(text);
            }
        });

        System.out.println("over");
    }
    
    void test(CallBack callBack) {
        new Thread() {
            @Override
            public void run() {
                try {
                    sleep(3000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                callBack.complete("123");
            }
        }.start();
    }
}

class CallBack {
    public void complete(String text) {

    }
}
```

# 🍎 注解

https://www.runoob.com/w3cnote/java-annotation.html

Java 定义了一套注解，共有 7 个，3 个在 java.lang 中，剩下 4 个在 java.lang.annotation 中

 - @Override - 检查该方法是否是重写方法。如果发现其父类，或者是引用的接口中并没有该方法时，会报编译错误。
 - @Deprecated - 标记过时方法。如果使用该方法，会报编译警告。
 - @SuppressWarnings - 指示编译器去忽略注解中声明的警告。
 - @Retention - 标识这个注解怎么保存，是只在代码中，还是编入class文件中，或者是在运行时可以通过反射访问。
 - @Documented - 标记这些注解是否包含在用户文档中。
 - @Target - 标记这个注解应该是哪种 Java 成员。
 - @Inherited - 标记这个注解是继承于哪个注解类(默认 注解并没有继承于任何子类)
 - @SafeVarargs - Java 7 开始支持，忽略任何使用参数为泛型变量的方法或构造函数调用产生的警告。
 - @FunctionalInterface - Java 8 开始支持，标识一个匿名函数或函数式接口。
 - @Repeatable - Java 8 开始支持，标识某注解可以在同一个声明上使用多次。

## 🌲 三个重要的组成部分

### 🌸 Annotation

Annotation 就是个接口。
"每 1 个 Annotation" 都与 "1 个 RetentionPolicy" 关联，并且与 "1～n 个 ElementType" 关联。可以通俗的理解为：每 1 个 Annotation 对象，都会有唯一的 RetentionPolicy 属性；至于 ElementType 属性，则有 1~n 个。

```java
package java.lang.annotation;
public interface Annotation {

    boolean equals(Object obj);

    int hashCode();

    String toString();

    Class<? extends Annotation> annotationType();
}
```

### 🌸 ElementType

ElementType 是 Enum 枚举类型，它用来指定 Annotation 的类型。
"每 1 个 Annotation" 都与 "1～n 个 ElementType" 关联。当 Annotation 与某个 ElementType 关联时，就意味着：Annotation有了某种用途。例如，若一个 Annotation 对象是 METHOD 类型，则该 Annotation 只能用来修饰方法。

```java
public enum ElementType {
    TYPE,               /* 类、接口（包括注释类型）或枚举声明  */

    FIELD,              /* 字段声明（包括枚举常量）  */

    METHOD,             /* 方法声明  */

    PARAMETER,          /* 参数声明  */

    CONSTRUCTOR,        /* 构造方法声明  */

    LOCAL_VARIABLE,     /* 局部变量声明  */

    ANNOTATION_TYPE,    /* 注释类型声明  */

    PACKAGE             /* 包声明  */
}
```

### 🌸 RetentionPolicy

RetentionPolicy 是 Enum 枚举类型，它用来指定 Annotation 的策略。通俗点说，就是不同 RetentionPolicy 类型的 Annotation 的作用域不同。
"每 1 个 Annotation" 都与 "1 个 RetentionPolicy" 关联。
a) 若 Annotation 的类型为 SOURCE，则意味着：Annotation 仅存在于编译器处理期间，编译器处理完之后，该 Annotation 就没用了。 例如，" @Override" 标志就是一个 Annotation。当它修饰一个方法的时候，就意味着该方法覆盖父类的方法；并且在编译期间会进行语法检查！编译器处理完后，"@Override" 就没有任何作用了。
b) 若 Annotation 的类型为 CLASS，则意味着：编译器将 Annotation 存储于类对应的 .class 文件中，它是 Annotation 的默认行为。
c) 若 Annotation 的类型为 RUNTIME，则意味着：编译器将 Annotation 存储于 class 文件中，并且可由JVM读入。
这时，只需要记住"每 1 个 Annotation" 都与 "1 个 RetentionPolicy" 关联，并且与 "1～n 个 ElementType" 关联。学完后面的内容之后，再回头看这些内容，会更容易理解。

```java
package java.lang.annotation;
public enum RetentionPolicy {
    SOURCE,            /* Annotation信息仅存在于编译器处理期间，编译器处理完之后就没有该Annotation信息了  */

    CLASS,             /* 编译器将Annotation存储于类对应的.class文件中。默认行为  */

    RUNTIME            /* 编译器将Annotation存储于class文件中，并且可由JVM读入 */
}
```

## 🌲 自定义注解

### 🌸 @interface

使用`@interface`来定义一个注解, 这种写法意味着它实现了 `java.lang.annotation.Annotation` 接口

```java
@Retention(RetentionPolicy.RUNTIME)
public @interface TestAnnotation {
    String name();
}
```

然后我们可以使用反射来获取注解传递的参数

```java
@TestAnnotation(name = "123")
public class Test {
    public static void main(String[] args) throws InterruptedException {
        Class<Test> testClass = Test.class;
        TestAnnotation testAnnotation = testClass.getAnnotation(TestAnnotation.class);
        System.out.println(testAnnotation.name()); // "123"
    }
}
```

# 🍎 动态代理

动态代理分成`JDK动态代理`和`CGLIB`两种方式

## 🌲 简单实现

### 🌸 创建Handle

```java
public class TestInvocationHandler<T> implements InvocationHandler {

    private final T target;

    public TestInvocationHandler(T target) {
        this.target = target;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        System.out.println("方法要执行了!");
        return method.invoke(target, args);
    }
}
```

### 🌸 创建代理对象

```java
Peron.java

public interface Person {
    void hello();
}

Student.java

public class Student implements Person {
    @Override
    public void hello() {
        System.out.println("hello world!");
    }
}

TestDynamicProxy.java

public class TestDynamicProxy {
    public static void main(String[] args) {
        Person student = new Student();
        InvocationHandler invocationHandler = new TestInvocationHandler<>(student);
        Person personProxy = (Person) Proxy.newProxyInstance(Person.class.getClassLoader(), new Class[]{Person.class}, invocationHandler);
        personProxy.hello();
    }
}
```

我们发现使用代理对象调用`hello`方法时, `handle`就会收到, 这个时候就可以在`invoke`方法中`Hook`到方法从而实现`AOP`面向切面编程了

## 🌲 Hook

当我们`Hook`到方法的时候我们就能在调用方法前和调用方法后执行一些其他方法了, 比如我在调用`hello`方法前先`洗个脸`在调用方法后再`洗个脚`

```java
@Override
public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
	if (proxy instanceof Person) {
		if (method.getName().equals("hello")) {
			System.out.println("先洗个脸!");
			Object result = method.invoke(target, args);
			System.out.println("再洗个脚!");
			return result;
		}
	}
	return method.invoke(target, args);
}
```

# 🍎 Lambda

### 🌲 Lambda 表达式实例

```java
() -> 5;
(a) -> a * 2;
(a, b) -> a + b;
(a) -> System.out.println(a);
```

### 🌲 基本使用

```java
public class TestLambda {

    interface Lambda1 {
        int getNumber();
    }

    interface Lambda2 {
        int mul(int a);
    }

    interface Lambda3 {
        int add(int a, int b);
    }

    interface Lambda4 {
        void print(int a);
    }

    public static void main(String[] args) {
        Lambda1 lambda1 = () -> 5;
        System.out.println(lambda1.getNumber());

        Lambda2 lambda2 = (a) -> a * 2;
        System.out.println(lambda2.mul(2));

        Lambda3 lambda3 = (a, b) -> a + b;
        System.out.println(lambda3.add(1, 2));

        Lambda4 lambda4 = (a) -> System.out.println(a);
        lambda4.print(1);
    }
}

// 或

public class TestLambda {
    
    interface Lambda1 {
        int getNumber();
    }

    interface Lambda2 {
        int mul(int a);
    }

    interface Lambda3 {
        int add(int a, int b);
    }

    interface Lambda4 {
        void print(int a);
    }

    public static void main(String[] args) {
        Lambda1 lambda1 = () -> {
            return 5;
        };
        System.out.println(lambda1.getNumber());

        Lambda2 lambda2 = (a) -> {
            return a * 2;
        };
        System.out.println(lambda2.mul(2));

        Lambda3 lambda3 = (a, b) -> {
            return a + b;
        };
        System.out.println(lambda3.add(1, 2));

        Lambda4 lambda4 = (a) -> {
            System.out.println(a);
        };
        lambda4.print(1);
    }
}
```

### 🌲 forEach

```java
List<Integer> list = new ArrayList<>();
list.add(1);
list.add(2);
list.add(3);
list.add(4);
```

我们想打印这个数组一般是下面这种写法

```java
for (Integer integer : list) {
	System.out.println(integer);
}
```

使用forEach和lambda改良后可以这么写了, 写的时候是list.forEach(souc)就可以了

```java
list.forEach(System.out::println);
```

或者下面这个样子, 下面的是比上面可操作性高的, 因为括号里可以写其他代码

```java
list.forEach(a -> {
	System.out.println(a);
});
```

# 🍎 Stream

Java 8 API添加了一个新的抽象称为流Stream，可以让你以一种声明的方式处理数据, `stream`适用于对集合迭代器的增强, 使之能够完成更高大小的聚合操作(过滤, 排序, 统计分组), 或者大批量数据操作, 此外`stream`与`lambada`表达式结合后编码效率大大提高, 并且可读性更强

## 🌲 创建

我们可以使用数组的`stream`方法来创建流

```java
List<String> list = Arrays.asList("张三", "李四", "王五");
Stream<String> stream = list.stream();
System.out.println(stream);
```

也可以直接使用steam自带的方法

```java
Stream<String> stream = Stream.of("张三", "李四", "王五", null);
System.out.println(stream);
```

## 🌲 filter过滤

我们可以filter对流进行过滤, 中间的语法可以使用lambda表达式

```java
stream.filter(s -> true).forEach(System.out::println);
/**
张三
李四
王五
null
*/
```

filter中的lambda表达式可以返回两个值, true或false, 如果返回true就把这个值留下来, 否则移除掉, 因为我们始终是返回true, 所以流里面的元素都保留下来了, 那我们就来看几个常用的用法

### 🌸 过滤null值

我们可以直接在filter中填写`s -> s != null`意思是如果不为null就留下来

```java
stream.filter(s -> s != null).forEach(System.out::println);
/**
张三
李四
王五
*/
```

我们也可以写成

```java
stream.filter(Objects::nonNull).forEach(System.out::println);
```

如果你不嫌麻烦也可以写成这个样子, 思维更加清晰了, 如果不等于空就留下来, 否则移除

```java
stream.filter(s -> {
	if (s != null) {
		return true;
	} else {
		return false;
	}
}).forEach(System.out::println);
```

## 🌲 map

map 方法用于映射每个元素到对应的结果，以下代码片段使用 map 输出了元素对应的平方数：

```java
stream.map(s -> s += 1).forEach(System.out::println);
/**
张三1
李四1
王五1
null1
*/
```

## 🌲 limit/skip

limit 返回 Stream 的前面 n 个元素；skip 则是扔掉前 n 个元素。以下代码片段使用 limit 方法保留4个元素

```java
stream.limit(3).forEach(System.out::println);
/**
张三
李四
王五
*/

stream.skip(3).forEach(System.out::println);
/**
null
*/
```

## 🌲 sorted

我们可以使用流来方便的进行排序

```java
Stream<Integer> stream = Stream.of(3, 2, 1, 5, 4);
stream.sorted().forEach(System.out::println);
/**
1
2
3
4
5
*/
```

## 🌲 distinct

我们可以使用这个方法去重

```java
Stream<Integer> stream = Stream.of(3, 2, 1, 5, 4, 5, 6, 5);
stream.distinct().sorted().forEach(System.out::println);
/**
1
2
3
4
5
6
*/
```

## 🌲 分组

分组就是把符合条件的装到一个数组里, 最终返回对象是一个`map`, `key`就是我们分组的值本身, `value`是元素的集合

```java
System.out.println(list.stream().collect(Collectors.groupingBy(a -> a)));
// {1=[1], 2=[2], 3=[3], 4=[4]}
```

```java
System.out.println(list.stream().collect(Collectors.groupingBy(a -> a > 1)));
// {false=[1], true=[2, 3, 4]}
```

## 🌲 提取

我们使用`Collectors.toList()`来把流转换成数组

```java
List<Student> list = new ArrayList<>();
list.add(new Student("张三", "18"));
list.add(new Student("李四", "18"));
list.add(new Student("王五", "18"));
list.add(new Student("赵六", "18"));
System.out.println(list.stream().map(Student::getName).collect(Collectors.toList()));
// [张三, 李四, 王五, 赵六]
```

或者你想用逗号把他们分隔开

```java
System.out.println(list.stream().map(Student::getName).collect(Collectors.joining(",")));
// 张三,李四,王五,赵六
```

# 🍎 文件

## 🌲 File

`File`在java中表示文件或路径, 使我们读写文件时需要用到的类, 我们可以通过下面的代码创建, 我这里用的是苹果电脑, 如果是windows请自己修改路径, 如`D://test/1.txt`

```java
// 创建文件夹路径
File dir = new File("/users/objcat/test");
// 创建文件路径
File file = new File("/users/objcat/test", "1.txt");
```

注意我们上面创建了两个路径, 一个是文件夹的路径, 一个是文件的路径, 然而目前为止这两个文件都没保存在我的硬盘上, 那么要怎么保存呢, 首先我们需要根据路径创建出文件夹, 很简单哈, 我们只需要调用`mkdirs`方法即可

```java
if (dir.mkdirs()) {
	System.out.println("创建文件夹成功");
} else {
	System.out.println("创建文件夹失败");
}
```

创建成功了我们就可以向文件中写东西了, 继续看下一个章节

## 🌲 FileOutputStream

我们使用`FileOutputStream`向文件中写东西

```java
FileOutputStream fileOutputStream = null;
try {
	fileOutputStream = new FileOutputStream(file);
	fileOutputStream.write("hello world!".getBytes());
} catch (IOException e) {
	throw new RuntimeException(e);
}
```

如果没有报错就可以看到文件已经被创建好了, 里面的字符串就是`hello world!`

![](images/Pasted%20image%2020230418141428.png)

但是我们会发现有一个警告

![](images/Pasted%20image%2020230418141429.png)

出现这个问题的原因是我们的`FileOutputStream`用完了并没有关闭, 这回造成内存泄漏, 我们需要这么改

```java
FileOutputStream fileOutputStream = null;
try {
	fileOutputStream = new FileOutputStream(file);
	fileOutputStream.write("hello world!".getBytes());
} catch (IOException e) {
	throw new RuntimeException(e);
} finally {
	try {
		if (fileOutputStream != null) {
			fileOutputStream.close();
		}
	} catch (IOException e) {
		e.printStackTrace();
	}
}
```

我们会发现这有点麻烦, 要怎么简化一下呢, 我们需要用到上面提到的`try-with-resources`, 只需要稍微改写就可以了

```java
try(FileOutputStream fileOutputStream = new FileOutputStream(file)) {
	fileOutputStream.write("hello world!".getBytes());
} catch (IOException e) {
	throw new RuntimeException(e);
}
```

使用 try-with-resources 后,无需在 finally 中关闭 FileOutputStream,它会自动进行后续处理了

# 🍎 Scanner

从字面上理解是打印机, 其实它是在程序中接收用户输入的一个类

## 🌲 基本用法

```java
Scanner scanner = new Scanner(System.in);  
System.out.println("请输入数字:");  
int i = scanner.nextInt();  
System.out.println(i);
```

然后我们来测试一下, 我们发现控制台上打印让我们输入数字

```
请输入数字:
1
1
```

这就是最基本的用法了, 但当我们输入多个数字的时候, 我们发现打印的只有一个

```
请输入数字:
1 2 3 4
1
```

因为Scanner的规则是, 当遇到空格、tab、enter回车时,读取结束, 那我们想要都读取要怎么做呢, 接着往下看

## 🌲 连续获取

我们可以改造一下代码让Scanner获得连续读取的能力

```java
Scanner scanner = new Scanner(System.in);  
System.out.println("请输入数字:");  
while (scanner.hasNextInt()) {  
    int i = scanner.nextInt();  
    System.out.println(i);  
}
```

然后我们看一下输出

```
请输入数字:
1 2 3
1
2
3
```

这就解决了我们数字读取不完整的问题了

# 🍎 环境变量

## 🌲 classpath

用来确定class文件的位置, 如果不设置默认就是在`./`当前路径下去找`.class`文件, 如果配置后就会去配置的路径下找`.class`类文件

## 🌲 JAVA_HOME

其实就是`java`环境的根路径, 因为`tomcat`服务器启动的时候会使用该环境变量来运行`java`, 所以网上都教着把`JAVA_HOME`流传下来了, 配置java的环境变量的时候就使用`%JAVA_HOME%/bin`

上面两个都是针对于windows的方案

# 🍎 运行项目

这个很简单 两个步骤

编译
```
javac Test.java
```

运行
```
java Test
```

但是如果在Mac平台会有些问题 导致 `错误: 找不到或无法加载主类`

那么如何解决呢

首先我们先看包名

```java
package com.objcat.playground.test;

/**
 * description: Test <br>
 * date: 2022/3/16 9:21 AM <br>
 * author: objcat <br>
 * version: 1.0 <br>
 */
public class Test {
    public static void main(String[] args) {
        System.out.println("hello world!");
    }
}

```

我们可以看到包名是 `com.objcat.playground.test;`, 这说明了我们的资源根目录就在com的上级, 也就是java目录, 所以我们首先是要进入java目录

首先我们javac编译一下

```
javac /Users/objcat/project/Java/test-java/src/main/java/com/objcat/playground/test/Test.java
```

然后我们运行

```
cd /Users/objcat/project/Java/test-java/src/main/java
```

然后我们使用java命令来执行

```
java com.objcat.playground.test.Test
```

这样就可以执行了, 与windows上面不同

# 🍎 坑

## 🌲 float计算不准确的问题

阿里巴巴摘自java开发手册

```java
float a = 1.0f - 0.9f;
float b = 0.9f - 0.8f;
System.out.println(a);
System.out.println(b);
if (a == b) {
	// 预期进入此代码快，执行其它业务逻辑
	// 但事实上 a==b 的结果为 false
	System.out.println("相等");
} else {
	System.out.println("不相等");
}
```

修改为

```java
BigDecimal a = new BigDecimal("1.0");
BigDecimal b = new BigDecimal("0.9");
BigDecimal c = new BigDecimal("0.8");
BigDecimal x = a.subtract(b);
BigDecimal y = b.subtract(c);
System.out.println(x);
System.out.println(y);
if (x.equals(y)) {
	System.out.println("true");
}
```

# 🍎 推荐书籍

## 🌲 Java工程师成神之路

https://hollischuang.gitee.io/tobetopjavaer/#/basics/object-oriented/principle

# 🍎 环境搭建

[跳转 java_env](../../3-program/env/java/java_env.md)

# 🍎 官方文档

文档在线

https://docs.oracle.com/en/java/javase/17/docs/api/index.html

文档下载

https://www.oracle.com/java/technologies/javase-jdk17-doc-downloads.html
https://www.oracle.com/java/technologies/javase-jdk8-doc-downloads.html