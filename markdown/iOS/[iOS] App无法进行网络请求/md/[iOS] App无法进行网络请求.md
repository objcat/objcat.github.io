>由于苹果ATS策略的影响 新建的工程都是无法进行http网络访问的 所以这里提供两种解决办法

1.在info.plist文件中添加如下代码 `这样就能解决99%的问题`

```
    <key>NSAppTransportSecurity</key>
    <dict>
        <key>NSAllowsArbitraryLoads</key>
        <true/>
   </dict>
```
![3.gif](https://upload-images.jianshu.io/upload_images/2194379-d6737373480ec617.gif?imageMogr2/auto-orient/strip)

2.不过不排除某些公司对自己的网络请求策略有限制 不允许全局设置http请求放行 那么我们也可以进行单独配置
```
	<key>NSAppTransportSecurity</key>
	<dict>
		<key>NSExceptionDomains</key>
		<dict>
			<key>baidu.com</key>
			<dict>
				<key>NSExceptionAllowsInsecureHTTPLoads</key>
				<true/>
				<key>NSExceptionRequiresForwardSecrecy</key>
				<false/>
				<key>NSIncludesSubdomains</key>
				<true/>
			</dict>
		</dict>
	</dict>
```