# 🍎 简介

据说是`Unity`采用的新版的热更新方案, 英文名是`HybirdCLR`, 中文名是`华佗`

https://hybridclr.doc.code-philosophy.com/docs/intro

# 🍎 快速开始

## 🌲 起因

我打开了别人的项目后, 想运行项目出现了一个问题

```shell
System.IO.FileNotFoundException: Could not find file "D:\project\unity\zzjb2d\Unity\Assets\Bundles\Code\Unity.Model.dll.bytes"
```

经过多方询问, 这个东西就是`HybirdCLR`生成的, 所以我只能先学习一下这个

## 🌲 对比其他热更新方案

![](images/Pasted%20image%2020250814204544.png)

## 🌲 热更新流程

![](images/Pasted%20image%2020250814204922.png)

## 🌲 安装

在`unity`安装是比较简单的

![](images/Pasted%20image%2020250814204650.png)

直接点击`Installer`安装即可, 然后点击下面的`install`

![](images/Pasted%20image%2020250814204717.png)

安装完成后我们可以看到`installed`变成了`True`, 后来知道了, 我们在包管理器中也是可以安装这个的

![](images/Pasted%20image%2020250814223015.png)

官方的方法是

![](images/Pasted%20image%2020250814223040.png)

然后输入

```
https://gitee.com/focus-creative-games/hybridclr_unity.git
```

如果把这个包卸载掉`HybirdCLR`选项就会消失, 然后我要说这个包应该是`Unity`内置了, 所以我们只需要做第一步的`install初始化(打开菜单HybridCLR/Installer...， 点击安装按钮进行安装。 耐心等待30s左右，安装完成后会在最后打印 安装成功日志。)`就好, 如果发现`HybirdCLR`消失了再去按照官方的做法安装包, 那到这里我们就安装好了

# 🍎 ET8.1配置热更新

参考大佬的方案

https://toxicstar.top/index.php/archives/281/

## 🌲 新建组件

运行Unity，定位到目录Assets/Scripts/Loader/Resource，创建DownloadComponent.cs。

```cs
public class DownloadComponent: Singleton<DownloadComponent>, ISingletonAwake
{
    //添加Awake方法，检查是否需要下载

    //添加CheckDownloadStatus方法，检查是否下载完毕
}
```


打开YooAsset资源更新篇，拷贝官方推荐代码，填入即可。
然后打开场景的Global，找到Init，双击进去，在热更代码前添加DownloadComponent，修改后如下：

```cs
private async ETTask StartAsync()
{
    //xxxxxx

    DownloadComponent downloadComponent = World.Instance.AddSingleton<DownloadComponent>();
    await downloadComponent.CheckDownloadStatus();

    CodeLoader codeLoader = World.Instance.AddSingleton<CodeLoader>();

    //xxxxxx
}
```

打开ResourcesComponent调整CDN地址，选择菜单YooAsset/AssetBundle Collector配置要打包的目录。如此，热更新就配置完成了。