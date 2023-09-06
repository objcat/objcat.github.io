# 🍎  简介

Objective-C，通常写作ObjC或OC和较少用的Objective C或Obj-C，是扩充C的面向对象编程语言。它主要使用于Mac OS X和GNUstep这两个使用OpenStep标准的系统，而在NeXTSTEP和OpenStep中它更是基本语言。

# 🍎 Hello World
```objc
NSLog(@"hello world!");
```

# 🍎 变量
```objc
// 整型
int testInt = 100;
// 浮点型
float testFloat = 1000.0;
// 字符串
NSString *testString = @"objcat";
// 数组
NSArray *testArray = @[@"1", @"2", @"3"];
// 字典
NSDictionary *testDictionary = @{@"name": @"objcat", @"age": @"18"};
// 集合
NSSet *testSet = [NSSet setWithArray:@[@"1", @"1", @"2", @"3"]];
```

# 🍎 方法

## 🌲 无参数无返回值
```objc
- (void)hello {
    NSLog(@"hello world!");
}
```

## 🌲 有参数无返回值
```objc
- (void)hello:(NSString *)text {
    NSLog(@"%@", text);
}
```

## 🌲 有参数有返回值
```objc
- (NSString *)hello:(NSString *)text {
    return text;
}
```

## 🌲 类方法
```objc
+ (NSString *)hello {
    NSLog(@"hello world!");
}
```

# 🍎 字符串

## 🌲 定义字符串

```objc
NSString *my_str = @"123";
```

## 🌲 截取字符串

```objc
// 截取 开始 ~ 1
NSLog(@"%@", [my_str substringToIndex:1]); // 1
// 截取 1 ~ 结尾
NSLog(@"%@", [my_str substringFromIndex:1]); // 23
```

## 🌲 拼接字符串

```objc
NSString *my_str = @"123";
NSString *my_str2 = @"456";
NSLog(@"%@", [NSString stringWithFormat:@"%@%@", my_str, my_str2]);
```

## 🌲 替换字符串

```objc
NSString *my_str = @"123";
NSLog(@"%@", [my_str stringByReplacingOccurrencesOfString:@"1" withString:@"5"]);
```

## 🌲 查找字符串位置

```objc
NSString *my_str = @"123";
NSLog(@"%@", NSStringFromRange([my_str rangeOfString:@"1"])); // {0, 1} 从9开始取1个
```

## 🌲 遍历字符串

```objc
NSString *my_str = @"123";
for (NSInteger i = 0; i < my_str.length; i++) {
	NSLog(@"%c", [my_str characterAtIndex:i]);
}
```

# 🍎 数组

## 🌲 定义数组
```objc
// 不可变数组
NSArray *my_arr = @[@"1", @"2", @"3"];
NSLog(@"%@", my_arr);
// 可变数组 可以添加元素并对元素进行操作
NSMutableArray *my_marr = [NSMutableArray array];
[my_marr addObject:@"1"];
[my_marr addObject:@"2"];
[my_marr addObject:@"3"];
```

## 🌲 获取元素
```objc
NSLog(@"%@", my_arr[0]);
```

## 🌲 添加元素
```objc
// oc中有只有可变数组可以添加元素
NSMutableArray *my_marr = [NSMutableArray array];
[my_marr addObject:@"1"];
NSLog(@"%@", my_marr);
```

## 🌲 删除元素
```objc
// 删除元素可以根据元素本身或索引
[my_marr removeObject:@"1"];
[my_marr removeObjectAtIndex:0];
```

## 🌲 截取元素
```objc
NSArray *my_arr = @[@"1", @"2", @"3"];
[my_arr subarrayWithRange:NSMakeRange(0, 1)]; // [1]
```

## 🌲 拼接数组
```objc
[my_marr addObjectsFromArray:@[@"4", @"5"]];
```

# 🍎 字典

## 🌲 定义字典
```objc
// 不可变字典
NSDictionary *my_dic = @{@"name": @"张三", @"age": @"18"};
// 可变字典
NSMutableDictionary *my_mdic = [NSMutableDictionary dictionary];
```

## 🌲 获取元素
```objc
NSString *str = my_dic[@"name"];
NSLog(@"%@", str);
```

## 🌲 添加元素
```objc
// 添加元素必须使用可变字典
NSMutableDictionary *my_mdic = [NSMutableDictionary dictionary];
my_mdic[@"name"] = @"张三";
```

## 🌲 删除元素
```objc
[my_mdic removeObjectForKey:@"name"];
NSLog(@"%@", my_mdic);
```

## 🌲 截取字典
```objc
无法截取或没找到截取方法
```

## 🌲 拼接字典

```objc
NSMutableDictionary *my_mdic = [NSMutableDictionary dictionary];
my_mdic[@"sex"] = @"男";
[my_mdic addEntriesFromDictionary:my_dic];
NSLog(@"%@", my_mdic);
/**
2022-07-29 10:41:43.056086+0800 playground_cmd_oc[6849:91696] {
    age = 18;
    name = "\U5f20\U4e09";
    sex = "\U7537";
}
*/
```

# 🍎 集合

## 🌲 定义集合

```objc
// 不可变集合
NSSet *set = [NSSet setWithArray:@[@"1", @"1", @"2", @"3"]];
NSLog(@"%@", set);
/**
2022-07-29 10:47:14.420783+0800 playground_cmd_oc[7136:95413] {(
    3,
    1,
    2
)}
*/
// 可变集合
NSMutableSet *my_mset = [NSMutableSet set];
```

## 🌲 获取元素

```objc
// 集合不能获取元素 但可以先转成数组再操作
NSSet *my_set = [NSSet setWithArray:@[@"1", @"1", @"2", @"3"]];
NSLog(@"%@", my_set);
NSArray *my_arr = [my_set allObjects];
NSLog(@"%@", my_arr);
```

## 🌲 添加元素

```objc
NSMutableSet *my_mset = [NSMutableSet set];
[my_mset addObject:@"4"];
```

## 🌲 删除元素

```objc
[my_mset removeObject:@"4"];
NSLog(@"%@", my_mset);
```

## 🌲 截取元素

```objc
无法截取或没找到截取方法
```

## 🌲 拼接集合

```objc
[my_mset addObjectsFromArray:@[@"1", @"2", @"3"]];
NSLog(@"%@", my_mset);
```

# 🍎 条件语句

## 🌲 if

```objc
NSInteger a = 1;
// 如果a等于1 执行条件
if (a == 1) {
    NSLog(@"a等于1");
}
```

## 🌲 if - else if

```objc
NSInteger a = 1;
// 如果a等于1执行1 如果a等于2执行2
if (a == 1) {
    NSLog(@"a等于1");
} else if (a == 2) {
    NSLog(@"a等于2");
}
```

## 🌲 if - else if - else

```objc
NSInteger a = 1;
// 如果都不等 执行else
if (a == 1) {
	NSLog(@"a等于1");
} else if (a == 2) {
	NSLog(@"a等于2");
} else {
	NSLog(@"a既不等于1也不等于2");
}
```

# 🍎 循环语句

## 🌲 for

```objc
for (NSInteger i = 0; i < 10; i++) {
	NSLog(@"%ld", i); // 0 1 2 ... 9
}
    
for (NSString *str in @[@"1", @"2", @"3"]) {
	NSLog(@"%@", str); // 1 2 3
}
```

## 🌲 while

```objc
NSInteger i = 0;
while (i < 3) {
	NSLog(@"%ld", i); // 0 1 2
	i++;
}
```

# 🍎 类和对象

## 🌲 类

```objc
// .h
#import <Foundation/Foundation.h>

@interface Person : NSObject

@end

// .m
#import "Person.h"

@implementation Person

@end
```

## 🌲 定义成员变量

```objc
@interface Person : NSObject
@property (strong, nonatomic) NSString *name;
@property (assign, nonatomic) NSInteger age;
@end
```

## 🌲 定义初始化方法

```objc
// .h
@interface Person : NSObject
@property (strong, nonatomic) NSString *name;
@property (assign, nonatomic) NSInteger age;
@end

// .m
@implementation Person
- (instancetype)init {
    self = [super init];
    if (self) {
        
    }
    return self;
}
@end
        
# 初始化

# 方法1
Person *per = [[Person alloc] init];
per.name = @"张三";
per.age = 18;
```

## 🌲 定义方法

