---
title: 动态获取WebViewCell的高度
date: 2016-03-18
---

上个月接手了一个资讯类项目，详情页中部嵌入了一个webViewCell(即，tableViewCell的contentView铺满了WebView)。

接手的时候该页已经完成。之前的开发人员逻辑是：

在webView的代理方法：

-(void)webViewDidFinishLoad:(UIWebView *)webView中获取webView的高度，然后赋值刷新表。

但是，这样会出现webview高度加载不正确，经常会有webView看起来被截断的问题。于是换成KVO来监听webView.scrollView的ContentSize，动态改变webView的高度。具体实现如下：

### webView初始化的时候添加监听

[self.webView.scrollView addObserver:self forKeyPath:@"contentSize" options:NSKeyValueObservingOptionNew context:nil];

### 实现监听的方法

- (void)observeValueForKeyPath:(NSString *)keyPath ofObject:(id)object change:(NSDictionary<NSString *,id> *)change context:(void *)context{
    if ([keyPath isEqualToString:@"contentSize"]) {
        CGFloat height = [[_webView stringByEvaluatingJavaScriptFromString:@"document.body.offsetHeight"] floatValue];
        CGRect rect = self.webView.frame;
        rect.size.height = height ;
        self.webView.frame = rect;
        [self.tableView reloadRowsAtIndexPaths:@[[NSIndexPath indexPathForRow:1 inSection:0]] withRowAnimation:UITableViewRowAnimationNone];
    }
}

#### 注意：获取到高度之后，最好只刷新需要刷新的那一行，不要整个刷新。

### 不要忘了移除监听

- (void)viewWillDisappear:(BOOL)animated{
    [super viewWillDisappear:animated];
    [_webView.scrollView removeObserver:self forKeyPath:@"contentSize" context:nil];
}
