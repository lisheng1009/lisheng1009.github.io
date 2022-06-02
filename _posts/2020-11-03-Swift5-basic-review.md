---
title: Swift5-basic-review
categories:
 - Swift5
tags:
---


## 可选项


### 定义


var age: Int?

age的类型是可选项(盒子!)不是Int
age的值 会是a.包含着Int的盒子 b.nil

在这里 age没有被设置为一个初始值 但因为类型是可选项 默认为nil
相对的 如果类型是Int 在swift中 没有被设置初始值的情况下 没有初始值(而不是像在OC中会默认为0)


### 使用

强制解包 使用感叹号!  如果为nil 系统报错

#### 可选项绑定

```swift
if let number = Int("123") {
    print("字符串转换成整数成功:\(number)")
    //number是强制解包之后的Int值
    //number作用域仅限于这个大括号
}else{
    print("字符串转换成整数失败")
}
```

可选项绑定的多个判断不能用&&  要用逗号,

#### 空合并运算符
a??b  
a是可选项
若a不为nil,则返回a  若为nil,则返回b
返回类型取决于b
