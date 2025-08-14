# 🍎 简介

大佬开发的一个Unity框架, 相当于一个项目, 我们可以在此基础上去开发我们自己的东西

https://github.com/egametang/ET

# 🍎 快速开始

我这里选择了ET`8.1`, 参考视频如下

https://www.bilibili.com/video/BV1rhYyeKExP

## 🌲 下载

直接去官网, 切换分支到`8.1`

![](images/Pasted%20image%2020250814234248.png)

然后直接把项目下载下来就可以了, 解压出来大概是`84M`

![](images/Pasted%20image%2020250814234413.png)

可以看到里面的目录结构是这样的

![](images/Pasted%20image%2020250814234804.png)

## 🌲 打开项目

我们使用`Unity`打开里面的`Unity`文件夹

![](images/Pasted%20image%2020250815002712.png)

如果提示我们版本不对, 我们就选择以当前版本打开就好, 然后会有一个修改版本号的提示, 不用管它, 然后就开始载入了

![](images/Pasted%20image%2020250815001132.png)

## 🌲 加载场景

我们双击`init`

![](images/Pasted%20image%2020250815001453.png)

我打开后是这样的

![](images/Pasted%20image%2020250815002020.png)

## 🌲 安装Rider

https://www.jetbrains.com/zh-cn/rider/

![](images/Pasted%20image%2020250814235146.png)

## 🌲 打开ET

打开`Unity`会给我们下载很多的库, 这样一来我们就可以使用`Rider`进行开发了, 首先我们打开ET项目, 在目录中找到`ET.sln`, 这个就是我们打开工程的入口

![](images/Pasted%20image%2020250815002739.png)

打开后是这个样子

![](images/Pasted%20image%2020250815003251.png)

## 🌲 Unity

### 🌸 工程讲解

我们先来看这个模块, 根据视频讲解, 这是作者给我们根据功能分出来的不同的`程序集`, 在我理解和Java中的`Maven Module`差不多, 就是根据分工把一个项目拆成一堆小项目

- 与定义相关的放在`Model`中
- 与实现相关的放在`Hotfix`中
- 与视图相关的定义放在`ModelView`中
- 与视图相关的实现放在`HotfixView`中

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





