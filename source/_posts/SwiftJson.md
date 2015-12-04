title: SwiftyJSON 技术分析
date: 2014-11-10 06:08:34
tags:
---

在 OC 中，得益于开源社区的努力，我们可以很愉快的使用 JSON 格式的数据进行各种功能实现。
但是在 Swift 中，JSON的使用成为了一件痛苦的事情，如下

```Swift
let jsonObject : AnyObject! = NSJSONSerialization.JSONObjectWithData(dataFromTwitter, options: NSJSONReadingOptions.MutableContainers, error: nil)
if let statusesArray = jsonObject as? NSArray{
    if let aStatus = statusesArray[0] as? NSDictionary{
        if let user = aStatus["user"] as? NSDictionary{
            if let userName = user["name"] as? NSDictionary{
                //Finally We Got The Name

            }
        }
    }
}
```

由于可选值类型的出现，再也不能像以前那样愉快的玩耍了 :(
你也可以用可选值链 optional chain 来进行操作，但是这样一来，代码基本不能读了


```Swift
let jsonObject : AnyObject! = NSJSONSerialization.JSONObjectWithData(dataFromTwitter, options: NSJSONReadingOptions.MutableContainers, error: nil)
if let userName = (((jsonObject as? NSArray)?[0] as? NSDictionary)?["user"] as? NSDictionary)?["name"]{
  //What A disaster above
}
```

但是开发者都是很聪明的，很快就有了 Swift 版本的开源库 SwiftJSON
https://github.com/SwiftyJSON/SwiftyJSON
透过 SwiftJSON 代码可能是这样的 

```Swift
let json = JSON(data: dataFromNetworking)
if let userName = json[0]["user"]["name"].string{
  //就这么简单取到了。
}
```

其中，作者重写了所有的下标方法，不论怎么玩都不会崩溃了，妈妈再也不担心我被服务端API绑架了 :)




