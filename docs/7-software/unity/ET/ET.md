# 🍎 简介

`ET`是`熊猫`大佬开发的一个`Unity全栈游戏框架`, 它采用了`ESC`的`设计思想`, 包含`前端+后端`, 我们可以在此基础上去开发我们自己的游戏, 更专注与设计

https://github.com/egametang/ET

竞争产品是`GameFramework`

https://github.com/EllanJiang/GameFramework

本文主要讲ET

# 🍎 快速开始

## 🌲 下载并解压

直接去官网, 切换分支到`8.1`

![](images2/Pasted%20image%2020250814234248.png)

然后直接把项目下载下来就可以了, 解压出来大概是`84M`

![](images2/Pasted%20image%2020250814234413.png)

可以看到里面的目录结构是这样的

![](images2/Pasted%20image%2020250814234804.png)

文件的变化我这里建了一个`Git`仓库

https://gitee.com/objcat/test-et81

## 🌲 打开项目

我们使用`Unity Hub`中的`Open`打开里面的`Unity`文件夹, 作者推荐的`Unity`的版本是`2022.3.15`, 非必要不要去用别的版本, 我是懒得下载了, 用的`2022.3.16` - -

![](images2/Pasted%20image%2020250815002712.png)

我的会提示我版本不对

![](images2/Pasted%20image%2020250816121006.png)

然后我选择的是`Choose another Editor verison`, 然后点击下面的`Open`

![](images2/Pasted%20image%2020250816121048.png)

然后会有一个修改`Unity版本`的提示

![](images2/Pasted%20image%2020250816121129.png)

直接点击`Change verison`, 如果和作者使用的不是同一个`Unity`版本中间会有多个对话框, 我们直接点击继续就可以了

![](images2/Pasted%20image%2020250816121252.png)

点击`Continue`就可以了

![](images2/Pasted%20image%2020250815001132.png)

可以看到, 它在安装我们游戏所需要的库

![](images2/Pasted%20image%2020250816121327.png)

## 🌲 解决管理员提示

![](images2/Pasted%20image%2020250823120954.png)

有时候打开会提示这个框, 我们可以直接选择`I wish`, 如果你嫌烦也可以修改`win`系统配置

我们需要在系统配置中开启两个东西, 首先我们`win+R`打开控制台界面

![](images2/Pasted%20image%2020250823121232.png)

输入`secpol.msc`

出现`本地安全策略`窗口, 在里面选择`安全选项`, 把图中标记的两个配置改为`已启用`即可

![](images2/Pasted%20image%2020250823121147.png)

## 🌲 打开项目

项目打开是这个样子

![](images2/Pasted%20image%2020250816121911.png)

初始项目打开是这样的, 啥也没有, 我们来看一下文件变化

![](images2/Pasted%20image%2020250816122132.png)

我们可以看到都是有关项目的配置, 这类配置是`Unity`帮我们自动改动的

## 🌲 加载场景

我们首先双击`Scenes`中的`init`

![](images2/Pasted%20image%2020250815001453.png)

我打开后是这样的, 到这里我们场景就看到了

![](images2/Pasted%20image%2020250815002020.png)

## 🌲 配置Unity

### 🌸 配置默认工具

打开工程后, 点击`菜单上的 -> Edit -> Preferences -> External Tools`, 点击下拉框`External Script Editor`选择`Rider`, `Generate .csproj files for`中的选项全部不要勾选  

![](images2/Pasted%20image%2020250816115413.png)

我的`Generate .csproj files for`保持了默认配置

### 🌸 配置AppType

在`Project`视图中选中`Assets/Resources/GlobalConfig`, 把`AppType`选择成`Demo(状态同步)`或者`LockStep(帧同步)`

![](images2/Pasted%20image%2020250816122319.png)

默认是这样的, 我们可以看到`App Type`默认选中的是`Demo`, 那这个我们可以不用改动

## 🌲 安装Rider

一笔带过, 太简单了

https://www.jetbrains.com/zh-cn/rider/

![](images2/Pasted%20image%2020250814235146.png)

## 🌲 编译项目

### 🌸 编译Unity

![](images2/Pasted%20image%2020250816122613.png)

我们在项目窗口, 空白的地方点击鼠标右键, 选择`Open C# Project`, 打开是这样的

![](images2/Pasted%20image%2020250816122804.png)

然后我们等待它加载项目

![](images2/Pasted%20image%2020250816122748.png)

![](images2/Pasted%20image%2020250816122821.png)

这个时间比较长, 需要耐心等待, 然后点击小锤子编译下

![](images2/Pasted%20image%2020250816122917.png)

我们发现会出现错误

```
2>------- Started building project: Unity.Core
D:\ms\Microsoft Visual Studio\2022\Professional\MSBuild\Current\Bin\Roslyn\csc.exe /noconfig /unsafe+ /nowarn:0169,0649,3021,8981,1701,1702,2008 /fullpaths 
...
2>CSC: Error CS0006 : 未能找到元数据文件“D:\project\unity\test-et81\Share\Analyzer\bin\Debug\Share.Analyzer.dll”
2>------- Finished building project: Unity.Core. Succeeded: False. Errors: 1. Warnings: 0
Build completed in 00:00:01.511
```

