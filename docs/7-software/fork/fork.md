# 🍎 简介

a fast and friendly git client for Mac and Windows

Fork is getting better and better day after day and we are happy to share our results with you.

https://git-fork.com/

# 🍎 快速开始

我使用的是`windows`平台

## 🌲 下载

略

## 🌲 安装

![](images/Pasted%20image%2020250822195524.png)

这个是自动安装的应该, 那默认应该是`C盘`, 看了一下, 默认安装到了这里

```
C:\Users\Administrator\AppData\Local\Fork\current
```

## 🌲 填写用户信息

![](images/Pasted%20image%2020250822195709.png)

填写完是需要加载一会的

![](images/Pasted%20image%2020250822195743.png)

很快界面就在这了

## 🌲 克隆仓库

![](images/Pasted%20image%2020250822195758.png)

我们来拉取一个仓库试试吧, 点击这个克隆即可

![](images/Pasted%20image%2020250822200000.png)

然后输入网址, 我这里拉取的`luban`, 我们可以看到默认是拉取到最开始选择的工程文件夹, 也可以进行手动设置

![](images/Pasted%20image%2020250822200329.png)

拉取下来是这样的

![](images/Pasted%20image%2020250822202430.png)

然后我们再拉取一个自己的项目

![](images/Pasted%20image%2020250822202446.png)

可以看到它是可以管理多标签的, 可以点击最右边的`+`看到管理的项目

![](images/Pasted%20image%2020250822202409.png)

## 🌲 提交代码

![](images/Pasted%20image%2020250822200818.png)

修改了代码后可以在`Local Changes`下面看到改动, 先暂存

![](images/Pasted%20image%2020250822200858.png)

写个评语提交

![](images/Pasted%20image%2020250822200939.png)

然后你要注意了, 为了保持良好的习惯你需要先进行一下拉取

![](images/Pasted%20image%2020250822202109.png)

然后点击推送即可

![](images/Pasted%20image%2020250822201005.png)

## 🌲 丢弃暂存

如果有代码已经修改了, 但是不想提交上去, 直接可以右键`Discard changes`来丢弃`修改`

![](images/Pasted%20image%2020250822202744.png)

## 🌲 重置代码到某个提交记录

为了给大家谋取福利, 简单说一下, 我也喜欢把它叫做`回退`, 有时候你会发现当前版本有问题, 你想回退到某一个提交上, 我用代码来演示吧, 也不怕麻烦了

![](images/Pasted%20image%2020250822202647.png)

我修改了这里, 然后提交推送, 推送过程我省略了

![](images/Pasted%20image%2020250822202936.png)

如果我们不想要这个改动了, 可以`回退`, 我们点击右键点击`前一个提交`, 也就是名字是`回滚`这个提交记录, 不要在意它的名字, 我们我们点击`Reset 'master' to Here`重置`master`到这里

![](images/Pasted%20image%2020250822203104.png)

然后会弹出框框

![](images/Pasted%20image%2020250822203153.png)

选择一种重置模式, 注意听, 这是重点, 是我平时`99%`都用到的功能

### 🌸 Hard

这个模式是最强硬的, 丢弃所有改动, 我很喜欢用这个, 因为简单粗暴, 完全重置到某一个提交记录

![](images/Pasted%20image%2020250822203327.png)
.
重置后发现`啪~没了`, 那你可能会问, 我还想回去怎么办? 其实是可以的, 我们可以选择最新的提交记录, 「再次重置」

![](images/Pasted%20image%2020250822203459.png)

然后`Hard`重置, 你会看到代码回来了

![](images/Pasted%20image%2020250822203527.png)

所以`Hard`其实是可以`反复横跳`的, 不需要太多的担心, 每天跳个百十来次你就记住了

### 🌸 Soft/Mixed

这俩功能类似, 都是回退到之前版本, 但是`改动回保留下来`, 并且他们之间的区别是一个是暂存状态, 一个不是

- Soft 

这个会自动暂存

![](images/Pasted%20image%2020250822203718.png)

- Mixed

这个不会

![](images/Pasted%20image%2020250822203754.png)

需要注意的是, 这两个模式并不会丢弃你的代码, 而是告诉你改动了哪些东西

![](images/Pasted%20image%2020250822210235.png)

可以看到代码还是有的, 你需要想丢弃可以手动丢弃

## 🌲 云端重置

这个有意思了, 你仔细听, 如果你回退到一个`提交记录`对吧, 然后你还想让云端的记录也跟着回退

第一步选择你要回退到的记录, 选择`Hard`

![](images/Pasted%20image%2020250822203459.png)

这个时候你本地的注释已经被干掉了, 因为你`Hard`了

![](images/Pasted%20image%2020250822204444.png)


保持这个状态, 并且我们可以看到`master`在回滚这个提交记录上, 这个是我们没写注释时候的状态

![](images/Pasted%20image%2020250822204538.png)

然后选择最新的提交记录, 再次重置, 选`soft/mixed`都可以

![](images/Pasted%20image%2020250822204050.png)

我选择`soft`, 然后发现`git`会把你那时`重置的代码`带过来, 这个代码就是`反操作`, 自己理解一下

![](images/Pasted%20image%2020250822204723.png)

我们提交这个就能`改变远程`的代码了, 查看远程的代码, 是没加注释的

![](images/Pasted%20image%2020250822204846.png)

可以看到云端已经`回退成功了`

## 🌲 更多功能等待你探索...

未完待续