```objc

// .h
@interface Person : NSObject
@property (strong, nonatomic) NSString *name;
@property (assign, nonatomic) NSInteger age;
- (void)say;
@end

// .m
@implementation Person
- (instancetype)init {
    self = [super init];
    if (self) {
        
    }
    return self;
}

- (void)say {
    NSLog(@"人说");
}
@end
```

# 🍎 协议

`iOS中协议即Protocol`, 是一种给类和对象添加方法的技术, 它类似于Java中的接口, 但是很多培训班经常把它作为传值的手段, 这是非常误人子弟的, 也是不可取的教学方式, 我们接下来就一起来看看它的用法吧

## 🌲 给类添加方法

```objc
@protocol TestProtocol
+ (void)hello;
@end

@interface ViewController () <TestProtocol>

@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view.
    [ViewController hello];
}

+ (void)hello {
    NSLog(@"hello world!");
}

@end
```

## 🌲 给对象添加方法

```objc
@protocol TestProtocol
- (void)hello;
@end

@interface ViewController () <TestProtocol>

@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view.
    [self hello];
}

- (void)hello {
    NSLog(@"hello world!");
}

@end
```

## 🌲 协议多态

我们给对象签了协议后就可以使用协议来承接这个对象, 无论是`实例`还是`类对象`都可以承接

```objc
- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view.
    id <TestProtocol> testProtocol = self;
    [testProtocol hello];
}

- (void)hello {
    NSLog(@"hello world!");
}
```

```objc
- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view.
    id <TestProtocol> testProtocol = (id <TestProtocol>)[ViewController class];
    [testProtocol hello];
}

+ (void)hello {
    NSLog(@"hello world!");
}
```

## 🌲 判断是否遵守协议

我们使用`conformsToProtocol`来进行判断

```objc
NSLog(@"%d", [self conformsToProtocol:@protocol(TestProtocol)]);
NSLog(@"%d", [[self class] conformsToProtocol:@protocol(TestProtocol)]);
```

## 🌲 协议传值

来了, 培训班最喜欢教的东西, 协议传值, 我们透过现象来看本质

### 🌸 创建协议

首先是一个协议叫`VC1Delegate`不要纠结为啥上面我起的叫`Protocol`都是一样的

```objc
@protocol VC1Delegate <NSObject>
- (void)callBack:(NSString *)value;
@end
```

### 🌸 实现协议

创建一个`VC1`实现协议, 实现协议的过程就是给这个对象增加了一个方法, 这个方法的用途是`callback`顾名思义回调方法

```objc
@interface VC1 : NSObject <VC1Delegate>

@end
```


### 🌸 配置代理人

```objc
@interface VC2 : NSObject
@property (weak, nonatomic) id <VC1Delegate> delegate;
@end
```

一说到代理人肯定很多人都懵了, 什么是代理人呢? 其实就是`VC1`的一个实例
1. 为什么要传这个实例呢? 因为要调用回调方法
2. 为什么要调用回调方法? 因为要传值啊, 演示传值
3. 为什么叫代理人, 因为传递的对象是用`id <VC1Delegate>`来接收的, 这个属性名叫`delegate`
4. 为什么要用`id <VC1Delegate>`而不是`VC1 *delegate`呢, 运用了多态的特性, 写出通用代码, 可以少写很多代码,  如果设置成`VC1`那么就只能接受`VC1`的实例, 那么其他的控制器传值的时候还要定义个`VC3`, `VC4`, 这样就不通用了

### 🌸 调用代理

`VC2`使用持有的`VC1`对象调用`callBack`方法进行回调, `VC1`自己就能接到值了, 所以说这整个过程是谁给谁传值, 其实是自己给自己传值, 什么时候传值?  这取决于`VC2`, 他会在你指定的任何时候告诉VC1传值

这就好比你去仓库取个东西, 你需要先`进入仓库`, 仓库管理非常严格, 你需要先登记, 登记就相当于设置代理人了, 然后你进入仓库管理员把东西递给你并`告诉你`快点回家吧, 这个时候你才回去

```objc
- (void)viewDidLoad {
    [super viewDidLoad];
    // 创建控制器1
    VC1 *vc1 = [[VC1 alloc] init];
    // 创建控制器2
    VC2 *vc2 = [[VC2 alloc] init];
    // 把控制器1放在控制器2的属性上, 方便调用代理方法
    vc2.delegate = vc1;
    // 控制器2吃饱了 想告诉你他吃了
    if (vc2.delegate && [vc2.delegate respondsToSelector:@selector(callBack:)]) {
        [vc2.delegate callBack:@"吃过了"];
    }
}
```

### 🌸 完整代码
```objc
@protocol VC1Delegate <NSObject>
- (void)callBack:(NSString *)value;
@end

@interface VC1 : NSObject <VC1Delegate>

@end

@implementation VC1
- (void)callBack:(NSString *)value {
    NSLog(@"VC1接收到值: %@", value);
}
@end

@interface VC2 : NSObject
@property (weak, nonatomic) id <VC1Delegate> delegate;
@end

@implementation VC2
@end

@interface ViewController ()

@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    VC1 *vc1 = [[VC1 alloc] init];
    VC2 *vc2 = [[VC2 alloc] init];
    vc2.delegate = vc1;
    if (vc2.delegate && [vc2.delegate respondsToSelector:@selector(callBack:)]) {
        [vc2.delegate callBack:@"吃过了"];
    }
}
@end
```

### 🌸 传值回复

假如我VC1接到值后想给VC2回个话要怎么做呢, 有两种方法, 第一种是调用VC2提供的传值方法, 第二种就是callback后面跟一个block, 下面就来看看第二种吧

```objc
@protocol VC1Delegate <NSObject>
- (void)callBack:(NSString *)value complete:(void (^) (NSString *))complete;
@end

@interface VC1 : NSObject <VC1Delegate>

@end

@implementation VC1
- (void)callBack:(NSString *)value complete:(void (^)(NSString *))complete {
    NSLog(@"VC1接收到值: %@", value);
    if (complete) {
        complete(@"知道了");
    }
}
@end

@interface VC2 : NSObject
@property (weak, nonatomic) id <VC1Delegate> delegate;
@end

@implementation VC2
@end

@interface ViewController ()

@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    VC1 *vc1 = [[VC1 alloc] init];
    VC2 *vc2 = [[VC2 alloc] init];
    vc2.delegate = vc1;
    if (vc2.delegate && [vc2.delegate respondsToSelector:@selector(callBack:complete:)]) {
        [vc2.delegate callBack:@"吃完了" complete:^(NSString *msg) {
            NSLog(@"VC2接收到值: %@", msg);
        }];
    }
}
@end

```

我们可以看出来第二种稍微好用一点因为不需要借助VC2这个对象, 使用block就能够通信了

# 🍎 Block

block是oc中常用的匿名函数

## 🌲 声明

我们使用`返回值 (^名字) (参数) = ^(参数){}`来声明一个block, 下面就来看看吧

## 🌲 无参数无返回值

```objc
void (^testBlock) (void) = ^{
	NSLog(@"你好block");
};
testBlock();
```

## 🌲 无参数有返回值

```objc
NSString * (^testBlock) (void) = ^{
	return @"你好block";
};
NSLog(@"%@", testBlock());
```

## 🌲 有参数有返回值

```objc
NSString * (^testBlock) (NSString *) = ^(NSString *text){
	return text;
};
NSLog(@"%@", testBlock(@"你好block"));
```

## 🌲 函数参数

我们可以把block用在函数参数上, 我们接下来就看看吧

```objc
- (void)viewDidLoad {
    [super viewDidLoad];
    [self hello:^{
        NSLog(@"说完了");
    }];
}

- (void)hello:(void (^) (void))callback {
    NSLog(@"说");
    if (callback) {
        callback();
    }
}
```

我们发现用在函数上面的block和普通的稍微有一点区别, 那就是它的名字不见了, 只有`^`, 名字会放在参数名上面去, 因为block作为函数的参数, 所以我们使用的时候要先判断一下它是否是空值, 如果不为空再去执行它否则可能会发生崩溃(block为空的情况)

```
Thread 1: EXC_BAD_ACCESS (code=1, address=0x10)
```

## 🌲 属性参数

有时候我们需要把block放在对象的属性上, 主要用于属性传值和定义块方法

```objc
@interface ViewController ()
@property (strong, nonatomic) void (^block) (void);
@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    
    self.block = ^{
        NSLog(@"我牛逼啊");
    };
    
    self.block();
    
}
@end
```

## 🌲 block内部修改局部变量报错

```objc
int main(int argc, char * argv[]) {
    int a = 1;
    void (^testBlock) (void) = ^{
        a = 2;
    };
    testBlock();
    NSLog(@"%d", a);
}
```

