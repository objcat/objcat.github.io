# 🍎 简介

```
ioGame
无锁异步化、事件驱动的架构设计；轻量级，无需依赖任何第三方中间件或数据库就能支持集群、分布式
通过 ioGame 可以很容易的搭建出一个集群无中心节点、集群自动化、多进程的分布式游戏服务器
包体小、启动快、内存占用少、更加的节约、无需配置文件、提供了优雅的路由访问权限控制
可同时支持多种连接方式：WS、UDP、TCP...等；框架已支持全链路调用日志跟踪特性
让开发者用一套业务代码，能轻松切换和扩展不同的数据协议：Protobuf、JSON
近原生的性能；业务框架在单线程中平均每秒可以执行 1152 万次业务逻辑
代码即联调文档、JSR380验证、断言 + 异常机制 = 更少的维护成本
框架具备智能的同进程亲和性；开发中，业务代码可定位与跳转
架构部署灵活性与多样性：既可相互独立，又可相互融合
一次编写到处对接，能为客户端生成可交互的代码
逻辑服之间可相互跨进程、跨机器进行通信
支持玩家对游戏逻辑服进行动态绑定
能与任何其他框架做融合共存
对 webMVC 开发者友好
无 spring 强依赖
零学习成本
javaSE
```

https://iohao.github.io/game/

# 🍎 快速开始

## 🌲 环境

这是我的环境

- IDEA 2025.1
- MAVEN 3.9.11
- JAVA21

该下的下, 该配的配, 都准备好

## 🌲 启动服务端

我们直接下载他的`Demo`

https://github.com/iohao/ioGameSimpleOne

然后我们打开项目

![](images/Pasted%20image%2020250819183413.png)

配置`maven`

[maven教程](../../../4-package-manager/maven/maven.md)

然后直接拉取依赖...

好的进入正题, 根据文档我们知道了两件事

```
运行步骤

服务器启动类 DemoApplication.java
模拟客户端启动类 DemoClient.java
```

可以看得出来作者非常贴心, 设计了启动服务器的案例和模拟客户端连接的案例, 我们首先找到`DemoApplication`, 然后直接右键启动就行

```java
package com.iohao.one.example;

import com.iohao.game.external.core.netty.simple.NettySimpleHelper;

import java.util.List;
import java.util.Locale;

/**
 * @author 渔民小镇
 * @date 2023-01-06
 */
public class DemoApplication {
    public static void main(String[] args) {
        Locale.setDefault(Locale.US);
        // 游戏对外服端口
        int port = 10100;
        // 游戏逻辑服
        var demoLogicServer = new DemoLogicServer();
        // 启动
        NettySimpleHelper.run(port, List.of(demoLogicServer));
    }
}
```

启动后是这样的日志

