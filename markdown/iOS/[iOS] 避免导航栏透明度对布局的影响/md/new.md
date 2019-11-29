#一.写在前面的话
身为一名算是有点些经验的开发工程师表示仍然会导航栏透明度会改变视图布局的这个设定深恶痛疾,特别是最近粪叉(iPhoneX)出来了,原本顶栏高度64底栏高度49的布局模式彻底被打破了,所以这里在强调一次.
>1.在iOS 9以下需要使用layoutGuide进行布局.
2.在iOS 9以上需要使用safeArea进行布局.

#二.发现问题

原本用Xcode8写的项目去Xcode9上运行之后,我只能用MMP来形容了, iPhoneX的适配简直是一团糟,以前适配方式都是上面空出来64下面空出来49,到了X上面顶栏和底栏的高度都变了,所以我使用了官方推荐的layoutGuide进行适配,但是在使用的过程中发现了一个问题:
>如果导航栏不透明,使用layoutGuide做适配的时候是无法适配子视图控制器的,会发现约束标准会整体下移64, 而在导航栏透明的时候就不会出现任何问题.

#三.解决问题
去网上搜索了很多方法都不能解决我遇到的问题,所以我在想是否可以从导航栏透明度上面入手,最终找到了解决方案

>其实很简单,就是直接使用99%透明度的导航栏图片 如下面代码所示 这样既不会出现太多的色差 又可以保证系统导航栏是透明的 即 保证默认的 `navigationBar.translucent = YES`
```
[self.navigationBar setBackgroundImage:[UIImage imageNamed:@"navi"] forBarMetrics:UIBarMetricsDefault];
```

![99%透明度导航栏](http://upload-images.jianshu.io/upload_images/2194379-1e3ead6e37b577fd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)