---
title: RunLoop（一）
date: 2015-11-10 
---

## RunLoop 概念

一个run loop是一个事件处理的循环，用来不停的调度工作以及处理输入事件。使用run loop的目的是让你的线程在有工作的时候忙于工作，而没工作的时候处于休眠状态。如下图所示：


OSX/iOS 系统中，提供了两个这样的对象：NSRunLoop和CFRunLoopRef。CFRunLoopRef 是在 CoreFoundation 框架内的，它提供了纯 C 函数的 API，所有这些 API 都是线程安全的。NSRunLoop 是基于 CFRunLoopRef 的封装，提供了面向对象的 API，但是这些 API 不是线程安全的。

## RunLoop 作用

* 使程序一直处于运行状态并接受用户输入
* 决定程序在何时应该处理哪些Events
* 调用解耦
* 节省CPU时间

## RunLoop 与线程的关系

RunLoop与线程是一一对应的关系，线程刚创建的时候并没有RunLoop，如果你不主动获取，他一直都不会有。RunLoop的创建时发生在第一次获取时，RunLoop的销毁是发生在线程结束时。你只能在一个线程的内部获取其RunLoop（主线程除外）。

## RunLoop 对外接口

### 获取当前线程的RunLoop

在任何一个Cocoa程序的线程中，都可以通过：

* NSRunLoop *runloop = [NSRunLoop currentRunLoop];
来获取当前线程的RunLoop。

OSX/iOS 系统中，提供了两个这样的对象：NSRunLoop 和 CFRunLoopRef。

CFRunLoopRef 是在 CoreFoundation 框架内的，它提供了纯 C 函数的 API，所有这些 API 都是线程安全的。

NSRunLoop 是基于 CFRunLoopRef 的封装，提供了面向对象的 API，但是这些 API 不是线程安全的。

以下为关系图：



在CoreFoundation里关于RunLoop有5个类：

#### CFRunLoopRef

#### CFRunLoopModeRef

mode,即被监听的事件源。一个RunLoop包含若干个mode，每个mode又包含若干个Source/timer/observer，每次调用RunLoop的主函数时，只能指定其中一个mode，被称为CurrentMode。如需切换mode，需退出loop，再重新制定一个mode进入。这样做主要是为了分隔开不同组的 Source/Timer/Observer，让其互不影响。

#### CFRunLoopSourceRef

事件产生的地方。Run Loop 对象处理的事件源分为两种：Input sources 和 Timer sources。二者的区别在于：基于端口的输入源由内核自动发送，而自定义的则需要人工从其他线程发送。



Input sources：用分发异步事件，通常是用于其他线程或程序的消息，比如： performSelector:onThread:…
Timer sources：用分发同步事件，通常这些事件发生在特定时间或者重复的时间间隔上，比如： [NSTimer scheduledTimerWithTimeInterval:target:selector:…]。Timer sources在预设的时间点同步的传递消息，Timer 是线程通知自己做某件事的一种方式。Foundation 中 NSTimer Class 提供了相关方法来设置 Timer sources。需要注意的是除了 scheduledTimerWithTimeInterval 开头的方法创建的 Timer以外， 都需要手动添加到当前 Run Loop中（scheduledTimerWithTimeInterval 创建的 Timer 会自动以 Default Mode 加载到当前 Run Loop中。）
Timer 在选择使用一次后，在执行完成时，会从 Run Loop 中移除。选择循环时，会一直保存在当前 Run Loop 中，直到调用 invalidated 方法。
上面图中展示了 Run Loop 的概念结构及各种事件源。其中 Input sources 分发异步事件给相应的处理程序并且调用 runUntilDate: 方法（这个方法会在该线程关联的 NSRunLoop 对象上被调用）来退出其 Run Loop。Timer sources 分发事件到相应的处理程序，但不会引起 Run Loop 退出。

#### CFRUNLoopTimerRef

基于时间的触发器。当其加入到 Run Loop 时，Run Loop 会注册对应的时间点，当时间点到时，Run Loop 会被唤醒以执行那个回调。

#### CFRunLoopObserverRef

观察者，当RunLoop发生变化时，观察者能够通过会掉接收到这里边的变化。

### RunLoop的事件处理流程



每次运行run loop，你线程的run loop对会自动处理之前未处理的消息，并通知相关的观察者。具体的顺序如下：

通知观察者run loop已经启动
通知观察者任何即将要开始的定时器
通知观察者任何即将启动的非基于端口的源
启动任何准备好的非基于端口的源
如果基于端口的源准备好并处于等待状态，立即启动；并进入步骤9。
通知观察者线程进入休眠
将线程置于休眠直到任一下面的事件发生：
某一事件到达基于端口的源
定时器启动
Run loop设置的时间已经超时
run loop被显式唤醒
通知观察者线程将被唤醒。
处理未处理的事件
如果用户定义的定时器启动，处理定时器事件并重启run loop。进入步骤2
如果输入源启动，传递相应的消息
如果run loop被显式唤醒而且时间还没超时，重启run loop。进入步骤2
通知观察者run loop结束。

### 苹果用 RunLoop 实现的功能

AutoreleasePool
事件响应
手势识别
界面更新
定时器
preformSelector
何时使用Run Loop

仅当在为你的程序创建辅助线程的时候，你才需要显式运行一个run loop。Run loop是程序主线程基础设施的关键部分。所以，Cocoa和Carbon程序提供了代码运行主程序的循环并自动启动run loop。iOS程序中UIApplication的run方法（或Mac OS X中的NSApplication）作为程序启动步骤的一部分，它在程序正常启动的时候就会启动程序的主循环。类似的，RunApplicationEventLoop函数为Carbon程序启动主循环。如果你使用xcode提供的模板创建你的程序，那你永远不需要自己去显式的调用这些例程。

对于辅助线程，你需要判断一个run loop是否是必须的。如果是必须的，那么你要自己配置并启动它。你不需要在任何情况下都去启动一个线程的run loop。比如，你使用线程来处理一个预先定义的长时间运行的任务时，你应该避免启动run loop。Run loop在你要和线程有更多的交互时才需要，比如以下情况：

使用端口或自定义输入源来和其他线程通信
使用线程的定时器
Cocoa中使用任何performSelector…的方法
使线程周期性工作
如果你决定在程序中使用run loop，那么它的配置和启动都很简单。和所有线程编程一样，你需要计划好在辅助线程退出线程的情形。让线程自然退出往往比强制关闭它更好。关于更多介绍如何配置和退出一个run loop,请听下回分解。