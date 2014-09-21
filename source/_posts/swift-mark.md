title: Swift 中的标记
date: 2014-09-22 01:27:10
tags: XCode
---

在 Objective-C 中 我们使用 #pragma mark 来分割代码，并且使IDE支持跳转

```Objective-C
#pragma mark – Initialization 
code here...
 
#pragma mark – Table Management
more code here...
```

![xcode-mark](/img/xcode-mark.png)

那么在 Swift 中我们如何实现类似的效果呢？
答案是 // MARK: 这个 XCode6 新引入的特性

```Swift
// MARK: - Initialization
code here...
 
// MARK: - View Management
more code here...

```

在 Swift 中我们同样也可以使用 // TODO: 以及 // FIXME:

```Swift
override func viewDidLoad()
{
  super.viewDidLoad()
 
  // TODO: add configuration code
  self.configureView()
}
```

![xcode-todo](/img/xcode-todo.png)

```
override func tableView(tableView: UITableView, cellForRowAtIndexPath indexPath: NSIndexPath) -> UITableViewCell
{
  // FIXME: bug 2102
  let cell = tableView.dequeueReusableCellWithIdentifier("Cell", forIndexPath: indexPath) as UITableViewCell
  let object = objects[indexPath.row] as NSDate
  cell.textLabel.text = object.description
  return cell
}
```

![xcode-fixme](/img/xcode-fixme.png)