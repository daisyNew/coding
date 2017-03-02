---
title: App 内直接下载应用，不用跳转到App Store
date: 2016-06-10
---

一般情况下，我们要下载某一个应用时，都需要先跳转到app store中对应应用的页面，然后再进行下载。iOS 6之后，苹果提出，也可以直接在app中调起下载页面进行下载，不需要再跳转了，这对于浏览器应用来说可谓是一个福利啊！下面我们来看看这个功能怎么实现：

主要涉及到一个类：SKStoreProductViewController,这个类继承自UIViewController，基本用法与UIViewController相同。话不多说，请看代码：

1、首先需要导入库

``` objc
   #import < StoreKit/StoreKit.h >
``` 
 
2、实现代理

``` objc
@interface ViewController() < SKStoreProductViewControllerDelegate >
 ``` 

3、点击下载按钮触发的方法：

``` objc
   - (void)showStoreInApp:(NSString *)appid{
   Class isAllow = NSClassFromString(@"SKStoreProductViewController");
   if (isAllow != nil) {
      SKStoreProductViewController *skStoreProductViewController = [[SKStoreProductViewController alloc]init];
      skStoreProductViewController.delegate = self;
     [skStoreProductViewController loadProductWithParameters:@{SKStoreProductParameterITunesItemIdentifier:appID} 
      completionBlock:^(BOOL result, NSError * _Nullable error) {
        if (result) {
           [self presentViewController:skStoreProductViewController                          
                              animated:YES 
                            completion:nil];
        }else{
           NSLog(@"error==%@",error);
        }
      }];
   }else{
      NSString *string = [NSString stringWithFormat:@"itms-apps://itunes.apple.com/us/app/id%@?mt=8",appID];
      [[UIApplication sharedApplication]openURL:[NSURL URLWithString:string]];
   }
}
```
 
4、点击取消按钮出发的方法：

``` objc
- (void)productViewControllerDidFinish:(SKStoreProductViewController *)viewController{
    [viewController dismissViewControllerAnimated:YES
                                       completion:nil];
}
 ``` 