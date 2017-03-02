
---
title: OC 经验总结
date: 2016-05-08
---

## 关于NSString拼接

1、保留两位小数：

``` objc
NSString *str = [NSString stringWithFormat:@"%.2f",M_PI];
//输出是3.14
NSLog(@"%@",str);
``` 

2、用0补全的方法

``` objc
NSInteger count = 5;
//02表示：如果count不足2，用0在前面补全，2表总数
NSString *str = [NSString
stringWithFormat:@"%02zd",count];
//输出结果为05
NSLog(@"%@",str);
``` 

3、字符串中有特殊符号%

``` objc
NSInteger count = 50;
NSString *str = [NSString stringWithFormat:@"%zd%%",count];
//输出50%
NSLog(@"%@",str);
``` 

4、判断是否为gif/png图片的正确姿势

``` objc
 - (NSString *)contentTypeForImageData:(NSData *)data{
   uint8_t c;
   [data getBytes:&c  length:1];
   switch (c) {
    case 0xFF:
        return  @"jpeg";
    case 0x89:
        return  @"png";
    case 0x47:
        return  @"gif";
    case 0x49:
    case 0x4D:
        return  @"tiff";
    case 0x52:
        if ([data length] < 12) {
            return nil;
        }
        NSString *testString = [[NSString alloc] initWithData:[data subdataWithRange:NSMakeRange(0, 12)] encoding:NSASCIIStringEncoding];
        if ([testString hasPrefix:@"RIFF"] && [testString hasSuffix:@"WEBP"]) {
            return @"webp";
        }
        return nil;
 }
return nil;
}
``` 
5、设置图片圆角

``` objc
/** 设置圆形图片(放到分类中使用) */
- (UIImage *)cutCircleImage {
   UIGraphicsBeginImageContextWithOptions(self.size, NO, 0.0);
   // 获取上下文
   CGContextRef ctr = UIGraphicsGetCurrentContext();
   // 设置圆形
   CGRect rect = CGRectMake(0, 0, self.size.width, self.size.height);
   CGContextAddEllipseInRect(ctr, rect);
   // 裁剪
   CGContextClip(ctr);
   // 将图片画上去
   [self drawInRect:rect];
   UIImage *image = UIGraphicsGetImageFromCurrentImageContext();
   UIGraphicsEndImageContext();
   return image;
}
```

6、## 和 #@ 的使用

``` objc
#define LRWeakSelf(type)  __weak typeof(type) weak##type = type;
``` 

表示把weak和type联系起来，声明一个弱引用。

@#的使用, 我们添加一个普通的宏:

//随便写一个宏

``` objc
  #define LRToast(str) [NSString stringWithFormat:@"%@",str]
``` 
//这个宏需要这样写

``` objc
  LRToast(@"温馨提示");
  NSLog(@"%@",LRToast(@"温馨提示"));
``` 
添加@#之后:

//随便写一个宏

``` objc
#define LRToast(str) [NSString stringWithFormat:@"%@",@#str]
``` 

//这个宏需要这样写

``` 
LRToast(温馨提示);
``` 

//正常运行, 打印不会报错

``` objc
NSLog(@"%@",LRToast(温馨提示));
``` 

我们可以看出来 LRToast(温馨提示);与LRToast(@”温馨提示”);区别, 也就是说@#可以代替@”” 那么我们以后开发就省事了, 不用再添加@””了!

## 监听UITextFiled内容的变化

通常要监听UITextFiled内容的变化，会用它的一个代理方法，但是每次监听都不准确，其实可以这样监听：

``` objc
[textField addTarget:self action:@selector(textEditingChanged) forControlEvents:UIControlEventEditingChanged];
``` 

## 修改了leftBarButtonItem如何恢复系统侧滑返回功能

在开发中系统的leftBarButtonItem不是我们想要的, 如果我们修改了leftBarButtonItem那么系统自带的侧滑返回功能就不好使了!

//设置代理 

``` objc                                                   self.interactivePopGestureRecognizer.delegate = self;
#pragma mark - <UIGestureRecognizerDelegate>
``` 

//实现代理方法:return YES :手势有效, NO :手势无效

``` objc
- (BOOL)gestureRecognizerShouldBegin:(UIGestureRecognizer *)gestureRecognizer
{
 //当导航控制器的子控制器个数 大于1 手势才有效
 return self.childViewControllers.count > 1;
}
```

以上摘自 简书

## 设置虚线边框

即如图效果：


``` objc
CAShapeLayer *borderLayer = [CAShapeLayer layer];
borderLayer.bounds = CGRectMake(0, 0, ScreenW - 36, _btnImage.bounds.size.height);
borderLayer.position = CGPointMake(CGRectGetMidX(_btnImage.bounds), CGRectGetMidY(_btnImage.bounds));
borderLayer.path = [UIBezierPath bezierPathWithRect:borderLayer.bounds].CGPath;
borderLayer.lineWidth = 1;
borderLayer.lineCap = @"square";
borderLayer.frame = _btnImage.bounds;
//虚线边框
borderLayer.lineDashPattern = @[@4, @4];
borderLayer.fillColor = [UIColor clearColor].CGColor;
borderLayer.strokeColor = [UIColor getColor:@"b6b6b6"].CGColor;
[_btnImage.layer addSublayer:borderLayer];
``` 