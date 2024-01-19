# 🍎 简介

官方视频播放插件

https://pub-web.flutter-io.cn/packages/video_player

# 🍎 快速开始

## 🌲 安装

```
flutter pub add video_player
```

## 🌲 初始化

我们想要使用`VideoPlayer`需要先初始化`VideoPlayerController`

```dart
_videoPlayerController = VideoPlayerController.network(
        'https://sample-videos.com/video321/mp4/720/big_buck_bunny_720p_1mb.mp4');
    _videoPlayerController.initialize();
```

我们可以在`https://sample-videos.com`中找到测试视频

## 🌲 使用

```dart
import 'package:flutter/material.dart';
import 'package:video_player/video_player.dart';

class VideoPage extends StatefulWidget {
  const VideoPage({super.key});

  @override
  State<StatefulWidget> createState() => _VideoPageState();
}

class _VideoPageState extends State<VideoPage> {
  late VideoPlayerController _videoPlayerController;

  @override
  void initState() {
    _videoPlayerController = VideoPlayerController.network(
        'https://sample-videos.com/video321/mp4/720/big_buck_bunny_720p_1mb.mp4');
    _videoPlayerController.initialize();
    // TODO: implement initState
    super.initState();
  }

  void _click() {
    
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
        appBar: AppBar(
          backgroundColor: Theme.of(context).colorScheme.inversePrimary,
          title: const Text("video页面"),
        ),
        body: _videoPlayerController.value.isInitialized ? VideoPlayer(_videoPlayerController) : const Text("视频正在加载中..."),
        floatingActionButton: FloatingActionButton(
          onPressed: _click,
          tooltip: 'test click',
          child: const Icon(Icons.add),
        ));
  }
}
```

然后我们进入到视频页面看一下

![](images/Pasted%20image%2020240119112909.png)

刚进入到视频页面就会看到加载中, 然后就没有然后了, 为什么呢? 很简单, 我们没有在加载完毕后把页面刷新出来, 我们修改一个地方即可

```dart
_videoPlayerController.initialize().then((value) {
  setState(() {

  });
});
```

写法如上, 意思就是当视频播放器初始化完成后就刷新一下页面

然后我们再次进入页面加载完毕后是这个样子的

![](images/Pasted%20image%2020240119113157.png)

我们会看到视频不太美观, 我们先不管, 然后我们来实现一个功能, 让视频动起来, 我们可以看到屏幕上有一个floatbutton, 我们就用这个按钮来控制视频的播放

```dart
void _click() {
if (_videoPlayerController.value.isInitialized) {
  if (_videoPlayerController.value.isPlaying) {
	_videoPlayerController.pause();
  } else {
	_videoPlayerController.play();
  }
} else {
  print("视频播放器未初始化");
}
}
```

我们使用`play()`让视频播放, 使用`pause()`让视频暂停, 我们看到视频已经可以正常播放了

## 🌲 调整视频比例

我们使用`AspectRatio`来控制视频的长宽比

```dart
@override
Widget build(BuildContext context) {
return Scaffold(
	appBar: AppBar(
	  backgroundColor: Theme.of(context).colorScheme.inversePrimary,
	  title: const Text("video页面"),
	),
	body: AspectRatio(
	  aspectRatio: _videoPlayerController.value.aspectRatio,
	  child: _videoPlayerController.value.isInitialized
		  ? VideoPlayer(_videoPlayerController)
		  : const Text("视频正在加载中..."),
	),
	floatingActionButton: FloatingActionButton(
	  onPressed: _click,
	  tooltip: 'test click',
	  child: const Icon(Icons.add),
	));
}
```

然后我们看一下效果

![](images/Pasted%20image%2020240119115534.png)

我们可以看到视频的比例变得正常了

# 🍎 进阶

是一套第三方的UI框架, 核心还是`video_player`

[chewie](../chewie/chewie.md)

# 🍎 自定义

暂时略





