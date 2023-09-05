# 🍎 前言

这章是基于`Maven`配置的那篇文章的续篇, 一些基本的`SpringCloud`版本选择和框架选型就不冗余赘述了, 只讲使用`Gradle`来配置项目的过程, 推荐先从[SpringCloudMaven](SpringCloudMaven/SpringCloudMaven.md)开始看

# 🍎 Gradle

Gradle是一个基于Apache Ant和Apache Maven概念的项目自动化构建开源工具。它使用一种基于Groovy的特定领域语言(DSL)来声明项目设置，也增加了基于Kotlin语言的kotlin-based DSL，抛弃了基于XML的各种繁琐配置。

# 🍎 优势

## 🌲 使用Groovy语法简洁

对比`Maven`冗余的XML配置来说, `Gradle`简洁了不少, 语法清晰明了

## 🌲 编译速度提升

可以增量编译, 对于大型项目来说编译速度提升明显

# 🍎 配置镜像源

因为我们的环境是国内, 安装依赖可能比较吃力, 所以我们要选择国内的网络比较好的源

https://developer.aliyun.com/mvn/guide

## 🌲 单项目配置

所谓单项目配置就是把`镜像源`配置到项目本地, 这样的好处是无论走到哪里, 项目都可以下载到好的依赖

```groovy
allProjects {
  repositories {
	mavenLocal()
    maven {
      url 'https://maven.aliyun.com/repository/public/'
    }
    maven {
      url 'https://maven.aliyun.com/repository/spring/'
    }
    mavenCentral()
  }
}
```

## 🌲 本地配置

本地配置就是把`镜像源`配置到本地电脑中, 有一个好处就是本地的所有项目都可以享受镜像源, 但是在其他电脑上下载项目的人安装依赖就要重新去配置了

我们首先找到本地配置文件的路径

### 🌸 windows

新建`init.gradle`然后进行上述配置, 但是经过我并没有成功, 这项配置暂时被搁置了

# 🍎 搭建SpringCloud

## 🌲 创建gradle项目

大体上分三种创建方式

使用IDEA创建

使用spring官方脚手架创建 `http://start.spring.io/`

使用命令行创建`gradle init`

## 🌲 配置dependencyManagement

https://plugins.gradle.org/plugin/io.spring.dependency-management

https://central.sonatype.dev/artifact/org.springframework.boot/spring-boot-dependencies/2.6.4

在`maven`中我们会在父`pom`中引入`dependencyManagement`来管理我们的依赖版本, 这样就不需要单独配置我们的依赖库版本了

### 🌸 plugins模块

首先配置插件, 我们打开`build.gradle`, 一般插件模块是`build.gradle`的起始模块, 也就是第一行, 复制下面的代码粘贴上就可以了

```groovy
plugins {
    id 'java'
    id 'io.spring.dependency-management' version "1.0.14.RELEASE"
    id 'org.springframework.boot' version '2.6.4'
}
```

### 🌸 dependencies模块

这是我们要下载的依赖库, `spring-boot-starter-web`是最基础的功能, 没有它写不了接口

```groovy
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-web'
}
```

然后我们发现`springboot`系可以正常下载了

### 🌸 配置springcloud和阿里巴巴dependencyManagement

与springboot不同的是, 这两个货需要使用`dependencyManagement`来配置, 我们一起来看看吧

```groovy
dependencyManagement {
        imports {
            mavenBom 'org.springframework.cloud:spring-cloud-dependencies:2021.0.1'
            mavenBom 'com.alibaba.cloud:spring-cloud-alibaba-dependencies:2.2.7.RELEASE'
        }
    }
```

如此配置后我们使用springcloud系的依赖库也不用写版本号了, 比如

```groovy
implementation 'org.springframework.cloud:spring-cloud-starter-openfeign'
implementation 'org.springframework.boot:spring-boot-starter-web'
```

能导入依赖库说明配置过关了

## 🌲 子模块依赖配置

我们在父项目中配置的诸如, `repositories`, `dependencyManagement`, `plugins`也想让子项目去享受这些配置而不需要重新配置了, 如果重新配置会很冗余, 除非你的子模块和父模块使用不同的依赖, 那我们要怎么配置呢

### 🌸 allprojects