```shell
Sofa-Middleware-Log SLF4J : Actual binding is of type [ com.alipay.remoting Logback ]
37:53 [ain] INFO  c.a.s.c.log.report(ReportUtil.java:30) Sofa-Middleware-Log SLF4J : Actual binding is of type [ com.alipay.remoting Logback ]
37:54 [d-3] INFO  CommonStdout.createMicroBootstrap(DefaultExternalCore.java:71) GameExternalServer port: [10100] - Connection way: [WebSocket] 
37:54 [d-1] INFO  CommonStdout.extractedLog(BrokerServer.java:113) GameBrokerServer port:[10200] - Startup Mode:[Standalone] 
37:54 [d-1] INFO  ConnectionTopic.extractedPrint(ConnectionEventBrokerProcessor.java:87) Broker ConnectionEventType:【CONNECT】 remoteAddress:【127.0.0.1:53141】，Connection:【com.alipay.remoting.Connection@73880afd】
37:54 [6-3] INFO  ConnectionTopic.extracted(ConnectEventClientProcessor.java:86) ConnectionEventType:【CONNECT】 remoteAddress:【127.0.0.1:10200】，Number of GameBrokerServer connections:【1】，registerActive:【0】
37:54 [d-2] INFO  CommonStdout.print(RegisterBrokerClientModuleMessageBrokerProcessor.java:129) Module registration information --- GameBrokerServer port: [10200] --- BrokerClientModuleMessage(header={}, name=GameExternalServer, tag=external, status=1, withNo=-1488348302, brokerClientType=EXTERNAL, id=a1263671-9f6d-45d5-bef0-422fd0ea8733, idHash=-1279349633, address=127.0.0.1:53141, ioGamePid=86aff8cd-bef6-49cb-a483-b85d6abd6458)
37:54 [d-2] INFO  CommonStdout.print(BrokerPrintKit.java:73) GameBrokerServer:10200 --- gameLogicServerList: 
	{Number of servers:1, tag:'external'}
======================== ioGame Business Framework ========================= 
21.30
BLACK RED GREEN YELLOW BLUE MAGENTA CYAN WHITE DEFAULT
======================== Handler ========================= 
To turn off printing, see BarSkeletonBuilder.setting.printHandler
class com.iohao.game.action.skeleton.core.ActionCommandHandler 
======================== Plugin ========================= 
To turn off printing, see BarSkeletonBuilder.setting.printInout
class com.iohao.game.action.skeleton.core.flow.internal.DebugInOut 
======================== DataCodec ========================= 
To turn off printing, see BarSkeletonBuilder.setting.printDataCodec
j-protobuf - com.iohao.game.action.skeleton.core.codec.ProtoDataCodec 
======================== action ========================= 
To turn off printing, see BarSkeletonBuilder.setting.printAction
Print the full package name, see BarSkeletonBuilder.setting.printActionShort
Routing: 1 - 0 --- action : DemoAction.here(HelloMessage helloMessage)  --- return HelloMessage  ~~~ see.(DemoAction.java:43)
Routing: 1 - 1 --- action : DemoAction.jackson(HelloMessage helloMessage)  --- return HelloMessage  ~~~ see.(DemoAction.java:56)
Routing: 1 - 2 --- action : DemoAction.list()  --- return List<HelloMessage>  ~~~ see.(DemoAction.java:69)

37:54 [6-3] INFO  ConnectionTopic.extracted(ConnectEventClientProcessor.java:86) ConnectionEventType:【CONNECT】 remoteAddress:【127.0.0.1:10200】，Number of GameBrokerServer connections:【1】，registerActive:【0】
37:54 [d-1] INFO  ConnectionTopic.extractedPrint(ConnectionEventBrokerProcessor.java:87) Broker ConnectionEventType:【CONNECT】 remoteAddress:【127.0.0.1:53142】，Connection:【com.alipay.remoting.Connection@1f23d074】
37:54 [d-4] INFO  CommonStdout.print(RegisterBrokerClientModuleMessageBrokerProcessor.java:129) Module registration information --- GameBrokerServer port: [10200] --- BrokerClientModuleMessage(header={}, name=demoLogicServer, tag=demoLogicServer, status=1, withNo=-1488348302, brokerClientType=LOGIC, id=0d986c28-f5f7-4ea7-8a30-6ad76bdab9fb, idHash=789338557, address=127.0.0.1:53142, ioGamePid=86aff8cd-bef6-49cb-a483-b85d6abd6458)
37:54 [d-4] INFO  CommonStdout.print(BrokerPrintKit.java:73) GameBrokerServer:10200 --- gameLogicServerList: 
	{Number of servers:1, tag:'demoLogicServer'}
	{Number of servers:1, tag:'external'}

.------..------..------..------..------..------.
|I.--. ||O.--. ||G.--. ||A.--. ||M.--. ||E.--. |
| (\/) || :/\: || :/\: || (\/) || (\/) || (\/) |
| :\/: || :\/: || :\/: || :\/: || :\/: || :\/: |
| '--'I|| '--'O|| '--'G|| '--'A|| '--'M|| '--'E|
`------'`------'`------'`------'`------'`------'

+----------+-----------------------##-------------+---------##---------+-------------------------
| ioGame                           ## Memory                ## Time                            
+----------+-----------------------##-------------+---------##---------+-------------------------
| pid      | 12924                 ## used        | 64.39MB ## start   | 2025-08-19 18:37:53.924 
| version  | 21.30                 ## freeMemory  | 15.61MB ## end     | 2025-08-19 18:37:54.408 
| document | http://game.iohao.com ## totalMemory | 80.0MB  ## consume | 0.48 s                  
+----------+-----------------------##-------------+---------##---------+-------------------------
|          | ioGame issues - https://github.com/iohao/ioGame/issues
+----------+--------------------------------------------------------------------------------------
| adv      | Room games in action - https://iohao.github.io/game/docs/practices/room_in_action
+----------+--------------------------------------------------------------------------------------
| News     | Business Framework - Introduction - https://iohao.github.io/game/docs/core/framework
| News     | ExternalServer - User online and offline hooks - https://iohao.github.io/game/docs/external/user_hook
+----------+--------------------------------------------------------------------------------------
```

