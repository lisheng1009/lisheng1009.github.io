---
title: Swift5-date-method
categories:
 - Swift5
tags:
---

##### 1.是否为今日
```swift
    static func isToday(_ dateString:String) -> Bool {
        
        let dateFormatter = DateFormatter()
        dateFormatter.dateFormat = "y'yyy-MM-dd"
        if let date = dateFormatter.date(from: dateString) {
            return Calendar.current.isDate(Date(), inSameDayAs: date)
        }
        return false
    }
```

##### 2.日期间隔
```swift
    func daysBetweenDate(toDate: Date) -> Int {
        let components = Calendar.current.dateComponents([.day], from: self, to: toDate)
        return components.day ?? 0
    }
```
#### 3. 是否为过去日
```swift
    static func isBeforeToday(_ dateString:String) -> Bool {
        let fmt = DateFormatter()
        fmt.dateFormat = "yyyy-MM-dd"
        let result = fmt.date(from: dateString)!
        let timeInterval = result.timeIntervalSince1970
        let todayInterval = Date().timeIntervalSince1970
        if isToday(dateString) {
            return false
        }
        return timeInterval < todayInterval
    }
```

#### 4. 如何避免两位数年份产生的世纪判断错误

```swift
 let fmt = DateFormatter()
 fmt.twoDigitStartDate = Calendar.current.date(from: DateComponents(year: 2000))

这样则能保证 输入50-03-03 不会被认为是1950年 而是2050年
        
```
