---
layout: post
title: "全屏UIView朝向调整"
description: ""
category: 
tags: [iOS]
---
{% include JB/setup %}

有一个全屏的UIView，它没有controller，放在一个另外的UIWindows中，要监测设备的朝向并调整自身显示

### 1. 监测设备朝向：

	
	// device orientation change
	[[NSNotificationCenterdefaultCenter] addObserver:self
	                                           selector:@selector(_deviceOrientationDidChange:)
	                                               name:UIDeviceOrientationDidChangeNotification 
	                                             object:nil];
	

### 2. 监测到改变后，调整界面  
		

1. 获得UIView的目标大小  


	首先要了解frame, bounds, center的定义

```	
		frame：描述当前视图在其父视图中的位置和大小。  
		bounds：描述当前视图在其自身坐标系统中的位置和大小。  
		center：描述当前视图的中心点在其父视图中的位置。   
```

	由定义可知这时取得[UIScreen mainScreen].applicationFrame 可能不是最终大小，应该从UIWindow的bounds中获取UIView的目标大小


2. 设置bounds：

		_orientation = [UIApplicationsharedApplication].statusBarOrientation;
		if (UIInterfaceOrientationIsLandscape(_orientation)) {
			self.bounds = CGRectMake(0, 0, height, width);
		} else {
			self.bounds = CGRectMake(0, 0, width, height);
		}

3. 设置center:


		CGRect windowBounds = [_showWindow bounds];
		CGPoint center = CGPointMake(ceil(windowBounds.size.width/2),ceil(windowBounds.size.height/2));

4. 宽高设置完后需要设置view的transform 


		CGAffineTransform rotateTransformForOrientation(UIInterfaceOrientation orientation) {
			if (orientation == UIInterfaceOrientationLandscapeLeft) {
				return CGAffineTransformMakeRotation(M_PI*1.5);
			} else if (orientation == UIInterfaceOrientationLandscapeRight) {
				return CGAffineTransformMakeRotation(M_PI/2);
			} else if (orientation == UIInterfaceOrientationPortraitUpsideDown) {
				return CGAffineTransformMakeRotation(-M_PI);
			} else {
				return CGAffineTransformIdentity;
			}
		}

5. 这个view的每算一次transform不是乘以以前的transform的，而是用原始的没有变形去变形。（大概像下面这样）


		_orientation = [UIApplicationsharedApplication].statusBarOrientation;
		self.transform = rotateTransformForOrientation(_orientation);

6. 刷新一下view里面的布局

		[self setNeedsLayout];

7. 重写view的layoutSubviews方法