## 🌲 启动客户端

服务端我们已经启动好了, 接下来我们来模拟一下客户端连接服务端, 找到`DemoClient.java`

```java
/*
 * ioGame
 * Copyright (C) 2021 - 2023  渔民小镇 （262610965@qq.com、luoyizhu@gmail.com） . All Rights Reserved.
 * # iohao.com . 渔民小镇
 *
 * This program is free software: you can redistribute it and/or modify
 * it under the terms of the GNU Affero General Public License as
 * published by the Free Software Foundation, either version 3 of the
 * License, or (at your option) any later version.
 *
 * This program is distributed in the hope that it will be useful,
 * but WITHOUT ANY WARRANTY; without even the implied warranty of
 * MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
 * GNU Affero General Public License for more details.
 *
 * You should have received a copy of the GNU Affero General Public License
 * along with this program.  If not, see <https://www.gnu.org/licenses/>.
 */
package com.iohao.one.example.client;

import com.iohao.game.external.client.join.ClientRunOne;
import com.iohao.game.external.client.kit.ClientUserConfigs;

import java.util.List;

/**
 * @author 渔民小镇
 * @date 2023-07-17
 */
public class DemoClient {
    public static void main(String[] args) {
        // closeLog. cn: 关闭模拟请求相关日志
        ClientUserConfigs.closeLog();
        // Startup Client. cn: 启动模拟客户端
        new ClientRunOne()
                .setInputCommandRegions(List.of(new DemoRegion()))
                .startup();
    }
}
```

然后我们直接右键运行, 模拟客户端启动是这样的, 看到这个界面说明我们已经和服务器`连接成功了`, 如果服务器没有启动客户端会提示来连接错误

```shell
41:39 [ain] INFO  CommonStdout.startup(ClientRunOne.java:99) 启动成功
提示：[命令执行 : cmd-subCmd] [退出 : q] [帮助 : help]
```

然后我们可以在客户端下去输入指令`help`就是弹出帮助菜单

```shell
help
---------- cmd help ----------
1-0    :    here
1-1    :    Jackson
1-2    :    list
```

然后我们可以看到作者提供了几个命令让我们尝试, 我们先试试`1-0`

```shell
1-0
1-0    :    here
提示：[命令执行 : cmd-subCmd] [退出 : q] [帮助 : help]
42:19 [-47] INFO  c.i.o.e.c.DemoRegion.lambda$initInputCommand$1(DemoRegion.java:49) HelloMessage{name='1, I'm here'}
```

可以看到是有输出的, 我们来看看服务器的日志

```shell
┏━━━━━ Debug. [(DemoAction.java:43).here] ━━━━━ [cmd:1-0 65536] ━━━━━ [demoLogicServer] ━━━━━ [id:0d986c28-f5f7-4ea7-8a30-6ad76bdab9fb]
┣ userId: 0
┣ RequestParam: helloMessage : HelloMessage{name='1'}
┣ ResponseData: HelloMessage{name='1, I'm here'}
┣ ExecutionTime: 7 ms
	┗━━━━━ [ioGame:21.30] ━━━━━ [Thread:User-16-4] ━━━━━ [Connection way:WebSocket] ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

不得不说, 这个日志格式做的真是`赏心悦目`, 我这里我们的服务器和客户端已经可以通信了

## 🌲 查看socket的连接情况

我们使用`netstat`命令

```shell
netstat -ano | findstr ":10100"

