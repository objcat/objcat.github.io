# 🍎 简介

这是`重装机兵-霸主`的战斗服务器代码, 使用了`ioGame`的框架编写

https://github.com/iohao/ioGame

https://iohao.github.io/game/

# 🍎 快速开始

我们来快速把项目跑起来

## 🌲 准备

我这里写的都是我的`IDE`和`环境`

- IDEA 2025.1
- MAVEN 3.9.11
- JAVA21

该下的下, 该配的配, 都准备好

## 🌲 打开项目

![](images/Pasted%20image%2020250819175629.png)

可以看到目录是这样的, 可以看到这是一个`单工程的项目`, 可能是因为项目`规模比较小`, 没有使用`maven module`的模式去开发, 受`spring-cloud`影响, 在开发中我一般会使用父子工程的模式, 因为方便扩展并且可以运用微服务的思想, 这不重要我们接着看

## 🌲 配置maven

由于是新手教程, 所以这里还是说一下, `maven`是`apache`开发的`java包管理工具`, 我们可以通过`pom`文件来引入项目所需要的依赖, 下面是我以前写的配置`maven`的教程

[maven配置教程](../../../4-package-manager/maven/maven.md)

配置好`maven`后点击同步就可以了

![](images/Pasted%20image%2020250819181450.png)

然后等待依赖库拉取完

## 🌲 学习文档

运行项目我们需要去`ioGame`文档学习

https://iohao.github.io/game/docs/installation

```
ioGame 是一个轻量级的网络编程框架，适用于网络游戏服务器、物联网、内部系统及各种需要长连接的场景。
源码完全开放、最新文档阅读完全开放，使用完全自由、免费（遵守开源协议）。

ioGame 具备一次编写到处对接的能力，为客户端提供了代码生成的辅助功能，能够帮助客户端开发者减少巨大的工作量。

你只需要编写一次 java 代码，就能为 Godot、UE、Unity、CocosCreator、Laya、React、Vue、Angular ...等项目生成统一的交互接口。 交互接口确保了方法的参数类型安全且明确，使我们在编译阶段就能发现潜在问题。 这种做法有效避免了安全隐患，并减少了联调时可能出现的低级错误。

支持语言: C#、TypeScript、GDScript、C++、Lua。
```

![](images/Pasted%20image%2020250819182013.png)

这就是`ioGame`的文档了, 目前看到的写的最全面的文档之一, 完爆`ET`的文档

## 🌲 从0开始学习

我们可以看到上面有醒目的`从0开始编写`, 这种教程通常最容易理解, 我们直接过去看关键信息就可以学会怎么运行项目了, 万变不离其中

https://iohao.github.io/game/docs/quick_zero_demo

我一眼就从中发现了东西

```
运行步骤

服务器启动类 DemoApplication.java
模拟客户端启动类 DemoClient.java
```

我们从这里可以得知有里面有两个启动类, 一个是启动服务器的, 一个是启动客户端模拟的, 通常关键信息都是一步一步收集来的

# 🍎 项目分析

待完善



