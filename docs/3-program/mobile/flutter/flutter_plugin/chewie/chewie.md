# 🍎 简介

第三方视频播放UI框架

https://pub-web.flutter-io.cn/packages/chewie

# 🍎 快速开始

## 🌲 安装

```
flutter pub add chewie
```

## 🌲 使用

很简单哈, 我们直接用

```dart
import 'package:chewie/chewie.dart';
import 'package:flutter/material.dart';
import 'package:video_player/video_player.dart';

class VideoPage extends StatefulWidget {
  const VideoPage({super.key});

  @override
  State<StatefulWidget> createState() => _VideoPageState();
}

class _VideoPageState extends State<VideoPage> {
  late VideoPlayerController _videoPlayerController;

  late ChewieController _chewieController;

  @override
  void initState() {
    _videoPlayerController =
        VideoPlayerController.network('http://192.168.2.191:4076/1.mp4');
    _videoPlayerController.initialize().then((value) {
      _chewieController = ChewieController(
        videoPlayerController: _videoPlayerController,
        autoPlay: true,
        looping: true,
      );

      setState(() {

      });
    });

    // TODO: implement initState
    super.initState();
  }

  void _click() {
    if (_videoPlayerController.value.isInitialized) {
      if (_videoPlayerController.value.isPlaying) {
        print("暂停");
        _videoPlayerController.pause();
      } else {
        print("开始");
        _videoPlayerController.play();
      }
    } else {
      print("视频播放器未初始化");
    }
  }

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
              ? Chewie(controller: _chewieController)
              : const Text("视频正在加载中..."),
        ),
        floatingActionButton: FloatingActionButton(
          onPressed: _click,
          tooltip: 'test click',
          child: const Icon(Icons.add),
        ));
  }
}
```