很简单的一段代码, 我们发现它报错了

```objc
Variable is not assignable (missing __block type specifier)
```

意思是变量不可赋值, 因为在block中是不能修改局部变量的, 但可以修改全局变量

```objc
int a = 1;

int main(int argc, char * argv[]) {
    void (^testBlock) (void) = ^{
        a = 2;
    };
    testBlock();
    NSLog(@"%d", a);
}
```


如果想要对局部变量进行修改, 需要在变量前面加上`__block`修饰符, 这样在block中就能对变量进行操作了

```objc
int main(int argc, char * argv[]) {
    __block int a = 1;
    void (^testBlock) (void) = ^{
        a = 2;
    };
    testBlock();
    NSLog(@"%d", a);
}
```
但是有一种情况是可以修改的, 就是引用类型是可以修改里面的值, 比如我创建一个对象person, 然后去修改人的名字

```objc
int main(int argc, char * argv[]) {
    Person *person = [[Person alloc] init];
    person.name = @"张三";
    void (^testBlock) (void) = ^{
        person.name = @"李四";
    };
    testBlock();
    NSLog(@"%@", person.name);
}
```

或者是一个可变数组, 我们可以修改容器里面的变量而不用添加`__block`

```objc
int main(int argc, char * argv[]) {
    NSMutableArray *arr = [[NSMutableArray alloc] init];
    void (^testBlock) (void) = ^{
        [arr addObject:@"张三"];
    };
    testBlock();
    NSLog(@"%@", arr);
}
```

# 🍎 NSDecimalNumber

在iOS程序开发中有可能会遇到精度丢失的问题, 这种问题经常出现在后台返回是数字类型小数的时候

```objc
double a = 8;
for (NSInteger i = 0; i < 10; i++) {
	NSString *json = [NSString stringWithFormat:@"{\"price\": %lf}", a += 0.1];
	NSData *jsonData = [json dataUsingEncoding:NSUTF8StringEncoding];
	NSDictionary *dic = [NSJSONSerialization JSONObjectWithData:jsonData options:0 error:nil];
	NSLog(@"%@", [dic[@"price"] stringValue]);
}
/**
22-11-23 10:32:08.734848+0800 ZYKit[5530:104934] 8.1
2022-11-23 10:32:08.735820+0800 ZYKit[5530:104934] 8.199999999999999
2022-11-23 10:32:08.736068+0800 ZYKit[5530:104934] 8.300000000000001
2022-11-23 10:32:08.736265+0800 ZYKit[5530:104934] 8.4
2022-11-23 10:32:08.736438+0800 ZYKit[5530:104934] 8.5
2022-11-23 10:32:08.736610+0800 ZYKit[5530:104934] 8.6
2022-11-23 10:32:08.736776+0800 ZYKit[5530:104934] 8.699999999999999
2022-11-23 10:32:08.737558+0800 ZYKit[5530:104934] 8.800000000000001
2022-11-23 10:32:08.738801+0800 ZYKit[5530:104934] 8.9
2022-11-23 10:32:08.743911+0800 ZYKit[5530:104934] 9
*/
```

我们可以看到8.2, 8.7都不正常, 这个时候如果是直接显示到Label上就会出现大问题, 针对这种问题有两种解决方案

1. 后台传递字符串, 使用字符串传递的数值只要不进行类型转换不会出现精度丢失的问题
2. 后台传递数字, 前端使用`NSDecimalNumber`设置规则

## 🌲 创建NSDecimalNumber

我们下面就使用8.2这个数字进行举例说明, 我们使用`decimalNumberWithString`来创建

```objc
NSString *json = [NSString stringWithFormat:@"{\"price\": \"8.2\"}"];
NSData *jsonData = [json dataUsingEncoding:NSUTF8StringEncoding];
NSDictionary *dic = [NSJSONSerialization JSONObjectWithData:jsonData options:0 error:nil];

NSDecimalNumber *decimalNumber = [NSDecimalNumber decimalNumberWithString:[dic[@"price"] stringValue]];
NSLog(@"%@", decimalNumber);
// 8.199999999999999
```

## 🌲 舍入策略

因为这个数字不精确, 所以我们要配置舍入策略, 比如四舍五入

```objc
NSDecimalNumberHandler *decimalNumberHandler = [NSDecimalNumberHandler										   decimalNumberHandlerWithRoundingMode:NSRoundPlain scale:1 raiseOnExactness:NO raiseOnOverflow:NO raiseOnUnderflow:NO raiseOnDivideByZero:YES];
double price = [decimalNumber decimalNumberByRoundingAccordingToBehavior:decimalNumberHandler].doubleValue;
NSLog(@"%lf", price);
// 8.20000
```

然后我们把它转化为字符串就能进行显示了, 比如我这里是保留一位小数

```objc
NSLog(@"%@", [NSString stringWithFormat:@"%.1lf", price]);
// 8.2
```

上面使用的是四舍五入, 我们来看看其他策略都有哪些

```objc
typedef NS_ENUM(NSUInteger, NSRoundingMode) {
	// 四舍五入
    NSRoundPlain,   // Round up on a tie
    // 永远退位
    NSRoundDown,    // Always down == truncate
    // 永远进位
    NSRoundUp,      // Always up
    // 最后舍入成偶数 比如 1.25不会返回1.3而是1.2，因为1.3不是偶数
    NSRoundBankers  // on a tie round so last digit is even
};
```

# 🍎 空值

我们都知道OC中有三种空, 针对字符串来说 有 nil "" 和 NSNull, 虽然第三种并不是并不是应该属于字符串范畴的, 但是它实打实的有用接下来我就说一说

## 🌲 用途1: 判断空
 
 ```
 - (BOOL)isEmpty:(NSString *)str {
     if ([@"" isEqualToString:str] || str == nil || [str isKindOfClass:[NSNull class]]) {
         return YES;
     } else {
         return NO;
     }
 }
 ```
 
 我们判断字符串应该判断这三种空, 否则第三种情况, 就有可能造成崩溃的问题
 
## 🌲 用途2: 序列化

当我们把想把对象序列化的时候, 如果要表达一个对象中的值是null要怎么表达呢? 你第一想到的应该是nil, 但是很不幸的是几乎所有主流框架都没有这么做, 因为它们区分出来了`nil`和`null`, 而`null`在OC中就代表着`NSNull`, 虽然它不常用但是很有用, 我们接下来就看一看


我们有这样一个对象

```objc
@interface Person : NSObject
@property (strong, nonatomic) NSNumber *id;
@property (strong, nonatomic) NSString *name;
@end
```

当我们初始化后, 把它转化为字典会发生什么呢

```objc
Person *person = [[Person alloc] init];
NSLog(@"%@", [person mj_JSONObject]);
/**
2022-10-12 16:53:54.570622+0800 ZYKit[28132:432746] {
}
*/
```

答案是空, 因为对象里面的初始值都是nil, 又因为`MJExtension`这款第三方解析库默认是不去序列化这些为nil值的属性的, 所以是空, 这没什么毛病

我们想序列化就要先赋值对吧

```objc
Person *person = [[Person alloc] init];
person.id = @1;
person.name = @"张三";
NSLog(@"%@", [person mj_JSONObject]);
/**
2022-10-12 16:57:31.566715+0800 ZYKit[28288:435789] {
    id = 1;
    name = "张三";
}
*/
```

这看起来也没有什么问题, 我们似乎不需要null

但是有一种情况你可能想要构造这种数据, 你只使用nil和字符串是办不到的

```objc
2022-10-12 16:59:06.481932+0800 ZYKit[28357:437550] {
    id = 1;
    name = "<null>";
}
```

说到这里杠精又问了, 我不需要!  这玩意有啥用啊

那我问你, 如果想向数据库中插入null值你要怎么做呢? 除非手写SQL, 如果是全量插入你都要做序列化去解析插入的, 这个时候想要表达一个值为null, 那么`NSNull`将是你唯一的解, 你无法使用nil来转化成null, 因为不对等, nil表示的是没有, NSNull表示的空对象, 这里是不能去混乱的

## 🌲 nil - NSNull - null - None

提到这四个值, 我们就要引出两个新的语言`Java`和`Python`, 我们以下的言论都依据于成员变量的属性是对象和包装类型的情况下, 不讨论基本数据类型

### 🌸 初始值比较

Java: null
Python: 
OC: nil

你可能看不懂, 我来说明一下

