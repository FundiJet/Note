## Question

之前使用 OC 中，可以将 block 直接添加到 NSDictionary 中，但是 Swift 中执行类似添加却报错：

**Cannot assign value of type 'XXX'  to type 'AnyObject?'**

-----

### Answer

##### 解决办法之一

```swift
typealias CompletionHandler = @convention(block) () -> Void
```

那么存储时，可以使用如下代码：

```swift
var dict = [String: AnyObject]()
// 假设我们传入的 CompletionHandler 类型的参数名为 completionCallback
let completionAction: AnyObject = unsafeBitCast(completionCallback, AnyObject.self)
dict["Completed"] = completionAction
```

取值时：

```swift
let completionAction: AnyObject = dict["Completed"]
let completionCallback = unsafeBitCast(completionAction, CompletionHandler.self)
completionCallback()
```

其实解决办法有很多种，我们还可以封装一层，比如新建一个对象，通过此对象持有闭包，然后存储此对象。

```swift
// 借助于 <T> 还可以扩展一些其他的功能。
class Callback<T> {
  
}
```
