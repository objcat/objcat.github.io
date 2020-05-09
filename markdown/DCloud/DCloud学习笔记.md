# MUI-Class

```
mui-bar mui-bar-nav 
导航栏class属性
```

```
mui-content 
设置内容区域 会自动绕过导航栏
```

# 开启回弹效果
目测只需要在首页设置 就可以全局应用
```
// 开启回弹
var self = plus.webview.currentWebview();
// 设置样式
self.setStyle({
    // 开启上下回弹
    bounce: "vertical",
    // 背景颜色
    bounceBackground:"#efeff4",
    // 关闭返回手势
    popGesture:'none'
});
```


```
// 安卓开启沉浸模式
"statusbar": {
	"immersed": true,
        "style": "dark"	
},

// iOS开启沉浸模式
"UIReserveStatusbarOffset": false
```

原生导航栏
```
"titleNView": {
				"backgroundcolor": "#ffffff",
				"titletext": "首页",
				"titlecolor": "#000000",
				 "splitLine": { // 标题栏控件的底部分割线，类似borderBottom
					"color" : "#CCCCCC", // 分割线颜色,默认值为"#CCCCCC"  
					"height" : "1px" // 分割线高度,默认值为"2px"
				}
			},
```
