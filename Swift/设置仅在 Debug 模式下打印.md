#### Test in Swift 2.2, Xcode 7.3

首先看看最终的代码，仅在 Debug 模式下打印：

```swift
func debugPrint(object: Any) {
    #if DEBUG
        Swift.print(object)
    #endif
}
```

为了能够达到我们的目的，我们只需要如下图进行一点额外的设置：

![screen](https://cloud.githubusercontent.com/assets/16789361/20346181/a8679310-ac35-11e6-9190-f109aaf28487.png)

在对应处添加 `-DDEBUG` 即可。

-----

#### 苹果文档中关于 Build Configurations 的描述

> Swift code and Objective-C code are conditionally compiled in different ways. Swift code can be conditionally compiled based on the evaluation of *build configurations*. Build configurations include the literal `true` and`false` values, command line flags, and the platform and language-version testing functions listed in the table below. You can specify command line flags using `-D <#flag#>`.