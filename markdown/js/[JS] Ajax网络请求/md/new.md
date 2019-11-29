最近在学js 有幸听了海牙老师的直播讲课 所以在这里整理一下Ajax的POST请求和GET请求

我先说一下`ajax`是什么

>Ajax 即“**A***synchronous ***J***avascript **A**nd ***X***ML*”（异步 JavaScript 和 XML），是指一种创建交互式[网页](https://baike.baidu.com/item/%E7%BD%91%E9%A1%B5)应用的网页开发技术。
Ajax = 异步 [JavaScript](https://baike.baidu.com/item/JavaScript) 和 [XML](https://baike.baidu.com/item/XML)（[标准通用标记语言](https://baike.baidu.com/item/%E6%A0%87%E5%87%86%E9%80%9A%E7%94%A8%E6%A0%87%E8%AE%B0%E8%AF%AD%E8%A8%80)的子集）。
Ajax 是一种用于创建快速动态网页的技术。
Ajax 是一种在无需重新加载整个网页的情况下，能够更新部分网页的技术。<sup>[1]</sup> 
通过在后台与服务器进行少量数据交换，Ajax 可以使网页实现异步更新。这意味着可以在不重新加载整个网页的情况下，对网页的某部分进行更新。
传统的网页（不使用 Ajax）如果需要更新内容，必须重载整个网页页面。
> ###摘自 百度百科 = =

如果上面都没看懂没关系 我用一句话总结
>就他丫的是个用js写的网络请求

好了废话不多说了直接上代码

- 下面是GET方法


```
     var URL = 'https://route.showapi.com/181-1';
        // 初始化实例
        var request = new XMLHttpRequest();
        // 构造参数
        var sendData = {
            showapi_appid: '30603',
            showapi_sign: '98960666afeb4992ae91971d13494090',
            page: 1,
            num: '8'
        }
        // 拼接参数构造URL
        URL = URL + '?showapi_appid=' + sendData.showapi_appid + "&showapi_sign=" + sendData.showapi_sign + "&page=" + sendData.page + "&num=" + sendData.num;
        // 构造请求  请求类型 URL 是否异步 true异步 false同步
        request.open('GET', URL, true);
        // 发送请求
        request.send(null);
        //设置异步回调
        request.onreadystatechange = function (ev) {

            switch (request.readyState) {
                case 0:
                    console.log(new Date() + ' : ' + '未初始化 - 还没有调用send方法');
                case 1:
                    console.log(new Date() + ' : ' + '正在发送请求' + 'time:');
                case 2:
                    console.log(new Date() + ' : ' + '已经接收到全部响应内容' + 'time:');
                case 4:
                    console.log(new Date() + ' : ' + '响应内容解析完成, 可以在客户端使用了' + 'time:');
            }

            //网络请求成功 打印数据
            if (request.readyState == 4 && request.status == 200) {
                // 打印json字符串
                document.write(request.response);
                // json字符串转对象
                let responseObject = JSON.parse(request.response);
                // 打印json对象
                console.log(responseObject);
                // 打印json里面的层级用点(.)来访问
                console.log(responseObject.showapi_res_body.newslist);
            }
        }
```


- 下面是POST方法 下面是我抓取微信游戏的接口 = =
```
     var URL = 'https://game.weixin.qq.com' + '/cgi-bin/gametetrisws/syncgame?session_id=xxxx';
                    // 初始化实例
                    var request = new XMLHttpRequest();
                    //构造POST请求正文数据
                    var json = '{"appid":"xxxx","game_behav_list":[{"key":"newscore","value":0},{"key":"level","value":100},{"key":"baoshi","value":0},{"key":"combo","value":0}],"sync_type":1,"sig":23026853,"use_time":3}';
                    // 构造请求  请求类型 URL 是否异步 true异步 false同步
                    request.open('POST', URL, true);
                    // 发送请求
                    // request.send(JSON.stringify(sendData));
                    request.send(json);
                    //设置异步回调
                    request.onreadystatechange = function (ev) {

                        switch (request.readyState) {
                            case 0:
                                console.log(new Date() + ' : ' + '未初始化 - 还没有调用send方法');
                            case 1:
                                console.log(new Date() + ' : ' + '正在发送请求' + 'time:');
                            case 2:
                                console.log(new Date() + ' : ' + '已经接收到全部响应内容' + 'time:');
                            case 4:
                                console.log(new Date() + ' : ' + '响应内容解析完成, 可以在客户端使用了' + 'time:');
                        }

                        //网络请求成功 打印数据
                        if (request.readyState == 4 && request.status == 200) {
                            // 打印json字符串
                            document.write(request.response);
                            // json字符串转对象
                            let responseObject = JSON.parse(request.response);
                            // 打印json对象
                            console.log(responseObject);
                        }
                    }
```