`Java`中如果你初始化一个`自定义对象`, 里面有声明了一个字符串类型的成员变量name, 它的值默认会被置为null, 当你使用它时会报出空指针异常

```java
public static void main(String[] args) {
	User user = new User();
	System.out.println(user.name); // null
	System.out.println(user.name.length());
}
/**
Exception in thread "main" java.lang.NullPointerException
	at com.objcat.playground.test.Test.main(Test.java:24)
*/
```

`Python`中如果你初始化一个`自定义对象`, 里面声明了一个字符串类型的成员变量name, 它默认是不加入到对象中的, 当你打印它的时候会报出`AttributeError: 'User' object has no attribute 'name'`异常

```python
user = User()
print(user.name)
```

而OC中默认会置为nil, 打印和使用它都是没有问题的

```objc
Person *person = [[Person alloc] init];
NSLog(@"%@", person.name); // (null)
NSLog(@"%lu", person.name.length); // 0
```

我们再来看一下nil和NSNull的区别

```objc
NSLog(@"%@", nil); // (null)
NSLog(@"%@", [NSNull null]); // <null>
```

## 🌲 第三方框架解析思路

### 🌸 MJExtension

1. 默认不序列化为nil的属性, 当遇到`NSNull`的时候会有两种情况, 如果是实体转字典, 那么会解析成`NSNull`以便于占位, 因为字典不允许存在nil值, 如果是实体转字符串, 那么会解析成`Json`所认识的`null`

2. 相反的, 当字典转实体的时候, 里面的`NSNull`因为和对象里面的值类型不符合, 会被解析成nil

#### 🌵 转换丢失问题

上述两点会导致一个问题, 就当一个模型转换成`字典`或`Json`后, 再转换回实体的时候, 转换不是等价的, 因为所有`NSNull`的值会被转为nil, 这是由于转换的过程中进行了类型判断, 如果类型不同就无法互转

```objc
- (void)testExample {
    Person *person = [[Person alloc] init];
    person.id = @1;
    person.name = [NSNull null];
    NSLog(@"%@", [person mj_JSONObject]);
    Person *person2 = [Person mj_objectWithKeyValues:[person mj_JSONObject]];
    NSLog(@"%@", [person2 mj_JSONObject]);
}

/**
2022-10-13 09:34:27.711513+0800 ZYKit[3092:46071] {
    id = 1;
    name = "<null>";
}
2022-10-13 09:34:27.712339+0800 ZYKit[3092:46071] {
    id = 1;
}
*/
```

不过分析下来, 这个问题兵不严重, 因为不会有太多的人去使用这个`NSNull`类型, 除非想做数据库插入空值

# 🍎 ({})写法

```objc
self.myView = ({
	UIView *view = [[UIView alloc] init];
	view.backgroundColor = [UIColor redColor];
	view.frame = CGRectMake(100, 100, 100, 100);
	view;
});
NSLog(@"%@", self.myView);
```

# 🍎 可变参数

## 🌲 可变参数读取

可变参数是从C语言传承下来的一种传参数的方法, 可以获取无限多的参数, 在OC中常见的函数实现是`arrayWithObjects`或是`NSLog`

我们来先看一下`arrayWithObjects`的使用方法

```objc
NSArray *arr = [NSArray arrayWithObjects:@"1", @"2", @"3", nil];

声明是这样的

+ (instancetype)arrayWithObjects:(ObjectType)firstObj, ... NS_REQUIRES_NIL_TERMINATION;
```

那么实现原理到底是什么呢, 实际上就是用了我们的可变参数了, 我们也仿照写一个方法来获取所有传进来的参数

```objc

/// 获取参数列表
/// @param firstArg 第一个参数
/// @param ... 可以无限向后拼接的参数
- (NSMutableArray *)argsList:(NSString *)firstArg, ... NS_REQUIRES_NIL_TERMINATION {
    // 创建一个数组来装参数
    NSMutableArray *argsList = [NSMutableArray array];
    // 装进第一个参数
    [argsList addObject:firstArg];
    // 声明ap指针
    va_list ap;
    // 让ap指针，使其指向第一个可变参数, firstArg是变参列表的前一个参数
    va_start(ap, firstArg);
    // 声明一个字符串对象obj
    NSString *obj = nil;
    // 开启无限循环
    while (1) {
        // va_arg 该宏返回当前变参值, 并使ap指向列表中的下个变参
        NSString *arg = obj = va_arg(ap, NSString *);
        if (arg) {
            // 获取到非空参数直接放进数组
            [argsList addObject:obj];
        } else {
            // 当参数为nil证明到了最后一个 white循环结束
            break;
        }
    }
    va_end(ap);
    return argsList;
}

// 使用
NSLog(@"%@", [self argsList:@"1", @"2", @"3", nil]);

注意结尾必须是nil否则上述方法会陷入死循环
```

## 🌲 重写NSLog

既然知道可变参数的原理, 我们就可以重写NSLog了, 我们使用NSLog提供的方法NSLogv

```objc
void ZYLog(NSString *format, ...) {
    va_list ap;
    va_start(ap, format);
    NSLogv(format, ap);
    va_end(ap);
}
```

## 🌲 打印详情信息

```objc
void ZYLog(NSString *format, ...) {
#ifdef DEBUG
    // 文件名
    NSString *fileName = [NSString stringWithCString:__FILE_NAME__ encoding:NSUTF8StringEncoding];
    // 获取调用堆栈
    NSArray *callStackSymbols = [NSThread callStackSymbols];
    NSString *callStackString = @"";
    if (callStackSymbols.count > 0) {
        // 获取上层方法调用堆栈信息
        callStackString = [NSThread callStackSymbols][1];
    }
    // 提取方法名
    NSString *funcName = [ViewController zy_firstMatchWithString:callStackString Partten:@"[+|-]\\[.*?]"];
    NSLog(@"\n[ 文件名: %@ ] \n[ 方法名: %@ ] \n[ 行号: %d ]", fileName, funcName, __LINE__);
    
    va_list ap;
    va_start(ap, format);
    NSLogv(format, ap);
    va_end(ap);
#endif
}

+ (NSString *)zy_firstMatchWithString:(NSString *)string Partten:(NSString *)pattern {
    NSTextCheckingResult *result = [[NSRegularExpression regularExpressionWithPattern:pattern options:0 error:nil] firstMatchInString:string options:0 range:NSMakeRange(0, string.length)];
    return [string substringWithRange:result.range];
}

/**
2022-09-06 17:00:14.982499+0800 playground_app_oc[23531:453052] 
[ 文件名: ViewController.m ] 
[ 方法名: -[ViewController viewDidLoad] ] 
[ 行号: 101 ]
2022-09-06 17:00:14.982752+0800 playground_app_oc[23531:453052] 123
*/
```

## 🌲 别人写的宏

```objc
#ifdef DEBUG
#define DHLog(fmt, ...) NSLog((@"[文件名:%s]\n" "[函数名:%s]\n" "[行号:%d] \n" fmt), __FILE__, __FUNCTION__, __LINE__, ##__VA_ARGS__);
#else
#define DHLog(...);
#endif
```

# 🍎 属性修饰符

## 🌲 atomic/nonatomic

在多个线程下去修改同一个变量的值可能存在线程安全问题

```objc
@property (strong, nonatomic) NSString *name;

for (NSInteger i = 0; i < 10000; i++) {
	dispatch_async(dispatch_get_global_queue(0, 0), ^{
		self.name = [NSString stringWithFormat:@"第%ld个", i];
		NSLog(@"%@", self.str);
	});
}
```

我们可以看一下`ARC`中的`setter`方法的实现

```objc
- (void)setName:(NSString *)name{
    if (_name != name) {
	    // 释放旧值
        [_name release];
        // 让新值引用计数+1
        [name retain];
        // 赋值给内部变量
        _name = name;
    }
}
```

`ARC`中编译器在编译过程中会给自动插入retain和release

这就导致了多线程情况下获取到了同一个`_name`出现两次`release`导致过度释放而崩溃

怎么解决呢 其中的一个方案就是加锁, 让锁中的内容每次只有一个线程可以执行

```objc
NSLock *lock = [[NSLock alloc] init];
for (NSInteger i = 0; i < 10000; i++) {
	dispatch_async(dispatch_get_global_queue(0, 0), ^{
		[lock lock];
		self.str = [NSString stringWithFormat:@"第%ld个", i];
		NSLog(@"%@", self.str);
		[lock unlock];
	});
}
```

这样就不会发生崩溃

