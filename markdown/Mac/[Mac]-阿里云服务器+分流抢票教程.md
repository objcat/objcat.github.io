# 一.前言
临近过年, 抢春运的票成为了我们人生的头等大事, 我们都知道分流是最好用的抢票软件之一, 但是只支持windows, 又因为我工作的电脑是Mac所以不能安装, 是得想个办法才好, 最终决定了使用 `阿里云服务器 + 分流` 进行抢票, 是不是听起来很高级呢? 下面就跟着我们的上下文一起来看吧!

# 二.准备工作
1.一台Mac电脑(废话有windows谁看这破教程)

# 三.开始
首先打开阿里云官方网站
[https://www.aliyun.com/](https://www.aliyun.com/)

注册阿里云账号, 注册过程略.


![](./images/b0a5013096f0e781b930a0a9c9bab0ee/1.png)

注册完成进入主界面选择控制台
![](./images/b0a5013096f0e781b930a0a9c9bab0ee/2.png)


然后选择云服务器ecs
![](./images/b0a5013096f0e781b930a0a9c9bab0ee/3.png)

选择实例
![](./images/b0a5013096f0e781b930a0a9c9bab0ee/4.png)

右上角创建

![](./images/b0a5013096f0e781b930a0a9c9bab0ee/5.png)

可以看到套餐五花八门
![](./images/b0a5013096f0e781b930a0a9c9bab0ee/6.png)

这里如果是要求稳 推荐使用 `按量付费` 资费大概在每小时 `0.2 ~ 0.6`

如果是只用一小时 就直接使用抢占实例 资费大概在每小时 `0.075 ?`

这里我就选择抢占实例了 因为就用一会还省钱 这里提醒一下 抢占实例保护期一个小时 超过这个时间 服务器随时有可能被别人抢占

我的套餐选择如下

![](./images/b0a5013096f0e781b930a0a9c9bab0ee/7.png)


选择操作系统
![](./images/b0a5013096f0e781b930a0a9c9bab0ee/8.png)

然后点击下一步开始配置网络
![](./images/b0a5013096f0e781b930a0a9c9bab0ee/9.png)

可以选择`流量模式`和`固定宽带模式`, 我这里选的流量, 流量大概是`0.7元`一个G

然后选择下一步系统配置
![](./images/b0a5013096f0e781b930a0a9c9bab0ee/10.png)

把你的密码填上, 登录服务器会用到, 然后直接点击确认订单就行了

确认无误后选择创建实例(这个步骤可能会花钱 - - 我也记不清了)

![](./images/b0a5013096f0e781b930a0a9c9bab0ee/11.png)

创建实例在控制台 -> 云服务器ecs -> 实例 中可以看到你的创建的服务器

![](./images/b0a5013096f0e781b930a0a9c9bab0ee/12.png)

点击远程连接

![](./images/b0a5013096f0e781b930a0a9c9bab0ee/13.png)

```
用户名: Administrator
密码:   你刚才注册时候的
```

然后进入服务器 这里也可以使用`Microsoft Remote Desktop Beta`来优化远程连接体验, 下载地址
[https://install.appcenter.ms/orgs/rdmacios-k2vy/apps/Microsoft-Remote-Desktop-for-Mac/distribution_groups/All-users-of-Microsoft-Remote-Desktop-for-Mac](https://install.appcenter.ms/orgs/rdmacios-k2vy/apps/Microsoft-Remote-Desktop-for-Mac/distribution_groups/All-users-of-Microsoft-Remote-Desktop-for-Mac)


之后我们进入到服务器, 首先要进行一下IE浏览器的设置
桌面点击右键个性化, 然后找到下面的东西

![](./images/b0a5013096f0e781b930a0a9c9bab0ee/14.png)


勾上计算机后 桌面上就会出现此电脑

右键管理

![](./images/b0a5013096f0e781b930a0a9c9bab0ee/15.png)

按照图中设置 否则你用不了IE浏览器

之后我们就可以当做正常电脑用了

![](./images/b0a5013096f0e781b930a0a9c9bab0ee/16.png)

打开软件使用12306账号登录
![](./images/b0a5013096f0e781b930a0a9c9bab0ee/17.png)


附上一张抢票成功的照片 请务必往下看!!!
![](./images/b0a5013096f0e781b930a0a9c9bab0ee/18.png)

抢票成功后千万不要忘记关闭实例, 不然会一直扣你钱!!!!

![](./images/b0a5013096f0e781b930a0a9c9bab0ee/19.png)



# 最后祝大家都能抢到票, 回家过大年!
# finally enjoy it.
# by objcat
# 2019.12.24






















