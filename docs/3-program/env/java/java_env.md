# 🍎 前言

JDK_是 Java 语言的软件开发工具包，主要用于移动设备、嵌入式设备上的java应用程序。_JDK_是整个java开发的核心，它包含了JAVA的运行环境（JVM+Java系统类库）和JAVA工具。

# 🍎 怎么选版本

我们一般使用JDK会需要两个版本, 一个是`Java8`也叫`Java1.8`, 一般公司中都会跑这个版本, 另外一个是`Java17`是一个长期支持版本其中`SpringBoot3.0`最低支持版本就是`17`, 所以这两个都推荐安装, 开发中可以换着用

如果只想用一个优先使用`Java8`, 如果你是`m1`以上的`Mac`推荐`Java17`, 因为`Java17`原生支持`ARM`

# 🍎 安装

## 🌲 IDEA安装

IDEA安装, 比较方便

![](images/Pasted%20image%2020230902110350.png)

但容易翻车, 可能需要搬梯子

![](images/Pasted%20image%2020230902110429.png)

## 🌲 镜像源安装

清华大学镜像源OpenJdk
https://mirrors.tuna.tsinghua.edu.cn/Adoptium/

### 🌸 Mac

在Mac上安装是比较容易的, 只需要去下面的网址下载即可

Java17
https://mirrors.tuna.tsinghua.edu.cn/Adoptium/17/jdk/aarch64/mac/

Java8
https://mirrors.tuna.tsinghua.edu.cn/Adoptium/8/jdk/x64/mac/

打开网页会发现有两个文件, 一个是pkg, 一个是tar.gz, 他们都可以安装, 新手推荐`pkg`

![](images/Pasted%20image%2020231001204727.png)

#### 🌵 pkg

是一个全自动的安装包, 下载完成双击就可以安装了

安装完成后会在`/usr/bin`中留存一个快捷方式方便去执行java命令

```
cd /usr/bin
ls java*

java javadoc javap javaws
javac javah javapackager
```

安装完成后, 我们可以使用命令来查看是否安装成功了

```shell
java -version

openjdk version "17.0.8.1" 2023-08-24
OpenJDK Runtime Environment Temurin-17.0.8.1+1 (build 17.0.8.1+1)
OpenJDK 64-Bit Server VM Temurin-17.0.8.1+1 (build 17.0.8.1+1, mixed mode)
```

#### 🌵 tar.gz

这个适合高手或者需要配置多个java环境的人, 因为使用这种压缩文件默认是不会给你配置任何环境变量, 没有污染, 你可以放在某个文件夹中为某个项目去用, 比如我下载解压完是这个样子

![](images/Pasted%20image%2020231001214145.png)

然后我在IDEA中就可以配置了

![](images/Pasted%20image%2020231001214318.png)

如图所示, 点击JDK添加, 然后我们选择刚才下载的jdk的home文件夹

![](images/Pasted%20image%2020231001214424.png)

然后会有一个提示

![](images/Pasted%20image%2020231001214453.png)

点击同意, 然后就可以愉快的使用了, 不好用再换回去即可, 在列表里

### 🌸 Win

待补充

# 🍎 环境变量

我们想使用java就要配置环境变量, 首先打java确认一下本机有没有

```
java -version

openjdk version "17.0.8" 2023-07-18
OpenJDK Runtime Environment GraalVM CE 17.0.8+7.1 (build 17.0.8+7-jvmci-23.0-b15)
OpenJDK 64-Bit Server VM GraalVM CE 17.0.8+7.1 (build 17.0.8+7-jvmci-23.0-b15, mixed mode, sharing)
```

这种不需要配置, 因为已经可以使用了

## 🌲 JAVA_HOME

其实就是`java`环境的根路径, 因为`tomcat`服务器启动的时候会使用该环境变量来运行`java`, 所以网上都教着把`JAVA_HOME`流传下来了, 配置`java`的环境变量的时候就使用`%JAVA_HOME%/bin`, 如果是`mac`就是`$JAVA_HOME/bin`

```
export JAVA_HOME="/Library/Java/JavaVirtualMachines/jdk1.8.0_221.jdk/Contents/Home"
export PATH="$JAVA_HOME/bin:$PATH"
```

## 🌲 classpath

用来确定class文件的位置, 如果不设置默认就是在`./`当前路径下去找`.class`文件, 如果配置后就会去配置的路径下找`.class`类文件


## 🌲 其他安装

甲骨文Java17
https://www.oracle.com/cn/java/technologies/downloads/#jdk17-linux
https://www.oracle.com/cn/java/technologies/downloads/#jdk21-mac

Adoptium
https://adoptopenjdk.net/

亚马逊
https://aws.amazon.com/cn/corretto/?filtered-posts.sort-by=item.additionalFields.createdDate&filtered-posts.sort-order=desc

腾讯kona
https://cloud.tencent.com/product/tkjdk

阿里Dragonwell
https://dragonwell-jdk.io/#/index

微软
https://learn.microsoft.com/zh-cn/java/openjdk/overview