我们可以看到一个未能找到元数据文件的问题

```
D:\project\unity\test-et81\Share\Analyzer\bin\Debug\Share.Analyzer.dll
```

不要着急, 我们来编译`ET`

### 🌸 编译ET

我们在工程目录使用`Rider`打开`ET`

![](images2/Pasted%20image%2020250816123156.png)

打开项目是这样的

![](images2/Pasted%20image%2020250816123514.png)

我们等待加载

![](images2/Pasted%20image%2020250816123450.png)

我们直接点击编译

![](images2/Pasted%20image%2020250816123609.png)

可以看到日志滚动

![](images2/Pasted%20image%2020250816124454.png)

这里我是能编译成功的

![](images2/Pasted%20image%2020250816124521.png)

然后我们再回头编译`Unity`发现可以了

![](images2/Pasted%20image%2020250816124604.png)

所以我们得到了结论, 缺少`dll`是因为没有编译`ET`导致的

我们也可以使用另外一种更简单的编译`ET`的方式

![](images2/Pasted%20image%2020250816145855.png)

点击`Compile`或者是`F6`快捷键

![](images2/Pasted%20image%2020250817003416.png)

但是这个`编译时间`感觉要比在`Rider`中的时间长, 编译前是空的

![](images2/Pasted%20image%2020250817003146.png)

编译后发现确实都出来了

![](images2/Pasted%20image%2020250817003458.png)

`.bytes`是为了让`Unity`读取更加方便, 可以通过`TextAsset`进行加载, `.pdb`是调试文件

## 🌲 运行项目

![](images2/Pasted%20image%2020250816124818.png)

我们点击`Unity`的运行场景, 可以看到场景可以运行起来

![](images2/Pasted%20image%2020250816125145.png)

我们可以右键`Game`, 能够看到一个`Maximize`可以把窗口放大到全屏

![](images2/Pasted%20image%2020250816125114.png)

然后是输入密码

![](images2/Pasted%20image%2020250816125234.png)

这个没有做限制随便输入即可, 然后点击进入

![](images2/Pasted%20image%2020250816125252.png)

我们可以看到能进入游戏了

![](images2/Pasted%20image%2020250816125313.png)

这就是`ET`给我们提供的`Demo`, 鼠标右键点击可以移动角色

![](images2/Pasted%20image%2020250816125422.png)

R是热重载, 如果我们修改了`ET`的代码可以重新`Build`一次`ET`, 然后点击`R`可以直接热重载`dll`, 也就是这些东西

![](images2/Pasted%20image%2020250816125759.png)

在工程目录看到的会不太一样, `Unity`可能隐藏了后缀

![](images2/Pasted%20image%2020250816150211.png)

T是切换地图

![](images2/Pasted%20image%2020250816125829.png)

我这里项目就运行好了

## 🌲 调试

### 🌸 第二种运行项目的方式

我们回到`Rider`, 我指的这个`Rider`是打开`Unity`端的, 发现它的上面也有几个按钮类似`Unity`

![](images2/Pasted%20image%2020250817190534.png)

这几个按钮的意思和`Unity`一样, 绿色的按钮就是运行游戏, 很明显`Rider`给我们提供了快捷关联

### 🌸 打断点

我们想调试第一步就需要打断点, 打断点告诉你需要在哪里中断, 打断点的方式是鼠标左键点击序号, 比如我在`Init`入口上打断点

![](images2/Pasted%20image%2020250817191305.png)

### 🌸 启动调试

断点打好了已经完成`50%`, 下面就开始运行项目调试, 往右看, 有一个小虫子一样的图标

![](images2/Pasted%20image%2020250817190922.png)
点击后会进入到调试模式, 

![](images2/Pasted%20image%2020250817190936.png)

这时候再点击绿色按钮, 让游戏重新启动即可, 如果不是重新启动有可能断不到, 因为我们打断点的位置在`Init`入口

![](images2/Pasted%20image%2020250817191022.png)

然后程序就会停在你打断点的地方, 然后我们可以通过鼠标放在变量上, 看里面的属性

![](images2/Pasted%20image%2020250817191621.png)

到这里断点教程完成

# 🍎 工程讲解

## 🌲 目录结构

我们先总览一下目录结构, 其中`DotNet`这个区域是`服务端`, `Share`是工具区, `Unity`是`游戏端`也可以叫`客户端`

![](images2/Pasted%20image%2020250815003251.png)

## 🌲 Unity(客户端)

### 🌸 ET程序入口

`ET`的程序入口文件是这个`Assets/Scripts/Loader/MonoBehaviour/Init.cs`, 它是绑定在`Glocal`场景下的, 我们点击场景就能够看到一个`Init.cs`

![](images2/Pasted%20image%2020250817143529.png)

我们简略的看看它都做了什么

