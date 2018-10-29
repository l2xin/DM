## 前言
最近开始正式使用TypeScript。考虑用[typedoc](http://typedoc.org/)这个TypeScript 文档化工具。结果家里的新Mac居然npm都没有.

### 首先，npm是个啥？

npm在Node v0.6.x版本之后，内建于Node系统。通过npm可以协助开发者安装、卸载、删除、更新Node.件，并且可以通过npm发布自己的插件。那么就好办了，先安装Node后就自带npm了。

Node.js 是一个基于 Chrome V8 引擎的 JavaScript 运行环境。 
Node.js 使用了一个事件驱动、非阻塞式 I/O 的模型，使其轻量又高效。摘自：[nodejs.cn](http://nodejs.cn/)

于是乎。。

-------------

## MacOS安装xcode command line tool的两种方法
> 参考：[百度经验-如何安装command line tools](https://jingyan.baidu.com/article/fec4bce2904b3ef2618d8bcc.html)
* 打开终端输入： `xcode-select --install` 然后点击安装
* 登录[https://developer.apple.com/download/more/](https://developer.apple.com/download/more/) 然后下载dmg安装


--------------------------



## Homebrew安装  
*Homebrew*简称brew，是Mac OSX上的软件包管理工具，能在Mac中方便的安装软件或者卸载软件。 

打开终端，执行以下命令安装Homebrew,确保**xcode command line tool**已经安装，本文开头有安装步骤。
``` sh
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

执行上面命令后会提示输入系统密码，输入密码继续安装。

``` sh
l2xindeiMac:~ l2xin$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
==> This script will install:
/usr/local/bin/brew
/usr/local/share/doc/homebrew
/usr/local/share/man/man1/brew.1
/usr/local/share/zsh/site-functions/_brew
/usr/local/etc/bash_completion.d/brew
/usr/local/Homebrew

Press RETURN to continue or any other key to abort
==> Downloading and installing Homebrew...
remote: Enumerating objects: 127, done.
remote: Counting objects: 100% (127/127), done.
remote: Compressing objects: 100% (70/70), done.
remote: Total 113510 (delta 79), reused 83 (delta 57), pack-reused 113383
Receiving objects: 100% (113510/113510), 26.17 MiB | 23.00 KiB/s, done.
Resolving deltas: 100% (83016/83016), done.
From https://github.com/Homebrew/brew
 * [new branch]          master     -> origin/master
 * [new tag]             1.7.7      -> 1.7.7
 * [new tag]             1.8.0      -> 1.8.0
HEAD is now at 18bac4fa5 Merge pull request #5204 from GauthamGoli/immutable-args
==> Homebrew is run entirely by unpaid volunteers. Please consider donating:
  https://github.com/Homebrew/brew#donations
==> Tapping homebrew/core
Cloning into '/usr/local/Homebrew/Library/Taps/homebrew/homebrew-core'...

remote: Enumerating objects: 4858, done.
remote: Counting objects: 100% (4858/4858), done.
remote: Compressing objects: 100% (4654/4654), done.
remote: Total 4858 (delta 55), reused 327 (delta 14), pack-reused 0
Receiving objects: 100% (4858/4858), 4.04 MiB | 49.00 KiB/s, done.
Resolving deltas: 100% (55/55), done.
Tapped 2 commands and 4644 formulae (4,900 files, 12.6MB).
==> Migrating /Library/Caches/Homebrew to /Users/l2xin/Library/Caches/Homebrew..
==> Deleting /Library/Caches/Homebrew...
Already up-to-date.
==> Installation successful!

==> Homebrew has enabled anonymous aggregate formulae and cask analytics.
Read the analytics documentation (and how to opt-out) here:
  https://docs.brew.sh/Analytics.html

==> Homebrew is run entirely by unpaid volunteers. Please consider donating:
  https://github.com/Homebrew/brew#donations
==> Next steps:
- Run `brew help` to get started
- Further documentation: 
    https://docs.brew.sh
```

如安装成功则会看到上面：`Installation successful! `

--------------------------

## 安装Node



``` shell
brew install node
    执行以下命令查看是否安装成功
    node -v:查看node版本
    npm -v：查看npm版本
```

### <font color=FF0000>有人遇到问题 mac上用brew把node装好了，却没有npm，怎么办？</font>
建议node不用brew装 卸载node，安装包安装

``` sh
brew uninstall node
```
官方下载
[https://nodejs.org/dist/v11.0.0/node-v11.0.0.pkg](https://nodejs.org/dist/v11.0.0/node-v11.0.0.pkg)

taobao镜像
[https://npm.taobao.org/mirrors/node](https://npm.taobao.org/mirrors/node)


``` sh
l2xindeiMac:~ l2xin$ npm -v
6.4.1
l2xindeiMac:~ l2xin$ node -v
v11.0.0
l2xindeiMac:~ l2xin$ 
```

搞定了。。


## 升级node

[https://blog.csdn.net/u012982629/article/details/80526385](https://blog.csdn.net/u012982629/article/details/80526385)

## 解决国内NPM(node.js)安装依赖速度慢问题  
这个我倒是没验证,搜到的:[https://blog.csdn.net/nnsword/article/details/54096268](https://blog.csdn.net/nnsword/article/details/54096268)

不知道各位是否遇到这种情况，使用NPM（Node.js包管理工具）安装依赖时速度特别慢，后来在网上找了好久才找到一种最佳解决办法，在安装时可以手动指定从哪个镜像服务器获取资源，我们可以使用阿里巴巴在国内的镜像服务器，命令如下：

``` sh
npm install -gd webpack --registry=http://registry.npm.taobao.org
```

只需要使用–registry参数指定镜像服务器地址，为了避免每次安装都需要--registry参数，可以使用如下命令进行永久设置：
``` sh
npm config set registry http://registry.npm.taobao.org
```

## 后记

全部居然已经很晚了，安装下载太慢了。。有空再接着搞typedoc

------------------------------------

## 参考

* [https://blog.csdn.net/moyummy/article/details/54317866](https://blog.csdn.net/moyummy/article/details/54317866)
* [https://blog.csdn.net/zfangls/article/details/55098299](https://blog.csdn.net/zfangls/article/details/55098299)
* [https://segmentfault.com/q/1010000006447817/a-1020000006449033](https://segmentfault.com/q/1010000006447817/a-1020000006449033)
* [https://blog.csdn.net/nnsword/article/details/54096268](https://blog.csdn.net/nnsword/article/details/54096268)
* [http://nodejs.cn/](http://nodejs.cn/)

