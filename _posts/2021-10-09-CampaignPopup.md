---
title: CampaignPopup
categories:
 - iOS-UI
tags:
---

根据服务器传的图片计算显示时的大小(即按比例放大或缩小至最大限度)

+ sb中设置好左右上下margin
+ 拉好imageViewWidthCons和imageViewHeightCons

代码计算宽高:

```swift
func calculateImageSize() {
	let image = self.campaignImageView.image
	let imageViewWidth = self.view.bounds.size.width - 40(上下margin)
	let imageViewHeight = image.size.height / image.size.width * imageViewWidth
	let maxHeight = self.view.bounds.size.height - 64 - 70 (上下)

	if imageViewWidth <= maxHeight {
  	self.imageViewWidthCons.constant = imageViewWidth
  	self.imageViewHeightCons.constant = imageViewHeight
	} else {
  	let imageViewWidth = maxHeight * image.size.width / image.size.height
  	self.imageViewWidthCons.constant = imageViewWidth
  	self.imageViewHeightCons.constant = maxHeight
	}
}
```

在ViewdidLoad中

加载图片前先隐藏图片 

加载并计算宽高后 再显示图片 如果允许横屏 要在 viewWillLayoutSubviews里也调用一次calculateImageSize()

