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




