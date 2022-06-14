---
title: UIView-UIViewController-lifeCycle
categories:
 - Swift5-UI
tags:
---

### UIView生命周期

##### xib创建:

1. initWithCoder:
2. awakeFromNib
3. willMoveToSuperview
4. ...
5. layoutSubviews
6. drawRect

##### 代码initWithFrame创建:

1. initWithFrame
2. willMoveToSuperview,,,

##### 代码init创建

1. initWithFrame
2. init
3. willMoveToSuperview,,,

```objective
//构造方法,初始化时调用,不会调用init方法
- (instancetype)initWithFrame:(CGRect)frame;
//添加子控件时调用
- (void)didAddSubview:(UIView *)subview ;
//构造方法,内部会调用initWithFrame方法
- (instancetype)init;
//xib归档初始化视图后调用,如果xib中添加了子控件会在didAddSubview方法调用后调用
- (instancetype)initWithCoder:(NSCoder *)aDecoder;
//唤醒xib,可以布局子控件
- (void)awakeFromNib;
//父视图将要更改为指定的父视图,当前视图被添加到父视图时调用
- (void)willMoveToSuperview:(UIView *)newSuperview;
//父视图已更改
- (void)didMoveToSuperview;
//其窗口对象将要更改
- (void)willMoveToWindow:(UIWindow *)newWindow;
//窗口对象已经更改
- (void)didMoveToWindow;
//布局子控件
- (void)layoutSubviews;
//绘制视图
- (void)drawRect:(CGRect)rect;
//从父控件中移除
- (void)removeFromSuperview;
//销毁
- (void)dealloc;
//将要移除子控件
- (void)willRemoveSubview:(UIView *)subview;
```



### UIViewController生命周期

当加载展示ViewController时,三种方式调用是生命周期方法顺序大致相同,但是有些方法调用的还是有些不同的:

1. 通过Storyboard创建控制器并通过Segue方式展示控制器时,会调用`- (instancetype)initWithCoder:(NSCoder *)coder;`和`-(void)awakeFromNib;`方法,不会调用`- (instancetype)initWithNibName:(NSString *)nibNameOrNil bundle:(NSBundle *)nibBundleOrNil ;`和`- (instancetype)init;`方法;
2. `init`方法初始控制器并push展示控制器,会调用`- (instancetype)initWithNibName:(NSString *)nibNameOrNil bundle:(NSBundle *)nibBundleOrNil ;`和`- (instancetype)init;`方法;不会调用`- (instancetype)initWithCoder:(NSCoder *)coder;`和`-(void)awakeFromNib;`方法
3. `initWithNibName`初始化控制器并push展示,会调用`- (instancetype)initWithNibName:(NSString *)nibNameOrNil bundle:(NSBundle *)nibBundleOrNil`,不会调用`- (instancetype)init;`,`- (instancetype)initWithCoder:(NSCoder *)coder;`和`-(void)awakeFromNib;`方法.
4. 除此以外其他生命周期方法都会调用,并且调用顺序相同.



1.先执行 initWithCoder:/ initWithNibName: bundle:方法

2.loadView

3.viewDidLoad

4.viewWillAppear

5.loadViewIfLayoutNedded

6.viewWillLayoutSubviews

7.viewDidLayoutSubviews

8.viewDidAppear

9.viewWillDisappear

10.viewDidDisappear

11.viewDidUnLoad：这个函数时viewDidLoad的对立函数。在程序内存欠缺时，这个函数被controller调用，来释放他的view以及view相关的对象

12.didReceiveMemoryWarning:当程序内存过度时，系统会调用该方法

13.dealloc



### layoutSubviews调用

1、addSubview会触发layoutSubviews，如果addSubview 如果连续2个 只会执行一次，具体原因下面说。
 2、设置view的Frame会触发layoutSubviews，必须是frame的值设置前后发生了变化。
 3、滚动一个UIScrollView会触发layoutSubviews。
 4、旋转Screen会触发父UIView上的layoutSubviews事件。
 5、改变一个UIView大小的时候也会触发父UIView上的layoutSubviews事件。

如果要立即执行`layoutsubview` ,要先调用[view setNeedsLayout]，把标记设为需要布局，然后马上调用[view layoutIfNeeded]，实现布局.其中的原理是：执行setNeedsLayout后会在receiver标上一个需要被重新布局的标记，在系统runloop的下一个周期自动调用layoutSubviews。这样刷新会产生延迟，所以我们需要马上执行layoutIfNeeded。就会开始遍历subviews的链，判断该receiver是否需要layout。如果需要立即执行`layoutsubview`.

每一个视图只能有唯一的一个父视图。如果当前操作视图已经有另外的一个父视图，则addsubview的操作会把它先从上一个父视图中移除（包括响应者链），再加到新的父视图上面。

并且连续2次的addSubview，只会执行一次layoutsubview。因为一次的runLoop结束后，如果有需要刷新，执行一次即可
