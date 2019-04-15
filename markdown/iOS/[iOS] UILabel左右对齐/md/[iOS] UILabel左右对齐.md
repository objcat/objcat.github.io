最近遇到了一个问题, 就是在做UILabel显示文字的时候, 会出现label文字左右参差不齐的现象 如图1所示.

![图1](http://upload-images.jianshu.io/upload_images/2194379-a8fd75b06c5a335b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

下面我们就是用系统原生方法的富文本就可以轻松解决

```
NSString *str = @"😝😝而“散文”一词大概出现在北宋太平兴国（976年十二月—984年十一月）时期。《辞海》认为[1]  ：中国六😝😝朝以来，为区别于韵文和骈文，把凡不押韵、不重排偶的散体文章，包括经传史书在内，概称“😝😝散文”。后又泛😝指诗歌[2]  以外的所有文学体裁。😝";
    
// 设置段落为左右对齐 NSTextAlignmentJustified
NSMutableParagraphStyle *par = [[NSMutableParagraphStyle alloc]init];
    par.alignment = NSTextAlignmentJustified;
    
NSDictionary *dic = @{NSParagraphStyleAttributeName : par, NSUnderlineStyleAttributeName: @(NSUnderlineStyleNone)};
    
// 配置富文本
NSAttributedString *mstr = [[NSAttributedString alloc] initWithString:str attributes:dic];
    
UILabel *label2 = [[UILabel alloc]initWithFrame:CGRectMake(0, 20, [UIScreen mainScreen].bounds.size.width, 0)];
    [self.view addSubview:label2];
    
// 自适应高度
label2.attributedText = mstr;
label2.numberOfLines = 0;
label2.backgroundColor = [UIColor redColor];
[label2 sizeToFit];
```

其中关键性的是两句代码:
1.这句代码的意思是设置文字左右对齐
```
par.alignment = NSTextAlignmentJustified;
```
2.经过测试发现 配置富文本只带段落属性 左右对齐并不生效 所以我在后面配置了 下划线后左右对齐就生效了
```
NSDictionary *dic = @{NSParagraphStyleAttributeName : par, NSUnderlineStyleAttributeName: @(NSUnderlineStyleNone)};
```

下面我们来预览一下效果

![](https://upload-images.jianshu.io/upload_images/2194379-aad1444b4cb19a8c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