```cs
using System;
using CommandLine;
using UnityEngine;

namespace ET
{
	public class Init: MonoBehaviour
	{
		private void Start()
		{
			this.StartAsync().Coroutine();
		}
		
		private async ETTask StartAsync()
		{
			DontDestroyOnLoad(gameObject);
			
			AppDomain.CurrentDomain.UnhandledException += (sender, e) =>
			{
				Log.Error(e.ExceptionObject.ToString());
			};

			// 命令行参数
			string[] args = "".Split(" ");
			Parser.Default.ParseArguments<Options>(args)
				.WithNotParsed(error => throw new Exception($"命令行格式错误! {error}"))
				.WithParsed((o)=>World.Instance.AddSingleton(o));
			Options.Instance.StartConfig = $"StartConfig/Localhost";
			
			World.Instance.AddSingleton<Logger>().Log = new UnityLogger();
			ETTask.ExceptionHandler += Log.Error;
			
			World.Instance.AddSingleton<TimeInfo>();
			World.Instance.AddSingleton<FiberManager>();

			await World.Instance.AddSingleton<ResourcesComponent>().CreatePackageAsync("DefaultPackage", true);
			
			CodeLoader codeLoader = World.Instance.AddSingleton<CodeLoader>();
			await codeLoader.DownloadAsync();
			
			codeLoader.Start();
		}

		private void Update()
		{
			TimeInfo.Instance.Update();
			FiberManager.Instance.Update();
		}

		private void LateUpdate()
		{
			FiberManager.Instance.LateUpdate();
		}

		private void OnApplicationQuit()
		{
			World.Instance.Dispose();
		}
	}
}
```

#### 🌵 Update

首先我们可以看到这个类是继承于我们最熟悉的`MonoBehaviour`, 在`Update`方法里表示我们每一帧更新的时候做什么, 可以看到它做了两件事

```cs
TimeInfo.Instance.Update();
FiberManager.Instance.Update();
```

`TimeInfo`是`ET`框架里的时间管理器, `Update`用于记录每一帧的时间, 在里面就调用了一个方法`ClientNow`, 这个很好理解, `DateTime.UtcNow.Ticks`是现在的时间戳, `this.dt1970.Ticks`是`1970-01-01 00:00:00`的时间戳, 两个相减就是当前的时间戳, 注意`DateTime.UtcNow.Ticks`并不是从`1970`开始的, 所以需要相减, 代码如下

```cs
public long ClientNow()
{
	return (DateTime.UtcNow.Ticks - this.dt1970.Ticks) / 10000;
}
```

然后这个变量保存在`this.FrameTime = this.ClientNow();`下面

然后我们来看`FiberManager`, 他有一个好听的名字叫做`纤程管理器`, `纤程`就是比线程还要轻量的级的线程, 这不禁让我想到了`协程`, 它可以负责的东西有

```cs
FiberA：战斗逻辑

里面跑各种 System：AI、技能、血量计算……

全部封闭在这个 Fiber 内，不会被外部线程打扰。

FiberB：聊天逻辑

里面跑聊天相关的 System：频道消息、好友私聊……

和战斗 Fiber 完全独立，互不干扰。

举个例子：当客户端存在复杂计算时，可以单独创建一个Fiber进行专门处理，每次通过Actor进行消息传递，再将计算结果通过Actor返回到主Fiber，把更多的性能留给主Fiber用于渲染以及物理计算等。

ET框架的客户端初始时有两个Fiber，分别是Main和NetClient，分别处理游戏主逻辑与网络逻辑。
所以为了充分发挥主机性能，可以提前规划好主要功能的分类，以便于创建对应的Fiber。
例：游戏主逻辑（包括渲染和物理计算）、网络逻辑（负责消息分发）、战斗、热更新（边玩边下）、资源加载等等。

... fiber还有很多用途 我这一知半解只能讲这些
```

引用大佬的一张图

http://www.toxicstar.top/index.php/archives/266/

![](images2/Pasted%20image%2020250817154513.png)

#### 🌵 Word

我们还能看到一个关键的东西`Word`

```cs
World.Instance
```

这个东西是一个`全局单例管理器`, 它也是一个单例, 没错就是单例设计模式, 可以在整个程序的声明周期内都可以向里面存东西或者把东西取出来, 我们可以看到下面几行代码就是使用`AddSingleton`往`World`中去注册组件

```cs
World.Instance.AddSingleton<Logger>().Log = new UnityLogger();
ETTask.ExceptionHandler += Log.Error;
			
World.Instance.AddSingleton<TimeInfo>();
World.Instance.AddSingleton<FiberManager>();
```

注册组件的方法是`AddSingleton`

```cs
public T AddSingleton<T>() where T : ASingleton, ISingletonAwake, new()
{
	T singleton = new();
	singleton.Awake();

	AddSingleton(singleton);
	return singleton;
}
```

我们可以看到, 注册的具体步骤就是通过`new()`推断泛型来创建对象, 然后来调用`Awake`, 这个`Awake`我们称为注册方法, 那么它是怎么知道我们的组件中有`Awake`这个方法呢? 没错就是继承, 我们可以看到`public abstract class Singleton<T>: ASingleton where T: Singleton<T>`是个抽象类

```cs
public class TimeInfo: Singleton<TimeInfo>, ISingletonAwake
{
	public void Awake()
	{
		this.dt1970 = new DateTime(1970, 1, 1, 0, 0, 0, DateTimeKind.Utc);
		this.dt = new DateTime(1970, 1, 1, 0, 0, 0, DateTimeKind.Utc);
		this.FrameTime = this.ClientNow();
	}
	此处省略一万行......
}
```

