---
title: RunLoop (二)
date: 2015-12-23
---

之前有篇博客写了RunLoop的一些理论知识，这篇研究一下RunLoop在实践中的使用。

## 什么时候使用RunLoop

Cocoa和Carbon程序提供了代码运行主程序的循环并自动启动run loop。所以，当在为你的程序创建辅助线程的时候，你才需要显式运行一个run loop。

对于辅助线程，你需要判断一个run loop是否是必须的。如果是必须的，那么你要自己配置并启动它。你不需要在任何情况下都去启动一个线程的run loop。比如，你使用线程来处理一个预先定义的长时间运行的任务时，你应该避免启动run loop。Run loop在你要和线程有更多的交互时才需要，比如以下情况：

* 使用端口或自定义输入源来和其他线程通信
* 使用线程的定时器
* Cocoa中使用任何performSelector…的方法
* 使线程周期性工作

如果你决定在程序中使用run loop，那么它的配置和启动都很简单。和所有线程编程一样，你需要计划好在辅助线程退出线程的情形。让线程自然退出往往比强制关闭它更好。

## 使用实例

``` objc
- (void)viewDidLoad{
[superviewDidLoad];
[NSThreaddetachNewThreadSelector: @selector(newThreadProcess)
                         toTarget: self
                       withObject: nil];
}
```

``` objc
- (void)newThreadProcess{
  @autoreleasepool {
    ////获得当前thread的Runloop
    NSRunLoop* myRunLoop = [NSRunLoop currentRunLoop];
    //设置Run loop observer的运行环境 
    CFRunLoopObserverContext context = {0,self,NULL,NULL,NULL};
    //创建Run loop observer对象 
    //第一个参数用于分配observer对象的内存 
    //第二个参数用以设置observer所要关注的事件，详见回调函数myRunLoopObserver中注释 
    //第三个参数用于标识该observer是在第一次进入runloop时执行还是每次进入run loop处理时均执行 
    //第四个参数用于设置该observer的优先级 
    //第五个参数用于设置该observer的回调函数 
    //第六个参数用于设置该observer的运行环境 
    CFRunLoopObserverRef observer = CFRunLoopObserverCreate(kCFAllocatorDefault,kCFRunLoopAllActivities, YES, 0, &myRunLoopObserver, &context);
    if(observer){
        //将Cocoa的NSRunLoop类型转换成CoreFoundation的CFRunLoopRef类型 
        CFRunLoopRef cfRunLoop = [myRunLoop getCFRunLoop];
        //将新建的observer加入到当前thread的runloop 
        CFRunLoopAddObserver(cfRunLoop, observer, kCFRunLoopDefaultMode);
    }
    [NSTimerscheduledTimerWithTimeInterval: 1
                                    target: self
                                  selector: @selector(timerProcess)
                                  userInfo: nil
                                   repeats: YES];
    NSInteger loopCount = 2;
    do{
        //启动当前thread的loop直到所指定的时间到达，在loop运行时，runloop会处理所有来自与该run loop联系的inputsource的数据 
        //对于本例与当前run loop联系的inputsource只有一个Timer类型的source。 
        //该Timer每隔1秒发送触发事件给runloop，run loop检测到该事件时会调用相应的处理方法。 
        //由于在run loop添加了observer且设置observer对所有的runloop行为都感兴趣。  
        //当调用runUnitDate方法时，observer检测到runloop启动并进入循环，observer会调用其回调函数，第二个参数所传递的行为是kCFRunLoopEntry。 
        //observer检测到runloop的其它行为并调用回调函数的操作与上面的描述相类似。 
        [myRunLoop runUntilDate:[NSDate dateWithTimeIntervalSinceNow:5.0]];
        //当run loop的运行时间到达时，会退出当前的runloop。observer同样会检测到runloop的退出行为并调用其回调函数，第二个参数所传递的行为是kCFRunLoopExit。
        loopCount--;
    }while (loopCount);
  }
}
void myRunLoopObserver(CFRunLoopObserverRef observer,CFRunLoopActivity activity,void *info){
switch (activity) { 
        //The entrance of the run loop, beforeentering the event processing loop.  
        //This activity occurs once for each callto CFRunLoopRun and CFRunLoopRunInMode 
    case kCFRunLoopEntry: 
        NSLog(@"run loop entry"); 
        break; 
        //Inside the event processing loop beforeany timers are processed 
    case kCFRunLoopBeforeTimers: 
        NSLog(@"run loop before timers"); 
        break;
        //Inside the event processing loop beforeany sources are processed 
    case kCFRunLoopBeforeSources: 
        NSLog(@"run loop before sources"); 
        break; 
        //Inside the event processing loop beforethe run loop sleeps, waiting for a source or timer to fire.  
        //This activity does not occur ifCFRunLoopRunInMode is called with a timeout of 0 seconds.  
        //It also does not occur in a particulariteration of the event processing loop if a version 0 source fires 
    case kCFRunLoopBeforeWaiting: 
        NSLog(@"run loop before waiting"); 
        break; 
        //Inside the event processing loop afterthe run loop wakes up, but before processing the event that woke it up.  
        //This activity occurs only if the run loopdid in fact go to sleep during the current loop 
    case kCFRunLoopAfterWaiting: 
        NSLog(@"run loop after waiting"); 
        break; 
        //The exit of the run loop, after exitingthe event processing loop.  
        //This activity occurs once for each callto CFRunLoopRun and CFRunLoopRunInMode 
    case kCFRunLoopExit: 
        NSLog(@"run loop exit"); 
        break; 
        /*
         A combination of all the precedingstages
         case kCFRunLoopAllActivities:
         break;
         */ 
    default: 
        break; 
  } 
}
- (void)timerProcess{
for (int i=0; i<5; i++) {       
    NSLog(@"In timerProcess count = %d.", i);
    sleep(1);
  }
}
``` 