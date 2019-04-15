# 前言

随着网络技术的发展, 越来越多的公司开始使用**https**作为网络请求协议, 但是身为这个时代的开发者, 却很少有人了解其中的原理, 每次调接口的时候都是浪费大量的时间来对接, 联调, 排查错误, 经常会说的一句, 接口怎么不通, 而这个地方的资料虽然产量高但每一篇都很重量级, 导致了开发者学习困难, 所以这篇文章产生了, 我会用解决问题的方式来推进文章, 由浅入深, 依次讲解, 下面就跟着我们的文章一起来看吧.

# 一.概念

百度上一大堆, 想看概念的可以先查一下, 这里使用普通话来叙述.

**http:**
全名: Hyper Text Transfer Protocol
描述: 常用的网络协议

**https:**
全名: Hyper Text Transfer Protocol over Secure Socket Layer
描述: 常用的网络协议, 比http更安全

#### 1.https安全在什么地方呢?

下面就要引入一个概念ssl, 全名(Secure Sockets Layer 安全套接层), 它是一种安全协议, 协议原理是在传输层对请求信息进行了加密, 这样别人就不太会轻易看到你发送的信息了.

`https = http + ssl`

#### 2.ssl证书

读了上文已经知道了, **ssl**就是**https**协议的加密核心, 那我要怎么使用它呢, 
下面就要介绍主角**ssl证书**了, **ssl证书**的种类有两种, 一种是**自建证书**, 一种是**CA证书**, **自建证书**顾名思义就是自己制作出来的, **CA证书**则是从**正规的证书颁发机构**花钱买的或免费申请的, 网络请求过程大概是这样的, 发送请求的时候使用证书里面的**秘钥**对请求信息做加密处理, 到达客户端再用**秘钥**进行解密, 这两个步骤都是系统帮你自动完成的, 因为存在这个过程所以`https`通常来讲请求耗时**更长**, 不过为了安全, 这些牺牲还是可以接受的.

# 二.快速开始

我会从**前台**和**后台**两个角度依次讲解, 先说一下`iOS`端如何正常访问`https`接口, 再来说一下`JAVA`后台是怎么配置证书来实现`https`访问的.

#### 1.iOS配置https

**CA证书:**

上面已经说过了, ssl证书一共分为两种, **自建证书**和**CA证书**, 那么对于我们来说CA证书自然是最省事的, 可以用一句话概括, 跟正常网络请求没有任何区别.

