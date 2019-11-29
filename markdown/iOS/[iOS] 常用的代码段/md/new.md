
# 一.前言

代码段在编程中可以让我们少打重复的代码, 有一定编程经验之后可以借助他们来提高效率, 所以这里就提供一下我在编程中会经常使用的代码段.

# 二.什么是代码段

代码段就是你事先写好的代码保存到IDE中, 在编程的时候输入快捷命令直接拿出来用, 下面我用`tableView`来举个例子.

![](../img/1.png)


大家可以在图上看到, 我这里写tableView是相当快的, 这就在编码过程中节省了大量的时间(此处应该有掌声 - -).

# 三.代码段如何创建

下面就教大家如何创建和使用代码段, 我们这里就也拿`tableView`做例子, 我先把我的`tableView`代码段放出来, 然后跟着我做, 就可以顺利创建代码段了, 我这里使用的Xcode版本为`10.1`, 以前的版本跟这个略有不同, 好的我们开始, 首先你要有一个代码段

```
@property (strong, nonatomic) UITableView *tableView;

<UITableViewDelegate, UITableViewDataSource>

- (void)createTableView {
self.tableView = [[UITableView alloc] initWithFrame:self.view.bounds style:UITableViewStyleGrouped];
self.tableView.delegate = self;
self.tableView.dataSource = self;
self.tableView.separatorStyle = UITableViewCellSeparatorStyleSingleLine;
[self.view addSubview:self.tableView];
[self.tableView registerClass:[UITableViewCell class] forCellReuseIdentifier:@"UITableViewCell"];
}

- (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView {
return 1;
}

- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section {
return 10;
}

- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath {
UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:@"UITableViewCell" forIndexPath:indexPath];
cell.textLabel.text = @"你好";
return cell;
}

- (CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath {
return 50;
}

- (UIView *)tableView:(UITableView *)tableView viewForHeaderInSection:(NSInteger)section {
return nil;
}

- (UIView *)tableView:(UITableView *)tableView viewForFooterInSection:(NSInteger)section {
return nil;
}

- (CGFloat)tableView:(UITableView *)tableView heightForHeaderInSection:(NSInteger)section {
return 0.0001;
}

- (CGFloat)tableView:(UITableView *)tableView heightForFooterInSection:(NSInteger)section {
return 0.0001;
}
```

之后选择

![](../img/2.png)


我们可以看到上面的英文是创建代码段

点击后我们会看到这样的界面

![](../img/3.png)


我们就来设置一下

![](../img/4.png)

我们这么设置完之后, 点任意地方关闭设置窗口, 然后直接在代码中打出`table`就可以看见我们的代码了, 是不是非常简单呢?

如果想要修改我们的代码段, 可以点击这里呼出代码段的设置界面

![](../img/5.png)


# 四.常用代码段

> collectionView

```
@property (strong, nonatomic) UICollectionView *collectionView;

<UICollectionViewDelegate, UICollectionViewDataSource, UICollectionViewDelegateFlowLayout>

- (void)createCollectionView {
UICollectionViewFlowLayout *layout = [[UICollectionViewFlowLayout alloc] init];
layout.scrollDirection = UICollectionViewScrollDirectionVertical;
layout.minimumLineSpacing = 0;
layout.minimumInteritemSpacing = 0;
self.collectionView = [[UICollectionView alloc] initWithFrame:self.view.bounds collectionViewLayout:layout];
self.collectionView.delegate = self;
self.collectionView.dataSource = self;
self.collectionView.pagingEnabled = NO;
self.collectionView.backgroundColor = [UIColor whiteColor];
self.collectionView.contentInset = UIEdgeInsetsMake(0, 0, 0, 0);
[self.view addSubview:self.collectionView];
[self.collectionView registerClass:[UICollectionViewCell class] forCellWithReuseIdentifier:@"UICollectionViewCell"];
}

- (NSInteger)collectionView:(UICollectionView *)collectionView numberOfItemsInSection:(NSInteger)section {
return 100;
}

- (__kindof UICollectionViewCell *)collectionView:(UICollectionView *)collectionView cellForItemAtIndexPath:(NSIndexPath *)indexPath {
UICollectionViewCell *cell = [collectionView dequeueReusableCellWithReuseIdentifier:@"UICollectionViewCell" forIndexPath:indexPath];
cell.backgroundColor = [UIColor colorWithRed:arc4random() % 255 / 255.0 green:arc4random() % 255 / 255.0 blue:arc4random() % 255 / 255.0 alpha:1];
return cell;
}

- (CGSize)collectionView:(UICollectionView *)collectionView layout:(UICollectionViewLayout *)collectionViewLayout sizeForItemAtIndexPath:(NSIndexPath *)indexPath {
CGFloat width = [UIScreen mainScreen].bounds.size.width / 4;
return CGSizeMake(width, width);
}
```

> property

```
@property (strong, nonatomic)
```

> weakSelf

```
__weak typeof(self) weakSelf = self;
```

> rgba

```
[UIColor colorWithRed:<#(CGFloat)#> / 255.0 green:<#(CGFloat)#> / 255.0 blue:<#(CGFloat)#> / 255.0 alpha:<#(CGFloat)#>]
```

> 屏幕尺寸

```
[UIScreen mainScreen].bounds.size
```

> 全屏layoutGuide约束

```
[<#UIView#> mas_makeConstraints:^(MASConstraintMaker *make) {
make.top.equalTo(self.mas_topLayoutGuide);
make.bottom.equalTo(self.mas_bottomLayoutGuide);
make.left.right.equalTo(self.view);
}];
```

> viewWillAppear

```
- (void)viewWillAppear:(BOOL)animated {
[super viewWillAppear:animated];
// [self.navigationController setNavigationBarHidden:YES animated:animated];
<#code#>
}
```

>  viewWillDisappear

```
- (void)viewWillDisappear:(BOOL)animated {
[super viewWillDisappear:animated];
// [self.navigationController setNavigationBarHidden:NO animated:animated];
<#code#>
}
```

> awakeFromNib

```
- (void)awakeFromNib {
[super awakeFromNib];
<#code#>
}
```

> viewDidLayoutSubviews

```
- (void)viewDidLayoutSubviews {
[super viewDidLayoutSubviews];
<#code#>
}
```

> layoutSubviews

```
- (void)layoutSubviews {
[super layoutSubviews];
<#code#>
}
```

> mark

```
// ZY_MARK:
```

这个要特殊说明一下, 我在看到一些重点功能的时候会使用注释的方式进行标注, 需要修改的时候立刻就可以找到

![](../img/6.png)

我目前只使用这些代码段, 如果有更好的可以在下方留言分享给大家, 如果我有错误的地方也请不吝赐教, 谢谢.

# 五.为什么使用代码段

很多人会说, 有些代码段, 你完全可以自己封装成方法, 这样使用起来不是更方便吗, 比如给UIColor写一个类目或者写一个宏, 然后使用封装的方法一行就可以搞定了, 在这里说一下我的看法, 有些代码`复杂程度非常低`, 我不认为封装他们是好的解决方案, 反之我更倾向于代码段, 因为封装涉及到跨类的调用和引入等问题, 迁移代码的难度就会增加, 在程序设计中尽量使用系统本身的方法来用, 这样当别人想移植这段代码到别处去的时候, 就可以无痛引入了, 不需要在引入多余的什么类, 仅代表个人看法, 如有异议可以在评论区与我讨论 - - , 好了就写到这.


# finally enjoy it.
# by objcat 2019.5.29.