TCP    0.0.0.0:10100          0.0.0.0:0              LISTENING       12924
TCP    127.0.0.1:10100        127.0.0.1:53344        ESTABLISHED     12924
TCP    127.0.0.1:53344        127.0.0.1:10100        ESTABLISHED     31220
TCP    [::]:10100             [::]:0                 LISTENING       12924
```

可以看到`[::]:10100和0.0.0.0:10100`表示服务器在`10100`端口使用`IPV6和IPV4`进行`LISTENING`监听

```
TCP    127.0.0.1:10100        127.0.0.1:53344        ESTABLISHED     12924
TCP    127.0.0.1:53344        127.0.0.1:10100        ESTABLISHED     31220
```

中间的两条表示`客户端与服务端`已经建立了`TCP`连接, 一个 TCP 连接只有一条(协议层面)，但是在操作系统里,, `服务端`和`客户端`各自持有一个 socket, 所以`netstat`会显示两条记录, 这两个 socket 互为对端, 它们构成完整的双向通信通道

# 🍎 架构

学一个东西要先了解一下架构知识, 不然学着学着就懵了, 这里引用官方的图

![](images/Pasted%20image%2020250819200125.png)

从架构简图中, 我们知道了整体架构由三部分组成, 分别是

- 1.游戏对外服
- 2.游戏网关
- 3.游戏逻辑服，

三者既可相互独立，又可相互融合。

我觉得作者设计的非常好, 可以相互独立和可以融合, 独立方便我们去拓展业务, 融合可以在单应用时节省大量的部署时间

下面是作者的介绍

```
游戏对外服、Broker（游戏网关）、游戏逻辑服这三部分，在一个进程中。(单体应用，在开发分布式时，调试更加方便)
游戏对外服、Broker（游戏网关）、游戏逻辑服这三部分，在多个进程中。(分布式)
游戏对外服、Broker（游戏网关）这两部分在一个进程中，而游戏逻辑服在多个进程中。(类似之前游戏的传统架构)
甚至可以不需要游戏对外服，只使用 Broker（游戏网关）和游戏逻辑服这两部分，用于其他系统业务。

因为 ioGame 遵循面向对象的设计原则（单一职责原则、开闭原则、里式替换原则、依赖倒置原则、接口隔离原则、迪米特法则）等， 所以使得架构的职责分明，可以灵活的进行组合。

开发人员几乎都遇见过这么一种情况，在项目初期阶段，通常是以单体项目的方式进行开发，随着需求不断的增加与迭代，会演变成一个臃肿的项目。 此时在对一个整体进行拆分是困难的，成本是极高的。甚至是不可完成的，最后导致完全的重新重构。

ioGame 提供了在结构组合上的部署多样性，通过组合的方式，在项目初期就可以避免这些拆分问题。 在开发阶段中，我们可以使用单体应用开发思维，降低了开发成本。 通过单体应用的开发方式，在开发分布式项目时，调试更加的方便。 这既能兼顾分布式开发、项目模块的拆分，又能降低团队的开发成本。
```

可以看到作者非常懂架构的, 而且考虑到了开发初期到后期的这个过程, 服务器的变化

我们查看官方文档, 他说

```
游戏逻辑服需要继承 AbstractBrokerClientStartup ， 有三个方法需要实现

