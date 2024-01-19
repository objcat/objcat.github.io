# 🍎 简介

网络图片加载缓存框架

# 🍎 快速开始

## 🌲 最简单的使用

就是放一个`url`就能加载这个组件了

```dart
CachedNetworkImage( imageUrl: "https://img1.baidu.com/it/u=2854923112,324687528&fm=253&fmt=auto&app=138&f=JPEG?w=749&h=500")
```

## 🌲 占位图

有时候我们需要在图片未加载出来之前放一个占位图上去, 比如我放的是一个圆型的进度条

```dart
CachedNetworkImage(imageUrl: "https://img1.baidu.com/it/u=2854923112,324687528&fm=253&fmt=auto&app=138&f=JPEG?w=749&h=500",
            placeholder: (context, url) => const CircularProgressIndicator()
```

我们只需要设置`placeholder`即可

## 🌲 错误图

当图片加载出错显示一个默认图片

```dart
CachedNetworkImage(
                imageUrl:
                    "https://img1.baidu.com/it/u=2854923112,324687528&fm=253&fmt=auto&app=138&f=JPEG?w=749&h=500",
                placeholder: (context, url) => const CircularProgressIndicator(),
                errorWidget: (context, url, error) => const Icon(Icons.error)
```

通常情况下当图片出错时flutter中的图片会很不好看, 所以这个功能是有必要的

## 🌲 修改宽高

```dart
CachedNetworkImage(
              imageUrl:
                  "https://img1.baidu.com/it/u=2854923112,324687528&fm=253&fmt=auto&app=138&f=JPEG?w=749&h=500",
              placeholder: (context, url) => const CircularProgressIndicator(),
              errorWidget: (context, url, error) => const Icon(Icons.error),
              width: 100,
              height: 100
            )
```