gradle给我们提供了`子模块`直接沿用`父模块`配置的方法, 那就是`allprojects`, 代表整个项目, 如果没有它你需要在每个子模块配置相同的配置, 使用起来很简单, 只需要把需要公开给子模块的依赖放到里面去, 我们一起来看看

```groovy
allprojects {
    apply plugin: 'java'
    apply plugin: 'org.springframework.boot'
    apply plugin: 'io.spring.dependency-management'
    repositories {
	    mavenLocal()
        maven {
            url 'https://maven.aliyun.com/repository/public/'
        }
        maven {
            url 'https://maven.aliyun.com/repository/spring/'
        }
        mavenCentral()
    }
    dependencyManagement {
        imports {
            mavenBom 'org.springframework.cloud:spring-cloud-dependencies:2021.0.1'
            mavenBom 'com.alibaba.cloud:spring-cloud-alibaba-dependencies:2.2.7.RELEASE'
        }
    }
}
```

我们可以看到用法是超级的简单的只需要加上`allprojects {}`即可

### 🌸 子模块插件依赖

我们需要注意的是插件的配置并不是把`plugins`搬进来, 而是使用了`apply`关键字进行处理, 上面已经配置过了

```groovy
allprojects {
    apply plugin: 'java'
    apply plugin: 'org.springframework.boot'
    apply plugin: 'io.spring.dependency-management'
    ...
```

### 🌸 子模块包依赖

有些包我们只需要子模块中使用, 我们可以把它放在`subprojects`中, 比如

```groovy
subprojects {
    dependencies {
        implementation('org.springframework.boot:spring-boot-starter-web')
    }
}

或者下面这种写法 都可以

subprojects {
    dependencies {
        implementation 'org.springframework.boot:spring-boot-starter-web'
    }
}
```

之后我们就可以在子模块中使用依赖库了

### 🌸 子模块排除依赖

有些时候我们不希望子模块继承父模块的一些依赖, 因此需要把他们排除, 或是要解决依赖冲突问题, 这个也很简单, 在gradle中, 我们可以直接使用条件判断来导入依赖库, 例子如下

```groovy
// 配置公共的依赖包
subprojects {
    dependencies {
        // gateway不需要写接口, 所以不用引入starterWeb
        if (!project.name.equals("test-gateway")) {
            implementation starterWeb
        }
        // 配置spring-boot-starter-test测试依赖(很重要)
        testImplementation starterTest
        // 配置数据库
        implementation mybatisPlus
        // 配置数据库连接库
        implementation mysql
        // 导入zykit
        implementation(zykit) {
            exclude group: '*', module: '*'
        }
        // 糊涂工具包
        implementation hutool
        // 配置lombok
        compileOnly lombok
        annotationProcessor lombok
        testCompileOnly lombok
        testAnnotationProcessor lombok
    }
}
```

当我们模块的名字不是`test-gateway`, 就导入依赖包, 那么就轻松的把这个模块排除了

# 🍎 白猫的配置清单

你可能对上面的东西看不进去, 想看看白猫配置好的配置吗

## 🌲 父工程

```groovy
plugins {
    // 配置Java插件, 必须的
    id 'java'
    // 配置springboot依赖管理插件
    id 'io.spring.dependency-management' version "1.0.14.RELEASE"
    // 配置springboot版本
    id 'org.springframework.boot' version '2.6.4'
}

group 'com.objcat'
version '1.0-SNAPSHOT'

allprojects {
    // 在全局项目中使用Java插件, 子模块不需要再写一遍了
    apply plugin: 'java'
    apply plugin: 'org.springframework.boot'
    apply plugin: 'io.spring.dependency-management'
    // 配置仓库
    repositories {
        // 先搜索本地仓库
        mavenLocal()
        // 再搜索阿里云仓库
        maven {
            url 'https://maven.aliyun.com/repository/public/'
        }
        maven {
            url 'https://maven.aliyun.com/repository/spring/'
        }
        // 再搜索Maven仓库
        mavenCentral()
    }
    // 配置springCloud和springCloud阿里巴巴的依赖管理
    dependencyManagement {
        imports {
            mavenBom 'org.springframework.cloud:spring-cloud-dependencies:2021.0.1'
            mavenBom 'com.alibaba.cloud:spring-cloud-alibaba-dependencies:2.2.7.RELEASE'
        }
    }
    // 配置test, 否则无法进行测试
    test {
        useJUnitPlatform()
    }
}

// 定义所需要的依赖包别名, 子类引用的时候很方便
ext {
    starterWeb = "org.springframework.boot:spring-boot-starter-web"
    starterTest = "org.springframework.boot:spring-boot-starter-test"
    actuator = "org.springframework.boot:spring-boot-actuator"
    mybatisPlus = "com.baomidou:mybatis-plus-boot-starter:3.5.1"
    mysql = "mysql:mysql-connector-java:8.0.28"
    log4j2 = "org.springframework.boot:spring-boot-starter-log4j2"
    lombok = "org.projectlombok:lombok"
}

// 配置公共的依赖包
subprojects {
    dependencies {
        // 配置spring-boot-starter-web, 没有这个写不了接口
        implementation starterWeb
        // 配置spring-boot-starter-test测试依赖(很重要)
        testImplementation starterTest
        // 配置lombok
        compileOnly lombok
        annotationProcessor lombok
        testCompileOnly lombok
        testAnnotationProcessor lombok
    }
}
```

