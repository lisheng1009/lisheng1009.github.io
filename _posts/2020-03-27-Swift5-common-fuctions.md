---
title: Swift5-common-fuctions
categories:
 - Swift5
tags:
---

#### 根据类名转化成类
```swift
//第一步：获取项目名称
        let clsName = Bundle.main.infoDictionary!["CFBundleExecutable"] as? String 
        print(clsName!)
        //第二步：拼接参数字符串 项目名+.+需要加载的类的名称
        let vcView = NSClassFromString(clsName! + "." + "PGCompanyListViewController") as! PGCompanyListViewController.Type
        //第三步：调用初始化方法
        let vc = vcView.init()
        //第四步执行跳转
        self.navigationController?.pushViewController(vc, animated: true)
```        
#### 单例
```swift
 public class FileManager {
    public static let shared = {
        // ....
        // ....
        return FileManager()
     }()
    private init() { }
}
```

#### 用户偏好设置
```swift
//存
    func setUserDefault(){
        UserDefaults.standard.set("wtf", forKey: "today")
    }
//取
    func getUserDefault(){
        let s = UserDefaults.standard.string(forKey: "today")
        print(s!)
    }
```

#### plist存取
```swift
存:
var numberList:[String] = ["1","2","3","4"]
let putData: NSDictionary = ["List": numberList]
        let manager = FileManager.default
        let documentDir = manager.urls(for: .documentDirectory, in: .userDomainMask)[0]
let url = documentDir.appendingPathComponent("order.plist")
putData.write(to:url, atomically: true)

取:
let manager = FileManager.default
             let documentDir = manager.urls(for: .documentDirectory, in: .userDomainMask)[0]
             let url = documentDir.appendingPathComponent("order.plist")
             let plist = NSDictionary(contentsOfFile: url.path)
             if let order = plist?["List"] {
                 if (order as AnyObject).count != 0{
                     
                 }
             }
```


#### 归档解档
```swift
类中: 
class PersonModel: NSObject,Codable {
    var firstName: String
    var lastName: String
    var age: Int
    
    // 如果定义一个实例Person，打印结果将是这里定义的描述字符串
    var descirption: String {
        return "\(self.firstName) \(self.lastName) \(age)"
    }
    
    init(firstName: String, lastName: String, age: Int) {
        self.firstName = firstName
        self.lastName = lastName
        self.age = age
    }
}
操作:
class ViewController: UIViewController {
    var filePath : URL {
        let manager = FileManager.default
        let url = manager.urls(for: .documentDirectory, in: .userDomainMask).first! as NSURL
        return url.appendingPathComponent("objects")!
    }
    
    override func viewDidLoad() {
        super.viewDidLoad()
//        encode()
        decode()
    }
    
    //归档
    func encode(){
        let person = PersonModel(firstName: "yuuki", lastName: "lili", age: 20)
        let encodedPerson = try? PropertyListEncoder().encode(person) // 编码
        // 写入到该文件
        try! encodedPerson?.write(to: self.filePath, options: .atomic)
    }
    
    //解档
    func decode(){
        let contents = (try? Data(contentsOf: filePath))!
        let decodedPerson = try? PropertyListDecoder().decode(PersonModel.self, from: contents)
        print(decodedPerson?.firstName ?? "nonono")
    }
}
```
#### app之间跳转(DemoB->DemoA)
```swift
1.  添加scheme。  target-> info-> URL Types -> URL Scheme写"DemoB"
2. plist.info 直接添加一个 "LSApplicationQueriesSchemes" 字段  type为Array   item中写入"DemoA"
3. 代码
 let urlString = "DemoA://"
        let url = URL(string: urlString)
        if UIApplication.shared.canOpenURL(url!) {
            UIApplication.shared.open(url!)
        }else{
            
        }
```
跳转传值
<https://qiita.com/MASATO_SSS/items/734da388c9e8f135911e>

#### 播放本地音频
```swift
var audioPlayer = AVAudioPlayer()  注意: 必须设置为全局变量

    func playSuccessAudio() {
        let path = Bundle.main.path(forResource: "SEgood_20200212", ofType: "mp3")
        let soundUrl = URL(fileURLWithPath: path!)
        
        do {
            audioPlayer = try AVAudioPlayer(contentsOf: soundUrl)
            audioPlayer.play()
        }
        catch let error as NSError {
            print(error)
        }
    }
音频文件放在最外文件夹即可
```

#### hex颜色, 16进制颜色
```swift
/// Hex String -> UIColor
extension UIColor {
       convenience init(hexString: String) {
           let hexString = hexString.trimmingCharacters(in: .whitespacesAndNewlines)
           let scanner = Scanner(string: hexString)
            
           if hexString.hasPrefix("#") {
               scanner.scanLocation = 1
           }
            
           var color: UInt32 = 0
           scanner.scanHexInt32(&color)
            
           let mask = 0x000000FF
           let r = Int(color >> 16) & mask
           let g = Int(color >> 8) & mask
           let b = Int(color) & mask
            
           let red   = CGFloat(r) / 255.0
           let green = CGFloat(g) / 255.0
           let blue  = CGFloat(b) / 255.0
            
           self.init(red: red, green: green, blue: blue, alpha: 1)
    }
}


or

extension UIColor {
    class func hex (hexStr : NSString, alpha : CGFloat) -> UIColor {
        let str = hexStr.replacingOccurrences(of: "#", with: "")
        let scanner = Scanner(string: str as String)
        var color: UInt32 = 0
        if scanner.scanHexInt32(&color) {
            let r = CGFloat((color & 0xFF0000) >> 16) / 255.0
            let g = CGFloat((color & 0x00FF00) >> 8) / 255.0
            let b = CGFloat(color & 0x0000FF) / 255.0
            return UIColor(red:r,green:g,blue:b,alpha:alpha)
        } else {
            konsole.log("不正な値だよ")
            return UIColor.white
        }
    }
}

```
#### 检查是否接入网络

```swift

import SystemConfiguration

func isInternetAvailable() -> Bool
{
    var zeroAddress = sockaddr_in()
    zeroAddress.sin_len = UInt8(MemoryLayout.size(ofValue: zeroAddress))
    zeroAddress.sin_family = sa_family_t(AF_INET)
    
    let defaultRouteReachability = withUnsafePointer(to: &zeroAddress) {
        $0.withMemoryRebound(to: sockaddr.self, capacity: 1) {zeroSockAddress in
            SCNetworkReachabilityCreateWithAddress(nil, zeroSockAddress)
        }
    }
    
    var flags = SCNetworkReachabilityFlags()
    if !SCNetworkReachabilityGetFlags(defaultRouteReachability!, &flags) {
        konsole.log("** netowrk unreachable.")
        return false
    }
    let isReachable = flags.contains(.reachable)
    let needsConnection = flags.contains(.connectionRequired)
    return (isReachable && !needsConnection)
}

```


#### scheme

#### framework导入

#### 最基础网络请求