我们也可以使用`atomic`来修饰属性 atomic底层会调用`objc_setProperty_atomic`然后调用函数`reallySetProperty`, 里面会加线程锁保证赋值操作同一时间内只有一个线程执行

```objc
@property (strong, atomic) NSString *str;

for (NSInteger i = 0; i < 10000; i++) {
	dispatch_async(dispatch_get_global_queue(0, 0), ^{
		self.str = [NSString stringWithFormat:@"第%ld个", i];
		NSLog(@"%@", self.str);
	});
}
```

`atomic`的官方实现

https://blog.csdn.net/weixin_39950838/article/details/107832407

## 🌲 atomic能保证线程安全吗

首先说一下线程安全的概念, 就是自始至终得到的值都是正确的, 而不是不崩溃就是安全的

答案是`atomic`不能保证线程安全, 它只能保证在`setter`和`getter`的时候值是安全的, 也就是不会发生由于资源抢占释放造成的崩溃, 另外对于可变型容器比如可变数组, 在进行元素修改的时候也是不能保证安全的

我们来举几个例子

### 🌸 资源抢占导致程序崩溃

这个上面的文章已经讲过了, 这里就不多加赘述了

### 🌸 可变对象导致崩溃

```objc
self.arr = [NSMutableArray array];
for (NSInteger i = 0; i < 10000; i++) {
	dispatch_async(dispatch_get_global_queue(0, 0), ^{
		[self.arr addObject:@(i)];
		NSLog(@"%ld", i);
	});
}
```

解决方案也是加锁

```objc
NSLock *lock = [[NSLock alloc] init];
self.arr = [NSMutableArray array];
for (NSInteger i = 0; i < 10000; i++) {
	dispatch_async(dispatch_get_global_queue(0, 0), ^{
		[lock lock];
		[self.arr addObject:@(i)];
		NSLog(@"%ld", i);
		[lock unlock];
	});
}
```

也可以使用`syncchronized`, 但是一个synchronized执行两次也是有概率会出现崩溃的

```objc
@synchronized (self) {
	self.arr = [NSMutableArray array];
	for (NSInteger i = 0; i < 10000; i++) {
		dispatch_async(dispatch_get_global_queue(0, 0), ^{
			[self.arr addObject:@(i)];
			NSLog(@"%ld", i);
		});
	}
}
```

### 🌸 资源抢占导致线程不安全

我们用卖票举例, 开若干个线程去卖票

```objc
@interface ViewController ()
@property (assign, nonatomic) NSInteger tickets;
@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    self.tickets = 1000;
    
    NSOperationQueue *queue = [[NSOperationQueue alloc] init];
    for (NSInteger i = 1; i <= 1000; i++) {
        [queue addOperationWithBlock:^{
            self.tickets--;
            NSLog(@"第%ld次 售票员1 剩余%ld", i, self.tickets);
        }];
    }
}
```

按理来说, 应该卖出所有的票, 但实际运行结果0~40之间浮动, 为什么票没有卖出去?

因为不同的线程去读取同一个内存资源有可能出现读取相同的情况, 这种情况出现后会发现票少减了1, 比如有两个线程同时读到剩余票为50, 那么减完的值本来应该是48, 因为少减了1成49, 所以会出现少卖票的情况, 我们把`nonatomic`改成`atomic`发现问题并没有解决

解决方案是加锁

```objc
@interface ViewController ()
@property (assign, nonatomic) NSInteger tickets;
@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    self.tickets = 1000;
    
    NSOperationQueue *queue = [[NSOperationQueue alloc] init];
    NSLock *lock = [[NSLock alloc] init];
    for (NSInteger i = 1; i <= 1000; i++) {
        [queue addOperationWithBlock:^{
            [lock lock];
            self.tickets--;
            NSLog(@"第%ld次 售票员1 剩余%ld", i, self.tickets);
            [lock unlock];
        }];
    }
}
```

## 🍎 Swizzling

## 🌲 类目Swizzling

黑魔法`swizzling`的实现主要是靠一个方法叫做`method_exchangeImplementations`, 里面传递两个参数, 旧IMP, 新IMP, 交换后的作用相当于一个hook(钩子), 可以在捕获的方法执行前去执行一段代码

```objc
+ (void)swizzleWithOldSelector:(SEL)oldSelector newSelector:(SEL)newSelector {
    // 新类型
    Class classz = [self class];
    // 原有方法
    Method oldMethod = class_getInstanceMethod(classz, oldSelector);
    // 替换原有方法的新方法
    Method newMethod = class_getInstanceMethod(classz, newSelector);
    // 交换两个方法
    method_exchangeImplementations(oldMethod, newMethod);
}
```

我们使用最简单的`viewDidLoad`来举例子

```objc
#import "UIViewController+ZYSwizzling.h"
#import <objc/runtime.h>

@implementation UIViewController (ZYSwizzling)

+ (void)load {
    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
        [self swizzleWithOldSelector:@selector(viewDidLoad) newSelector:@selector(swizzle_viewDidLoad)];
    });
}

+ (void)swizzleWithOldSelector:(SEL)oldSelector newSelector:(SEL)newSelector {
    // 新类型
    Class classz = [self class];
    // 原有方法
    Method oldMethod = class_getInstanceMethod(classz, oldSelector);
    // 替换原有方法的新方法
    Method newMethod = class_getInstanceMethod(classz, newSelector);
    // 交换两个方法
    method_exchangeImplementations(oldMethod, newMethod);
}

- (void)swizzle_viewDidLoad {
    NSLog(@"成功捕获viewDidLoad");
    NSLog(@"%@", self);
    [self swizzle_viewDidLoad];
}
@end

// 当前类

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    NSLog(@"viewDidLoad");
}

@end

// 父类

@implementation ZYKitBaseViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    NSLog(@"superViewDidLoad");
}

@end

最常用的方式是在类目中全局`钩`方法 例如

我们交换`viewDidLoad`和`swizzle_viewDidLoad`, 那么执行顺序是什么样呢

+ (void)load
+ (void)swizzleWithOldSelector:(SEL)oldSelector newSelector:(SEL)newSelector
当前类 - (void)viewDidLoad
当前类 - [super viewDidLoad]
父类 - (void)viewDidLoad
父类 - [super viewDidLoad]
类目 - (void)swizzle_viewDidLoad
类目 - NSLog(@"swizzle_viewDidLoad");
类目 - NSLog(@"%@", self);
类目 - [self swizzle_viewDidLoad];
父类 - NSLog(@"superViewDidLoad");
当前类 - NSLog(@"viewDidLoad");

我们可以看到

尽管先走了`当前类`的`viewDidLoad`然后跳到`父类`的`viewDidLoad`, 但是并没有先执行里面的打印, 在父类的`[super viewDidLoad]`中直接跳到了`swizzle_viewDidLoad`, 因为方法被交换了, `viewDidLoad`找不到父类的`selector`就会跳入`IMP`, 这个时候IMP早就被`swizzle_viewDidLoad`, 所以自然就执行`swizzle_viewDidLoad`, 所以可以说`swizzle_viewDidLoad`优先执行

你可能会有一个问题`[self swizzle_viewDidLoad]`是不是写错了, 不会递归吗

答案是不会, 因为`swizzle_viewDidLoad`的IMP被换成了`viewDidLoad`了, 执行`[self swizzle_viewDidLoad]`就会跳到`viewDidLoad`

但是在实际编码中我们发现类目中的`[self swizzle_viewDidLoad]`去掉对结果没有影响, 执行完`swizzle_viewDidLoad`还是会继续执行`viewDidLoad`, 所以我写了另外一个例子
```


## 🌲 类Swizzling

```objc
@implementation ViewController

+ (void)load {
    static dispatch_once_t onceToken;
	dispatch_once(&onceToken, ^{
    [self swizzleWithOldSelector:@selector(viewDidLoad) newSelector:@selector(swizzle_viewDidLoad)];
    });
}

- (void)swizzle_viewDidLoad {
    NSLog(@"swizzle_viewDidLoad: %@", self);
    [self swizzle_viewDidLoad];
}

- (void)viewDidLoad {
    [super viewDidLoad];
    NSLog(@"viewDidLoad");
}

@end

与类目不同的是, 这次把代码放在控制器中, 我们发现执行顺序有些不同

+ (void)load
- (void)swizzle_viewDidLoad
NSLog(@"swizzle_viewDidLoad: %@", self);
[self swizzle_viewDidLoad];
当前类 - (void)viewDidLoad
当前类 - [super viewDidLoad]
父类 - (void)viewDidLoad
父类 - [super viewDidLoad]
父类 - NSLog(@"superViewDidLoad");
当前类 - NSLog(@"viewDidLoad");

如果我们这次去掉`[self swizzle_viewDidLoad];`, 会发现当前类和父类的`viewDidLoad`就都不会执行了, 所以`[self swizzle_viewDidLoad]`在任何时候最好还是加上, 除非你不想执行下面的`viewDidLoad`方法了

```