我们可以看到, 它继承了`Singleton`并实现了里面的`Awake`方法

### 🌸 CodeLoader

这个东西是重中之重, 它的主要用途和他的名字一样, 就是加载代码用的, 为了实现热更新而采取程序内动态加载`dll`的方式

```cs
CodeLoader codeLoader = World.Instance.AddSingleton<CodeLoader>();
await codeLoader.DownloadAsync();			
codeLoader.Start();
```

我们首先看这三行, 第一行就是创建一个`代码加载器`, 然后是`await codeLoader.DownloadAsync();`这句话从字面意义上看是`异步下载`, 说到下载我们都会第一时间想到从远程服务器下载

```cs
public async ETTask DownloadAsync()
{
	if (!Define.IsEditor)
	{
		this.dlls = await ResourcesComponent.Instance.LoadAllAssetsAsync<TextAsset>($"Assets/Bundles/Code/Unity.Model.dll.bytes");
		this.aotDlls = await ResourcesComponent.Instance.LoadAllAssetsAsync<TextAsset>($"Assets/Bundles/AotDlls/mscorlib.dll.bytes");
	}
}
```

但是我们发现它是在本地去加载代码的, 这一点还是比较奇怪的, 我们后面再看, 加载`dll`成功后`this.dlls`和`this.aotDlls`就是两个装有`TextAsset`的字典, 而字典的key是`Unity.Model.dll, Unity.Model.pdb, Unity.ModelView.dll, Unity.ModelView.pdb`这些`dll`的名字, 这个`TextAsset`类官方的介绍是`Represents a raw text or binary file asset.` 它是一种可以被`Unity`加载的资源, 是`dll`被加载进来的二进制数据

```cs
public async ETTask<Dictionary<string, T>> LoadAllAssetsAsync<T>(string location) where T : UnityEngine.Object
{
	AllAssetsHandle allAssetsOperationHandle = YooAssets.LoadAllAssetsAsync<T>(location);
	await allAssetsOperationHandle.Task;
	Dictionary<string, T> dictionary = new Dictionary<string, T>();
	foreach (UnityEngine.Object assetObj in allAssetsOperationHandle.AllAssetObjects)
	{
		T t = assetObj as T;
		dictionary.Add(t.name, t);
	}

	allAssetsOperationHandle.Release();
	return dictionary;
}
```

我们再来看一下`codeLoader.Start();`

```cs
public void Start()
{
	if (!Define.IsEditor)
	{
		byte[] modelAssBytes = this.dlls["Unity.Model.dll"].bytes;
		byte[] modelPdbBytes = this.dlls["Unity.Model.pdb"].bytes;
		byte[] modelViewAssBytes = this.dlls["Unity.ModelView.dll"].bytes;
		byte[] modelViewPdbBytes = this.dlls["Unity.ModelView.pdb"].bytes;
		// 如果需要测试，可替换成下面注释的代码直接加载Assets/Bundles/Code/Unity.Model.dll.bytes，但真正打包时必须使用上面的代码
		//modelAssBytes = File.ReadAllBytes(Path.Combine(Define.CodeDir, "Unity.Model.dll.bytes"));
		//modelPdbBytes = File.ReadAllBytes(Path.Combine(Define.CodeDir, "Unity.Model.pdb.bytes"));
		//modelViewAssBytes = File.ReadAllBytes(Path.Combine(Define.CodeDir, "Unity.ModelView.dll.bytes"));
		//modelViewPdbBytes = File.ReadAllBytes(Path.Combine(Define.CodeDir, "Unity.ModelView.pdb.bytes"));

		if (Define.EnableIL2CPP)
		{
			foreach (var kv in this.aotDlls)
			{
				TextAsset textAsset = kv.Value;
				RuntimeApi.LoadMetadataForAOTAssembly(textAsset.bytes, HomologousImageMode.SuperSet);
			}
		}
		this.modelAssembly = Assembly.Load(modelAssBytes, modelPdbBytes);
		this.modelViewAssembly = Assembly.Load(modelViewAssBytes, modelViewPdbBytes);
	}
	else
	{
		if (this.enableDll)
		{
			byte[] modelAssBytes = File.ReadAllBytes(Path.Combine(Define.CodeDir, "Unity.Model.dll.bytes"));
			byte[] modelPdbBytes = File.ReadAllBytes(Path.Combine(Define.CodeDir, "Unity.Model.pdb.bytes"));
			byte[] modelViewAssBytes = File.ReadAllBytes(Path.Combine(Define.CodeDir, "Unity.ModelView.dll.bytes"));
			byte[] modelViewPdbBytes = File.ReadAllBytes(Path.Combine(Define.CodeDir, "Unity.ModelView.pdb.bytes"));
			this.modelAssembly = Assembly.Load(modelAssBytes, modelPdbBytes);
			this.modelViewAssembly = Assembly.Load(modelViewAssBytes, modelViewPdbBytes);
		}
		else
		{
			Assembly[] assemblies = AppDomain.CurrentDomain.GetAssemblies();
			foreach (Assembly ass in assemblies)
			{
				string name = ass.GetName().Name;
				if (name == "Unity.Model")
				{
					this.modelAssembly = ass;
				}
				else if (name == "Unity.ModelView")
				{
					this.modelViewAssembly = ass;
				}

				if (this.modelAssembly != null && this.modelViewAssembly != null)
				{
					break;
				}
			}
		}
	}
	
	(Assembly hotfixAssembly, Assembly hotfixViewAssembly) = this.LoadHotfix();

	World.Instance.AddSingleton<CodeTypes, Assembly[]>(new[]
	{
		typeof (World).Assembly, typeof (Init).Assembly, this.modelAssembly, this.modelViewAssembly, hotfixAssembly,
		hotfixViewAssembly
	});

	IStaticMethod start = new StaticMethod(this.modelAssembly, "ET.Entry", "Start");
	start.Run();
}
```

