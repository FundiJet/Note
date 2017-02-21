关于 Core Animation 的东西，《[iOS Core Animation: Advanced Techniques](https://www.gitbook.com/book/zsisme/ios-/details)》基本都涵盖了。这本书也是两年前读的，只可惜大部分内容当时只是短期记忆。最近工作中需要做动画相关的东西，涉及到隐式动画，又重新翻出，仅作记录。

### 隐式动画

Core Animation 认为屏幕上的东西都可以进行动画，所以动画并不需要我们显式地去声明开启，相反，我们需要显式地声明关闭。

作为内容的载体，CALayer 的很多属性都具备动画特性，当属性改变的时候，会自动平滑过渡属性值的变动，这种行为也就被称为“隐式动画”。

以下是在开发文档中找出的，会进行隐式动画的 CALayer 的属性。

| **Property**                             | **Default animation**                    |
| ---------------------------------------- | ---------------------------------------- |
| [anchorPoint](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/CALayer_class/index.html#//apple_ref/occ/instp/CALayer/anchorPoint) | Uses the default implied [CABasicAnimation](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/CABasicAnimation_class/index.html#//apple_ref/occ/cl/CABasicAnimation) object, described in Table B-2. |
| [backgroundColor](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/CALayer_class/index.html#//apple_ref/occ/instp/CALayer/backgroundColor) | Uses the default implied [CABasicAnimation](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/CABasicAnimation_class/index.html#//apple_ref/occ/cl/CABasicAnimation) object, described in Table B-2. |
| [backgroundFilters](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/CALayer_class/index.html#//apple_ref/occ/instp/CALayer/backgroundFilters) | Uses the default implied [CATransition](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/CATransition_class/index.html#//apple_ref/occ/cl/CATransition) object, described in Table B-3. Sub-properties of the filters are animated using the default implied CABasicAnimation object, described in Table B-2. |
| [borderColor](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/CALayer_class/index.html#//apple_ref/occ/instp/CALayer/borderColor) | Uses the default implied [CABasicAnimation](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/CABasicAnimation_class/index.html#//apple_ref/occ/cl/CABasicAnimation) object, described in Table B-2. |
| [borderWidth](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/CALayer_class/index.html#//apple_ref/occ/instp/CALayer/borderWidth) | Uses the default implied [CABasicAnimation](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/CABasicAnimation_class/index.html#//apple_ref/occ/cl/CABasicAnimation) object, described in Table B-2. |
| [bounds](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/CALayer_class/index.html#//apple_ref/occ/instp/CALayer/bounds) | Uses the default implied [CABasicAnimation](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/CABasicAnimation_class/index.html#//apple_ref/occ/cl/CABasicAnimation) object, described in Table B-2. |
| [compositingFilter](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/CALayer_class/index.html#//apple_ref/occ/instp/CALayer/compositingFilter) | Uses the default implied [CATransition](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/CATransition_class/index.html#//apple_ref/occ/cl/CATransition) object, described in Table B-3. Sub-properties of the filters are animated using the default implied CABasicAnimation object, described in Table B-2. |
| [contents](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/CALayer_class/index.html#//apple_ref/occ/instp/CALayer/contents) | Uses the default implied [CABasicAnimation](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/CABasicAnimation_class/index.html#//apple_ref/occ/cl/CABasicAnimation) object, described in Table B-2. |
| [contentsRect](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/CALayer_class/index.html#//apple_ref/occ/instp/CALayer/contentsRect) | Uses the default implied [CABasicAnimation](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/CABasicAnimation_class/index.html#//apple_ref/occ/cl/CABasicAnimation) object, described in Table B-2. |
| [cornerRadius](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/CALayer_class/index.html#//apple_ref/occ/instp/CALayer/cornerRadius) | Uses the default implied [CABasicAnimation](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/CABasicAnimation_class/index.html#//apple_ref/occ/cl/CABasicAnimation) object, described in Table B-2. |
| [doubleSided](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/CALayer_class/index.html#//apple_ref/occ/instp/CALayer/doubleSided) | There is no default implied animation.   |
| [filters](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/CALayer_class/index.html#//apple_ref/occ/instp/CALayer/filters) | Uses the default implied [CABasicAnimation](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/CABasicAnimation_class/index.html#//apple_ref/occ/cl/CABasicAnimation) object, described in Table B-2. Sub-properties of the filters are animated using the default implied CABasicAnimation object, described in Table B-2. |
| [frame](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/CALayer_class/index.html#//apple_ref/occ/instp/CALayer/frame) | This property is not animatable. You can achieve the same results by animating the [bounds](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/CALayer_class/index.html#//apple_ref/occ/instp/CALayer/bounds) and [position](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/CALayer_class/index.html#//apple_ref/occ/instp/CALayer/position) properties. |
| [hidden](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/CALayer_class/index.html#//apple_ref/occ/instp/CALayer/hidden) | Uses the default implied [CABasicAnimation](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/CABasicAnimation_class/index.html#//apple_ref/occ/cl/CABasicAnimation) object, described in Table B-2. |
| [mask](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/CALayer_class/index.html#//apple_ref/occ/instp/CALayer/mask) | Uses the default implied [CABasicAnimation](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/CABasicAnimation_class/index.html#//apple_ref/occ/cl/CABasicAnimation) object, described in Table B-2. |
| [masksToBounds](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/CALayer_class/index.html#//apple_ref/occ/instp/CALayer/masksToBounds) | Uses the default implied [CABasicAnimation](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/CABasicAnimation_class/index.html#//apple_ref/occ/cl/CABasicAnimation) object, described in Table B-2. |
| [opacity](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/CALayer_class/index.html#//apple_ref/occ/instp/CALayer/opacity) | Uses the default implied [CABasicAnimation](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/CABasicAnimation_class/index.html#//apple_ref/occ/cl/CABasicAnimation) object, described in Table B-2. |
| [position](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/CALayer_class/index.html#//apple_ref/occ/instp/CALayer/position) | Uses the default implied [CABasicAnimation](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/CABasicAnimation_class/index.html#//apple_ref/occ/cl/CABasicAnimation) object, described in Table B-2. |
| [shadowColor](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/CALayer_class/index.html#//apple_ref/occ/instp/CALayer/shadowColor) | Uses the default implied [CABasicAnimation](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/CABasicAnimation_class/index.html#//apple_ref/occ/cl/CABasicAnimation) object, described in Table B-2. |
| [shadowOffset](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/CALayer_class/index.html#//apple_ref/occ/instp/CALayer/shadowOffset) | Uses the default implied [CABasicAnimation](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/CABasicAnimation_class/index.html#//apple_ref/occ/cl/CABasicAnimation) object, described in Table B-2. |
| [shadowOpacity](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/CALayer_class/index.html#//apple_ref/occ/instp/CALayer/shadowOpacity) | Uses the default implied [CABasicAnimation](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/CABasicAnimation_class/index.html#//apple_ref/occ/cl/CABasicAnimation) object, described in Table B-2. |
| [shadowPath](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/CALayer_class/index.html#//apple_ref/occ/instp/CALayer/shadowPath) | Uses the default implied [CABasicAnimation](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/CABasicAnimation_class/index.html#//apple_ref/occ/cl/CABasicAnimation) object, described in Table B-2. |
| [shadowRadius](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/CALayer_class/index.html#//apple_ref/occ/instp/CALayer/shadowRadius) | Uses the default implied [CABasicAnimation](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/CABasicAnimation_class/index.html#//apple_ref/occ/cl/CABasicAnimation) object, described in Table B-2. |
| [sublayers](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/CALayer_class/index.html#//apple_ref/occ/instp/CALayer/sublayers) | Uses the default implied [CABasicAnimation](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/CABasicAnimation_class/index.html#//apple_ref/occ/cl/CABasicAnimation) object, described in Table B-2. |
| [sublayerTransform](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/CALayer_class/index.html#//apple_ref/occ/instp/CALayer/sublayerTransform) | Uses the default implied [CABasicAnimation](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/CABasicAnimation_class/index.html#//apple_ref/occ/cl/CABasicAnimation) object, described in Table B-2. |
| [transform](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/CALayer_class/index.html#//apple_ref/occ/instp/CALayer/transform) | Uses the default implied [CABasicAnimation](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/CABasicAnimation_class/index.html#//apple_ref/occ/cl/CABasicAnimation) object, described in Table B-2. |
| [zPosition](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/CALayer_class/index.html#//apple_ref/occ/instp/CALayer/zPosition) | Uses the default implied [CABasicAnimation](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/CABasicAnimation_class/index.html#//apple_ref/occ/cl/CABasicAnimation) object, described in Table B-2. |

几乎所有属性的隐式动画效果是由 CABasicAnimation 实现的，动画时长 0.25 秒。

让我们来看看隐式动画生效的过程：

- 图层首先检测它是否有委托，并且是否实现`CALayerDelegate`协议指定的`-actionForLayer:forKey`方法。如果有，直接调用并返回结果。
- 如果没有委托，或者委托没有实现`-actionForLayer:forKey`方法，图层接着检查包含属性名称对应行为映射的 **actions** 字典。
- 如果 **actions** 字典没有包含对应的属性，那么图层接着在它的 **style** 字典接着搜索属性名。
- 最后，如果在 **style** 里面也找不到对应的行为，那么图层将会直接调用定义了每个属性的标准行为的`-defaultActionForKey:`方法。

所以一轮完整的搜索结束之后，`-actionForKey:`要么返回空（这种情况下将不会有动画发生），要么是 CAAction 协议对应的对象，最后 CALayer 拿这个结果去对先前和当前的值做动画。

### 禁用隐式动画

以上的过程也就是 UIKit 禁用隐式动画的方式所在：UIView 作为关联的 Root Layer 的委托，实现了`-actionForLayer:forKey`方法。当不在动画的代码块中，所有的行为都返回 nil，否则返回一个非空值。

当然返回 nil 并不是禁用隐式动画的唯一办法，CATransacition 有个方法叫做 `+setDisableActions:`，也可以用来对所有属性打开或者关闭隐式动画。

### 实际运用

在工作中，动画的效果是由 CAShapeLayer 实现的，所以我通过方式禁用了相关属性的隐式动画：

```swift
layer.actions = [
                "strokeStart": NSNull(),
                "strokeEnd": NSNull(),
                "transform": NSNull()
            ]
```

当动画时，再通过 CABasicAnimetion 重新实现相关属性的动画，并添加的 layer 上：

```swift
let strokeStart = CABasicAnimation(keyPath: "strokeStart")
strokeStart.toValue = kAnimationToValue
...
layer.addAnimation(strokeStart, forKey:strokeStart.keyPath!)
```