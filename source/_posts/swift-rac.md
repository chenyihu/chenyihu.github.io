title: 在 Swift 项目中集成 ReactiveCocoa (一)
date: 2014-09-28 18:07:27
tags: Swift ReactiveCocoa

---

## 安装 ReactiveCocoa 

最简单的就是使用 Cocoapods 了。

```
pod 'ReactiveCocoa'
```

但是由于 ReactiveCocoa 目前尚未对 Swift 做原生支持，所以我们需要使用桥接的模式来使用。把 #import <ReactiveCocoa/ReactiveCocoa.h> 放入 Briging Header。然后我们就可以开心的使用啦。

## Sequence

ReactiveCocoa 对 ObjC 容器类提供了 Sequence 扩展。如下

```Objective-C
RACSignal *letters = [@"A B C D E F G H I" componentsSeparatedByString:@" "].rac_sequence.signal;

// 依次输出 A B C D…
[letters subscribeNext:^(NSString *x) {
    NSLog(@"%@", x);
}];
```

但是到了 Swift 世界，Array 和 NSArray 就分道扬镳了，虽然可以相互转换，但是 NSArray 的扩展，Array 是不能用的，所以就有了下面的代码

```Swift
    var signal = NSArray(array: "A B C D".componentsSeparatedByString(" ")).rac_sequence.signal()
    signal.subscribeNext { (a) -> Void in
        println(a)
    }
```

## Signals

由于 Signal 在 OC 中一直返回的是 id 类型，如下

```Objective-C
[self.signal subscribeNext:^(id x) {
  NSString *text = (NSString *)x;
  NSLog(text);
}];
```

id 类型在 OC 中可谓是万能类型，可以随意的转换和发消息。
但是 Swift 号称类型安全，就不能这样了，要实现上面的代码，在 Swift 中可能是这样的

```Swift

self.signal.subscribeNext {
  (next:AnyObject!) -> () in
  if let text = next as? String {
    println(countElements(text))
  }
}

```

但我们的武器箱里又多了一把武器，泛型。

通过泛型，我们实现一个 RACSignal 的扩展，让其支持任意类型的转换。

```Swift
extension RACSignal {  
  func subscribeNextAs<T>(nextClosure:(T) -> ()) -> () {
    self.subscribeNext {
      (next: AnyObject!) -> () in
      let nextAsT = next as T
      nextClosure(nextAsT)
    }
  }
}
```

使用 subscribeNextAs 我们甚至可以写出比 OC 更好的代码

```
self.signal.subscribeNextAs {
  (text:String) -> () in
  println(countElements(text))
}
```

今天到这里，未完待续...









