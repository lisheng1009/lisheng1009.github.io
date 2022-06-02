---
title: UIView
categories:
 - Swift5-UI
tags:
---

自定义UIView 初始化  
UIView有两个指定初始化器:
- (instancetype)initWithFrame:(CGRect)frame NS_DESIGNATED_INITIALIZER;
- (nullable instancetype)initWithCoder:(NSCoder *)coder NS_DESIGNATED_INITIALIZER;


创建自定义UIView, 无非两种途径: 纯代码或IB.   
如果用代码创建自定义UIView, 并且想要在初始化后进行一些配置, 则需要重写initWithFrame()方法并在其中进行配置. 因为纯代码创建的情况下 会调用initWithFrame()方法. swift中如果重写了此方法, 会直接报错'required' initializer 'init(coder:)' must be provided by subclass of 'UIView'.   
网上大多博主都笼统的解释为纯代码创建会调用initWithFrame(), xib创建则会调用initWithCoder() 这其实很明显是不充分的 因为上述情况下也要求你实现
initWithCoder(). 到底为什么要实现? 因为UIView是遵守NSCoding协议的 必须要实现initWithCoder()这个协议方法. 继承协议方法有两种情况: 隐式和显式. 当我们在子类定义了其他指定初始化器(包括自定义和重写父类指定初始化器), 就必须显式实现, 也就是为什么会报错来让我们补上. 其他情况下则会隐式继承 比如刚创建子类 还没有重写initWithFrame()方法时就不会报错.   

init(coder:)这个方法到底在做什么?  

cocoa具备一种机制来将对象自身转换为某种格式并保存中磁盘上。   
对象可以将它们的实例变量和其他数据编码为数据块，然后保存到磁盘中。以后将这些数据块都会到内存中，并且还能基于保存的数据创建新对象。这个过程称为编码和解码，或称为序列化和反序列化。   

当对象需要保存自身时－encoderWithCoder:方法被调用   
当对象需要加载自身时－initWithCoder:方法被调用  

initWithCode:和其他init方法一样，中对对象执行操作之前，需要使用超类对它们进行初始化。为此，可以采用两种方式，具体取决于父类，如果父类采用了NSCoding协议，则应该调用[super initWithCoder:decoder]; 否则，只需要调用super.init即可。NSobject不采用NSCoding协议，因此我们可以使用简单的init方法.  


使用xib创建UIView:  

和UIViewController, UITableViewCell之类的创建不同, UIView在创建时是没有同时创建xib文件的选项的. 只在Stack Overflow找到一个解释   

Because the relationship between a view controller and a nib is totally different from the relationship between a view and a nib. UIView and nibs do not "go together" in any magic or important way, as do a UIViewController and its view nib.

With a view controller, if there is a nib with the same name as the view controller class, and if the File's Owner in that nib is typed as the class of the view controller, and if the view outlet of the File's Owner is pointed at the top-level UIView in the nib, the view controller can load its view automatically from the nib. That is a complicated arrangement, and it is doubtful that you would know how to configure it correctly (and it's a lot of work even if you do know how), so the template offers to configure it for you. This is a standard, important, automatic relationship.
But with a view and a nib there is no such standard automatic relationship, and there is no complexity. If you want a certain view in a certain nib to be of a certain UIView subclass, you just say so in its Identity inspector, and kaboom you're done. So just do that and move on.


大致翻译:   
因为一个ViewController和它的nib的关系 和 一个View和它的nib的关系 是完全不同的. 后者的关系没有前者那么"紧密". 前者的话, 如果有一个nib和此ViewController同名, 并且此nib的File's Owner从属于该ViewController类, 并且输出于window最上层, 则会自动加载该nib. 这是个略复杂但是标准, 重要, 并且自动的关系. 相对的, 后者就没有这种关系更没有类似的复杂性.   
(这个说法似乎不能解释为什么cell之类的视图也可以直接创建xib而uiview就是不行...)  


这里提到了nib, nib和xib是什么关系?  
ib 是 Interface Builder 的缩写，即界面构造器。这里简要说下，Xib 和 Nib 各是什么，有什么区别。  
Xib 实际是一个 XML 文件，而 Nib 是二进制文件。当应用编译时，Xib 文件被翻译为 Nib。所以在 Xcode 中，我们可以自己新建 Xib 文件来构造 UI，而当编译时，Xcode 会自动生成相应的 Nib 文件，而不需我们额外关注。  

使用xib创建UIView, 需要在创建继承自UIView的swift文件后, 再创建一个同名的xib View文件. 并且在该xib的custom class中设置好同名.     
调用:  
let dyView = Bundle.main.loadNibNamed("DYView", owner: nil, options: nil)?.first as! DYView  