createBarSkeleton，当前游戏逻辑服使用的业务框架。
createBrokerClientBuilder，当前游戏逻辑服信息。
createBrokerAddress，游戏网关的连接地址。
```

# 🍎 三层架构

我们刚才简单了解了`整体架构`, 现在就分别学习这三个模块, 我给他的起的名`三层架构`, 哈哈

## 🌲 逻辑服

因为它离我们最近, 我就先学这个

### 🌸 从入口出发

逻辑服就是我们写业务的服务器, 本人理解应该是类似于`spring`中的`Controller`控制器, 因为它能写业务, 但是又不完全一样, 因为它是服务器的角色, 我们先从`服务端入口`来看, 可以看到一个关键性代码

```java
// 游戏逻辑服
var demoLogicServer = new DemoLogicServer();
// 启动
NettySimpleHelper.run(port, List.of(demoLogicServer));
```

很明显这个就是创建一个`DemoLogicServer`, 然后在启动的时候应用它, 就能给我们服务器提供服务了, 我们来看看`DemoLogicServer`的代码

```java
public class DemoLogicServer extends AbstractBrokerClientStartup {
    @Override
    public BarSkeleton createBarSkeleton() {
        // 业务框架构建器 配置
        var config = new BarSkeletonBuilderParamConfig()
                // 扫描 action 类所在包
                .scanActionPackage(DemoAction.class);

        // 业务框架构建器
        var builder = config.createBuilder();

        // 添加控制台输出插件
        builder.addInOut(new DebugInOut());

        return builder.build();
    }

    @Override
    public BrokerClientBuilder createBrokerClientBuilder() {
        BrokerClientBuilder builder = BrokerClient.newBuilder();
        builder.appName("demoLogicServer");
        return builder;
    }

    @Override
    public BrokerAddress createBrokerAddress() {
        String localIp = "127.0.0.1";
        int brokerPort = IoGameGlobalConfig.brokerPort;
        return new BrokerAddress(localIp, brokerPort);
    }
}
```

和官方说的一样, 我们要实现三个方法, 下面我们一个一个看

### 🌸 createBarSkeleton

这个方法用于配置`当前游戏逻辑服使用的业务框架`, 我们来看代码

```java
// 业务框架构建器 配置
var config = new BarSkeletonBuilderParamConfig()
// 扫描 action 类所在包
.scanActionPackage(DemoAction.class);
```

这个就是创建一个配置, 然后把`DemoAction.class`绑定上去

```java
@ActionController(DemoCmd.cmd)
public class DemoAction {
    /**
     * 示例 here 方法
     *
     * @param helloMessage helloReq
     * @return HelloReq
     */
    @ActionMethod(DemoCmd.here)
    public HelloMessage here(HelloMessage helloMessage) {
        HelloMessage newHelloMessage = new HelloMessage();
        newHelloMessage.name = helloMessage.name + ", I'm here";
        return newHelloMessage;
    }
```

可以看到`DemoAction`中才是我们的业务代码, 代码最后是配置了个`日志打印器`

```java
// 业务框架构建器
var builder = config.createBuilder();
// 添加控制台输出插件
builder.addInOut(new DebugInOut());
return builder.build();
```

最后返回的是一个`BarSkeleton`业务框架对象

### 🌸 createBarSkeleton

配置游戏网关

```java
@Override
public BrokerClientBuilder createBrokerClientBuilder() {
	BrokerClientBuilder builder = BrokerClient.newBuilder();
	builder.appName("demoLogicServer");
	return builder;
}
```

我们可以看到这一块代码非常少, 它只是配置了一个`appName`也就是应用的名字, 我们来看看它里面还有啥

```java
public class BrokerClientBuilder {
    static final String ioGamePidRandom = UUID.randomUUID().toString();
    private final Map<ConnectionEventType, Supplier<ConnectionEventProcessor>> connectionEventProcessorMap = new NonBlockingHashMap();
    private final List<Supplier<UserProcessor<?>>> processorList = new ArrayList();
    private final List<Class<?>> removeProcessorList = new ArrayList();
    private final BrokerClientListenerRegion brokerClientListenerRegion = new BrokerClientListenerRegion();
    private String id;
    private String tag;
    private String appName;
    private BarSkeleton barSkeleton;
    private BrokerClientType brokerClientType;
    private BrokerAddress brokerAddress;
    private int timeoutMillis;
    private ClientProcessorHooks clientProcessorHooks;
    private BrokerClientManager brokerClientManager;
    private AwareInject awareInject;
    private int status;
    private int withNo;
```

可以看到属性还是蛮多的, 这一块他没去动, 我们也先不动, 知道就行

### 🌸 createBrokerAddress

游戏网关的连接地址





