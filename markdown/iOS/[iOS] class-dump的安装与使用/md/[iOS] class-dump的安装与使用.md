由于Mac系统10.11 引入安全防护程序Rootless有些系统目录中不允许添加文件 但是class-dump需要在每个文件夹下均可以正常使用 所以就需要配置环境变量 下面是10.12安装class-dump的方法

####1.下载class-dump
```
http://stevenygard.com/projects/class-dump/
```
####2.解压class-dump
下载的class-dump包 自己把文件拿出来 只需要一个名字为 dump-class 的文件

####3.安装步骤
打开家目录
```
cd ~/
```
创建文件夹
```
mkdir bin
```
获取权限
```
sudo -s
```
进入bin文件夹
```
cd ~/bin
```
打开当前文件夹 将`class-dump`文件拖进文件夹
```
open .
```


设置文件权限
```
cd ~/bin
chmod 777 class-dump
```

用xocde编辑.bash_profile文件
```
cd ~/
open -a xcode .bash_profile
```

打开的文件中`第一行`加上如下`代码` 
```
export PATH=$HOME/bin/:$PATH
```
`莫忘保存`

####4.测试文件是否好用
打开控制台输入
```
class-dump
```
出现如下代码则成功
```
class-dump 3.5 (64 bit)
Usage: class-dump [options] <mach-o-file>

  where options are:
        -a             show instance variable offsets
        -A             show implementation addresses
        --arch <arch>  choose a specific architecture from a universal binary (ppc, ppc64, i386, x86_64, armv6, armv7, armv7s, arm64)
        -C <regex>     only display classes matching regular expression
        -f <str>       find string in method name
        -H             generate header files in current directory, or directory specified with -o
        -I             sort classes, categories, and protocols by inheritance (overrides -s)
        -o <dir>       output directory used for -H
        -r             recursively expand frameworks and fixed VM shared libraries
        -s             sort classes and categories by name
        -S             sort methods by name
        -t             suppress header in output, for testing
        --list-arches  list the arches in the file, then exit
        --sdk-ios      specify iOS SDK version (will look in /Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS<version>.sdk
        --sdk-mac      specify Mac OS X version (will look in /Developer/SDKs/MacOSX<version>.sdk
        --sdk-root     specify the full SDK root path (or use --sdk-ios/--sdk-mac for a shortcut)

```

####5.使用方法

```
class-dump /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneSimulator.platform/Developer/SDKs/iPhoneSimulator.sdk/System/Library/Frameworks/UIKit.framework > ~/UIkit.h
```

打开导出文件夹并查看UIkit.h文件
```
open ~/
```