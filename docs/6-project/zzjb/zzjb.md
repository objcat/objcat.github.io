# 🍎 简介

这篇文档主要分析`重装机兵-霸主`项目, 感谢`Dr隆鹰`能提供给我学习项目的机会, 今天我的身份是一个刚学习了`Unity`刚4天的新手, 如果有解析错误的地方请指出

# 🍎 快速开始

## 🌲 安装指定版本`Unity`

打开项目我们需要与作者保持一样的`Unity`版本, 可以在团结引擎的`installs`选项中安装

![](images/Pasted%20image%2020250814195259.png)

在安装好`Unity Hub`的前提下, 点击下面的网页可以快速跳转到安装界面

https://unity.cn/releases

![](images/Pasted%20image%2020250814195408.png)

点击更多版本找到你想要安装的

## 🌲 打开项目

![](images/Pasted%20image%2020250814195505.png)

点击这个`Open`, 然后选择项目的`Unity`文件夹

![](images/Pasted%20image%2020250814195537.png)

经过漫长的加载就可以打开这个项目了

选择场景

项目打开后双击`Scenes`下面的`init`

![](images/Pasted%20image%2020250814195741.png)

然后我们就能在场景视图下面看到我们做游戏的场景了

![](images/Pasted%20image%2020250814195814.png)

## 🌲 尝试运行当前场景

我们尝试直接运行当前场景

![](images/Pasted%20image%2020250814200005.png)

发现是报错了, 有两个错误, 一个是没有找到`Unity.Model.dll.bytes`, 还有一个数据接收异常

## 🌲 尝试打包项目

这条路走不通我们换一个, 是不是可以直接打包个PC的项目看看呢

![](images/Pasted%20image%2020250814200927.png)

发现也是行不通的, 说我有东西没安装, 那我只能脑补一下, 有一个包管理器, 然后可以拉取package, 这条路也堵死了, 那我们尝试解决上面运行单个场景的错误吧

## 🌲 修补运行错误

```shell
System.IO.FileNotFoundException: Could not find file "D:\project\unity\zzjb2d\Unity\Assets\Bundles\Code\Unity.Model.dll.bytes"
File name: 'D:\project\unity\zzjb2d\Unity\Assets\Bundles\Code\Unity.Model.dll.bytes'
  at System.IO.FileStream..ctor (System.String path, System.IO.FileMode mode, System.IO.FileAccess access, System.IO.FileShare share, System.Int32 bufferSize, System.Boolean anonymous, System.IO.FileOptions options) [0x0019e] in <17d9ce77f27a4bd2afb5ba32c9bea976>:0 
  at System.IO.FileStream..ctor (System.String path, System.IO.FileMode mode, System.IO.FileAccess access, System.IO.FileShare share, System.Int32 bufferSize) [0x00000] in <17d9ce77f27a4bd2afb5ba32c9bea976>:0 
```

单凭现在的我是无法解决这个问题的, 我问了GPT它说是一个热更新用的东西, 就在一筹莫展的时候突然想起了前两天`霸哥`给发的文档, 顿时感觉我站在了巨人的肩膀上

![](images/Pasted%20image%2020250814201713.png)

第一步切换成`DEBUG`环境, 在我看来这个应该是可以切换成开发服务器的配置, 而开发服务器一般都是在自己本地的, 所以我推测有一个地方就可以配置本地的服务器+mysql+mango, 这里我就先切换过来, 然后再看

接下来`霸哥`说要安装`HybirdCLR`

![](images/Pasted%20image%2020250814202042.png)

我们点击`Installer`

![](images/Pasted%20image%2020250814202111.png)

点击`install`后卡住了, 说明程序是阻塞运行的, 我们耐心等一会看看

![](images/Pasted%20image%2020250814202440.png)

在这里可以看到已经安装好了, 但是我尝试运行了一下还是报一样的错误

我们跟随霸哥的文档去安装另一个, `IL2CPP`

![](images/Pasted%20image%2020250814202639.png)

在安装的过程中我查了一下

```
IL2CPP是Unity 官方的脚本后端（Scripting Backend）技术，全称是Intermediate Language To C++, 其作用是把C#代码转化成C++代码
```