![](https://upload-images.jianshu.io/upload_images/2194379-01b4f86450ebd1b5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

甚至你连这个都不用改动, YES和NO, 均可以访问通过.(亲测)

下面上AF代码 , 正常的不能再正常了 - -
```
AFHTTPSessionManager *manager = [AFHTTPSessionManager manager];
[manager GET:url parameters:nil progress:^(NSProgress * _Nonnull downloadProgress) {
} success:^(NSURLSessionDataTask * _Nonnull task, id  _Nullable responseObject) {
    NSLog(@"%@", responseObject);
} failure:^(NSURLSessionDataTask * _Nullable task, NSError * _Nonnull error) {
    NSLog(@"%@", error);
}];
```
所以公司使用CA证书你就偷着乐去吧, 对开发完全没影响.

**自建证书:**

这个比起CA证书就要麻烦很多了, 不过只要学会了方法还是很好解决的.

你通常会遇到的错误是这个

**错误1**
```
Task <6A6DB526-EC21-4E85-BACC-1415D23C3057>.<1> load failed with error Error Domain=NSURLErrorDomain Code=-999 "cancelled" UserInfo={NSErrorFailingURLStringKey=https://www.objcat.com:8082/hello, NSErrorFailingURLKey=https://www.objcat.com:8082/hello, _NSURLErrorRelatedURLSessionTaskErrorKey=(
    "LocalDataTask <6A6DB526-EC21-4E85-BACC-1415D23C3057>.<1>"
), _NSURLErrorFailingURLSessionTaskErrorKey=LocalDataTask <6A6DB526-EC21-4E85-BACC-1415D23C3057>.<1>, NSLocalizedDescription=cancelled} [-999]
2019-01-09 15:37:31.069920+0800 https网络测试[23697:794090] Error Domain=NSURLErrorDomain Code=-999 "cancelled" UserInfo={NSErrorFailingURLStringKey=https://www.objcat.com:8082/hello, NSErrorFailingURLKey=https://www.objcat.com:8082/hello, _NSURLErrorRelatedURLSessionTaskErrorKey=(
    "LocalDataTask <6A6DB526-EC21-4E85-BACC-1415D23C3057>.<1>"
), _NSURLErrorFailingURLSessionTaskErrorKey=LocalDataTask <6A6DB526-EC21-4E85-BACC-1415D23C3057>.<1>, NSLocalizedDescription=cancelled}
2019-01-09 15:37:31.075854+0800 https网络测试[23697:794147] Task <6A6DB526-EC21-4E85-BACC-1415D23C3057>.<1> finished with error - code: -999
2019-01-09 15:37:31.077351+0800 https网络测试[23697:794148] Task <6A6DB526-EC21-4E85-BACC-1415D23C3057>.<1> HTTP load failed (error code: -999 [1:89])
```

还是这个

**错误2**
```
Error Domain=NSURLErrorDomain Code=-1200 "An SSL error has occurred and a secure connection to the server cannot be made." UserInfo={NSLocalizedRecoverySuggestion=Would you like to connect to the server anyway?, _kCFStreamErrorDomainKey=3, NSErrorPeerCertificateChainKey=(
    "<cert(0x7f86d2017400) s: Andy i: Andy>"
), NSErrorClientCertificateStateKey=0, NSErrorFailingURLKey=https://www.objcat.com:8082/hello, NSErrorFailingURLStringKey=https://www.objcat.com:8082/hello, NSUnderlyingError=0x600001f21500 {Error Domain=kCFErrorDomainCFNetwork Code=-1200 "(null)" UserInfo={_kCFStreamPropertySSLClientCertificateState=0, kCFStreamPropertySSLPeerTrust=<SecTrustRef: 0x60000230f2a0>, _kCFNetworkCFStreamSSLErrorOriginalValue=-9802, _kCFStreamErrorDomainKey=3, _kCFStreamErrorCodeKey=-9802, kCFStreamPropertySSLPeerCertificates=(
    "<cert(0x7f86d2017400) s: Andy i: Andy>"
)}}, _NSURLErrorRelatedURLSessionTaskErrorKey=(
    "LocalDataTask <F8DB0C28-4CD3-417B-B89B-B0DCCABB6EA6>.<1>"
), _kCFStreamErrorCodeKey=-9802, _NSURLErrorFailingURLSessionTaskErrorKey=LocalDataTask <F8DB0C28-4CD3-417B-B89B-B0DCCABB6EA6>.<1>, NSURLErrorFailingURLPeerTrustErrorKey=<SecTrustRef: 0x60000230f2a0>, NSLocalizedDescription=An SSL error has occurred and a secure connection to the server cannot be made.}
```

或是这个

**错误3**
```
*** Terminating app due to uncaught exception 'Invalid Security Policy', reason: 'A security policy configured with `AFSSLPinningModeCertificate` can only be applied on a manager with a secure base URL (i.e. https)'
*** First throw call stack:
(
	0   CoreFoundation                      0x000000010b27029b __exceptionPreprocess + 331
	1   libobjc.A.dylib                     0x000000010a80c735 objc_exception_throw + 48
	2   httpsÁΩëÁªúÊµãËØï                   0x0000000109426325 -[AFHTTPSessionManager setSecurityPolicy:] + 613
	3   httpsÁΩëÁªúÊµãËØï                   0x000000010942276c -[ViewController viewDidLoad] + 172
	4   UIKitCore                           0x000000010f379781 -[UIViewController loadViewIfRequired] + 1186
	5   UIKitCore                           0x000000010f379be0 -[UIViewContro
```
先把下巴合上, 口水擦一擦, 我们下面就来说一下这几个错误都是什么, 该怎么解决.

**错误1 解决方案:**

这里提供一个最好的解法, 也是最简单的, 可以用一个成语来形容 - **敞门入场**, 我什么都不验证, 只想拿到我的数据, 其他跟我没关系.

首先设置下面两项

```
    AFHTTPSessionManager *manager = [AFHTTPSessionManager manager];
    // 是否允许无效证书, 默认为NO
    manager.securityPolicy.allowInvalidCertificates = YES;
    // 是否校验域名, 默认为YES
    manager.securityPolicy.validatesDomainName = NO;
```

然后这个地方设置为YES, `Allow Arbitrary Loads` 允许任意加载

![](https://upload-images.jianshu.io/upload_images/2194379-49bdf48a4adc33b7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上述设置任何网络都可以保证通畅访问, 如果是来找答案的, 恭喜你问题已经解决了.

 下面内容有可能引起不适 慎读:

**错误2 解决方案**

你在寻找答案的时候有可能看到另外一种解法, 就是在AF中配置证书来保持访问正常, 说实话这种方法是完全不推荐的, 在强调一遍, 让公司去买CA证书.

你可能对下面的代码很熟悉, 摘自互联网

```
- (AFSecurityPolicy *)customSecurityPolicy {
    
    // 先导入证书 证书由服务端生成，具体由服务端人员操作
    NSString *cerPath = [[NSBundle mainBundle] pathForResource:@"keystore" ofType:@"cer"];//证书的路径 xx.cer
    NSData *cerData = [NSData dataWithContentsOfFile:cerPath];
    
    // AFSSLPinningModeCertificate 使用证书验证模式
    AFSecurityPolicy *securityPolicy = [AFSecurityPolicy policyWithPinningMode:AFSSLPinningModeCertificate];
    // allowInvalidCertificates 是否允许无效证书（也就是自建的证书），默认为NO
    // 如果是需要验证自建证书，需要设置为YES
    securityPolicy.allowInvalidCertificates = YES;
    
    //validatesDomainName 是否需要验证域名，默认为YES;
    //假如证书的域名与你请求的域名不一致，需把该项设置为NO；如设成NO的话，即服务器使用其他可信任机构颁发的证书，也可以建立连接，这个非常危险，建议打开。
    //置为NO，主要用于这种情况：客户端请求的是子域名，而证书上的是另外一个域名。因为SSL证书上的域名是独立的，假如证书上注册的域名是www.google.com，那么mail.google.com是无法验证通过的；当然，有钱可以注册通配符的域名*.google.com，但这个还是比较贵的。
    //如置为NO，建议自己添加对应域名的校验逻辑。
    securityPolicy.validatesDomainName = NO;
    
    securityPolicy.pinnedCertificates = [[NSSet alloc] initWithObjects:cerData, nil];
    return securityPolicy;
}
```

路人甲: 对对对, 就是这垃圾东西, 我调了好几天.
路人乙: 麻蛋的, 我写的都对, 咋访问不通呢, 是不是后台证书给错了.

解决方法也简单
1.让后台提供有效的ssl证书, 亲测p12导出cer是完全可以使用的
2.检查你的代码有没有开启 `Allow Arbitrary Loads = YES`
3.帮助后台对接调试

错误2就会在没有开启`Allow Arbitrary Loads`的时候出现, 因为自建证书并非正规的ssl请求, 所以需要开启允许所有访问.

然而在你做完这些之后会遇到错误3

```
A security policy configured with `AFSSLPinningModeCertificate` can only be applied on a manager with a secure base URL (i.e. https)'
```

意思是使用`AFSSLPinningModeCertificate `模式必须配置一个baseURL, 这个可以是你的域名或者ip, 目前还不知道有什么用, 随便配置一个即可.

```
AFHTTPSessionManager *manager = [[AFHTTPSessionManager manager] initWithBaseURL:[NSURL URLWithString:@"https://www.baidu.com"]];
```

如: https://www.baidu.com

之后如果没什么失误的话, 就可以访问接口了...

到这里告一段落.

#### 2.Java配置https

**CA证书:**

首先来介绍一下CA证书:

一共分为三种

**DV SSL**
DV SSL证书是只验证网站域名所有权的简易型SSL证书，可10分钟快速颁发，能起到加密传输的作用，但无法向用户证明网站的真实身份。

目前市面上的免费证书都是这个类型的, 只是提供了对数据的加密, 但是对提供证书的个人和机构的身份不做验证。

例如我自己的证书:

![](https://upload-images.jianshu.io/upload_images/2194379-60081ce0b564990b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**OV SSL**
OV SSL, 提供加密功能, 对申请者做严格的身份审核验证和DV SSL的区别在于, OV SSL 提供了对个人或者机构的审核, 能确认对方的身份, 安全性更高.

所以这部分的证书申请是收费的~

例如百度的证书:

![](https://upload-images.jianshu.io/upload_images/2194379-af75a651ce1ce71a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**EV SSL**
EV最安全, 最严格, 超安EV SSL证书遵循全球统一的严格身份验证标准，是目前业界安全级别最高的顶级SSL证书, 验证过身份的公司名称可以直接显示在浏览器上, 是不是很高大上.

例如证书颁发机构的官网:

![](https://upload-images.jianshu.io/upload_images/2194379-59ee7e4c439cae21.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

好了下面我们就来申请一个免费的证书吧!

首先去阿里云或别的机构申请CA证书, 这里举例申请一个免费的.

![](https://upload-images.jianshu.io/upload_images/2194379-5e3a7aa9b4a6d308.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

阿里云控制台 -> 产品与服务 -> 搜索ssl

![](https://upload-images.jianshu.io/upload_images/2194379-0b3d81c39c5637a0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](https://upload-images.jianshu.io/upload_images/2194379-d9ce5150c98f9600.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可以看到证书很贵, 这里面有一个免费的, 但是点出来需要点技巧

![](https://upload-images.jianshu.io/upload_images/2194379-5ea297d0f6acb495.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

之后选择支付就可以了, 支付完成会发现多了个证书

![](https://upload-images.jianshu.io/upload_images/2194379-6346cade7887c1bc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](https://upload-images.jianshu.io/upload_images/2194379-72d1156aaceecfd1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


填写基本信息

![](https://upload-images.jianshu.io/upload_images/2194379-2e23fe9d3dff3897.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


然后点验证就可以了, 需要等待一些时间

------ 一段时间后 --------

![](https://upload-images.jianshu.io/upload_images/2194379-aa49a9c7cb2860d7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们可以看到证书审核好了, 我们就可以直接配置了, 首先把证书下载下来, 密码在同目录的文本文件里, 然后开始配置, 这里以`springcloud`为例

下载后的证书是`pfx`格式的, 我们先把它导入钥匙串, 然后导出为`p12`文件

![](https://upload-images.jianshu.io/upload_images/2194379-e5ff1abe51a69414.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](https://upload-images.jianshu.io/upload_images/2194379-e56723da417073ba.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后把证书放到`resources`目录下, 我改了个名请忽略 - -

![](https://upload-images.jianshu.io/upload_images/2194379-4bd4f8f7e1521b13.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
server:
  #服务端口号
  port: 8082
  ssl:
    key-store: classpath:haha.p12
    key-store-password: 123456
    key-store-type: PKCS12
```

之后配置这些即可, 项目会自动开启`https`.

之后访问接口试试

![](https://upload-images.jianshu.io/upload_images/2194379-e55270bec6817d78.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们可以看到证书是有效的, 这个服务器是配置在我本地的, 我们用ip来访问一下.

![](https://upload-images.jianshu.io/upload_images/2194379-af63b34290373e5f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们会发现, 同样的接口, 把域名换成ip就不可以访问了, 这也验证了我们DV证书是校验域名的这个原理.

我们可以直接在证书上查看我们信任的域名

![](https://upload-images.jianshu.io/upload_images/2194379-5eb002278f85fe74.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


**自建证书:**

自建证书配置也十分简单, 首先用java自带的keytool来生成一个自建证书

```
keytool -genkey -alias tomcat -dname "CN=objcat,OU=objcat,O=objcat,L=PuDongXinQu,ST=ShangHai,C=CN"  -storetype PKCS12 -keyalg RSA -keysize 2048  -keystore keystore.p12 -validity 3650
```

`-genkey`: 制作证书固有命令

`-alias `: 别名

![](https://upload-images.jianshu.io/upload_images/2194379-21cda2039718dcb8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

`-dname`: 填写证书基本信息

```
CN(Common Name 名字与姓氏)
OU(Organization Unit 组织单位名称)
O(Organization 组织名称)
L(Locality 城市或区域名称)
ST(State 省/市/自治区名称)
```

`-storetype`: 证书保存类型
`-keyalg`: 加密方式
`-keysize`: 秘钥长度
`-keystore`: 证书导出时的名称
`-validity`: 证书有效期

然后输入证书密码就可以完成创建了!

![](https://upload-images.jianshu.io/upload_images/2194379-640169ddfd5df6fa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

双击证书, 输入密码就能导入钥匙串了, 我们来看看

![](https://upload-images.jianshu.io/upload_images/2194379-e84a04660a75343d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这个证书的使用方法和CA证书一样
1.放在`resources`目录下
2.填写配置文件 - 参考CA证书配置

![](https://upload-images.jianshu.io/upload_images/2194379-9856f257b8c20e40.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

启动服务!

访问接口
https://localhost:8082/hello

![](https://upload-images.jianshu.io/upload_images/2194379-986db4f9ee1ccf28.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

之后点击继续前往

![](https://upload-images.jianshu.io/upload_images/2194379-36ccde18da547830.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这样虽然可以访问, 但是我们可以看到上面写的三个红色的大字, 不安全, 所以我们需要解决一下这个问题, 就是信任证书!

![](https://upload-images.jianshu.io/upload_images/2194379-23623f8b66ac663f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们发现信任证书之后`safari`是可以访问通的, 但是`chrome`还是会报错不安全

![](https://upload-images.jianshu.io/upload_images/2194379-00e10693e6d3d8da.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](https://upload-images.jianshu.io/upload_images/2194379-e8e246e5efe22908.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
Certificate - Subject Alternative Name missing
The certificate for this site does not contain a Subject Alternative Name extension containing a domain name or IP address.
```

```
Certificate - missing
This site is missing a valid, trusted certificate (net::ERR_CERT_COMMON_NAME_INVALID).
```

然后我们就开始着手解决这两个问题

根据错误提示, 是因为我的证书没有做域名认证造成的, 所以我们在生成证书的时候给它添加一个域名

加上下面这句即可

```
-ext san=dns:www.objcat.com
```

完整命令
```
keytool -genkey -alias tomcat -dname "CN=objcat,OU=objcat,O=objcat,L=PuDongXinQu,ST=ShangHai,C=CN"  -storetype PKCS12 -keyalg RSA -keysize 2048  -keystore keystore.p12 -validity 3650 -ext san=dns:www.objcat.com
```

我们再次生成证书, 然后重新配置

之后我们来修改 www.objcat.com 映射到本机域名

```
127.0.0.1 www.objcat.com
```

然后重启服务器

这次试用域名访问接口试试

![](https://upload-images.jianshu.io/upload_images/2194379-0b8aceadd267ddec.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们发现问题解决了!

#finally enjoy it.
#by objcat 2019.1.10



































 
