# 🍎 登录页面分析

本文分析登录页的代码

## 🌲 登录报错

当我输入我的账号密码时等待一会会有报错

![](images/Pasted%20image%2020250815234831.png)

### 🌸 问题

- 我们先看一下错误日志

```shell
Error: 100208
ET.RpcException: Rpc error: actorId: 1:10000002:1 request: ET.Main2NetClient_Login, response: { "_t" : "NetClient2Main_Login", "RpcId" : 2, "Error" : 100208, "Message" : "Error: 100208\nET.RpcException: session dispose: 2942900818 127.0.0.1:30002\r\n  ET.ETTask`1[T].GetResult () [0x00036]
...
ET.Init:Update () (at Assets/Scripts/Loader/MonoBehaviour/Init.cs:47)
```

这个问题我们通过分析错误日志就能看出来, 本地的`127.0.0.1:30002`服务器没有开启, 导致登录失败, 那么我们反推得到这个服务器就是我们登录用的, 我们现在需要做的就是找打服务器运行起来

- 我们看另外一个错误日志

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

![](../zzjb-unity/images/Pasted%20image%2020250817223731.png)

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

### 🌸 定位到请求位置(重要)

点进去代码是这样的, 符合登录代码的样子, 我们打断点后发现`登录`确实是调用的这个方法, 那这就是我们的请求位置, 我们通过注解`MessageHandler`可以看到这个类接到消息之后就会执行登录操作, 那到底是谁发送的消息呢? 我们继续往下看

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
			......此处略去一万字 以后解读
```

然后里面最有用的信息是`MessageHandler<Scene, Main2NetClient_Login, NetClient2Main_Login>`, 我可以看到`Main2NetClient_Login`和`NetClient2Main_Login`应该就是和我们消息有关的模块

所以我进入了`Main2NetClient_Login`

```cs
namespace ET
{
    [MemoryPackable]
    [Message(ClientMessage.Main2NetClient_Login)]
    [ResponseType(nameof(NetClient2Main_Login))]
    public partial class Main2NetClient_Login : MessageObject, IRequest
    {
        public static Main2NetClient_Login Create(bool isFromPool = false)
        {
            return ObjectPool.Instance.Fetch(typeof(Main2NetClient_Login), isFromPool) as Main2NetClient_Login;
        }
```

可以看到它有一个`Create`方法, 那么这个就是创建消息的方法, 我们只需要看到底是谁创建它, 只有一个地方调用了`Main2NetClient_Login.Create`, 这个文件是`ClientSenderComponentSystem.cs`

```cs
public static async ETTask<NetClient2Main_Login> LoginAsync(this ClientSenderComponent self, string account, string password, string version, string ip)
{
	if (self.netClientActorId == default)
	{
		self.fiberId = await FiberManager.Instance.Create(SchedulerType.ThreadPool, 0, SceneType.NetClient, "");
		self.netClientActorId = new ActorId(self.Fiber().Process, self.fiberId);
	}


	Main2NetClient_Login main2NetClientLogin = Main2NetClient_Login.Create();
	main2NetClientLogin.OwnerFiberId = self.Fiber().Id;
	main2NetClientLogin.Account = account;
	main2NetClientLogin.Password = password;
	main2NetClientLogin.Version = version;
	main2NetClientLogin.RouterHttpHost = ip;
	return await self.Root().GetComponent<ProcessInnerSender>().Call(self.netClientActorId, main2NetClientLogin) as NetClient2Main_Login;
}
```

我们可以看到, `Account`和`Password`都是在这里传递的, 我们看看是谁调用了`LoginAsync`, 可以看到最终的源头是`LoginHelper.cs`, 我们只看这个关键性的代码

```cs
public static async ETTask<(int, bool)> Login(Scene root, string account, string password, string version, string ip)
{
	root.RemoveComponent<ClientSenderComponent>();


	ClientSenderComponent clientSenderComponent = root.AddComponent<ClientSenderComponent>();

	NetClient2Main_Login response = await clientSenderComponent.LoginAsync(account, password, version, ip);

	if (response.Error != ErrorCode.ERR_Success)
	{
		return (response.Error, false);
	}

	root.GetComponent<PlayerComponent>().Account = account;
	root.GetComponent<PlayerComponent>().Password = password;
	root.GetComponent<PlayerComponent>().Address = response.Address;


	if (response.HavRole)
	{
		root.GetComponent<PlayerComponent>().MyId = response.PlayerId;
		root.GetComponent<PlayerComponent>().Gold = response.Gold;
		root.GetComponent<PlayerComponent>().PurpleGold = response.PurpleGold;
		root.GetComponent<PlayerComponent>().SwanGold = response.SwanGold;
		root.GetComponent<PlayerComponent>().SwanPoints = response.SwanPoints;
	}

	return (ErrorCode.ERR_Success, response.HavRole);
}
```

