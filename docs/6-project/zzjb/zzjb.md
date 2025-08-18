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

## 🌲 调研运行错误1

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

我不知道这是干啥的, 但是大致可以推断出来就是程序启动的时候要进行热更新, 在这里找到出错的代码

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

## 🌲 解决运行错误1(成功运行场景)

后来大佬说要按一下F6来运行

![](images/Pasted%20image%2020250815124110.png)

经过编译发现这些文件自动生成了, 然后点击运行单个场景是可以的

![](images/Pasted%20image%2020250815124328.png)

# 🍎 页面分析

经过上面的步骤我们已经可以运行项目了

## 🌲 登录页面分析

当我输入我的账号密码时等待一会会有报错

![](images/Pasted%20image%2020250815234831.png)

### 🌸 问题1

我们先看一下错误日志

```shell
Error: 100208
ET.RpcException: Rpc error: actorId: 1:10000002:1 request: ET.Main2NetClient_Login, response: { "_t" : "NetClient2Main_Login", "RpcId" : 2, "Error" : 100208, "Message" : "Error: 100208\nET.RpcException: session dispose: 2942900818 127.0.0.1:30002\r\n  ET.ETTask`1[T].GetResult () [0x00036]
...
ET.Init:Update () (at Assets/Scripts/Loader/MonoBehaviour/Init.cs:47)
```

这个问题我们通过分析错误日志就能看出来, 本地的`127.0.0.1:30002`服务器没有开启, 导致登录失败, 那么我们反推得到这个服务器就是我们登录用的, 我们现在需要做的就是找打服务器运行起来

### 🌸 问题2

我们先看一下错误日志

```shell
System.TimeoutException: A timeout occurred after 30000ms selecting a server using CompositeServerSelector{ Selectors = MongoDB.Driver.MongoClient+AreSessionsSupportedServerSelector, LatencyLimitingServerSelector{ AllowedLatencyRange = 00:00:00.0150000 }, OperationsCountServerSelector }. Client view of cluster state is { ClusterId : "1", Type : "Unknown", State : "Disconnected", Servers : [{ ServerId: "{ ClusterId : 1, EndPoint : "127.0.0.1:27017" }", EndPoint: "127.0.0.1:27017", ReasonChanged: "Heartbeat", State: "Disconnected", ServerVersion: , TopologyVersion: , Type: "Unknown", HeartbeatException: "MongoDB.Driver.MongoConnectionException: An exception occurred while opening a connection to the server. ---> System.Net.Sockets.SocketException: 由于目标计算机积极拒绝，无法连接。
```

这个问题我们能够看出来是向服务器发送了心跳包, 服务器没响应

### 🌸 分析UI

那我们就从`UI`开始入手, 一步一步的分析它是如何登录的

![](images/Pasted%20image%2020250817175454.png)

可以看到这就是我们的登录界面, 与之对应的游戏界面是

![](images/Pasted%20image%2020250817175537.png)

运行项目过一段时间后, 我们就能看到登录框出来了

![](images/Pasted%20image%2020250817175616.png)

这说明这个登录框最开始是隐藏的, 运行游戏之前先`检查更新`, 如果`没有可用更新`或者`检测更新失败`就显示登录框让玩家进行登录

我们从上面的层级面板也可以看到`EG_Login`是灰色的, 证明最开始它是隐藏的, 我们把他显示出来, 只需要在`inspector`中勾选显示即可

![](images/Pasted%20image%2020250817175917.png)

然后我们看画面就是显示的, 与检测更新叠加在一起了

![](images/Pasted%20image%2020250817175932.png)

我们看一下里面所有的对象

![](images/Pasted%20image%2020250817180002.png)

通过文字我们就能看出来`E_Username`是用户名输入框,, `E_Password`是密码输入框, `E_Login`是登录游戏, `E_Register`是注册, 当我点击`E_Login`是如何知道点击的呢? 新手肯定会很懵, 因为在`inspector`上面根本没有绑定任何脚本, 不要慌, 我们点击到`DlgLogin`可以看到这个大的组件上面绑定着脚本

![](images/Pasted%20image%2020250817180253.png)

![](images/Pasted%20image%2020250817180245.png)

我们来看一下代码

```cs
using UnityEngine;
using UnityEngine.UI;

namespace ET
{
    public class InitLogin : MonoBehaviour
    {
        private static InitLogin instance;
        public static InitLogin Instance
        {
            get
            {
                if (instance == null)
                {
                    instance = GameObject.Find("Canvas/DlgLogin").GetComponent<InitLogin>();
                }
                return instance;
            }
        }

        private GameObject eg_login;

        private Text e_update;

        private GameObject eg_register;

        private GameObject eg_name;
        
        private void Awake()
        {
            // Application.targetFrameRate = 30;
            
            eg_login = transform.Find("EG_Login").gameObject;
            eg_register = transform.Find("EG_Register").gameObject;
            eg_name = transform.Find("EG_Name").gameObject;
            e_update = transform.Find("E_Update").GetComponent<Text>();

            Init();
        }

        private void Init()
        {
            eg_login.SetActive(false);
            eg_register.SetActive(false);
            eg_name.SetActive(false);
        }

        // 显示更新界面
        public void ShowUpdate(string text)
        {
            e_update.text = text;
        }

        // 显示登录界面
        public void ShowLogin()
        {
            this.e_update.gameObject.SetActive(false);
            eg_login.SetActive(true);
            eg_register.SetActive(false);
            eg_name.SetActive(false);
        }

        // 关闭UI
        public void Close()
        {
            Destroy(gameObject);
        }

    }
}
```

代码并不多, 我们全贴出来, 可以看到这个脚本上来定义了很多组件, 这些组件用来接收`层级窗口`中定义好的`游戏对象`

```cs
private GameObject eg_login;
private Text e_update;
private GameObject eg_register;
private GameObject eg_name;
```

这些组件在`Awake`的时候从脚本中使用`transform.Find`加载进来, 有人可能会有疑问, 为什么用一个控制位置的变量来寻找子组件, 其实是这样的这个方法是用来找`固定名称`的子组件的`transform`, 然后通过`.gameObject`获取到的对应组件

```cs
private void Awake()
{
	// Application.targetFrameRate = 30;
	
	eg_login = transform.Find("EG_Login").gameObject;
	eg_register = transform.Find("EG_Register").gameObject;
	eg_name = transform.Find("EG_Name").gameObject;
	e_update = transform.Find("E_Update").GetComponent<Text>();

	Init();
}
```

其实有更好的方法来绑定, 那就是拖拽, 我们需要先把变量改成`public`

```cs
public GameObject eg_login;
public Text e_update;
public GameObject eg_register;
public GameObject eg_name;
```

然后再`Dlg_Login`中会出现这些变量的引用

![](images/Pasted%20image%2020250817222512.png)

在`Scene`场景窗口把对应的对象拖拽到上面即可

![](images/Pasted%20image%2020250817222936.png)

然后可以看到绑定上了

![](images/Pasted%20image%2020250817223041.png)

这个拖拽的实现原理其实是通过修改`Init.unity`文件来实现的, 它就是`ET`给我们的初始化场景文件, 其实它是一个`yaml`

![](images/Pasted%20image%2020250817223731.png)

我们拿刚才的东西举例子, 其实是通过id绑定的, 这样无论怎么改名字也都能找到组件

![](images/Pasted%20image%2020250817223912.png)

扯远了, 我们话说回来, 怎么找到点击登录按钮的方法呢

比如`Login`组件

![](images/Pasted%20image%2020250817182354.png)

我们获取到这个组件了, 当点击组件的时候发送一个网络请求去登录, 然后传递用户名和密码的参数就可以了, 我们来找找逻辑在哪里, 我们知道`eg_login`就是登录按钮, 那我们看看它绑定的方法在哪里?

呃, 在本页我是没有找到这个方法的

### 🌸 脚本Instance

就在一筹莫展的时候这段代码引起了我的注意

```cs
private static InitLogin instance;
public static InitLogin Instance
{
	get
	{
		if (instance == null)
		{
			instance = GameObject.Find("Canvas/DlgLogin").GetComponent<InitLogin>();
		}
		return instance;
	}
}
```

可以看到它使用了`GameObject.Find`方法, 获取到了场景中的`DlgLogin`组件的`InitLogin`组件, 也就是脚本本身, 因为脚本本身也是一个组件, 也就是我们脚本绑定的组件

![](images/Pasted%20image%2020250817224431.png)

我们可以会看一下, 确实是在`Canvas`对象下面的, `Canvas/DlgLogin`游戏对象竟然可以用这种路径, 那我是否可以理解游戏对象也是个`文件夹`手动滑稽

我们可以看到它是一个`static`的实例, 那我有充分理由怀疑他是在别的脚本中去获取这个`instance`实例, 然后取出`EG_Login`游戏对象并绑定方法的

![](images/Pasted%20image%2020250817231310.png)

但是在我看来大多都是去修改更新文案的, 所以应该不是用这种方法去做的, 这条路又断了

### 🌸 事件注册

又在一筹莫展的时候我找到了这玩意, 因为我在搜索的时候注意到了`DlgLogin`和我的场景是`重名的`, 所以我发现了这个代码

```cs
namespace ET.Client
{
	[FriendOf(typeof(UIBaseWindow))]
	[AUIEvent(WindowID.WindowID_Login)]
	public  class DlgLoginEventHandler : IAUIEventHandler
	{

		public void OnInitWindowCoreData(UIBaseWindow uiBaseWindow)
		{
		  uiBaseWindow.windowType = UIWindowType.Normal; 
		}

		public void OnInitComponent(UIBaseWindow uiBaseWindow)
		{
		  uiBaseWindow.AddComponent<DlgLogin>().AddComponent<DlgLoginViewComponent>();
		}
```

这个代码应该就是绑定一些东西, 但是在这里我还是没看到`Login`的方法, 无奈, 我只能从控制台的日志入手了, 直接控制台反爬登录接口

### 🌸 分析登录接口的日志

好我们开始, 首先我们点击登录按钮

![](images/Pasted%20image%2020250818130247.png)

可以在下方看到日志, 因为我是随便去输入的, 所以肯定是报错的

![](images/Pasted%20image%2020250818130416.png)

可以看到确实报错了, 我们向上去翻可以找到这个

![](images/Pasted%20image%2020250818130437.png)

当我输入用户名和密码的时候会出现一个打印, 我们顺着打印摸过去, 发现这貌似是一个打印日志的工具, 我们放上断点, 再次点登录

![](images/Pasted%20image%2020250818131743.png)

从调用堆栈我们看到了一个可疑的东西, 应该是一个`Login`登录事件的捕获类

![](images/Pasted%20image%2020250818131931.png)

点进去代码是这样的

```cs
namespace ET.Client
{
    [MessageHandler(SceneType.NetClient)]
    public class Main2NetClient_LoginHandler : MessageHandler<Scene, Main2NetClient_Login, NetClient2Main_Login>
    {
        protected override async ETTask Run(Scene root, Main2NetClient_Login request, NetClient2Main_Login response)
        {
            string account = request.Account;
            string password = request.Password;
            string version = request.Version;
            // 创建一个ETModel层的Session
            root.RemoveComponent<RouterAddressComponent>();
            ......此处略去一万行
```

我们打断点后发现`登录`确实是调用的这个方法, 并且我们可以看到这个逻辑是获取`Account`和`Password`, 然后就是验证, 并且使用的是`ET`的消息订阅机制, 我们只需要沿着这个`反向摸过去`就能找到登录按钮是如何绑定登录的了

# 🍎 FAQ

## 🌲 DOTween.Modules.dll

双编就能解决, 编译`霸主Unity`和`ET`

```
13>CSC: Error CS0006 : 未能找到元数据文件“D:\project\unity\zzjb2d\Unity\Temp\bin\Debug\DOTween.Modules.dll”
13>CSC: Error CS0006 : 未能找到元数据文件“D:\project\unity\zzjb2d\Unity\Temp\bin\Debug\game.dll”
13>------- Finished building project: Unity.HotfixView. Succeeded: False. Errors: 2. Warnings: 0
```