## 🌲 类蔟Swizzling

有一种类很特殊, 不同的方法初始化可能是多个类其中的一种, 称为类蔟(Class Clusters), 如NSNumber, NSArray, NSDictionary等, 在我理解它与多态类似, 父类指针指向子类, 不过用皮的时候都用的是父类

那么这与我们`swizzling`有什么关系呢, 答案是有关系, 需要使用`真实类`来钩, 下面我们就来看看吧

### 🌸 防止数组插入空值(错误方法)

```objc

#import <objc/runtime.h>
@implementation NSMutableArray (ZYSwizzling)

+ (void)load {
    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
        [self swizzleWithOldSelector:@selector(addObject:) newSelector:@selector(swizzling_addObject:)];
    });
}

+ (void)swizzleWithOldSelector:(SEL)oldSelector newSelector:(SEL)newSelector {
    // 新类型
    Class classz = [self class];
    // 原有方法
    Method oldMethod = class_getInstanceMethod(classz, oldSelector);
    // 替换原有方法的新方法
    Method newMethod = class_getInstanceMethod(classz, newSelector);
    // 交换两个方法
    method_exchangeImplementations(oldMethod, newMethod);
}

- (void)swizzling_addObject:(id)object {
    if (object == nil) {
        NSLog(@"元素为空不执行");
        return;
    }
    [self swizzling_addObject:object];
}

@end

NSMutableArray *arr = [NSMutableArray array];
[arr addObject:nil];

我们hook了addObject方法, 然后向数组中插入nil, 发现程序崩了

经过调试发现我们的方法hook失败了, 原因就出在类蔟上, 因为可变素组的类型其实是`__NSArrayM`, 所以hook`NSMutableArray`是没有用的, 我们需要改写一下方法
```

### 🌸  防止数组插入空值(改正)

```objc
+ (void)swizzleWithOldSelector:(SEL)oldSelector newSelector:(SEL)newSelector {
    // 新类型
    Class newClass = [self class];
    // 老类型
    Class oldClass = [self reallyClassWithObject:self];
    // 原有方法
    Method oldMethod = class_getInstanceMethod(oldClass, oldSelector);
    // 替换原有方法的新方法
    Method newMethod = class_getInstanceMethod(newClass, newSelector);
    // 交换两个方法
    method_exchangeImplementations(oldMethod, newMethod);
}

// 对象获取真实类型 针对类蔟类型 Array Dictionary
+ (Class)reallyClassWithObject:(id)object {
    if ([[object class] isEqual:[NSArray class]]) {
        return NSClassFromString(@"__NSArrayI");
    } else if ([[object class] isEqual:[NSMutableArray class]]) {
        return NSClassFromString(@"__NSArrayM");
    } else if ([[object class] isEqual:[NSDictionary class]]) {
        return NSClassFromString(@"__NSDictionaryI");
    } else if ([[object class] isEqual:[NSMutableDictionary class]]) {
        return NSClassFromString(@"__NSDictionaryM");
    }
    return [object class];
}

改写后我们hook就可以抓到`addObject`方法了
```

### 🌸  类蔟打印

```objc
NSArray *arr = [NSArray array];
NSLog(@"%s", object_getClassName(arr));

NSArray *arr2 = @[@"1"];
NSLog(@"%s", object_getClassName(arr2));

NSArray *arr3 = @[@"1", @"2"];
NSLog(@"%s", object_getClassName(arr3));

NSMutableArray *marr = [NSMutableArray array];
NSLog(@"%s", object_getClassName(marr));

NSDictionary *dic = [NSDictionary dictionary];
NSLog(@"%s", object_getClassName(dic));

NSDictionary *dic2 = @{@"name": @"张三"};
NSLog(@"%s", object_getClassName(dic2));

NSDictionary *dic3 = @{@"name": @"张三", @"age": @"18"};
NSLog(@"%s", object_getClassName(dic3));

NSMutableDictionary *mdic = [NSMutableDictionary dictionary];
NSLog(@"%s", object_getClassName(mdic));

2022-07-28 10:22:06.462349+0800 ZYKit[5818:99884] __NSArray0
2022-07-28 10:22:06.462498+0800 ZYKit[5818:99884] __NSSingleObjectArrayI
2022-07-28 10:22:06.462625+0800 ZYKit[5818:99884] __NSArrayI
2022-07-28 10:22:06.462736+0800 ZYKit[5818:99884] __NSArrayM

2022-07-28 10:22:06.462848+0800 ZYKit[5818:99884] __NSDictionary0
2022-07-28 10:22:06.462967+0800 ZYKit[5818:99884] __NSSingleEntryDictionaryI
2022-07-28 10:22:06.463075+0800 ZYKit[5818:99884] __NSDictionaryI
2022-07-28 10:22:06.463182+0800 ZYKit[5818:99884] __NSDictionaryM

```

## 🍎 指针

## 🌲 交换变量

```objc
#import <Foundation/Foundation.h>

@interface Algo : NSObject

@end

@implementation Algo : NSObject

+ (void)swap:(NSString *)str1 str2:(NSString *)str2 {
    NSString *temp = str2;
    str2 = str1;
    str1 = temp;
}

+ (void)swap2:(NSString **)str1 str2:(NSString **)str2 {
    NSString *temp = *str2;
    *str2 = *str1;
    *str1 = temp;
}

@end

typedef NSString * String;

int main(int argc, char * argv[]) {
    NSString *a = @"张三";
    NSString *b = @"李四";
    
    [Algo swap:a str2:b];
    NSLog(@"%@", a);
    NSLog(@"%@", b);
    
    [Algo swap2:&a str2:&b];
    NSLog(@"%@", a);
    NSLog(@"%@", b);
}

/**
2022-07-29 10:29:46.013225+0800 playground_cmd_oc[6418:83189] 张三
2022-07-29 10:29:46.014202+0800 playground_cmd_oc[6418:83189] 李四
2022-07-29 10:29:46.014281+0800 playground_cmd_oc[6418:83189] 李四
2022-07-29 10:29:46.014327+0800 playground_cmd_oc[6418:83189] 张三
*/

由打印可以看出, 想要交换变量需要传递指针, oc中引用类型默认带* 所以使用**来表示指针

```

# 🍎 归档/反归档

归档和反归档是数据持久化的一种方案, 其作用就是让自定义对象和数据流可以无缝转换, 虽然在项目开发中不常见, 但是作为一个功能, 我们还是要了解一下的

## 🌲 NSCoading

### 🌸 归档

归档本身并不复杂, 它需要自定义对象签署`NSCoading`协议并实现`encodeWithCoder:`方法即可

```objc
#import <Foundation/Foundation.h>

@interface Student : NSObject <NSCoding>
@property (strong, nonatomic) NSString *name;
@end

#import "Student.h"

@implementation Student

// 归档实现方法
- (void)encodeWithCoder:(NSCoder *)coder {
    [coder encodeObject:self.name forKey:@"name"];
}

@end
```

我们使用`NSKeyedArchiver`来归档

```objc
Student *stu = [[Student alloc] init];
stu.name = @"张三";
NSError *err = nil;
self.data = [NSKeyedArchiver archivedDataWithRootObject:stu requiringSecureCoding:NO error:&err];
if (!err) {
	NSLog(@"%@", self.data);
} else {
	NSLog(@"%@", err);
}
```

我们打印data发现`Student`已经转化成数据流了

### 🌸 反归档

反归档和很简单, 只需要实现`initWithCoder:`方法即可

```objc
#import "Student.h"

@implementation Student

- (nullable instancetype)initWithCoder:(NSCoder *)coder {
    self = [super init];
    if (self) {
        self.name = [coder decodeObjectForKey:@"name"];
    }
    return self;
}

- (void)encodeWithCoder:(NSCoder *)coder {
    [coder encodeObject:self.name forKey:@"name"];
}
```

我们来尝试一下反归档

```objc
Student *stu = [NSKeyedUnarchiver unarchiveTopLevelObjectWithData:self.data error:&err];
if (!err) {
	NSLog(@"%@", stu.name);
} else {
	NSLog(@"%@", err);
}
```

