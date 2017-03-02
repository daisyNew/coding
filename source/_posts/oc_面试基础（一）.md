---
title: OC 面试基础（一）
date: 2015-12-01 
---

### 什么是OC?

OC 即Objective-C，他是在C的基础上，增加了面向对象的特性。

### 什么是面向对象？

C是面向过程的语言，关注的是解决问题的步骤，而OC是面向对象的，关注的是设计实现所需功能的类。

#### 面向对象具有三个原则：封装、继承和多态。

##### 封装

封装是把过程和数据包围起来（即函数和数据结构，函数是行为，数据结构是描述），有限制的对数据的访问。

封装保证了模块具有很好的独立性，修改维护比较容易。

#### 继承

OC中只支持单继承。A类继承了B类，则A类为子类，B类为父类。A类继承B类之后，会继承B类的所有方法和实例变量，并且A类可以增加自己的方法，或者通过重写B类的方法，可以对B类方法进行修改。

在正确的继承方式中，父类应当扮演的是底层的角色，子类是上层的业务。父类只是给子类提供服务，并不涉及子类的业务逻辑；层级关系明显，功能划分清晰；父类的所有变化，都需要在子类中体现，此时耦合已经成为需求。

#### 多态

多态是指允许不同类的对象对同一方法作出响应。一般与继承结合来说，其本质是子类通过覆盖或重载父类的方法，来使得对同一类对象同一方法的调用产生不同的结果。

由于他必须先声明数据中每个对象或者容器的类型，因此他是一门静态输入的语言。

它是一门动态语言，具有运行时特性。代码在项目运行时可以修改或者扩展。

### 运行时特性（Runtime）

Runtime 即运行时，是用C和汇编写的一个库，其中最主要的就是消息机制。

比如：[People eat]这个函数调用

1、在程序运行的时候，编译器会将词句转化为objc_msgSend(People,@selector(eat))。

2、在objc_msgSend函数中，首先会通过People的isa指针找到对应的Class。

3、在Class中先去cache中通过SEL查找对应函数method（cache中method列表是以SEL为key通过hash表来存储的，这样能提高函数查找速度）

4、若cache中未找到,再去methodList中查找，若methodlist中未找到，则取superClass中查找。

5、若能找到，则将method加入到cache中，以方便下次查找，并通过method中的函数指针跳转到对应的函数中去执行。

### 动态性表现在三个方面：动态类型、动态绑定和动态加载

#### 动态类型

动态类型就是id类型。OC中内置的类型一般为静态类型，静态类型在编译时就能被识别出来，若类型不匹配，就会爆出警告。而动态类型在编译的时候是不会被识别的，只有在运行时，根据当时的语境来确定类型。

#### 动态绑定

动态绑定需要用到@selector/SEL。OC在运行的时候才会动态地添加函数调用，在运行时才决定最终调用什么的方法，需要传什么参数进去，这就是动态绑定。

要实现它就必须用SEL变量绑定一个方法，SEL并不是C里面的函数指针，虽然很像，但不是。SEL变量只是一个整数，他是该方法的ID。以前的函数调用，是根据函数名，也就是字符串去查找函数体。但现在，我们是根据一个ID整数来查找方法，整数的查找自然要比字符串的查找快得多！所以，动态绑定的特定不仅方便，而且效率更高。

#### 动态加载

动态加载是指根据需求动态地加载资源，在运行时加载新类。在运行时创建新类，只需要3步：

1、为 class pair分配存储空间 ,使用 objc_allocateClassPair函数

2、增加需要的方法使用class_addMethod函数,增加实例变量用class_addIvar

3 、用objc_registerClassPair函数注册这个类,以便它能被别人使用。

Method Swizzling

在Objective-C中调用一个方法，其实是向一个对象发送消息，查找消息的唯一依据是selector的名字。利用Objective-C的动态特性，可以实现在运行时偷换selector对应的方法实现，达到给方法挂钩的目的。每个类都有一个方法列表，存放着selector的名字和方法实现的映射关系。IMP类似函数指针，指向具体的Method实现。

用 method_exchangeImplementations 来交换2个方法中的IMP，

用 class_replaceMethod 来修改类，

用 method_setImplementation 来直接设置某个方法的IMP，归根结底，都是偷换了selector的IMP。

兼容C语言，可以混用。

### Runloop

#### 什么是Runloop?

Runloop 是一种让线程能够随时处理时间但不退出的机制，是一种事件处理的循环，RunLoop实际上是一个对象，这个对象管理了其需要处理的事件和消息，并提供了一个入口函数来执行Event Loop的逻辑。线程执行了这个函数后，就会一直处于这个函数内部 “接受消息->等待->处理” 的循环中，直到这个循环结束（比如传入 quit 的消息），函数返回。让线程在没有处理消息时休眠以避免资源占用、在有消息到来时立刻被唤醒。

一个runloop就是一个事件处理循环，用来不停的监听和处理输入事件并将其分配到对应的目标上进行处理。

#### Runloop 四大作用

1、使程序一直运行接受用户输入
2、决定程序在何时应该处理哪些events
3、调用解耦
4、节省CPU时间

### 事件响应链