# 🍎 ext

`ext`是`gradle`非常重要的组成部分, 用它来配置一些通用的东西如版本号, 可以管理整个项目中的版本

## 🌲 定义

我们可以在父工程的`build.gradle`中定义ext, 经过我的测试发现不需要在`allprojects`也是全局可见的

```groovy
ext {
    mysqlVersion = "8.0.28"
}
```

## 🌲 使用

然后我们在子模块中直接使用

```groovy
dependencies {
    implementation 'mysql:mysql-connector-java:' + mysqlVersion
}

或者使用模板语法, 注意使用模板语法必须是双引号

dependencies {
    implementation "mysql:mysql-connector-java:${mysqlVersion}"
}
```

# 🍎 config

有的时候我们喜欢把配置独立出来, 那让我们一起看看吧

新建`config.gradle`

```groovy
ext {
    mysql = "mysql:mysql-connector-java:8.0.26"
}
```

然后在子模块中引入

```
apply from: rootProject.file("config.gradle")

dependencies {
    implementation mysql
}
```

通过这种操作可以轻松导入模块了

如果不用这种独立配置也可以把`ext`配置到父工程`build.gradle`中, 不需要`allProjects`也可以全局识别参考`白猫的配置清单`模块

# 🍎 gradle.properties

我们可以在里面配置一些设置, 如消除警告, 直接在父工程中创建`gradle.properties`文件, 然后写下面的代码就可以了

```properties
# 配置警告模式(all,fail,summary,none)
org.gradle.warning.mode=none
```

# 🍎 Gradle语法

## 🌲 基本结构

我们在创建完`Gradle`项目中就可以看见一个叫做`build.gradle`文件, 这就是我们的配置文件, 我们来看一下内容

```groovy
plugins {
    id 'java'
}

group 'com.objcat'
version '1.0-SNAPSHOT'

repositories {
    mavenCentral()
}

dependencies {
    testImplementation 'org.junit.jupiter:junit-jupiter-api:5.8.1'
    testRuntimeOnly 'org.junit.jupiter:junit-jupiter-engine:5.8.1'
}

test {
    useJUnitPlatform()
}
```

## 🌲 依赖语法

依赖又名`dependencies`, 我们可以在这里配置第三方的依赖库

### 🌸 implementation

导入第三方库, 在编译时和运行时都是可见的

### 🌸 compileOnly

编译时可见, 比如`lombok`

### 🌸 runtimeOnly

运行时可见, 比如`mysql:mysql-connector-java`

### 🌸 testImplementation

测试阶使用依赖库, 不参与打包

### 🌸 annotationProcessor

### 🌸 api

和`implementation`类似, 只不过使用api来导入依赖后, 我们自己可以使用依赖库依赖的一些库, 正常情况下使用`implementation`导入的库是没办法使用它依赖的库的


# 🍎 Maven项目升级为Gradle

## 🌲 安装

### 🌸 安装全新gradle

首先在本地安装gradle

https://gradle.org/releases/

https://downloads.gradle-dn.com/distributions/gradle-7.5.1-bin.zip

### 🌸 你也可以使用idea自动下载的gradle进行配置

一般在这个路径下

`C:\Users\objcat\.gradle\wrapper\dists\gradle-7.1-bin\4pslxx9lrxt5svtz5wbnb6tkz\gradle-7.1\bin`