现阶段我们只关注一个核心代码`Assembly.Load`它是`C#`提供的方法, 可以把`dll`动态加载到内存中, 然后我们就能方便的使用`dll`中的代码了

```cs
namespace MyHotfix
{
    public class Hello
    {
        public static void Say() => Console.WriteLine("Hello Hotfix!");
    }
}
```

然后我们使用`cs`的反射来调用这个代码

```cs
byte[] dllBytes = File.ReadAllBytes("MyHotfix.dll");
Assembly hotfixAss = Assembly.Load(dllBytes);

// 反射调用
Type helloType = hotfixAss.GetType("MyHotfix.Hello");
MethodInfo say = helloType.GetMethod("Say");
say.Invoke(null, null); // 输出 "Hello Hotfix!"
```

所以`ET`的`热更新原理`就是动态替换这些`dll`, 这也是它不太好理解的原因

### 🌸 热更新

上面的一块有一个疑问就是为什么热更新没有远程下载`dll`而是读取本地的, 在一个大佬的帖子里找到了答案, 是因为`ET8.1`根本没去做热更新

https://toxicstar.top/index.php/archives/281/

### 🌸 反编译

我们接下来使用`dnSpy`来反编译生成的`dll`来看一下里面的代码

![](images2/Pasted%20image%2020250817085116.png)

直接把`dll`拖拽到里面即可, 我们来看一下`Model`, 可以看到反编译过来的代码文件和工程中都是一一对应的

![](images2/Pasted%20image%2020250817085851.png)

所以`dll`就是我们编译过的代码, `Unity`可以从远程拉取我们更新过的`dll`并替换这些, 然后重新启动程序就可以实现替换代码的逻辑, 不用重新发版本了, 省时省力, 但是实际上不是所有的脚本都支持热更新, 比如挂在`inspector`中的`Monobehaver`脚本就不能加入热更新, 但是可以通过套壳的形式来实现热更新, 这是我了解到的

### 🌸 编译过程

你可能有一个疑问, `dll`是如何编译到我们工程中的`Code`文件夹的, 这个我们以后再去研究, 你现在可以认为`ET`给我们封装了这样的流程, 我们直接用就好了, 这一章我后面会详细说

### 🌸 代码分区

我们先来看这个模块, 根据视频讲解, 这是作者给我们根据功能分出来的不同的`程序集`, 在我理解和Java中的`Maven Module`差不多, 就是根据分工把一个项目拆成一堆小项目

- 与定义相关的放在`Model`中
- 与实现相关的放在`Hotfix`中
- 与视图定义相关的放在`ModelView`中
- 与视图实现相关的放在`HotfixView`中

举个例子, 我们找到

![](images2/Pasted%20image%2020250815012037.png)

然后看一下它的代码

```cs
using UnityEngine;

namespace ET.Client
{
	[ComponentOf(typeof(UI))]
	public class UILoginComponent: Entity, IAwake
	{
		public GameObject account;
		public GameObject password;
		public GameObject loginBtn;
	}
}
```

它的实现就在`Hotfix`中

![](images2/Pasted%20image%2020250815012141.png)

```cs
using UnityEngine;
using UnityEngine.UI;

namespace ET.Client
{
	[EntitySystemOf(typeof(UILoginComponent))]
	[FriendOf(typeof(UILoginComponent))]
	public static partial class UILoginComponentSystem
	{
		[EntitySystem]
		private static void Awake(this UILoginComponent self)
		{
			ReferenceCollector rc = self.GetParent<UI>().GameObject.GetComponent<ReferenceCollector>();
			self.loginBtn = rc.Get<GameObject>("LoginBtn");
			
			self.loginBtn.GetComponent<Button>().onClick.AddListener(()=> { self.OnLogin(); });
			self.account = rc.Get<GameObject>("Account");
			self.password = rc.Get<GameObject>("Password");
		}

		
		public static void OnLogin(this UILoginComponent self)
		{
			LoginHelper.Login(
				self.Root(), 
				self.account.GetComponent<InputField>().text, 
				self.password.GetComponent<InputField>().text).Coroutine();
		}
	}
}
```