我们可以看到`data`已经反归档成`Student`对象了, 但是你会发现报了一个警告`'unarchiveTopLevelObjectWithData:error:' is deprecated: first deprecated in iOS 12.0 - Use +unarchivedObjectOfClass:fromData:error: instead`
想要去除警告我们需要使用`NSSecureCoding`, 这需要iOS11以上

## 🌲 NSSecureCoding

NSSecureCoding是NSCoading的升级版, 里面加入了`supportsSecureCoding`来保证安全

### 🌸 归档

requiringSecureCoding设置为YES即可

```objc
Student *stu = [[Student alloc] init];
stu.name = @"张三";
NSError *err = nil;
self.data = [NSKeyedArchiver archivedDataWithRootObject:stu requiringSecureCoding:YES error:&err];
if (!err) {
	NSLog(@"%@", self.data);
} else {
	NSLog(@"%@", err);
}
```

### 🌸 反归档

使用`unarchivedObjectOfClass:::`来代替`unarchiveTopLevelObjectWithData::`方法

```objc
- (IBAction)click2:(id)sender {
    NSError *err = nil;
    Student *stu = [NSKeyedUnarchiver unarchivedObjectOfClass:[Student class] fromData:self.data error:&err];
    if (!err) {
        NSLog(@"%@", stu.name);
    } else {
        NSLog(@"%@", err);
    }
}
```

但是我们发现会出现另外一个异常

```objc
 *** -[NSKeyedUnarchiver validateAllowedClass:forKey:] allowed unarchiving safe plist type ''NSString' (0x7fff86500530) [/Applications/Xcode13.app/Contents/Developer/Platforms/iPhoneOS.platform/Library/Developer/CoreSimulator/Profiles/Runtimes/iOS.simruntime/Contents/Resources/RuntimeRoot/System/Library/Frameworks/Foundation.framework]' for key 'name', even though it was not explicitly included in the client allowed classes set: '{(
    "'Student' (0x106809810) [/Users/objcat/Library/Developer/CoreSimulator/Devices/D43DA085-E7CC-4C9F-BEAB-F79909040B77/data/Containers/Bundle/Application/824A5412-3583-4725-85F6-CCDE86768146/NSCoding\U8c03\U7814.app]"
)}'. This will be disallowed in the future.
```

解决方案是修改反归档的方法

```objc
self.name = [coder decodeObjectForKey:@"name"];
替换为
self.name = [coder decodeObjectOfClass:[NSString class] forKey:@"name"];
```

替换之后我们发现警告消失

# 🍎 NSLock

线程锁, 为什么叫线程锁呢, 它会保证在同一个时间内只有一个线程可以访问被锁定的代码, 我们来看一下例子

首先是没有锁的

```objc
NSOperationQueue *queue = [[NSOperationQueue alloc] init];
for (NSInteger i = 0; i < 10; i++) {
	[queue addOperationWithBlock:^{
		NSLog(@"任务%ld", i);
	}];
}
```

理所当然打印无序的 任务0 ~ 任务9

然后是加了延时的

```objc
NSOperationQueue *queue = [[NSOperationQueue alloc] init];
for (NSInteger i = 0; i < 10; i++) {
	[queue addOperationWithBlock:^{
		sleep(3);
		NSLog(@"任务%ld", i);
	}];
}
```

现象是等待3秒钟后打印随机的 任务0 ~ 任务9 (因为线程是同时执行的, 同时开始记秒并且是无序的)

然后我们使用`NSLock`上锁, 此锁为线程锁, 保证同一时间只有一块代码执行, 现象就是任务一个一个执行了

```objc
NSLock *lock = [[NSLock alloc] init];
NSOperationQueue *queue = [[NSOperationQueue alloc] init];
    for (NSInteger i = 0; i < 10; i++) {
        [queue addOperationWithBlock:^{
            [lock lock];
            sleep(3);
            NSLog(@"任务%ld", i);
            [lock unlock];
        }];
    }
```

现象是 3秒 -> 任务0~9其中的一个 -> 3秒 -> 任务0~9其中的一个

我们再来修改一下

```objc
NSLock *lock = [[NSLock alloc] init];
NSOperationQueue *queue = [[NSOperationQueue alloc] init];
for (NSInteger i = 0; i < 10; i++) {
	[queue addOperationWithBlock:^{
		NSLog(@"未加锁区间%ld", i);
		[lock lock];
		sleep(3);
		NSLog(@"任务%ld", i);
		[lock unlock];
		NSLog(@"未加锁区间下%ld", i);
	}];
}
```

我们可以看到代码增加了未加锁区间, 执行代码的时候我们会发现

```
2022-08-22 17:22:34.276516+0800 回调地狱解决[40257:383468] 未加锁区间上1
2022-08-22 17:22:34.276516+0800 回调地狱解决[40257:383464] 未加锁区间上0
2022-08-22 17:22:34.276556+0800 回调地狱解决[40257:383465] 未加锁区间上2
2022-08-22 17:22:34.276606+0800 回调地狱解决[40257:383470] 未加锁区间上4
2022-08-22 17:22:34.276574+0800 回调地狱解决[40257:383463] 未加锁区间上3
2022-08-22 17:22:34.276651+0800 回调地狱解决[40257:383467] 未加锁区间上5
2022-08-22 17:22:34.277155+0800 回调地狱解决[40257:383493] 未加锁区间上6
2022-08-22 17:22:34.277567+0800 回调地狱解决[40257:383494] 未加锁区间上7
2022-08-22 17:22:34.279353+0800 回调地狱解决[40257:383495] 未加锁区间上8
2022-08-22 17:22:34.279412+0800 回调地狱解决[40257:383496] 未加锁区间上9
2022-08-22 17:22:37.277161+0800 回调地狱解决[40257:383468] 任务1
2022-08-22 17:22:37.277551+0800 回调地狱解决[40257:383468] 未加锁区间下1
2022-08-22 17:22:40.281822+0800 回调地狱解决[40257:383464] 任务0
2022-08-22 17:22:40.282329+0800 回调地狱解决[40257:383464] 未加锁区间下0
2022-08-22 17:22:43.283272+0800 回调地狱解决[40257:383465] 任务2
2022-08-22 17:22:43.283480+0800 回调地狱解决[40257:383465] 未加锁区间下2
2022-08-22 17:22:46.286063+0800 回调地狱解决[40257:383470] 任务4
2022-08-22 17:22:46.286270+0800 回调地狱解决[40257:383470] 未加锁区间下4
2022-08-22 17:22:49.289300+0800 回调地狱解决[40257:383467] 任务5
2022-08-22 17:22:49.289743+0800 回调地狱解决[40257:383467] 未加锁区间下5
2022-08-22 17:22:52.294645+0800 回调地狱解决[40257:383463] 任务3
2022-08-22 17:22:52.295004+0800 回调地狱解决[40257:383463] 未加锁区间下3
```

结论是锁只是锁定了`[lock lock]`里面和下面的代码, 上面的代码会优先执行, 这样的程序让人提升到另外一个境界, 就是一段程序是可以拆分执行的

# 🍎 @synchronized

线程锁, 与`NSLock`类似

```objc
NSOperationQueue *queue = [[NSOperationQueue alloc] init];
    for (NSInteger i = 0; i < 10; i++) {
        [queue addOperationWithBlock:^{
            NSLog(@"未加锁区间上%ld", i);
            @synchronized (self) {
                sleep(3);
                NSLog(@"任务%ld", i);
            }
            NSLog(@"未加锁区间下%ld", i);
        }];
    }
```

锁定`{}`中的代码块, 同一时间只允许一个线程访问

# 🍎 @try

try是oc中异常处理常用的方法, 但是由于oc是弱异常处理语言, 没有强制要求捕获异常, 除了大型框架, 并没有多少人去使用, 我们接下来就看看如何使用吧

## 🌲 用法

我们可以看到, try语法分为`@try`, `@catch`和`@finally`

```objc
@try {
	<#Code that can potentially throw an exception#>
} @catch (NSException *exception) {
	<#Handle an exception thrown in the @try block#>
} @finally {
	<#Code that gets executed whether or not an exception is thrown#>
}
```

- `@try`用来写代码, 代码中常常是包含会抛出异常的, 在java中如果方法可能会抛出异常会强制使用`try/catch`, 但是oc不会
- `@catch`用来处理异常, 如果代码发生了异常你会怎么做, 通常是进行打印或错误之后的处理
- `@finally`可以不写, 这里的意思是代码最后会执行什么, 不管是否抛出异常都会执行`finally`

