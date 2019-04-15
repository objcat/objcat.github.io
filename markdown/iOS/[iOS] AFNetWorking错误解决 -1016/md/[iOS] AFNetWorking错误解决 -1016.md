# 一.前言
很多新手在调试接口的时候都会遇到这个错误, 然而网上的大部分解决方案都不是很理想所以这里还是说一下.

# 二.发现问题
直接上报错
```
Error Domain=com.alamofire.error.serialization.response Code=-1016 "Request failed: unacceptable content-type: text/plain" UserInfo={NSLocalizedDescription=Request failed: unacceptable content-type: text/plain, NSErrorFailingURLKey=http://www.objcat.com:8082/hello, com.alamofire.serialization.response.error.data=<68656c6c 6f20776f 726c64>, com.alamofire.serialization.response.error.response=<NSHTTPURLResponse: 0x600003e621c0> { URL: http://www.objcat.com:8082/hello } { Status Code: 200, Headers {
    "Content-Length" =     (
        11
    );
    "Content-Type" =     (
        "text/plain;charset=UTF-8"
    );
    Date =     (
        "Tue, 08 Jan 2019 09:33:52 GMT"
    );
    "Keep-Alive" =     (
        "timeout=38"
    );
} }}
```
# 三.解决问题

我们可以看到错误信息中有一个`text/plain`, 然后可以在网上找到很多解决方案, 其中最常见的就是这个:

```
AFHTTPSessionManager *manager = [AFHTTPSessionManager manager];
manager.responseSerializer.acceptableContentTypes = [NSSet setWithObjects:@"text/plain", nil];
```

但是这种解法往往不能解决问题

```
Error Domain=NSCocoaErrorDomain Code=3840 "JSON text did not start with array or object and option to allow fragments not set." UserInfo={NSDebugDescription=JSON text did not start with array or object and option to allow fragments not set.}
```

#### 正解:

不难看出, 这个问题出现的原因是无法解析`json`字符串, 为什么无法解析?

因为后台返回的有可能就不是`json`

所以你有两种解决方案:

1.如果你确定返回的不是`json`, 那么你不需要做任何修改, 让后台去修改成`json`.(推荐使用postman进行调试接口)

2.关闭AF的自动解析模式, 返回原本的`NSData`, 然后手动解析.

```
AFHTTPSessionManager *manager = [AFHTTPSessionManager manager];
manager.responseSerializer = [AFHTTPResponseSerializer serializer];
```

返回的数据是这样的

```
<68656c6c 6f20776f 726c64>
```

我们把它使用字符串解析

```
NSString *str = [[NSString alloc] initWithData:responseObject encoding:NSUTF8StringEncoding];
```

```
hello world
```

发现是个`hello world`, 因为不是json, 所以AF才没有办法解析, 问题解决!

# finally enjoy it.
# by objcat 2019.1.9
