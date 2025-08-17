# 🍎 简介

这篇文档主要分析`重装机兵-霸主`项目, 中间会添加自己思考的方式, 也当是记录生活, 感谢作者`Dr隆鹰`

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

代码并不多, 我们全贴出来, 可以看到这个脚本上来定义了很多组件

```cs
private GameObject eg_login;
private Text e_update;
private GameObject eg_register;
private GameObject eg_name;
```






# 🍎 FAQ

## 🌲 DOTween.Modules.dll

双编就能解决, 编译`霸主Unity`和`ET`

```
13>CSC: Error CS0006 : 未能找到元数据文件“D:\project\unity\zzjb2d\Unity\Temp\bin\Debug\DOTween.Modules.dll”
13>CSC: Error CS0006 : 未能找到元数据文件“D:\project\unity\zzjb2d\Unity\Temp\bin\Debug\game.dll”
13>------- Finished building project: Unity.HotfixView. Succeeded: False. Errors: 2. Warnings: 0
```