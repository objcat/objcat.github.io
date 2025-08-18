# 🍎 简介

这篇文档主要分析`重装机兵-霸主`项目, 中间会添加自己思考的方式, 也当是记录生活, 感谢作者`Dr隆鹰`

# 🍎 学习日志

| 日期         | 学习内容                                    | --  |
| ---------- | --------------------------------------- | --- |
| 2025.08.11 | 没学, 拉取了一下项目加班去了                         | 第1天 |
| 2025.08.12 | 安装unity, 发现无法运行项目, 开始学习unity            | 第2天 |
| 2025.08.13 | 新建项目, 项目配置, 场景和层级, 灯光和摄影机, 创建游戏对象, 场景轴向 | 第3天 |
| 2025.08.14 | 练习1, 场景快捷键, 切换模式, 游戏窗口, 项目窗口, 控制台, 检查窗口 | 第4天 |
| 2025.08.15 | 打包模块, ET, HybridCLR                     | 第5天 |
| 2025.08.16 | 学习ET场景相关                                | 第6天 |
| 2025.08.17 | 学习ET中ESC的应用                             | 第7天 |
| 2025.08.18 | 分析霸主登录页面实现原理                            | 第8天 |

# 🍎 快速开始

拿到一个项目后把他运行起来是最重要的, 之后再分析它的源码

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

发现也是行不通的, 说我有东西没安装, 那我只能脑补一下, 有一个包管理器, 然后可以拉取package, 这条路也堵死了

## 🌲 调研运行错误

那我们尝试解决上面的`Unity.Model.dll.bytes`没找到的错误吧

```shell
System.IO.FileNotFoundException: Could not find file "D:\project\unity\zzjb2d\Unity\Assets\Bundles\Code\Unity.Model.dll.bytes"
File name: 'D:\project\unity\zzjb2d\Unity\Assets\Bundles\Code\Unity.Model.dll.bytes'
  at System.IO.FileStream..ctor (System.String path, System.IO.FileMode mode, System.IO.FileAccess access, System.IO.FileShare share, System.Int32 bufferSize, System.Boolean anonymous, System.IO.FileOptions options) [0x0019e] in <17d9ce77f27a4bd2afb5ba32c9bea976>:0 
  at System.IO.FileStream..ctor (System.String path, System.IO.FileMode mode, System.IO.FileAccess access, System.IO.FileShare share, System.Int32 bufferSize) [0x00000] in <17d9ce77f27a4bd2afb5ba32c9bea976>:0 
```

单凭现在的我是无法解决这个问题的, 我问了GPT它说是一个`热更新`用的东西, 就在一筹莫展的时候突然想起了前两天`霸哥`给发的文档, 顿时感觉我站在了巨人的肩膀上

![](images/Pasted%20image%2020250814201713.png)

第一步切换成`DEBUG`环境, 在我看来这个应该是可以切换成开发服务器的配置, 而开发服务器一般都是在自己本地的, 所以我推测有一个地方就可以配置本地的服务器+mysql+mango, 这里我就先切换过来, 配置的地方我先不去看 先解决问题

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

安装完之后运行单个场景仍然会报错, 点击报错信息定位到了一个`CodeLoader`中

```cs
namespace ET
{
    public class CodeLoader : Singleton<CodeLoader>, ISingletonAwake
    {
        private Assembly modelAssembly;
        private Assembly modelViewAssembly;

        private Dictionary<string, TextAsset> dlls;
        private Dictionary<string, TextAsset> aotDlls;
        private bool enableDll;
        ...
```

我不知道这是干啥的, 但是大致可以推断出来就是程序启动的时候要进行`热更新`, 在这里找到出错的代码, 运行的时候无法找到这些`dll`

```cs
if (this.enableDll)
{
    byte[] modelAssBytes = File.ReadAllBytes(Path.Combine(Define.CodeDir, "Unity.Model.dll.bytes"));
    byte[] modelPdbBytes = File.ReadAllBytes(Path.Combine(Define.CodeDir, "Unity.Model.pdb.bytes"));
    byte[] modelViewAssBytes = File.ReadAllBytes(Path.Combine(Define.CodeDir, "Unity.ModelView.dll.bytes"));
    byte[] modelViewPdbBytes = File.ReadAllBytes(Path.Combine(Define.CodeDir, "Unity.ModelView.pdb.bytes"));
    this.modelAssembly = Assembly.Load(modelAssBytes, modelPdbBytes);
    this.modelViewAssembly = Assembly.Load(modelViewAssBytes, modelViewPdbBytes);
}
```

可以看到它是在本地文件去读取这个, 但是热跟新去读本地文件是没用的啊, 所以我觉得肯定没这么简单, 说不定是先所以我查看了`enableDll`是做什么用的, 找到了这行

```cs
this.enableDll = Resources.Load<GlobalConfig>("GlobalConfig").EnableDll;
```

然后点入属性

```cs
[CreateAssetMenu(menuName = "ET/CreateGlobalConfig", fileName = "GlobalConfig", order = 0)]
public class GlobalConfig : ScriptableObject
{
    public CodeMode CodeMode;

    public bool EnableDll;

    public BuildType BuildType;

    public AppType AppType;

    public EPlayMode EPlayMode;

    [HideInInspector] public string ServerIP = "xxx.xxx.xxx";
    [HideInInspector] public string ABundlesIP = "https://xxx";
    [HideInInspector] public string ABundlesVersion = "v1.0";

    [HideInInspector] public ConnectType ConnectType = ConnectType.Remote;
}
```

`敏感信息我隐藏了`, 虽然不懂这是什么, 但是从`GlobalConfig`字面意义上理解和对代码中出现了服务器地址`ServerIP`, 可以推断出就是相当于一个配置文件, 要从远程服务器上获取, 到这里线索中断了, 我不知道它为什么要在本地读取一个补丁

## 🌲 大佬一句话成功解决

后来大佬说要按一下`F6`来运行, 这个步骤其实就是调用的`ET`选项中的编译插件

![](images/Pasted%20image%2020250815124110.png)

可以看到`ET`栏目中的`Compile`

![](images/Pasted%20image%2020250818234345.png)

经过编译发现这些文件自动生成了, 然后点击运行发现可以了

![](images/Pasted%20image%2020250815124328.png)

## 🌲 切换环境

切换环境也是用这个`GlobalConfig`配置文件, 把`Connect Type`选择为`Remote`即可

![](images/Pasted%20image%2020250818235034.png)

然后我们进入游戏看看

![](images/Pasted%20image%2020250818235211.png)

可以登录说明切换成功了, 但是我试过战斗服务器不能连接不知道为何

# 🍎 登录页分析

本模块在技术层面分析登录页是如何实现的

[登录页分析](../login/login.md)

# 🍎 FAQ

## 🌲 DOTween.Modules.dll

双编就能解决, 编译`霸主Unity`和`ET`

```
13>CSC: Error CS0006 : 未能找到元数据文件“D:\project\unity\zzjb2d\Unity\Temp\bin\Debug\DOTween.Modules.dll”
13>CSC: Error CS0006 : 未能找到元数据文件“D:\project\unity\zzjb2d\Unity\Temp\bin\Debug\game.dll”
13>------- Finished building project: Unity.HotfixView. Succeeded: False. Errors: 2. Warnings: 0
```