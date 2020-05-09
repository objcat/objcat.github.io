#一.修改状态栏文字颜色
#####这里修改文字颜色分两种情况
######(1)导航栏是隐藏状态
如果导航栏为隐藏状态 可以直接在控制器中重写如下方法
```
- (UIStatusBarStyle)preferredStatusBarStyle{
    return UIStatusBarStyleLightContent;
}
```
######(2)导航栏不是隐藏状态
如果导航栏不是隐藏状态 会发现方法(1)没有作用了
这时要采用第二种方法, 一共有两个步骤
1.在`info.plist`添加字段 `View controller-based status bar appearance` 并设置为`NO` (如果没有此字段 请添加)
2.在代码中写上如下代码
```
[[UIApplication sharedApplication] setStatusBarStyle:UIStatusBarStyleLightContent];
```
如果你做了上面两个步骤且操作没有错误的话 就会发现状态栏文字变为了白色

#二.修改状态栏背景颜色

> iOS13此方法会导致崩溃

~~UIView *statusBar = [[[UIApplication sharedApplication] valueForKey:@"statusBarWindow"] valueForKey:@"statusBar"];
if ([statusBar respondsToSelector:@selector(setBackgroundColor:)]) {
    statusBar.backgroundColor = [UIColor cyanColor];
}~~

> 取代方法

取代方法是直接在self.view上加一个view或layer
```
CGFloat stateBarHeight = [UIApplication sharedApplication].statusBarFrame.size.height;
UIView *blockView = [[UIView alloc] initWithFrame:CGRectMake(0, 0, [UIScreen mainScreen].bounds.size.width, stateBarHeight)];
blockView.backgroundColor = [UIColor redColor];
[self.view addSubview:blockView];
```
