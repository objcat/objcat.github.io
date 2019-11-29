    #一.写在前面的话
    苹果更新系统的同时也更新了iTunes 12.7这个版本中苹果移除了appstore, 对于这个操作已经不能用蠢来形容了, 垃圾的一批, 请容许我骂一句, `你个哈麻皮妈卖批哦`.
    
    #二.降级方案
    上有政策下有对策 跟着我们的镜头 一起来看吧
    ###1.去苹果官网上下载 12.6.2版本的itunes 
    下载完先放着 不删除新版本你是安不上的 所以继续往下看把
    ```
    https://support.apple.com/en_AU/downloads/itunes
    ```
    
    ###2.删除12.7版本的iTunes 并安装最新的iTunes
    ```
    sudo rm -rf /Applications/iTunes.app
    ```
    ###3.移除新版本的iTunes数据库 不然iTunes会打不开
    请把xxxx改成自己的家目录用户名
    ```
    sudo rm -rf /Users/xxxx/Music/iTunes/iTunes\ Library.itl
    ```
    #三.为什么要降级iTunes
    估计查看到这篇文章的人都知道iTunes上的appStore很方便, 但是为了保险起见我还是要写一下为什么要降级的理由
    1.降级后可以使用iTunes打包(测试包)  这个比Xcode打包方便很多
    iTunes打包直通车:http://www.jianshu.com/p/255069f719d3
    2.降级后可以下载其他应用IPA 用处就我就不多说了