## 🌲 例子

我们可以看下面一段代码, 如果`a=1`我就新建一个`exception`并使用`[exception raise]`抛出异常

```objc
int a = 1;
if (a == 1) {
	NSException *exception = [NSException exceptionWithName:@"a=1不行" reason:@"a=1我认为这是一个异常" userInfo:nil];
	[exception raise];
}
```

运行结果就是程序崩溃了

```shell
2022-09-13 16:00:17.085953+0800 ZYKit[34457:373989] *** Terminating app due to uncaught exception 'a=1', reason: 'a=1我认为这是一个异常'
```

我们可以看到崩溃结果是这个

那我们如果捕获异常要怎么写呢

```objc
int a = 1;

@try {
	if (a == 1) {
		NSException *exception = [NSException exceptionWithName:@"a=1不行" reason:@"a=1我认为这是一个异常" userInfo:nil];
		[exception raise];
	}
} @catch (NSException *exception) {
	NSLog(@"%@", exception);
} @finally {
	NSLog(@"finally");
}
```

再次运行代码, 我们发现不会发生崩溃了

```objc
2022-09-13 16:05:55.725092+0800 ZYKit[34687:379133] a=1我认为这是一个异常
2022-09-13 16:05:55.725603+0800 ZYKit[34687:379133] finally
```

但是在打印中, 我们可以看到异常的`reason`被打印出来了, 但是程序没有发生崩溃, 因为我们的异常被捕获了

# 🍎 Thread

## 🌲 打印调用堆栈

```objc
NSLog(@"%@", [NSThread callStackSymbols]);
```

# 🍎 GCD

GCD全称(Grand Central Dispatch)翻译过来是大中央调度, 是iOS多线程变成重要的组成部分

## 🌲 主队列 / 全局队列

队列是`GCD`中非常重要的概念, 把任务抛到队列中是执行调度的原理, 在iOS中系统给我们创建了两个队列, 一个是主队列, 一个是全局队列, 主队列是串行队列, 全局队列是并行队列

```objc
// 主队列
dispatch_get_main_queue()
// 全局队列
dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0)
```

## 🌲 调度 / 抛

调度 / 抛 分为同步和异步, 分别是`dispatch_sync`和`dispatch_async`, 对应着同步抛和异步抛, 同步抛就是抛入的任务会立即执行, 异步抛就是抛入的任务会等主线程任务结束后执行, 所以在代码中体现就是异步会在后面执行

执行任务是用抛的方式 把xx任务抛到某个队列中 队列会分配合适的线程去执行任务
- 抛到主队列中的任务由主线程执行
- 抛到全局队列中的任务 可能由主线程执行 也可能由子线程执行

```objc
dispatch_sync(<#dispatch_queue_t  _Nonnull queue#>, <#^(void)block#>)
dispatch_async(<#dispatch_queue_t  _Nonnull queue#>, <#^(void)block#>)
/**
sync和async通常的用法都是传入两个参数, 前者是一个队列, 后者是一个block, 里面可以写在队列中执行的任务
*/
```

## 🌲 线程阻塞

```
即主线程中执行`耗时任务`或`等待`

`耗时任务`: 例如从1循环到1亿
`等待`: 例如 sleep(5)
```

## 🌲 串行并行

```
串行: 按顺序执行
并行: 同时执行
```

## 🌲 主队列抛入任务(同步, 死锁)

```objc
dispatch_sync(dispatch_get_main_queue(), ^{
	NSLog(@"主队列执行任务(同步)");
});

NSLog(@"viewDidLoad执行完毕");

/**
崩溃
原因: 死锁
Thread 1: EXC_BAD_INSTRUCTION (code=EXC_I386_INVOP, subcode=0x0)
*/
```

## 🌲 主队列抛入任务(异步)

```objc
dispatch_async(dispatch_get_main_queue(), ^{
	NSLog(@"主队列执行任务(异步)");
});

NSLog(@"viewDidLoad执行完毕");

/**
viewDidLoad执行完毕
主队列执行任务(异步)
<NSThread: 0x60000174c380>{number = 1, name = main}
*/
```

## 🌲 全局队列抛入任务(同步)

```objc
dispatch_sync(dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0), ^{
	NSLog(@"全局队列执行任务(同步)");
	NSLog(@"%@", NSThread.currentThread);
});

NSLog(@"viewDidLoad执行完毕");

/**
主队列执行任务(异步)
<NSThread: 0x60000174c380>{number = 1, name = main}
viewDidLoad执行完毕
*/
```

## 🌲 全局队列抛入任务(异步)

```objc
dispatch_async(dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0), ^{
	NSLog(@"全局队列执行任务(异步)");
});

NSLog(@"viewDidLoad执行完毕");

/**
viewDidLoad执行完毕
全局队列执行任务(异步)
<NSThread: 0x600000e57240>{number = 3, name = (null)}
*/
```

# 🍎 OC内存五大区

## 🌲 栈区

- 连续的内存区域，大小一般在1M
- 在iOS中以`0x7`开头
- 主要存储 局部变量，函数参数，先进后出、一旦出了作用域就会被销毁；函数跳转地址，现场保护等；对象的指针放在桟区。
- 由编译器自动分配并释放, 虽然不用我们去管理，但是桟的大小也是限制的

## 🌲 堆区

- 不连续的内存区域，类似链表
- 在iOS中以`0x6`开头
- 我们自己创建的对象，需要自己释放，需要经常增加和删除，所以放在堆区
- 创建好后需要经常查找，所以对象的指针放在桟区。
- 当需要访问堆中内存时，堆中的所有东西都是匿名的，这样不能按名字访问，而只能通过指针访问，需要先通过指针变量读取到栈区的内存地址，然后通过地址访问堆区，
- 对于堆来讲, 频繁的new/delete势必会造成内存空间的不连续性，从而造成大量的碎片 ,使程序效率降低

## 🌲 全局区／静态区 (static)

- 在iOS中以`0x1`开头
- 包括两个部分：bss段( bss segment )、.data(data segment、数据段)；
- .bss段通常是指用来存放程序中未初始化的全局变量和静态变量的一块内存区域。
- .data 段通常是指用来存放程序中已经初始化的全局变量和静态变量的一块内存区域。数据段属于静态内存分配,可以分为只读数据段(常量区)和读写数据段。字符串常量等,是放在只读数据段中，结束程序时才会被收回。
- 也就是说，（全局区／静态区）在内存中是放在一起的，初始化的全局变量和静态变量在一块区域， 未初始化的全局变量和未初始化的静态变量在相邻的另一块区域；
- eg：int a;未初始化的。int a = 10;已初始化的。

## 🌲 常量区

- 常量区是编译时分配的内存空间，在程序结束后由系统释放，主要存放已经使用了的，且没有指向的字符串常量
- 字符串常量因为可能在程序中被多次使用，所以`在程序运行之前就会提前分配内存

## 🌲 代码区（.text）

- 代码区是编译时分配主要用于存放程序运行时的代码, 代码会被编译成二进制存进内存的

## 🌲 变量实测

```objc
// int类型a
int a = 100;
NSLog(@"a %p", &a); // 0x7ffee074203c a的地址 -> 存在于栈区
// 指针p
int *p = &a;
NSLog(@"p %p", p); // 0x7ffee074203c p的值 也就是a的地址 -> 存在于栈区
NSLog(@"&p %p", &p); // 0x7ffee074203c 指针p的地址 -> 存在于栈区
// 字符串
NSString *str = @"1";
NSLog(@"str %p", str); // 字符串的地址 0x10d14d040 全在于全局静态区
NSString *str2 = [NSString stringWithFormat:@"1"];
NSLog(@"str2 %p", str2); // 字符串的地址 0xd775f6bef71b5495 (不清楚这块区域猜测是在内核区) 0xc 0xd 0xe 0xf
NSString *str3 = [NSString stringWithFormat:@"你1"];
NSLog(@"str3 %p", str3); // 字符串的地址 0x6000010a48e0 存在于栈区 (使用 stringWithFormat 方法创建字符串 一旦字符串内存在汉字, 那么就会分配到栈区)
NSLog(@"sstr %p", sstr); // 字符串的地址 0x1067c0020
const NSString *str4 = @"123";
NSLog(@"str4 %p", str4);
// 对象
Student *stu = [[Student alloc] init];
NSLog(@"stu %p", stu); // 学生对象 0x600001284570 存在于栈区
```

