# 🍎 安装

## 🌲 常规安装

安装时可能出现错误

```
brew install ruby

Error: rust: undefined method `on_arm' for #<Resource:0x00007fe05240d8c0>
```

不要着急, 我们尝试更新brew

```
brew update

Auto packing the repository in background for optimum performance.
See "git help gc" for manual housekeeping.
```

然后在执行安装, 发现可以安装了, 但还是有错误

```shell
brew install ruby

objcat@yuanjun-2 ~ % brew install ruby
==> Fetching dependencies for ruby: readline
==> Fetching readline
==> Downloading https://ghcr.io/v2/homebrew/core/readline/manifests/8.2.1
Already downloaded: /Users/objcat/Library/Caches/Homebrew/downloads/ab483c9a913ae82f3a2b3ae20918791bc3bd6825c7122a29cd4f1e0c65413759--readline-8.2.1.bottle_manifest.json
==> Downloading https://ghcr.io/v2/homebrew/core/readline/blobs/sha256:19e6b02f577010a1a33c6ae6f09e40772d6ab22d94b6cf3455cfed9d301d28cf
Already downloaded: /Users/objcat/Library/Caches/Homebrew/downloads/819ad49742c0c4bde0b983e2fcd3f81e80365dae289587047fb8380a9386be42--readline--8.2.1.monterey.bottle.tar.gz
==> Fetching ruby
==> Downloading https://ghcr.io/v2/homebrew/core/ruby/manifests/3.2.2_1
Already downloaded: /Users/objcat/Library/Caches/Homebrew/downloads/b7df479895630ebb7fdc5e0915854094e1f610797d6eff15202cebf668cbad53--ruby-3.2.2_1.bottle_manifest.json
==> Downloading https://ghcr.io/v2/homebrew/core/ruby/blobs/sha256:8cf820914f34d82f6ae5b80a2eae7b75c133a5263e6ca34338a161542878c413
Already downloaded: /Users/objcat/Library/Caches/Homebrew/downloads/2aea02922b0ba609eb0edc08393519b77e70dd9e4cdef2662f6cf97adb9d45a6--ruby--3.2.2_1.monterey.bottle.tar.gz
==> Installing dependencies for ruby: readline
==> Installing ruby dependency: readline
==> Pouring readline--8.2.1.monterey.bottle.tar.gz
cp: /usr/local/Cellar/readline/./8.2.1: Permission denied
cp: /private/tmp/d20230802-17705-lf868c/readline/./8.2.1: unable to copy extended attributes to /usr/local/Cellar/readline/./8.2.1: Permission denied
cp: /usr/local/Cellar/readline/./8.2.1/.brew: No such file or directory
cp: /private/tmp/d20230802-17705-lf868c/readline/./8.2.1/.brew: unable to copy extended attributes to /usr/local/Cellar/readline/./8.2.1/.brew: No such file or directory
cp: /usr/local/Cellar/readline/./8.2.1/.brew/readline.rb: No such file or directory
```

通过分析我们可以看到它下载了`/usr/local/Cellar/readline/./8.2.1`这个工具, 但是没有安装上, 因为权限问题, 我尝试了在命令前面添加`sudo`来获取权限, 但是高版本的`brew`不允许获取高权限安装, 所以这里可能只有改变文件夹的权限了, 但我这里没有去更改文件夹权限而是在它下载目录把工具手动解压出来, 替代了原来的工具

```shell
/Users/objcat/Library/Caches/Homebrew/downloads/819ad49742c0c4bde0b983e2fcd3f81e80365dae289587047fb8380a9386be42--readline--8.2.1.monterey.bottle.tar.gz
```

我们去到这个文件夹, 然后解压出来了, 然后把新工具替换到这个里面`/usr/local/Cellar/readline`

然后再安装发现通过了, 我把这种安装方式叫做手动安装法, `homebrew`给我们安装到了这个路径`/usr/local/Cellar/ruby`

然后我们执行命令来设置环境变量

```shell
brew link --overwrite ruby

Warning: Refusing to link macOS provided/shadowed software: ruby
If you need to have ruby first in your PATH, run:
  echo 'export PATH="/usr/local/opt/ruby/bin:$PATH"' >> ~/.zshrc

For compilers to find ruby you may need to set:
  export LDFLAGS="-L/usr/local/opt/ruby/lib"
  export CPPFLAGS="-I/usr/local/opt/ruby/include"

For pkg-config to find ruby you may need to set:
  export PKG_CONFIG_PATH="/usr/local/opt/ruby/lib/pkgconfig"
objcat@yuanjun-2 ~ % ruby -v
ruby 2.6.10p210 (2022-04-12 revision 67958) [universal.x86_64-darwin21]
```

我们会发现它让我们自己设置, 首先要确认你的`ruby`安装路径, 我的是下面的

```
echo 'export PATH="/usr/local/opt/ruby/bin:$PATH"' >> ~/.zshrc
echo 'export LDFLAGS="-L/usr/local/opt/ruby/lib"' >> ~/.zshrc
echo 'export CPPFLAGS="-I/usr/local/opt/ruby/include"' >> ~/.zshrc
echo 'export PKG_CONFIG_PATH="/usr/local/opt/ruby/lib/pkgconfig"' >> ~/.zshrc
```

我对这个路径有点疑惑, 因为给我安装的路径并不是这个, 而是``

然后我打开`finder`按`cmd+shift+g`追踪到目录, 发现是同一个目录的不同种写法, 我不知道为什么可以这么写, 但是能用, 这还挺好的

```
/usr/local/opt/ruby/bin
等于
/usr/local/Cellar/ruby/3.2.2_1/bin
```

解决了疑惑 搞完是这个样子

```
cat ~/.zshrc
```

![](../../../4-package-manager/homebrew/homebrew/images/Pasted%20image%2020230802175308.png)

然后我们重启控制台或者`source ~/.zshrc`就可以使用最新的ruby了

> 需要注意的是, 最新版本的MacOS使用的是`zsh`, 配置文件为`~/.zshrc`, 而我们配置环境变量的文件是`~/.bash_profile`, 所以我们要在`~/.zshrc`中添加`source ~/.bash_profile`就可以让命令生效了, 如果你使用的是`.zshrc`进行配置就不需要多做这一部分操作

```
objcat@yuanjun-2 ~ % ruby -v
ruby 2.6.10p210 (2022-04-12 revision 67958) [universal.x86_64-darwin21]

objcat@yuanjun-2 ~ % ruby -v
ruby 3.2.2 (2023-03-30 revision e51014f9c0) [x86_64-darwin21]
```

好的3.2.2版本安装成功了

## 🌲 在m2的电脑上使用清华镜像源重装

重装之后我遇到了问题, 因为ruby安装路径和上面不一样了, 那么我先我就要确认我的安装目录

首先我们确定brew的安装目录

```
/opt/homebrew
```

然后再里面找ruby

```
/opt/homebrew/Cellar/ruby/3.2.2_1/bin
```

然后我就要改成下面这个样子

```
export PATH="/opt/homebrew/Cellar/ruby/3.2.2_1/bin:$PATH"
export LDFLAGS="-L/opt/homebrew/Cellar/ruby/3.2.2_1/lib"
export CPPFLAGS="-I/opt/homebrew/Cellar/ruby/3.2.2_1/include"
export PKG_CONFIG_PATH="/opt/homebrew/Cellar/ruby/3.2.2_1/lib/pkgconfig"
```

配置完就解决了