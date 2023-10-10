# 🍎 简介

阿里云对象存储 OSS（Object Storage Service）是一款海量、安全、低成本、高可靠的云存储服务，提供最高可达 99.995 % 的服务可用性。多种存储类型供选择，全面优化存储成本。

# 🍎 快速开始

## 🌲 开启OSS

我们使用`阿里云OSS`, 注册账号就不说了

![](images/Pasted%20image%2020231010164758.png)

然后点击创建`bucket`, 通常一个项目使用一个`bucket`

![](images/Pasted%20image%2020231010170119.png)

读写权限改成`公共读`, 然后点击创建

![](images/Pasted%20image%2020231010170217.png)

创建成功后是这个样子的, 我们点击上传图片, 可以尝试先上传一张

![](images/Pasted%20image%2020231010170609.png)

拖拽上去然后点击上传

![](images/Pasted%20image%2020231010170552.png)

上传成功后我们点击图片访问一下

![](images/Pasted%20image%2020231010170741.png)

下面的URL链接点击就可以下载, 这说明我们的`OSS存储`启动成功了

然后我们看一下计费标准

![](images/Pasted%20image%2020231010165646.png)

貌似有5个G的免费额度, 还待测试

## 🌲 上传方式

想要上传图片, 就要了解我们上传的图片要怎么存到`OSS`上

### 🌸 后台上传法

我们先把图片上传到我们后台, 然后再由后台调用`OSS`的接口上传到`OSS`, 优点是安全, 缺点是会造成服务器的压力, 因为上传图片会占用很大的带宽

### 🌸 后台签名上传法

由后台生成签名, 然后图片直接由前端通过`OSS`的接口带上签名进行上传

## 🌲 导入SDK

https://help.aliyun.com/zh/oss/developer-reference/java-installation?spm=a2c4g.11186623.0.0.a55fb417lsoY6Y

点击上面的文档可以看到, 阿里云给我们提供了一套`SDK`, 专门用于上传文件, 里面也会包含生成签名, 我们现在就来集成吧

### 🌸 添加依赖

```xml
<dependency>
    <groupId>com.aliyun.oss</groupId>
    <artifactId>aliyun-sdk-oss</artifactId>
    <version>3.15.1</version>
</dependency>
```

### 🌸 测试文件上传

根据文档(简单上传)

https://help.aliyun.com/zh/oss/developer-reference/simple-upload-11?spm=a2c4g.11186623.0.0.68cb2921ZEb6qy

我们写一个测试类来测试文件上传, 我们新建`BrandControllerTests`测试文件, 然后把文档里的代码直接粘贴过来

```

```

在配置过程中我遇到一个问题`Exception 'com.aliyuncs.exceptions.ClientException' is never thrown in the corresponding try block`, 我没有找到解决这个问题的方法, 所以我移除了`ClientException`的捕获

### 🌸 获取秘钥

获取秘钥也比较简单, 我们点击`AccessKey管理`

![](images/Pasted%20image%2020231010180604.png)

我们选择右边的「开始使用子用户AccessKey」, 因为前面的key权限太大, 后面的key比较安全

![](images/Pasted%20image%2020231010180405.png)

然后会进入到`RAM访问控制`, 注意这个`RAM`可不是内存, 而是`访问控制RAM（Resource Access Management）是阿里云提供的管理用户身份与资源访问权限的服务。`

然后我们点击创建用户

![](images/Pasted%20image%2020231010181001.png)

然后填写

![](images/Pasted%20image%2020231010181100.png)

然后就能看到`key`和`secret`了, 我的自己隐藏了, 你自己创建

![](images/Pasted%20image%2020231010181153.png)

然后配置权限, 我们点击用户

![](images/Pasted%20image%2020231010181402.png)

可以找到一个`添加权限`的按钮

![](images/Pasted%20image%2020231010181419.png)

然后点击添加权限

![](images/Pasted%20image%2020231010181341.png)

授权成功了




