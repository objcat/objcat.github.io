#一.卸载
卸载java其实很容易 这里需要删除三个文件夹中的文件

- step 1
```
sudo rm -fr /Library/Internet\ Plug-Ins/JavaAppletPlugin.plugin
```
- step 2
```
sudo rm -rf /Library/PreferencePanes/JavaControlPanel.prefPane
```
- step 3
```
sudo rm -rf /Library/Java
```

到这里卸载完成 测试卸载 直接在终端输入java或javac
```
java
javac
```

#二.安装
无脑点开
http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html