也就是说我们每一个`Model`实现的东西都在`HotFix`中是有一一对应的实现文件的, 并且这个文件是`xxxComponentSystem`来命名的, 这个文件在编译的时候就会编译到我们刚才`CodeLoader`读取`代码资产`的目录

### 🌸 程序集依赖

程序集之间是存在依赖关系的, 我们可以看一下, `.asmdef`就是记录当前`程序集`的依赖文件

![](images2/Pasted%20image%2020250815012954.png)

我们打开它

```json
{
    "name": "Unity.Core",
    "rootNamespace": "ET",
    "references": [
        "Unity.ThirdParty",
        "Unity.Mathematics",
        "MemoryPack"
    ],
    "includePlatforms": [],
    "excludePlatforms": [],
    "allowUnsafeCode": true,
    "overrideReferences": false,
    "precompiledReferences": [],
    "autoReferenced": true,
    "defineConstraints": [],
    "versionDefines": [],
    "noEngineReferences": false
}
```

可以看到`Core`依赖`Unity.ThirdParty`, 因此它可以使用`Unity.ThirdParty`里面的代码

## 🌲 DotNet

待完善

## 🌲 Tool

待完善

# 🍎 ECS

这一章非常的重要, 虽然我在写这章的时候也是一知半解的, 但是如果不学习`ECS`设计就无法深入理解实体和组件

首先`ESC`全程是`Entity-Component-System`, 也就是`实体-组件-系统`

## 🌲 Entity

它是一个对象, 或者一类对象的抽象, 可以表示具体的某个角色, 某棵树, 某个容器, 本身不包含逻辑, 只有成员变量, 可以描述对象的特征, 比如我设计的`Entity`是`Person`, 那么我的属性就是, `名字, 年龄, 性别, 出生年月日`

## 🌲 Component

组件, 组件可以通过挂载到实体上去发挥作用, 比如我把人的身上挂载`头, 身体, 手臂, 腿`, 那么这几个都是`Component`, 组件不仅可以表器官, 而且可以表行为, 比如`行走, 跳跃, 跑步`, 这些也是需要靠组件去实现的, 有人会问`行走`为什么不能直接绑定在腿上呢, 我也曾经有这个疑问, GPT回答我说行走是全身的运动, 需要各个部位去配合, 这就需要一个组件去支撑, 如果只是`伸个腿`这个动作可能不需要全身配合, 是可以写在腿的`System`中的

## 🌲 System

我们在上文提到了`Component`, 这个里面也只能表示纯数据, 如果需要实现逻辑需要写在对应的`system`中, 比如我写的组件是`LegComponent`里面有`腿的长度, 腿的颜色`, 那么我们实现逻辑的`system`就要命名为`LegComponentSystem`

## 🌲 总结

`ECS`又三部分组成, 其中`Entity`和`Component`都只能表示数据, 一般情况下, 一个主体比如`人`, 是需要定义成`Entity`的, 而`头, 手臂, 腿`是要定义成`Component`的因为他们不能单独存在, 需要挂载到`人`上, 而带有逻辑的需要写在`System`中

# 🍎 实体

在上一个章节我们已经学习过ECS设计思想了, 那么这一章就是对实体的具体讲解, 在`ET`中实体也是有分类的

```cs
Entity
 ├── Scene
 │    ├── Unit (玩家/怪物/NPC)
 │    │     └── Component (移动/AI/动画/血条...)
 │    ├── UI (窗口)
 │    │     └── Component (按钮/文本/逻辑...)
 │    └── Room (房间)
 │          ├── Player (玩家逻辑)
 │          └── Player (玩家逻辑)
```

## 🌲 Entity

实体基类, 所有的实体包括组件都是继承于这个

位置`Unity/Assets/Scripts/Core/Entity/Entity.cs`

```cs
// abstract 抽象 partial 多个同名合并成一个class
public abstract partial class Entity: DisposeObject, IPool
{
	public long InstanceId { get; protected set; }
	...此处略去一万字
}
```

- 提供唯一实例`id` -> InstanceId
- 实现对象的销毁逻辑
- 对象池接口 不用的对象放入对象池而不是直接销毁

我们来看看它的常用方法

- IsComponent 判断是否是一个组件 或者是一个实体

## 🌲 Scene

表示一个游戏场景或逻辑空间

位置`Unity/Assets/Scripts/Core/Entity/Scene.cs`

```cs
public class Scene: Entity, IScene
```

- 管理场景中的`Unit` -> 怪物/角色/树/草 .... 
- 常用组件`UnitComponent`负责管理Unit

## 🌲 Unit

表示场景中可操作对象（角色 / 怪物 / NPC）

- 可以挂载各种 Component（移动、AI、动画、血条…）
- 具备交互性，是场景中最常用的实体之一

## 🌲 UI

表示一个 UI 窗口或界面

- 可以挂载不同 Component（按钮逻辑、文本、关闭逻辑…）

## 🌲 Room

表示 一个逻辑房间，常用于多人游戏（对局/战斗/副本）

- 管理一个对局中的所有玩家

## 🌲 Player

表示 玩家逻辑实体，一般挂在 Room 下