可以看到下面的就是登录的代码, 这里进入我的擅长领域, 我一眼就能看出来`NetClient2Main_Login`就是我们登录后返回的一个参数模型, 里面有用户id之类的个人信息

```cs
NetClient2Main_Login response = await clientSenderComponent.LoginAsync(account, password, version, ip);
```

然后这个`response`在下面是有用途的

```cs
if (response.HavRole)
{
	root.GetComponent<PlayerComponent>().MyId = response.PlayerId;
	root.GetComponent<PlayerComponent>().Gold = response.Gold;
	root.GetComponent<PlayerComponent>().PurpleGold = response.PurpleGold;
	root.GetComponent<PlayerComponent>().SwanGold = response.SwanGold;
	root.GetComponent<PlayerComponent>().SwanPoints = response.SwanPoints;
}
```

这个意思应该就是如果有角色`HavRole`, 就把我们的`PlayerComponent`的属性设置上去, 然后就应该是进入游戏, 可以看到如果没有问题的话就返回一个错误码 这个码应该是没有问题的意思 - -, 然后把`haveRole`传回去这个用意应该是没有角色的时候让用户去创建角色之类的

```cs
return (ErrorCode.ERR_Success, response.HavRole);
```

 我们顺着看看调用`LoginHelper`的地方, 一下就找到了`DlgLoginSystem.cs`, 原来我与绑定页面失之交臂, 在最开始的时候就看到了`DlgLogin`和我的场景名一样, 并且我也点进去看了, 但是没有找到这个`System`, 下面的逻辑就比较简单了, 都是正常的

```cs
// 登录按钮事件
private static void OnLoginClickEventHandler(this DlgLogin self)
{
	self.Root().GetComponent<MusicComponent>().PlaySoundEffect(100).Coroutine();
	string account = self.View.E_UsernameInputField.text.Trim();
	string password = self.View.E_PasswordInputField.text;

	try
	{
		self.OnLogin(account, password).Coroutine();
	}
	catch (Exception e)
	{
		Log.Error(e.ToString());
	}
}
private static async ETTask OnLogin(this DlgLogin self, string account, string password)
{

	int fieldError = ValidateAccountAndPassword(account, password);

	if (fieldError != ErrorCode.ERR_Success)
	{
		self.OnMsg("账号或密码不能为空").Coroutine();
		return;
	}
	GlobalConfig globalConfig = Resources.Load<GlobalConfig>("GlobalConfig");

	(int err, bool havRole) = await LoginHelper.Login(self.Root(), account, password, Application.version, globalConfig.ServerIP);

	// 如果未绑定手机打开绑定界面绑定
	if (err == ErrorCode.ERR_NOTbindPhoneOrEmail)
	{
		await EventSystem.Instance.PublishAsync(self.Root(), new BindPhoneOrEmailEvent()
		{
			open = true,
			account = account
		});

		self.Root().GetComponent<UIComponent>().GetDlgLogic<DlgNotification>().NotificationWaring("您需要验证绑定的邮箱或电话号码！");
		return;
	}


	if (err == ErrorCode.ERR_Success)
	{
		Log.Debug("do login success");

		// 保存账号密码到PlayerPrefs
		PlayerPrefs.SetString(self.PlayerPrefsAccount, account);
		PlayerPrefs.SetString(self.PlayerPrefsPassword, password);

		if (!havRole)
		{
			//首次创建角色，打开命名页面
			self.View.EG_LoginRectTransform.gameObject.SetActive(false);
			self.View.EG_RegisterRectTransform.gameObject.SetActive(false);
			self.View.EG_NameRectTransform.gameObject.SetActive(true);
			return;
		}

		await EventSystem.Instance.PublishAsync(self.Root(), new LoginFinish());
		return;
	}

	Log.Debug("do login fail");
	switch (err)
	{
		case ErrorCode.ERR_LoginPasswordError:
			self.OnMsg("密码错误").Coroutine();
			break;
		case ErrorCode.ERR_LoginAccountNotExist:
			self.OnMsg("账号不存在").Coroutine();
			break;
		default:
			self.OnMsg("登录失败").Coroutine();
			break;
	}
}
```

