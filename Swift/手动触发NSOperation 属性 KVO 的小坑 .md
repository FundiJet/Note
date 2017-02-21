## ⚠️ 注意

使用 Swift 自定义 NSOperation 子类时，触发的 KeyPath 仍然是 isFinished、isExecuting 等 `get` 方法名。

## 一个小坑：

自定义并发的 NSOperation 子类时，需要手动触发 KVO，在 OC 中，使用类似如下的代码：

```objective-c
[self willChangeValueForKey:@"isFinished"];
__finish = YES;
[self didChangeValueForKey:@"isFinished"];
```

截取 NSOperation 的头文件中的部分，我们可以看到如下定义：

```objective-c
@interface NSOperation : NSObject {
@private
    id _private;
    int32_t _private1;
#if __LP64__
    int32_t _private1b;
#endif
}

- (void)start;
- (void)main;

@property (readonly, getter=isCancelled) BOOL cancelled;
- (void)cancel;

@property (readonly, getter=isExecuting) BOOL executing;
@property (readonly, getter=isFinished) BOOL finished;
@property (readonly, getter=isConcurrent) BOOL concurrent; // To be deprecated; use and override 'asynchronous' below
@property (readonly, getter=isAsynchronous) BOOL asynchronous NS_AVAILABLE(10_8, 7_0);
@property (readonly, getter=isReady) BOOL ready;
```

相对应的，在 Swift 中，头文件如下声明：

```swift
public class NSOperation : NSObject {
    
    public func start()
    public func main()
    
    public var cancelled: Bool { get }
    public func cancel()
    
    public var executing: Bool { get }
    public var finished: Bool { get }
    public var concurrent: Bool { get } // To be deprecated; use and override 'asynchronous' below
    @available(iOS 7.0, *)
    public var asynchronous: Bool { get }
    public var ready: Bool { get }
```

因此，我想当然的在 Swift 代码中使用了如下代码，结果就是付出了半天的调试时间！

```swift
willChangeValueForKey("finished")
__finished = true
didChangeValueForKey("finished")
```

-----

#### Test in Swift 2.2