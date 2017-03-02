---
title: React Native开始

date: 2016-10-14 
---

### 环境需求：

OS X
HomeBrew
Node.js 4.0或更高版本

watchman,推荐安装，否则可能会遇到Node.js监视文件系统的Bug,命令行输入 brew install watchman就可以安装。

iOS开发必备：Xcode 7.0或更高版本

### 快速开始：

#### 配置资源

由于网络原因，react native命令行从npm官方源托代码时可能会遇上麻烦，因此，请先将npm仓库源替换为国内镜像。使用如下命令：

* npm config set registry https://registry.npm.taoba.org –global
* npm config set disturl https://npm.taobao.org/dist –global

#### 创建新项目

* sudo npm install -g react-native-cli 需要输入root密码
react-native init AwesomeProject

这样一个新项目就创建好了，要运行iOS应用，执行命令：

* cd AwesomeProject

你会看到，已经存在一个AwesomeProject的文件夹，里边包含android、ios、node_modules三个文件夹和一些文件。打开ios文件夹，用Xcode打开工程文件即可。

#### 修改工程

使用你喜欢的编辑器打开index.ios.js,随便随便改上几行，在ios Emulator中按下command-R就可以刷新app并看到最新修改了。

#### 为已有的React Native工程添加Android支持：

打开package.json文件，在dependencies项中找到react-native,并将其后的版本号修改为最新版本。

* npm install react-native android