- 不是场景中的角色模型（那是 Unit），而是和服务器交互的逻辑层
- 管理玩家的账户数据（ID、名字、分数、连接状态…）

# 🍎 组件

```
Scene (Entity)
 ├── CurrentScenesComponent   (管理子场景)
 ├── UnitComponent            (管理所有Unit)
 ├── UIComponent              (管理所有UI窗口)
 ├── RoomManagerComponent     (管理所有房间)
 ├── PlayerComponent          (管理所有玩家逻辑)
```

```
UI (Entity)
 └── UILSRoomComponent  (房间列表UI逻辑)
```

## 🌲 CurrentScenesComponent

待完善

## 🌲 UnitComponent

待完善

## 🌲 UIComponent

待完善

## 🌲 RoomManagerComponent

待完善

## 🌲 UILSRoomComponent

待完善

## 🌲 PlayerComponent

待完善

# 🍎 生命周期

## 🌲 IAwake

待完善

## 🌲 IUpdate

待完善

## 🌲 ILateUpdate

待完善

## 🌲 IDestroy

待完善

## 🌲 IScene

待完善 Fiber SceneType

# 🍎 配表

配表的用途用一句话总结就是通过数据载体配置静态数据, 载体可以是`excel, json, csv`等, 配置完毕后通过生成工具生成代码, 可以在游戏中直接使用这些数据, 这对我来说是一个新鲜的东西, 在前后台开发10年我都没有听说过配表这个东西

## 🌲 配表目录

首先我们要打开`ET`的配表目录

![](images2/Pasted%20image%2020250823133508.png)

## 🌲 配表规则

![](images2/Pasted%20image%2020250823134329.png)

- 三行三列开始配表, 也就是第一个字段从`C3`开始
- 「标记1」所在行是字段的中文说明
- 「标记2」标记行是字段的实际使用名称, 生成代码的时候可以看到
- 「标记3」是数据类型, 生成代码的时候可以看到
- 「标记4和6」为注释, 使用`#`, 标记一列就是注释一列, 标记一行就是注释一行, 我用错了写的汉字
- 「标记5」标记`s`的数据只给服务端使用, 标记`c`的数据只给客户端使用

## 🌲 生成代码

我们就用自带的数据来测试, 接下来我们需要使用`ET`提供的代码生成工具来生成可以直接使用数据的`类`

![](images/Pasted%20image%2020250824175609.png)

我们点击ET栏目中的`Build Tool`

![](images2/Pasted%20image%2020250823135659.png)

![](images2/Pasted%20image%2020250823135713.png)

然后我们找到目录`Assets -> Model -> Generate -> ClientServer -> Config`

![](images2/Pasted%20image%2020250823140057.png)

然后我们来看看它给我们生成的代码

```cs
namespace ET
{
    [Config]
    public partial class UnitConfigCategory : Singleton<UnitConfigCategory>, IMerge
    {
        [BsonElement]
        [BsonDictionaryOptions(DictionaryRepresentation.ArrayOfArrays)]
        private Dictionary<int, UnitConfig> dict = new();
		
        public void Merge(object o)
        {
            UnitConfigCategory s = o as UnitConfigCategory;
            foreach (var kv in s.dict)
            {
                this.dict.Add(kv.Key, kv.Value);
            }
        }
		
        public UnitConfig Get(int id)
        {
            this.dict.TryGetValue(id, out UnitConfig item);

            if (item == null)
            {
                throw new Exception($"配置找不到，配置表名: {nameof (UnitConfig)}，配置id: {id}");
            }

            return item;
        }
		
        public bool Contain(int id)
        {
            return this.dict.ContainsKey(id);
        }

        public Dictionary<int, UnitConfig> GetAll()
        {
            return this.dict;
        }

        public UnitConfig GetOne()
        {
            if (this.dict == null || this.dict.Count <= 0)
            {
                return null;
            }
            
            var enumerator = this.dict.Values.GetEnumerator();
            enumerator.MoveNext();
            return enumerator.Current; 
        }
    }

	public partial class UnitConfig: ProtoObject, IConfig
	{
		/// <summary>Id</summary>
		public int Id { get; set; }
		/// <summary>Type</summary>
		public int Type { get; set; }
		/// <summary>名字</summary>
		public string Name { get; set; }
		/// <summary>位置</summary>
		public int Position { get; set; }
		/// <summary>身高</summary>
		public int Height { get; set; }

	}
}
```

我们可以看到这个代码中有同名的文件`UnitConfig`, 这个就是我们数据的字段了, 还有一个东西`UnitConfigCategory`, 这个就是我们使用数据的时候用的类, 可以看到它里面有很多的方法, 比如Get, GetAll, 我们之后会学习

## 🌲 在哪使用

这是一个好问题, 因为我尝试过在`Unity\Assets\Scripts\Loader\MonoBehaviour\Init.cs`脚本中打印

![](images/Pasted%20image%2020250824002035.png)

发现根本行不通, 提示的是找不到这个类, 后来经过调研是因为`ET`是分层的, 我们生成的代码在`Unity.Loader`程序集和`Unity.Model`访问不通, 为此我尝试了把依赖加上

