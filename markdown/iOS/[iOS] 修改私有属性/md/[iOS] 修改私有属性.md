#一.前言
由于最近十分空闲, 来写一修改私有属性的文章
1.什么是私有属性?
答: 就是写在.m延展里的属性, 这个属性在其他类中用点方法访问不到的.

#二.如何修改呢
目前有两种方法
##1.KVC
比如有个person类, 在.m中的延展里有个属性是name, 那么怎么访问呢

```
@interface Person ()
@property(nonatomic, strong) NSString *name;
@end
```

```
- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view, typically from a nib.
    
    Person *person = [[Person alloc] init];
    NSLog(@"修改前:%@", [person valueForKey:@"name"]);
    [person setValue:@"李四" forKey:@"name"];
    NSLog(@"修改后:%@", [person valueForKey:@"name"]);
}
```
打印结果如下:
修改前:(null)
修改后:李四

##2.runtime
使用runtime修改私有属性也非常简单, 基本上无脑.

```
- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view, typically from a nib.
    
    Person *person = [[Person alloc] init];
    
    
    //修改前打印
    NSLog(@"修改前:%@", [person valueForKey:@"name"]);
    
    //记录属性的个数
    unsigned int count = 0;
    
    //获取属性列表
    Ivar *members = class_copyIvarList([person class], &count);

    //遍历属性列表
    for (NSInteger i = 0; i < count; i++) {
        
        //取到属性
        Ivar ivar = members[i];
        
        //获取属性名
        const char *memberName = ivar_getName(ivar);
        NSString *ivarName = [NSString stringWithFormat:@"%s", memberName];
        
        NSLog(@"属性名:%@", ivarName);
        
        //当属性名是_name的时候对其值进行修改
        if ([ivarName isEqualToString:@"_name"]) {
            object_setIvar(person, ivar, @"李四");
        }
    }
    
    //修改后打印
    NSLog(@"修改后:%@", [person valueForKey:@"name"]);
}
```
打印结果如下:
修改前:(null)
_name
修改后:李四