---
title: UINavigationController
categories:
 - iOS-UI
tags:
---

计算到屏幕顶端的距离

```objective-c
CGFloat navigationHeight = self.navigationController.navigationBar.frame.size.height;
CGFloat statusHeight = [UIApplication sharedApplication] statusBarFrame].size.height;
self.topMarginCons.constant = navigationHeight + statusHeight;
```



