# 一.发现问题
![问题1](https://upload-images.jianshu.io/upload_images/2194379-e490e314bc96b1f2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![问题2](https://upload-images.jianshu.io/upload_images/2194379-141819832c6c331f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



# 二.解决问题

问题1:

检查是否引入  `#import <objc/message.h>`

问题2:

设置 `Build Settings -> Enable Strict Checking of objc_msgSend Calls` 为 NO

# 三.示例

然后再试一下应该就没有什么问题了, 下面送上写法, 这里分为四种`无参数无返回值`, `无参数有返回值`, `有参数无返回值`, `有参数有返回值`.

每种方法调用又有两种写法, 请欣赏~~

## 声明
```
Person *person = [[Person alloc]init];
```

## 无参数无返回值
```
((void (*) (id, SEL)) (void *)objc_msgSend)(person, sel_registerName("say1"));
// 或
objc_msgSend(person, @selector(say1));
```
## 无参数有返回值
```
NSString *say2Return1 = ((NSString *(*) (id, SEL)) (void *)objc_msgSend)(person, sel_registerName("say2"));
NSLog(@"%@", say2Return1);
// 或
NSString *say2Return2 = objc_msgSend(person, @selector(say2));
NSLog(@"%@", say2Return2);
```
## 有参数无返回值
```
((void (*) (id, SEL, NSString *)) (void *)objc_msgSend)(person, sel_registerName("say3:"), @"3333");
// 或
objc_msgSend(person, sel_registerName("say3:"), @"3333");
```
## 有参数有返回值
```
NSString *say4Return1 = ((NSString *(*) (id, SEL, NSString *)) (void *)objc_msgSend)(person, sel_registerName("say4:"), @"4444");
NSLog(@"%@", say4Return1);
// 或
NSString *say4Return2 = objc_msgSend(person, sel_registerName("say4:"), @"4444");
NSLog(@"%@", say4Return2);
```

# finally enjoy it