---
title: UITextField
categories:
 - iOS-UI
tags:
---

在textField中加入左侧image 一般是放大镜啦 然后调整位置 最简单就是写一个继承类 重写方法

```objective-c
import "LeftViewTextField.h"
@implementation LeftViewTextField
- (CGRect)leftViewRectForBounds:(CGRect)bounds {
  CGRect rect = [super leftViewRectForBounds: bounds];
  rect.origin.x = 11;(需要的数字)
  return rect;
}

- (CGRect)textRectForBounds:(CGRect)bounds {
  CGRect rect = [super textRectForBounds: bounds];
  rect.origin.x = 11;(需要的数字)
  return rect;
}

- (CGRect)editingRectForBounds:(CGRect)bounds {
  CGRect rect = [super editingRectForBounds: bounds];
  rect.origin.x = 11;(需要的数字)
  return rect;
}
```





