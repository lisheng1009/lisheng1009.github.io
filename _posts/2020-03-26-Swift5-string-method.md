---
title: Swift5-string-method
categories:
 - Swift5
tags:
---

#### 1.限制输入位数

```swift
    func textField(_ textField: UITextField, shouldChangeCharactersIn range: NSRange, replacementString string: String) -> Bool {
        
        let MAX_LENGHT = 16

        if ((string.count > range.length) &&
                        (textField.text?.count ?? 0 + string.count - range.length > MAX_LENGHT - 1)) {
            return false
        }
 }

```


#### 2.限制输入字符
a. 设置键盘类型可以解决:   
只允许输入数字(Num)  
只允许输入数字和小数点(Decimal) 等等  

b. 只允许输入特定字符 例如只允许输入 字母 数字 和一些指定字符.

```swift
定义一个宏. 限制只能输入这些字符
let ALPHANUM = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789.*%$-+/ "

写一个过滤方法
    func filterNumAlpha(originString: String) -> String {
        let cs = NSCharacterSet(charactersIn: ALPHANUM).inverted
        let filtered = originString.components(separatedBy: cs).joined(separator: "")
        return filtered
    }

在shouldChangeCharacters方法中进行过滤即可

```

b. 判断如果只输入空格则无效

```swift
为string写一个扩展方法
   var isBlank: Bool {
       let trimmedStr = self.trimmingCharacters(in: .whitespacesAndNewlines)
       return trimmedStr.isEmpty
   }
```


#### 3.根据需要的固定位数 添加前导零
12 -> 000012

string扩展方法

```swift
    static func addPrefixHolder(placeHoder: String?, length: Int, strSource: String) -> String {
        if placeHoder == nil {
            let lenth = strSource.count
            var rest = strSource
            if lenth >= length {
                return rest
            }else {
                for i in 0 ..< (length - lenth) {
                    rest = "0" + rest
                }
                return rest
            }
        }else {
            let lenth = strSource.count
            var rest = strSource
            if lenth >= length {
                return rest
            }else {
                for i in 0 ..< (length - lenth) {
                    rest = placeHoder! + rest
                }
                return rest
            }
        }
    }
```

#### 4.digital check

a. NW-7
b. CODE39

#### 5. 输入数字要求最多输入两位小数
```swift
1. 代码
func textField(_ textField: UITextField, shouldChangeCharactersIn range: NSRange, replacementString string: String) -> Bool {
        let futureString: NSMutableString = NSMutableString(string: textField.text!)
         futureString.insert(string, at: range.location)
         
         var flag = 0;
         
         let limited = 2;//小数点后需要限制的个数
         
         if !futureString.isEqual(to: "") {
             for i in stride(from: (futureString.length - 1), through: 0, by: -1) {
                 let char = Character(UnicodeScalar(futureString.character(at: i))!)
                 if char == "." {
                     if flag > limited {
                         return false
                     }
                     break
                 }
                 flag += 1
             }
         }
         return true
    }

2. keyboardType 设置为 decimal pad
```

#### 6. 初级模糊搜索
```swift
场景: tablaview
baseArray为全部数据
dataArray为搜索到的数据

@objc func textFieldDidChange(_ textField: UITextField) {
        print(textField.text!)
        
        let searchText = textField.text!
        
        if searchText == "" {
            dataArray = baseArray
        }else{
            let resultArray = self.baseArray.filter { (item) -> Bool in
                var res:Bool = true
                for i in searchText {
                    if item.lowercased().contains(i.lowercased()) {
                    }else{
                        res = false
                        break
                    }
                }
                return res
            }
            self.dataArray = resultArray
        }

        self.tv?.reloadData()
    }
```

#### 7.去掉输入联想

```swift
textField.autocorrectionType = .no

```
