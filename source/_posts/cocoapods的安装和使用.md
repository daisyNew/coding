---
title: Cocoapods的安装和使用
date: 2016-04-23 
---

## 什么是Cocoapods

Cocoapods是管理第三方库的一个工具，使用它可以节省开发时配置第三方库的时间，提高工作效率。这篇博客介绍一下如何安装Cocoapods。

## Cocoapods原理

CocoaPods的原理是将所有的依赖库都放到另一个名为Pods的项目中，然后让主项目依赖Pods项目，这样，源码管理工作都从主项目移到了Pods项目中。Pods项目最终会编译成一个名为libPods.a的文件，主项目只需要依赖这个.a文件即可。

## Cocoapods安装

### 需要先安装Ruby环境

Mac OS本身自带Ruby，但还是更新一下保险。输入如下命令可以查看当前的版本：

* ruby -v

### 更新ruby

* gem sources –-remove https://rubygems.org/
* gem sources -a https://ruby.taobao.org/
* gem sources -l (用来检查使用替换镜像位置成功)

### 下载安装Cocoapods：

* sudo gem install cocoapods

这样就搞定了，就可以开始使用了。

### Cocoapods使用

新建一个项目，在终端cd到当前项目的总目录，在终端输入：

* vim Podfile

然后会打开podfile文件，将所需要的第三方库添加进去，类似这样：

* platform :ios, ‘7.0’
* pod ‘MBProgressHUD’, ‘~> 0.8’

按ESC,接着：wq,就保存退出了。在终端执行命令：

* pod install

等几分钟后，就应该创建好了，这时候现在打开项目不是点击 PodTest.xodeproj了，而是点击 PodTest.xcworkspace。

搞定！