越来越兴奋了, 以为这个代码真的很正常, 哭了...., `OnLoginClickEventHandler`这玩意不就是点击登录按钮的`Handler`吗, 我们看看在哪

```cs
namespace ET.Client
{
    [FriendOf(typeof(DlgLogin))]
    public static class DlgLoginSystem
    {

        public static void RegisterUIEvent(this DlgLogin self)
        {

            self.View.E_LoginButton.AddListener(self.Root(), self.OnLoginClickEventHandler);
            self.View.E_RegisterButton.AddListener(self.Root(), self.OnRegisterClickEventHandler);

            self.View.E_Rgt_BackButton.AddListener(self.Root(), self.OnBackClickEventHandler);
            self.View.E_Rgt_SubmitButton.AddListener(self.Root(), self.OnRgtSubmitClickEventHandler);

            self.View.E_Nm_SubmitButton.AddListener(self.Root(), self.OnNmSubmitClickEventHandler);

            self.View.E_Rgt_SendSmsCodeButton.AddListener(self.Root(), () =>
            {
                self.OnRgtSendSmsCodeClickEventHandler().Coroutine();
            });

            self.View.E_Button_ForgetButton.AddListener(self.Root(), self.OnForgetClickHandler);
        }
......此处略去一万行
```

可以看到就是这句

```cs
self.View.E_LoginButton.AddListener(self.Root(), self.OnLoginClickEventHandler);
```

看似平平无奇, 但是我们还是有点可说的, 我们看到它的`self.View.E_LoginButton`直接就能获取到`登录按钮`, 这是怎么做到的呢

```cs
namespace ET.Client
{
    [ComponentOf(typeof(UIBaseWindow))]
    public class DlgLogin : Entity, IAwake, IUILogic
    {
        public DlgLoginViewComponent View { get => this.GetComponent<DlgLoginViewComponent>(); }

        public Dictionary<int, EntityRef<Scroll_Item_test>> Dictionary;

        public string PlayerPrefsAccount = "PlayerPrefsAccount";
        public string PlayerPrefsPassword = "PlayerPrefsPassword";

        public Boolean isSendingSmsCode = false;



    }
}
```

我们可以看到这个`self`表示的就是`DlgLogin`这个类继承`Entity`说明它是一个实体, 我们都学过组件实体表数据对吧, 而这个类中绑定了一个`View`, `DlgLoginViewComponent`, 这个东西就可以获取到我们绑定的`E_LoginButton`

```cs
public UnityEngine.UI.Button E_LoginButton
{
	get
	{
		if (this.uiTransform == null)
		{
			Log.Error("uiTransform is null.");
			return null;
		}
		if (this.m_E_LoginButton == null)
		{
			this.m_E_LoginButton = UIFindHelper.FindDeepChild<UnityEngine.UI.Button>(this.uiTransform.gameObject, "EG_Login/E_Login");
		}
		return this.m_E_LoginButton;
	}
}
```

可以看到它就是依靠名字来找的, 通过`FindDeepChild`方法, 这个方法中的原理我不用看都知道, 就是个`transform.Find`, 大不了就加个`递归`呗

```cs
public static Transform FindDeepChild(GameObject _target, string _childName)
{
	Transform resultTrs = null;
	resultTrs = _target.transform.Find(_childName);
	if (resultTrs == null)
	{
		foreach (Transform trs in _target.transform)
		{
			resultTrs = UIFindHelper.FindDeepChild(trs.gameObject, _childName);
			if (resultTrs != null)
				return resultTrs;
		}
	}
	return resultTrs;
}
```

所以可以看到在`DlgLoginViewComponent`这个里面写了大量的代码来获取`E_LoginButton`等数十个`UI`对象, 我从开发经验来看, 这里写的十分麻烦, 十分笨拙...

在问了`gpt`之后发现有`UI Prefab`这个东西, 原来这里面的代码是自动生成的, 有工具, 坏消息是砍了

![](images/Pasted%20image%2020250818205407.png)

真让真炸毛





