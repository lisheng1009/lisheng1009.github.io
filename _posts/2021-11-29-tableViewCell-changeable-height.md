---
title: tableViewCell-changeable-height
categories:
 - iOS-UI
tags:
---

使用场景:

本来是默认的行高 显示内容 显示不下的情况 点击下方"显示更多"以后增大行高全部显示

增大后的高度计算:

```objective-c
UIEdgeInsets insets = UIEdgeInsetsMake(10.0, 10.0, 10.0, 10.0);
CGSize size = CGSIzeMake(CGRectGetWidth(UIEdgeInsetsInsetRect(tableView.bounds, insets)), CGFLOAT_MAX);
CGRect rect = [self.contentText boundingRectWithSize:size
              															 options:NSStringDrawingUsesLineFragmentOrigin | NSStringDrawingUsesFontLeading
               														 attribute:@{NSFontAttributeName:[UIFont systemFontOfSize:12.0]} 
               															 context: nil ];
CGFloat height = insets.top + insets.bottom + CGRectGetHeight(rect) + 1.0;
```

