# 🍎 简介

`ET`是`tanghai`大佬开发的一个`Unity`游戏框架, 它是一个帮我们设置好架构的项目, 并且它包含`前端+后端`, 是全栈的项目, 我们可以在此基础上去开发我们自己的游戏, 把时间更多的用在设计上

https://github.com/egametang/ET

竞争产品是`GameFramework`

https://github.com/EllanJiang/GameFramework

本文主要讲ET

# 🍎 快速开始

我这里选择了ET`8.1`, 参考视频如下

https://www.bilibili.com/video/BV1rhYyeKExP

https://www.bilibili.com/video/BV1wy411Y7oY

## 🌲 下载并解压

直接去官网, 切换分支到`8.1`

![](images/Pasted%20image%2020250814234248.png)

然后直接把项目下载下来就可以了, 解压出来大概是`84M`

![](images/Pasted%20image%2020250814234413.png)

可以看到里面的目录结构是这样的

![](images/Pasted%20image%2020250814234804.png)

文件的变化我这里建了一个`Git`仓库

https://gitee.com/objcat/test-et81

## 🌲 打开项目

我们使用`Unity Hub`中的`Open`打开里面的`Unity`文件夹, 作者推荐的`Unity`的版本是`2022.3.15`, 非必要不要去用别的版本, 我是懒得下载了, 用的`2023.3.16` - -

![](images/Pasted%20image%2020250815002712.png)

我的会提示我版本不对

![](images/Pasted%20image%2020250816121006.png)

然后我选择的是`Choose another Editor verison`, 然后点击下面的`Open`

![](images/Pasted%20image%2020250816121048.png)

然后会有一个修改`Unity版本`的提示

![](images/Pasted%20image%2020250816121129.png)

直接点击`Change verison`, 如果和作者使用的不是同一个`Unity`版本中间会有多个小提示

![](images/Pasted%20image%2020250816121252.png)

点击`Continue`就可以了

![](images/Pasted%20image%2020250815001132.png)

项目打开是这个样子

![](images/Pasted%20image%2020250816121327.png)

可以看到, 它在安装我们游戏所需要的库

![](images/Pasted%20image%2020250816121911.png)

初始项目打开是这样的, 啥也没有, 我们来看一下文件变化

![](images/Pasted%20image%2020250816122132.png)

我们可以看到都是有关项目的配置, 这类配置是`Unity`帮我们自动改动的

## 🌲 加载场景

我们首先双击`Scenes`中的`init`

![](images/Pasted%20image%2020250815001453.png)

我打开后是这样的, 到这里我们场景就看到了

![](images/Pasted%20image%2020250815002020.png)

## 🌲 配置Unity

### 🌸 配置默认工具

打开工程后, 点击`菜单上的 -> Edit -> Preferences -> External Tools`, 点击下拉框`External Script Editor`选择`Rider`, `Generate .csproj files for`中的选项全部不要勾选  

![](images/Pasted%20image%2020250816115413.png)

我的`Generate .csproj files for`保持了默认配置

### 🌸 配置AppType

在`Project`视图中选中`Assets/Resources/GlobalConfig`, 把`AppType`选择成`Demo(状态同步)`或者`LockStep(帧同步)`

![](images/Pasted%20image%2020250816122319.png)

默认是这样的, 我们可以看到`App Type`默认选中的是`Demo`, 那这个我们可以不用改动

## 🌲 安装Rider

一笔带过, 太简单了

https://www.jetbrains.com/zh-cn/rider/

![](images/Pasted%20image%2020250814235146.png)

## 🌲 编译项目

### 🌸 编译Unity

![](images/Pasted%20image%2020250816122613.png)

我们在项目窗口, 空白的地方点击鼠标右键, 选择`Open C# Project`, 打开是这样的

![](images/Pasted%20image%2020250816122804.png)

然后我们等待它加载项目

![](images/Pasted%20image%2020250816122748.png)

![](images/Pasted%20image%2020250816122821.png)

这个时间比较长, 需要耐心等待, 然后点击小锤子编译下

![](images/Pasted%20image%2020250816122917.png)

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

![](images/Pasted%20image%2020250816123156.png)

打开项目是这样的

![](images/Pasted%20image%2020250816123514.png)

我们等待加载

![](images/Pasted%20image%2020250816123450.png)

我们直接点击编译

![](images/Pasted%20image%2020250816123609.png)

可以看到日志滚动

![](images/Pasted%20image%2020250816124454.png)

这里我是能编译成功的

![](images/Pasted%20image%2020250816124521.png)

然后我们再回头编译`Unity`发现可以了

![](images/Pasted%20image%2020250816124604.png)

所以我们得到了结论, 缺少`dll`是因为没有编译`ET`导致的

## 🌲 运行项目

![](images/Pasted%20image%2020250816124818.png)

我们点击`Unity`的运行场景, 可以看到场景可以运行起来

![](images/Pasted%20image%2020250816125145.png)

我们可以右键`Game`, 能够看到一个`Maximize`可以把窗口放大到全屏

![](images/Pasted%20image%2020250816125114.png)

然后是输入密码

![](images/Pasted%20image%2020250816125234.png)

这个没有做限制随便输入即可, 然后点击进入

![](images/Pasted%20image%2020250816125252.png)

我们可以看到能进入游戏了

![](images/Pasted%20image%2020250816125313.png)

这就是`ET`给我们提供的`Demo`, 鼠标右键点击可以移动角色

![](images/Pasted%20image%2020250816125422.png)

R是热重载, 如果我们修改了`ET`的代码可以重新`Build`一次`ET`, 然后点击`R`可以直接热重载`dll`, 营业就是这些东西

![](images/Pasted%20image%2020250816125759.png)

T是切换地图

![](images/Pasted%20image%2020250816125829.png)

我这里项目就运行好了

# 🍎 工程讲解

## 🌲 目录讲解

ET的目录结构是这样的, 我们先总览一下, 其中`DotNet`这个区域是`服务端`, `Share`是工具, `Unity`是`游戏端`

![](images/Pasted%20image%2020250815003251.png)

## 🌲 Unity

### 🌸 模块讲解

我们先来看这个模块, 根据视频讲解, 这是作者给我们根据功能分出来的不同的`程序集`, 在我理解和Java中的`Maven Module`差不多, 就是根据分工把一个项目拆成一堆小项目

- 与定义相关的放在`Model`中
- 与实现相关的放在`Hotfix`中
- 与视图定义相关的放在`ModelView`中
- 与视图实现相关的放在`HotfixView`中

举个例子, 我们找到

![](images/Pasted%20image%2020250815012037.png)

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

![](images/Pasted%20image%2020250815012141.png)

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

也就是说我们每一个`Model`实现的东西都在`HotFix`中是有一一对应的实现文件的, 并且这个文件是`xxxComponentSystem`来命名的

### 🌸 程序集依赖

程序集之间是存在依赖关系的, 我们可以看一下它的依赖文件

![](images/Pasted%20image%2020250815012954.png)

可以看到一个叫做`Unity.Core.asmdef`的, 我们打开

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

可以看到`Core`依赖`Unity.ThirdParty`, 其他程序集的依赖关系也可以自己看看

# 🍎 配表

配表的用途用一句话总结就是xxx

源码位置为`Share/Tool/ExcelExporter/ExcelExporter.cs`有兴趣可以自己看看