如果你问我为啥是这个路径, 那我推荐你去看我的`gradle-wrapper.properties`

### 🌸 配置环境变量

#### 🌵 windows

需要配置两个, 必须在系统环境变量下进行配置, 配置到用户变量下, `IDEA`会无法识别

首先创建`GRADLE_HOME`并指定路径

```
C:\Users\objcat\.gradle\wrapper\dists\gradle-7.1-bin\4pslxx9lrxt5svtz5wbnb6tkz\gradle-7.1\bin
```

然后加入Path

```
%GRADLE_HOME\bin
```

然后重启`IDEA`, 注意这里必须是大重启, 也就是所有项目都要关闭, 否则`IDEA`不会重新加载系统变量

配置重新打开一个控制台试一试

```shell
C:\WINDOWS\system32>gradle -v

------------------------------------------------------------
Gradle 7.5.1
------------------------------------------------------------

Build time:   2022-08-05 21:17:56 UTC
Revision:     d1daa0cbf1a0103000b71484e1dbfe096e095918

Kotlin:       1.6.21
Groovy:       3.0.10
Ant:          Apache Ant(TM) version 1.10.11 compiled on July 10 2021
JVM:          1.8.0_322 (Amazon.com Inc. 25.322-b06)
OS:           Windows 10 10.0 amd64
```

## 🌲 初始化gradle

在`maven`项目根路径下输入下面命令

```shell
gradle init
```

然后一路走下去就可以了

# 🍎 gradle-wrapper.properties

https://blog.csdn.net/cxu123321/article/details/102019659

每一个用`gradle`编译的工程, 都会有一个`gradle\wrapper`目录, 那么它使用来干什么的呢, 我们接下来就看一看吧

```
distributionBase=GRADLE_USER_HOME
distributionPath=wrapper/dists
distributionUrl=https\://services.gradle.org/distributions/gradle-7.1-bin.zip
zipStoreBase=GRADLE_USER_HOME
zipStorePath=wrapper/dists
```

里面的定义如下, 我们来一一说明

## 🌲 distributionUrl

是下载gradle工具的地址, 使用哪个版本的gradle就在这里修改

```
gradle的3种版本：

gradle-xx-all.zip是完整版，包含了各种二进制文件，源代码文件，和离线的文档。例如，https://services.gradle.org/distributions/gradle-3.1-all.zip

gradle-xx-bin.zip是二进制版，只包含了二进制文件（可执行文件），没有文档和源代码。例如，https://services.gradle.org/distributions/gradle-3.1-bin.zip

gradle-xx-src.zip是源码版，只包含了Gradle源代码，不能用来编译你的工程。例如，https://services.gradle.org/distributions/gradle-3.1-src.zip

如果只是为了编译，可以不用完整版，只需要二进制版即可，例如，gradle-3.1-bin.zip。
```

## 🌲 zipStoreBase

`zipStoreBase`和`zipStorePath`组合在一起, 是下载gradle的包的存放位置

`GRADLE_USER_HOME`默认是我们的用户文件夹`C:\Users\objcat\.gradle`

例如`C:\Users\objcat\.gradle\wrapper\dists\gradle-7.1-bin`

## 🌲 distributionBase

`distributionBase`和`distributionPath`组合在一起，是解压gradle-3.1-bin.zip之后的文件的存放位置。 

## 🌲 两种取值

```
zipStoreBase和distributionBase有两种取值：GRADLE_USER_HOME和PROJECT。

其中，GRADLE_USER_HOME表示用户目录。 
在windows下是%USERPROFILE%/.gradle，例如C:\Users\<user_name>\.gradle\。 
在linux下是$HOME/.gradle，例如~/.gradle。

PROJECT表示工程的当前目录，即gradlew所在的目录。
```

# 🍎 gradle使用maven仓库

https://blog.csdn.net/a386139471/article/details/107738615

答案是我们只需要保证maven仓库的路径是`/Users/objcat/.m2/repository`即可, gradle会自动过去找, 如果修改过路径就需要其他配置了, 所以建议配置maven的时候不要修改这个仓库的路径, 做自定义配置的时候只需要修改setting.xml和maven的目录即可

# 🍎 排除依赖

```
implementation(nacosDiscovery) {  
    exclude group: 'org.springframework.cloud', module: 'spring-cloud-starter-netflix-ribbon'  
}
```

