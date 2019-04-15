#一.写在前面的话
>今天在写代码的过程中意外的见到了奇怪的现象，经过深层的剖析发现这一部分知识还很有用，所以就写了一篇文章来记录。
#二.代码演示
1.有如下视图控制器
```
#import "ViewController.h"
#import "Student.h"

@interface ViewController ()

/*
这是一个学生的对象 用assign修饰 = = 
实际开发中不建议用assign修饰对象
这么做的目的 是为了观察对象是否被释放
*/
@property (nonatomic, assign) Student *student;
@end

@implementation ViewController

//控制器中有个按钮  按钮点击后就会执行该方法
- (IBAction)buttonAction:(id)sender {
    //点击按钮执行方法
    //让学生学习
    [self.student study];
}
- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view, typically from a nib.
}


- (void)didReceiveMemoryWarning {
    [super didReceiveMemoryWarning];
    // Dispose of any resources that can be recreated.
}
```
2.在ViewDidLoad中执行 代码1
```
    self.student = [[Student alloc] init];
    [self.student study];
```
执行结果1

![代码1执行结果](http://upload-images.jianshu.io/upload_images/2194379-7f35fec109b81a17.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/500)

3.在ViewDidLoad中执行 代码2
```
Student *student = [[Student alloc] init];
self.student = student;
[student study];
```

执行结果2

![代码2](http://upload-images.jianshu.io/upload_images/2194379-cecd6a884894cd62.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/500)

4.点击按钮 引发崩溃


![点击按钮崩溃](http://upload-images.jianshu.io/upload_images/2194379-37abecf7163c35f7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/500)

#三.引出问题

>为什么两个代码中的student都是用assign修饰，而执行结果却一个崩溃一个顺利执行了呢？


>看到这里如果知道答案的朋友有两个选择，如果觉得这是个低级问题或没有用直接点击浏览器左上角关闭，如果觉得还有点看头就请继续，如果文章有错误给您造成不良反应本人概不负责 = =



#四.揭晓答案

>本人特别不喜欢卖关子，我的博客重来不进行探究猜测直接揭晓我的结论

##1.为什么一个崩溃 一个顺利执行

因为`代码1`中的student在执行study方法前就被释放了，原因是assign在对象释放后不会对指针做任何处理，所以当对象释放了这个指针就是`野指针`，向`野指针`发送消息必然崩溃，代码2中的对象为什么没有崩溃呢，原因很简单，它执行方法的时候对象并没有被释放，点击按钮后为什么会崩溃，原因很简单，因为对象在点击按钮之前已经释放了。

##2.为什么代码1中对象会被提前释放
答案其实很简单，这是ARC处理的潜规则，如果`[[Student alloc] init]`前面是一个用assign修饰的属性`self.student`，就会发生接收后立即被释放的情况，但如前面是 `Student * student`就不会立刻被释放，而是出`作用域`后被释放，而作用域即为ViewDidLoad方法，出了这个方法就会被释放。

##3.ARC中代码做了哪些操作让对象释放呢

>这里复习一下ARC的原理，即编译器会在编译过程中自动向代码中插入release和autorelease等语句，自动管理对象的内存， 这里说明一下release的作用是让对象引用计数-1，而autorelease的作用是当对象出自动释放池或作用域时让对象引用计数-1，当对象引用计数为0时，就会调用对象本身的dealloc方法释放内存。

为了证明这一观点，我们切换到MRC模式下进行代码演示

代码1中系统做的操作是
```
self.student = [[Student alloc] init];
[self.student release];
[self.student study];
```

执行结果是：

![执行结果1](http://upload-images.jianshu.io/upload_images/2194379-f25c8ac328bb0df8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/500)

>与代码1中崩溃信息一致 即向野指针发送消息程序崩溃


代码2中系统所做的操作是

```
Student *student = [[Student alloc] init];
[student autorelease];
self.student = student;
[student study];
```

执行结果是：

![执行结果2](http://upload-images.jianshu.io/upload_images/2194379-7dd52c20ada88e56.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/500)

点击按钮后发生崩溃

![点击按钮崩溃](http://upload-images.jianshu.io/upload_images/2194379-c05f65406c545d90.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/500)


>这与代码2中的现象一样，所以可以证实我的结论是正确的




>以上观点为个人推理所得，如果有任何疑问或发现了错误，请及时与我联系，欢迎朋友们积极踊跃纠正我的错误。