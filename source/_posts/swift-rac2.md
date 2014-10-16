title: 在 Swift 项目中集成 ReactiveCocoa (二)
date: 2014-10-12 20:38:54
tags:
---

## Signals

当然，类似于 subscribeNextAs，其它一些操作的 Swift 版本

```Swift
extension RACSignal {
  
  func mapAs<T: AnyObject, U: AnyObject>(mapClosure:(T) -> U) -> RACSignal {
    return self.map {
      (next: AnyObject!) -> AnyObject! in
      let nextAsT = next as T
      return mapClosure(nextAsT)
    }
  }
  
  func filterAs<T: AnyObject>(filterClosure:(T) -> Bool) -> RACSignal {
    return self.filter {
      (next: AnyObject!) -> Bool in
      let nextAsT = next as T
      return filterClosure(nextAsT)
    }
  }
  
  func doNextAs<T: AnyObject>(nextClosure:(T) -> ()) -> RACSignal {
    return self.doNext {
      (next: AnyObject!) -> () in
      let nextAsT = next as T
      nextClosure(nextAsT)
    }
  }
}
```

Signal 之间可以联动，如下 

```Objective-C
RAC(self.submitButton.enabled) = [RACSignal combineLatest:@[self.usernameField.rac_textSignal, self.passwordField.rac_textSignal] reduce:^id(NSString *userName, NSString *password) {
    return @(userName.length >= 6 && password.length >= 6);
}];
```

但是 Swift 中不能使用复杂的宏，所以需要把宏转换为一个结构体

```Swift
struct RAC  {
  var target : NSObject!
  var keyPath : String!
  var nilValue : AnyObject!
  
  init(_ target: NSObject!, _ keyPath: String, nilValue: AnyObject? = nil) {
    self.target = target
    self.keyPath = keyPath
    self.nilValue = nilValue
  }
  
  
  func assignSignal(signal : RACSignal) {
    signal.setKeyPath(self.keyPath, onObject: self.target, nilValue: self.nilValue)
  }
}

infix operator ~> {}
func ~> (signal: RACSignal, rac: RAC) {
  rac.assignSignal(signal)
}
```

这样我们就可以很容易写出 Swift 的 RAC 版本

```Swift
searchTextField.rac_textSignal() ~> RAC(viewModel, "searchText")
```

Combining latest values

```Swift
class RACSignalEx {
  class func combineLatestAs<T, U, R: AnyObject>(signals:[RACSignal], reduce:(T,U) -> R) -> RACSignal {
    return RACSignal.combineLatest(signals).mapAs {
      (tuple: RACTuple) -> R in
      return reduce(tuple.first as T, tuple.second as U)
    }
  }
}
```

```Swift
RACSignalEx.combineLatestAs([favouritesSignal, commentsSignal]) {
      (favourites:NSString, comments:NSString) -> FlickrPhotoMetadata in
      return FlickrPhotoMetadata(favourites: favourites.integerValue, comments: comments.integerValue)
    }
```

RACObserve 监听属性的改变，使用block的KVO

```Swift
func RACObserve(target: NSObject!, keyPath: String) -> RACSignal  {
  return target.rac_valuesForKeyPath(keyPath, observer: target)
}
```

```Swift
let validSearchSignal = RACObserve(self, "searchText").mapAs {
    (text: NSString) -> NSNumber in
    return text.length > 3
  }.distinctUntilChanged();
```