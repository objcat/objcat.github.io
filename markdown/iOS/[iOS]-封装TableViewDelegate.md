其实就是把tableView的delegate和DataSource封装到一个其他的类中, 这样做虽然增加了耦合性, 但是我们可以适当使用在一些cell类型样式相同和数据源结构相同的tableview中,这样可以提高效率.
只需要几句代码就可以实现一个tableView的表格布局了
```
    self.arr = @[@"1", @"2", @"3", @"4", @"5", @"6"]; //构造数据源
    
    __weak typeof(self) weakSelf = self;//防止循环引用
    
    self.delegate = [[UITableViewDelegate alloc]initWithTableData:^NSArray *{
        return weakSelf.arr;
    }];
    
    //设置代理
    self.tableView.delegate = _delegate;
    self.tableView.dataSource = _delegate;﻿​
```
好了 直接上源码
[https://github.com/iwgo/encapsulationTableViewDelegate.git﻿](https://github.com/iwgo/encapsulationTableViewDelegate.git)
