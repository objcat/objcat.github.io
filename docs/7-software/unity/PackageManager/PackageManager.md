# 🍎 简介

`PackageManager`是`Unity`内置的包管理工具

## 🌲 概览

`Unity`提供给我们可视化的界面进行管理

![](images/Pasted%20image%2020250824191750.png)

我们打开它可以看到我们已经安装的包

![](images/Pasted%20image%2020250824191802.png)

他也有自己的配置文件, 处于`Package`文件夹下面

```
test-et81\Unity\Packages
```

我们打开文件夹可以看到

![](images/Pasted%20image%2020250824192004.png)

然后找到一个`manifest.json`文件, 用`vscode`打开就能看到里面管理的包了, 只不过在可视化界面上我们看的更清晰一些

```json
{
  "dependencies": {
    "com.code-philosophy.hybridclr": "https://gitee.com/focus-creative-games/hybridclr_unity.git",
    "com.cysharp.memorypack": "https://github.com/Cysharp/MemoryPack.git?path=src/MemoryPack.Unity/Assets/Plugins/MemoryPack#1.10.0",
    "com.tuyoogame.yooasset": "2.1.1",
    "com.unity.ai.navigation": "1.1.5",
    "com.unity.ide.rider": "3.0.27",
    "com.unity.ide.visualstudio": "2.0.22",
    "com.unity.ide.vscode": "1.2.5",
    "com.unity.nuget.newtonsoft-json": "3.2.1",
    "com.unity.render-pipelines.universal": "14.0.9",
    "com.unity.textmeshpro": "3.0.6",
    "com.unity.timeline": "1.7.6",
    "com.unity.ugui": "1.0.0",
    "com.unity.modules.ai": "1.0.0",
    "com.unity.modules.androidjni": "1.0.0",
    "com.unity.modules.animation": "1.0.0",
    "com.unity.modules.assetbundle": "1.0.0",
    "com.unity.modules.audio": "1.0.0",
    "com.unity.modules.cloth": "1.0.0",
    "com.unity.modules.director": "1.0.0",
    "com.unity.modules.imageconversion": "1.0.0",
    "com.unity.modules.imgui": "1.0.0",
    "com.unity.modules.particlesystem": "1.0.0",
    "com.unity.modules.physics": "1.0.0",
    "com.unity.modules.physics2d": "1.0.0",
    "com.unity.modules.screencapture": "1.0.0",
    "com.unity.modules.terrain": "1.0.0",
    "com.unity.modules.terrainphysics": "1.0.0",
    "com.unity.modules.tilemap": "1.0.0",
    "com.unity.modules.ui": "1.0.0",
    "com.unity.modules.uielements": "1.0.0",
    "com.unity.modules.umbra": "1.0.0",
    "com.unity.modules.unityanalytics": "1.0.0",
    "com.unity.modules.unitywebrequest": "1.0.0",
    "com.unity.modules.unitywebrequestassetbundle": "1.0.0",
    "com.unity.modules.unitywebrequestaudio": "1.0.0",
    "com.unity.modules.unitywebrequesttexture": "1.0.0",
    "com.unity.modules.unitywebrequestwww": "1.0.0",
    "com.unity.modules.vehicles": "1.0.0",
    "com.unity.modules.video": "1.0.0",
    "com.unity.modules.wind": "1.0.0"
  },
  "scopedRegistries": [
    {
      "name": "package.openupm.com",
      "url": "https://package.openupm.com",
      "scopes": [
        "com.tuyoogame.yooasset"
      ]
    }
  ]
}
```

这个`manifest.json`就是我们的包管理文件, 是不是第一时间就能联想到安卓的`manifest.xml`呢, 这个承载的功能比安卓的清单会少很多

## 🌲 安装新包

### 🌸 Package Manager安装

#### 🌼地址安装

通过地址安装, 一般都是`xxxx.git`

#### 🌼包名安装

顾名思义, 我们输入包名就能直接安装, 我这里使用`Newtonsoft Json`来举例子

![](images/Pasted%20image%2020250824222826.png)

然后输入

```
com.unity.nuget.newtonsoft-json
```

我们可以看到已经安装成功了

![](images/Pasted%20image%2020250824222228.png)

我们来查看一下`manifest`文件

```
"com.unity.nuget.newtonsoft-json": "3.2.1"
```

### 🌸 Asset Store安装

![](images/Pasted%20image%2020250824224150.png)

然后会出现这个

![](images/Pasted%20image%2020250824224209.png)

点击这个`Search online`

https://assetstore.unity.com/zh-CN

会打开这个网址, 在里面我们就可以搜索我们需要的包了

### 🌸 清单安装(不推荐)

不推荐新手手动管理清单文件, 没有可视化方便, 除非你老手...

首先找到清单文件

```
test-et81\Unity\Packages\manifest.json
```

dependencies中直接加上去, 比如`newtonsoft json`

```
"com.unity.nuget.newtonsoft-json": "3.2.1"
```

![](images/Pasted%20image%2020250824192436.png)

然后刷新发现可以用了

# 🍎 推荐库

这里主要介绍我用过的第三方库, 做一个记录

## 🌲 Newtonsoft json

https://docs.unity3d.com/Packages/com.unity.nuget.newtonsoft-json@3.2/manual/index.html

包名

```
com.unity.nuget.newtonsoft-json
```
