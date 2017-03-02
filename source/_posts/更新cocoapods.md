---
title: 更新Cocoapods

date: 2016-11-14 
---

### Cocoapods 更新过程：

* sudo gem update –system 先更新gem资源。国内的话需要切换资源。
* gem sources –remove https://rubygems.org/
* gem sources -a https://ruby.taobao.org/
* gem sources -l 查看当前资源
* sudo gem install -n /usr/local/bin cocoapods
* pod setup

这时候查看版本号：

* pod –version
就是新版本了。