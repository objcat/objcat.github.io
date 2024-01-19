# 🍎 简介

广告, 是免费`app`快速实现盈利的方式, 在flutter上谷歌提供了两个广告插件, `Google AdMob`和`Google Ad Manager`, 前者适用于小型公司, 后者适用于大型公司

https://flutter.cn/monetization

# 🍎 术语

## 🌲 Ad Unit

广告单元, 如果你有三个屏幕, 每个屏幕都想显示一条广告, 那么要创建三个广告单元, 以便分别对他们进行控制

## 🌲 Ad Format

广告形式, 就是广告的类型, 有「横幅广告/插屏广告/激励视频广告/原生广告」等

## 🌲 Ad Request

当广告要显示的时候会将请求发送到服务器来拉取广告内容

## 🌲 Test Ad

测试广告, 部署广告后用来测试的广告, 因为广告插件为了避免无效点击都会做一些安全措施, 主要是为了防止用户自己刷广告赚钱, 如果无效点击多了会被标记风险, 所以在开发的时候都使用测试的广告, 这样不会被标记次数

# 🍎 平台

## 🌲 穿山甲

字节跳动旗下, 国内不错

[跳转 穿山甲](../穿山甲/穿山甲.md)

## 🌲 Google

据说国内不行

https://pub-web.flutter-io.cn/packages/google_mobile_ads

```shell
flutter pub add google_mobile_ads
```




