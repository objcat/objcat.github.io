# 🍎 登录页面分析

经过上面的步骤我们已经可以运行项目了

## 🌲 登录报错

当我输入我的账号密码时等待一会会有报错

![](images/Pasted%20image%2020250815234831.png)

### 🌸 问题

我们先看一下错误日志

```shell
Error: 100208
ET.RpcException: Rpc error: actorId: 1:10000002:1 request: ET.Main2NetClient_Login, response: { "_t" : "NetClient2Main_Login", "RpcId" : 2, "Error" : 100208, "Message" : "Error: 100208\nET.RpcException: session dispose: 2942900818 127.0.0.1:30002\r\n  ET.ETTask`1[T].GetResult () [0x00036]
...
ET.Init:Update () (at Assets/Scripts/Loader/MonoBehaviour/Init.cs:47)
```

这个问题我们通过分析错误日志就能看出来, 本地的`127.0.0.1:30002`服务器没有开启, 导致登录失败, 那么我们反推得到这个服务器就是我们登录用的, 我们现在需要做的就是找打服务器运行起来

我们看另外一个错误日志

```shell
System.TimeoutException: A timeout occurred after 30000ms selecting a server using CompositeServerSelector{ Selectors = MongoDB.Driver.MongoClient+AreSessionsSupportedServerSelector, LatencyLimitingServerSelector{ AllowedLatencyRange = 00:00:00.0150000 }, OperationsCountServerSelector }. Client view of cluster state is { ClusterId : "1", Type : "Unknown", State : "Disconnected", Servers : [{ ServerId: "{ ClusterId : 1, EndPoint : "127.0.0.1:27017" }", EndPoint: "127.0.0.1:27017", ReasonChanged: "Heartbeat", State: "Disconnected", ServerVersion: , TopologyVersion: , Type: "Unknown", HeartbeatException: "MongoDB.Driver.MongoConnectionException: An exception occurred while opening a connection to the server. ---> System.Net.Sockets.SocketException: 由于目标计算机积极拒绝，无法连接。
```

这个问题我们能够看出来是向服务器发送了心跳包, 服务器没响应

### 🌸 开始分析

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

### 🌸 分析登录方法

点进去代码是这样的, 符合登录代码的样子, 我们打断点后发现`登录`确实是调用的这个方法, 那我们接下来就分析这个登录页面都做了哪些操作吧

```cs
using System;
using System.Net;
using System.Net.Sockets;

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
            
            // 获取路由跟realmDispatcher地址
            RouterAddressComponent routerAddressComponent =
                    root.AddComponent<RouterAddressComponent, string, int>(request.RouterHttpHost, ConstValue.RouterHttpPort);
            await routerAddressComponent.Init();
            root.AddComponent<NetComponent, AddressFamily, NetworkProtocol>(routerAddressComponent.RouterManagerIPAddress.AddressFamily, NetworkProtocol.UDP);
            root.GetComponent<FiberParentComponent>().ParentFiberId = request.OwnerFiberId;

            NetComponent netComponent = root.GetComponent<NetComponent>();

            IPEndPoint realmAddress = routerAddressComponent.GetRealmAddress(account);

            R2C_Login r2CLogin;
            using (Session session = await netComponent.CreateRouterSession(realmAddress, account, password))
            {
                C2R_Login c2RLogin = C2R_Login.Create();
                c2RLogin.Account = account;
                c2RLogin.Password = password;
                c2RLogin.Version = version;
                r2CLogin = (R2C_Login)await session.Call(c2RLogin);

                // 登录错误
                if (r2CLogin.Error != ErrorCode.ERR_Success)
                {
                    response.Error = r2CLogin.Error;
                    return;
                }
            }

            // 创建一个gate Session,并且保存到SessionComponent中
            Session gateSession = await netComponent.CreateRouterSession(NetworkHelper.ToIPEndPoint(r2CLogin.Address), account, password);
            gateSession.AddComponent<ClientSessionErrorComponent>();

            C2G_LoginGate c2GLoginGate = C2G_LoginGate.Create();
            c2GLoginGate.Key = r2CLogin.Key;
            c2GLoginGate.GateId = r2CLogin.GateId;
            c2GLoginGate.Account = account;
            G2C_LoginGate g2CLoginGate = (G2C_LoginGate)await gateSession.Call(c2GLoginGate);

            if (g2CLoginGate.Error != ErrorCode.ERR_Success)
            {
                response.Error = g2CLoginGate.Error;
                gateSession.Dispose(); // 登录失败，清理 Session
                Log.Error("登录 Gate 失败");
                return;
            }


            Log.Debug("登陆gate成功!");


            // 登陆Gate失败
            // TODO: 这里需要处理登陆Gate失败的逻辑 
            // 上面 root.AddComponent<SessionComponent>().Session = gateSession; 在成功之后在添加否则不保存

            root.AddComponent<SessionComponent>().Session = gateSession;
            response.Error = ErrorCode.ERR_Success;
            response.PlayerId = g2CLoginGate.PlayerId;
            response.HavRole = g2CLoginGate.HavRole;
            response.Gold = g2CLoginGate.Gold;
            response.PurpleGold = g2CLoginGate.PurpleGold;
            response.SwanGold = g2CLoginGate.SwanGold;
            response.SwanPoints = g2CLoginGate.SwanPoints;
            response.Address = r2CLogin.Address;
            
        }
    }
}
```