```json
D:\project\unity\test-et81\Unity\Assets\Scripts\Loader\Unity.Loader.asmdef

"references": [
    "Unity.ThirdParty",
    "Unity.Core",
    "HybridCLR.Runtime",
    "Unity.Model",
    "MemoryPack",
    "YooAsset",
    "YooAsset.Editor"
],
```

加上之后我先类是可以访问了, 但是访问的时候报错

![](images/Pasted%20image%2020250824002445.png)

```
FileNotFoundException: Could not load file or assembly 'Unity.Model, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null' or one of its dependencies.

NullReferenceException: Object reference not set to an instance of an object
ET.Init.Update () (at Assets/Scripts/Loader/MonoBehaviour/Init.cs:48)

NullReferenceException: Object reference not set to an instance of an object
ET.Init.LateUpdate () (at Assets/Scripts/Loader/MonoBehaviour/Init.cs:54)
```

后来经过研究发现是这样的, 因为我们调用代码的时候, 游戏还没有把数据加载到字典类中, 所以导致了这个问题, 既然问题知道了, 那我们如何解决呢, 其实老手一下就解决了, 我们可以放到`Update`中进行测试, 因为每一帧都会调用一个`Update`, 并且加上`if`判断即可

```cs
if (UnitConfigCategory.Instance != null)
{
	Debug.Log("我在这里");
	var a = UnitConfigCategory.Instance.Get(1001);
	Debug.Log(a);
}
else
{
	Debug.Log("空空如也");
}
```

可以看到数据是可以打印出来的

![](images/Pasted%20image%2020250824173915.png)

要记住使用`Debug.Log`不要使用`cw`否则打印不出来, 虽然我们做出来了, 但是不建议在`Init`方法中来学习, 我们可以放一个空对象在视图上, 然后在主目录挂载一个脚本上去, 这样不用配置域也是可以访问的

### 🌸 新建空对象和脚本

![](images/Pasted%20image%2020250824174057.png)

然后把上面做的依赖给还原回来, 测试代码删除掉就行了

```json
D:\project\unity\test-et81\Unity\Assets\Scripts\Loader\Unity.Loader.asmdef

"references": [
    "Unity.ThirdParty",
    "Unity.Core",
    "HybridCLR.Runtime",
    "MemoryPack",
    "YooAsset",
    "YooAsset.Editor"
],
```

## 🌲 使用方法

经过上面的学习, 我们已经知道可以在哪里测试配表的数据了, 我们就开始学习

![](images/Pasted%20image%2020250824175609.png)

### 🌸 Get

在上面我们已经使用过了, 这个就是通过id去找一条数据

```cs
if (UnitConfigCategory.Instance != null)
{
	Debug.Log("我在这里");
	var a = UnitConfigCategory.Instance.Get(1001);
	Debug.Log(a);
}
else
{
	Debug.Log("空空如也");
}
```

```
"{ \"_t\" : \"UnitConfig\", \"_id\" : 1001, \"Type\" : 1, \"Name\" : \"米克尔\", \"Position\" : 1, \"Height\" : 178, \"Weight\" : 68 }"
```

我们发现看的非常不清晰, 因为有转义符

https://learn.microsoft.com/zh-cn/visualstudio/debugger/string-visualizer-dialog-box?view=vs-2022

后来尝试了, 发现还是不行, `JsonUtility`是官方的库, 我把对象转化成`json`打印, 结果是`{}`, 说明并没有转换成功

```cs
void Update()
{
    if (UnitConfigCategory.Instance != null)
    {
        Debug.Log("我在这里");
        var a = JsonUtility.ToJson(UnitConfigCategory.Instance.Get(1001), true);
        Debug.Log(a);
    } else
    {
        Debug.Log("空空如也");
    }
}
```

`json`转换不研究了, 有点心累, 先把文档做出来

### 🌸 GetAll

获取所有的呗


# 🍎 编译原理

学习完这一章我有了更深刻的认识, 我推荐不知道的可以点进来看一下

[编译原理](../编译原理/编译原理.md)

# 🍎 FAQ

## 🌲 Unity项目崩溃后调试慢

这个是正常的, 项目崩溃后它可能会清除掉所有的`dll`重新进行编译, 所以启动时会非常慢

![](images2/Pasted%20image%2020250817190134.png)

# 🍎 参考资料

我这里选择了ET`8.1`, 参考视频如下

## 🌲 视频

- 和v诺
https://www.bilibili.com/video/BV1Ls421A7mS

- 上上签UpUpDraw
https://www.bilibili.com/video/BV1rhYyeKExP

- 字母哥ET 6.0教程
https://www.bilibili.com/video/BV1xK4y1E76q

- 烟雨迷离半世殇
https://www.bilibili.com/video/BV1BJ411H7oZ

## 🌲 文章

- 荷兰猪小灰灰
https://blog.csdn.net/m0_48781656/article/details/123483915?utm_source=chatgpt.com

- 官方社区
https://et-framework.cn/t/Course?utm_source=chatgpt.com

- 【ET框架】基础及实践难题解答 - 幸运淦淦
https://blog.csdn.net/u014685437/article/details/132273445

- 小弟的ET笔记
https://www.yuque.com/et-xd/docs