对于iOS设备用户来说，操作设备的方式主要有三种：触摸屏幕、晃动设备、通过遥控设施控制设备。对应事件有以下三种：触屏事件（touch event）、运动事件（motion event）和远程控制事件（remote-control event）。

时间的传递和相应分两个链：

#### 传递链：由系统向离用户最近的view传递

UIKit –> active app’s event queue –> window –> root view –>……–>lowest view

#### 响应链：由离用户最近的view向系统传递

initial view –> super view –> …..–> view controller –> window –> Application

响应者对象是指有响应和处理时间能力的对象。响应者链就是由一系列的响应者对象构成的一个层次结构。

响应者链有以下特点：

1、响应者链通常是由视图（UIView）构成的；

2、一个视图的下一个响应者是它视图控制器（UIViewController）（如果有的话），然后再转给它的父视图（Super View）；

3、视图控制器（如果有的话）的下一个响应者为其管理的视图的父视图；

4、单例的窗口（UIWindow）的内容视图将指向窗口本身作为它的下一个响应者，Cocoa Touch应用不像Cocoa应用，它只有一个UIWindow对象，因此整个响应者链要简单一点；

5、单例的应用（UIApplication）是一个响应者链的终点，它的下一个响应者指向nil，以结束整个循环。

### 手指触摸

iOS系统检测到手指触摸(Touch)操作时会将其打包成一个UIEvent对象，并放入当前活动Application的事件队列，单例的UIApplication会从事件队列中取出触摸事件并传递给单例的UIWindow来处理，UIWindow对象首先会使用hitTest:withEvent:方法寻找此次Touch操作初始点所在的视图(View)，即需要将触摸事件传递给其处理的视图，这个过程称之为hit-test view。

UIWindow实例对象会首先在它的内容视图上调用hitTest:withEvent:，此方法会在其视图层级结构中的每个视图上调用pointInside:withEvent:（该方法用来判断点击事件发生的位置是否处于当前视图范围内，以确定用户是不是点击了当前视图），如果pointInside:withEvent:返回YES，则继续逐级调用，直到找到touch操作发生的位置，这个视图也就是要找的hit-test view。

hitTest:withEvent:方法的处理流程如下:

首先调用当前视图的pointInside:withEvent:方法判断触摸点是否在当前视图内；若返回NO,则hitTest:withEvent:返回nil;若返回YES,则向当前视图的所有子视图(subviews)发送hitTest:withEvent:消息，所有子视图的遍历顺序是从最顶层视图一直到到最底层视图，即从subviews数组的末尾向前遍历，直到有子视图返回非空对象或者全部子视图遍历完毕；若第一次有子视图返回非空对象，则hitTest:withEvent:方法返回此对象，处理结束；如所有子视图都返回非，则hitTest:withEvent:方法返回自身(self)。

### OC内存管理

OC 使用引用计数来管理内存，每当alloc一个对象，该对象的引用计数加1，使用该对象一次，引用计数也会加1，如果对该对象release，该对象的引用计数就会减1，若该对象的引用计数为0，这个对象就会被释放。

OC中有MRC和ARC两种内存管理方式。

#### MRC

MRC是手动引用计数。需要程序员自己使用关键字对内存进行管理。

#### ARC

ARC是自动引用计数。是iOS 5推出的，只用ARC,不再需要程序员自己管理内存。但并不是说在ARC下不会出现内存问题。程序员仍需关注内存问题，比如循环引用或野指针的问题。

### app生命周期

app应用程序有5种状态：

Not running未运行：程序没启动。
Inactive未激活：程序在前台运行，不过没有接收到事件。在没有事件处理情况下程序通常停留在这个状态。
Active激活：程序在前台运行而且接收到了事件。这也是前台的一个正常的模式。
Backgroud后台：程序在后台而且能执行代码，大多数程序进入这个状态后会在在这个状态上停留一会。时间到之后会进入挂起状态(Suspended)。有的程序经过特殊的请求后可以长期处于Backgroud状态。
Suspended挂起：程序在后台不能执行代码。系统会自动把程序变成这个状态而且不会发出通知。当挂起时，程序还是停留在内存中的，当系统内存低时，系统就把挂起的程序清除掉，为前台程序提供更多的内存。
初次启动：

iOS_didFinishLaunchingWithOptions

iOS_applicationDidBecomeActive

按下home键：

iOS_applicationWillResignActive

iOS_applicationDidEnterBackground

点击程序图标进入：

iOS_applicationWillEnterForeground

iOS_applicationDidBecomeActive

程序终止

程序只要符合以下情况之一，只要进入后台或挂起状态就会终止：

①iOS4.0以前的系统

②app是基于iOS4.0之前系统开发的。

③设备不支持多任务

④在Info.plist文件中，程序包含了 UIApplicationExitsOnSuspend 键。

系统常常是为其他app启动时由于内存不足而回收内存最后需要终止应用程序，但有时也会是由于app很长时间才响应而终止。

如果app当时运行在后台并且没有暂停，系统会在应用程序终止之前调用app的代理的方法

1
- (void)applicationWillTerminate:(UIApplication *)application；
这样可以让你可以做一些清理工作。你可以保存一些数据或app的状态。这个方法也有5秒钟的限制。超时后方法会返回程序从内存中清除。用户可以手工关闭应用程序。

