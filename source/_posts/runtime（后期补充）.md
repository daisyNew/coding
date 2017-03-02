---
title: Runtime（后期补充）
date: 2015-11-01
---

## Runtime 是什么？

OC是一门动态语言，即运行时语言，它会尽可能的把编译时候的事情压到运行的时候去做。这样的话，我们可以在运行的时候改变代码的效果，比如交换方法等，为开发提供了很大的灵活性。OC就是通过Runtime来实现这一动态性的.

## Runtime 消息机制

举个OC中调用方法的栗子：

``` objc
id returnValue = [receiver messageName:parameter];
``` 

编译之后，在runtime中是这样的形式:

``` objc
id returnValue = objc_msgSend(receiver,@selector(messageName:),parameter);
```

查找并执行方法的过程如下：

编译器将代码[obj makeText];转化为objc_msgSend(obj, @selector (makeText));，在objc_msgSend函数中。首先通过obj的isa指针找到obj对应的class。在Class中先去cache中 通过SEL查找对应函数method（猜测cache中method列表是以SEL为key通过hash表来存储的，这样能提高函数查找速度），若 cache中未找到。再去methodList中查找，若methodlist中未找到，则取superClass中查找。若能找到，则将method加 入到cache中，以方便下次查找，并通过method中的函数指针跳转到对应的函数中去执行。

如果上面的过程中找到了叫messageName的方法，那么查找过程结束，可以调用相应的实现（IMP）。如果找到根类都没有找到，那么消息发送失败，进入消息转发流程。消息转发有三个方案:

## 添加方法

可以在类中实现下面两个方法：

``` objc
+ (BOOL)resolveInstanceMethod:(SEL)sel; //找不到实例方法时会调
+ (BOOL)resolveClassMethod:(SEL)sel; // 找不到类方法时会调
把消息发给别人处理
``` 

如果没有做上面的添加方法的事情，而在类中实现了

``` objc
- (id)forwardingTargetForSelector:(SEL)aSelector;
系统会自动调用到该方法。在这里你可以让别的对象处理这个消息。
``` 

网上有这么个例子
``` objc
- (id)forwardingTargetForSelector:(SEL)aSelector{
    NSString *selStr = NSStringFromSelector(aSelector);
    if ([selStr isEqualToString:@"eat"]) {
      return [[Child alloc] init];        // 这里返回Child类对象，让Child去处理eat消息
    }
    return [super forwardingTargetForSelector:aSelector];
}
``` 

## 整体转发

如果前两步都没有处理这条消息，那么系统会把消息有关的全部细节装在一个NSInvocation里面，调用到

``` objc
- (void)forwardInvocation:(NSInvocation *)anInvocation;
这个方法。
``` 

一般会在里面对消息做一些改变，改变内容，修改参数等等，力求能够合理处理。下图显示得非常清楚：


## Runtime 使用

在程序运行过程中, 动态创建一个类(比如KVO的底层实现)
在程序运行过程中, 动态地为某个类添加属性\方法, 修改属性值\方法
遍历一个类的所有成员变量(属性)\所有方法