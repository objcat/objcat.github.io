# 🍎 简介

这是`重装机兵-霸主`的战斗服务器代码, 使用了`ioGame`的框架编写

https://github.com/iohao/ioGame

https://iohao.github.io/game/

# 🍎 快速开始

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

[maven配置教程](../../../../4-package-manager/maven/maven.md)

配置好`maven`后点击同步就可以了

![](images/Pasted%20image%2020250819181450.png)

然后等待依赖库拉取完

## 🌲 学习ioGame

[iogame学习文档](../../../../1-tecnology/java/iogame/iogame.md)

## 🌲 启动项目

我们在目录中找到这样一个类

![](images/Pasted%20image%2020250820004431.png)

`SdkApplication`这就是我们的主启动类, 右键点击`Run`即可

```shell
+----------+-----------------------##-------------+----------##---------+-------------------------
| ioGame                           ## Memory                 ## Time                            
+----------+-----------------------##-------------+----------##---------+-------------------------
| pid      | 13008                 ## used        | 30.61MB  ## start   | 2025-08-20 00:41:47.753 
| version  | 21.28                 ## freeMemory  | 113.39MB ## end     | 2025-08-20 00:41:48.316 
| document | http://game.iohao.com ## totalMemory | 144.0MB  ## consume | 0.56 s                  
+----------+-----------------------##-------------+----------##---------+-------------------------
|          | ioGame javadoc - https://iohao.github.io/javadoc
+----------+--------------------------------------------------------------------------------------
| adv      | MMO - https://iohao.github.io/game/docs/practices/mmo
+----------+--------------------------------------------------------------------------------------
| News     | 游戏对外服 - 统一协议说明 - https://iohao.github.io/game/docs/manual_high/external_message
| News     | 游戏对外服 - 路由访问权限控制 - https://iohao.github.io/game/docs/external/access_authentication
+----------+--------------------------------------------------------------------------------------
```

这个字样说明成功了, 在日志中我们可以看到所有的路由

```shell
路由: 1 - 0 --- action : SdkAction.loginVerify(LoginVerifyMessage verifyMessage, FlowContext flowContext)  --- return UserMessage  ~~~ see.(SdkAction.java:58)
路由: 1 - 1 --- action : SdkAction.triggerBroadcast(FlowContext flowContext)  --- return void  ~~~ see.(SdkAction.java:224)
路由: 1 - 2 --- action : SdkAction.intValue(IntValue value, FlowContext flowContext)  --- return IntValue  ~~~ see.(SdkAction.java:79)
路由: 1 - 3 --- action : SdkAction.longValue(LongValue value)  --- return LongValue  ~~~ see.(SdkAction.java:92)
路由: 1 - 4 --- action : SdkAction.boolValue(BoolValue value)  --- return BoolValue  ~~~ see.(SdkAction.java:103)
路由: 1 - 5 --- action : SdkAction.stringValue(StringValue value)  --- return StringValue  ~~~ see.(SdkAction.java:114)
路由: 1 - 6 --- action : SdkAction.value(LoginVerifyMessage loginVerifyMessage)  --- return UserMessage  ~~~ see.(SdkAction.java:125)
路由: 1 - 12 --- action : SdkAction.listInt(IntValueList value)  --- return IntValueList  ~~~ see.(SdkAction.java:148)
路由: 1 - 13 --- action : SdkAction.listLong(LongValueList value)  --- return LongValueList  ~~~ see.(SdkAction.java:161)
路由: 1 - 14 --- action : SdkAction.listBool(BoolValueList value)  --- return BoolValueList  ~~~ see.(SdkAction.java:174)
路由: 1 - 15 --- action : SdkAction.listString(StringValueList value)  --- return StringValueList  ~~~ see.(SdkAction.java:187)
路由: 1 - 16 --- action : SdkAction.listValue(List<LoginVerifyMessage> value)  --- return List<UserMessage>  ~~~ see.(SdkAction.java:200)
路由: 1 - 20 --- action : SdkAction.testError(IntValue value)  --- return IntValue  ~~~ see.(SdkAction.java:213)
路由: 1 - 21 --- action : SdkAction.noParam()  --- return IntValue  ~~~ see.(SdkAction.java:256)
路由: 1 - 22 --- action : SdkAction.noReturn(StringValue name)  --- return void  ~~~ see.(SdkAction.java:267)
路由: 1 - 23 --- action : SdkAction.bulletMessage(BulletMessage message)  --- return BulletMessage  ~~~ see.(SdkAction.java:277)
路由: 1 - 30 --- action : SdkAction.internalAddMoney(IntValue money)  --- return IntValue  ~~~ see.(SdkAction.java:272)
路由: 2 - 1 --- action : MyAction.hello(StringValue name)  --- return StringValue  ~~~ see.(MyAction.java:41)
路由: 2 - 2 --- action : MyAction.loginVerify(LoginVerifyMessage verifyMessage)  --- return UserMessage  ~~~ see.(MyAction.java:52)
```



# 🍎 项目分析

待完善



