Objc.io 的《[Functional swift](https://www.objc.io/books/functional-swift/)》在寥寥翻页之后，便被束之高阁，此次翻出，认真过一遍，争取把书读“薄”。

老实说，在 OC 写多了之后，甫一接触函数式编程的思想，看着众多文章，感觉有些云里雾里，抓不住什么收获，而在看完“ThinkingFunctionally”这一章节之后，我的理解是：

函数式编程是对逻辑的抽象，更准确点说是对数学运算的抽象，将抽象出来的结果作为数据。

书中章节的开头利用船的攻击范围引出了对图形的探讨，以下记录的是我在阅读过程中的思考。

```swift
typealias Distance = Double
struct Position {
  var x: Double
  var y: Double
}

extension Position {
  func minus(p: Position) -> Position {
    return Position(x: x - p.x, y: y - p.y)
  }
  
  var length: Double {
    return sqrt(x * x + y * y)
  }
}
```

文章中对一个函数类型起了别名：Region

```swift
typealias Region = Position -> Bool
```

### Circle 函数

作为引子，文章定义了第一个区域：

```swift
func circle(radius: Distance) -> Region {
  return { point in point.length <= radius }
}
```

让我们看看`circle`函数，将上诉函数的运算结果图形化。

根据传入的 radius，将所有符合返回值 Region 的 point 绘制出来，结果如下图：

![circle](http://ocpmjsr1z.bkt.clouddn.com/circle.png)

之后，文章定义了一个新的函数：

### Circle2 函数

```swift
func circle2(radius: Distance, center: Position) -> Region {
  return { point in point.minus(center).length <= radius }
}
```

根据`circle2`返回的 Region，将所有符合 Region 的 point 绘制出来，结果如下图：

![circle2](http://ocpmjsr1z.bkt.clouddn.com/circle2.png)

此时，比较`circle`与`circle2`，我们就会发现，如果传入的 radius 相同，它们返回的 Region，最终结果只是偏离量不同，直观些的描述就是两者圆点位置不同。

由此，我们可以思考一下`circle`与`circle2`的关系：

两者的不同仅在于**偏移**，除此之外，代码的数学运算逻辑都是相同的。也就是说，如果能够设置偏移量，构造出一个能使 Region 变换位置的函数，完全可以由`circle`得出`circle2`，甚至可以应对矩形等其他图形。

### Shift 函数

《Functional Swift》给出了**区域变换函数**：

```swift
func shift(region: Region, offset: Position) -> Region {
  return { point in region(point.minus(offset)) }
}
```

调用`shift(region, offset: offset)`函数会将区域向右上方移动，偏移量分别是 offset.x 和 offset.y。

`shift(region, offset: offset)`函数的返回值是一个 Region 类型的函数：一个传参为 Position 并返回 Bool 的函数。为此，我们需要写一个闭包，把这个闭包作为返回值。

这个闭包接受我们要检验的点，这个点减去偏移量之后我们得到一个新的点。最后，我们将新点作为参数传递给 region 函数。

是不是有点绕？想象成是数学的抽象，或许更容易理解一点。

`circle`的返回值可以理解成是“坐标系内，判断一个点是否在以原点为圆心的圆内”的数学运算逻辑，那么要实现`circle2`的效果，只需要这样：

```swift
let radius = 10
let offset = Position(x: 5, y: 5)
// circle2(radius, offset)
shift(circle(radius), offset: offset)
```

### 类似 Shift 的函数

在理解上面内容之后，我们就可以推导出其他类似功能的函数。

例如，我们想要通过反转一个区域以定义另外一个区域，这个新产生的区域由原区域以外的的所有点组成：

```swift
func invert(region: Region) -> Region {
  return { point in !region(point) }
}
```

比如，我们可以计算参数中两个区域的交集和并集：

```swift
// 交集
func intersection(region1: Region, _ region2: Region) -> Region {
 return { point in region1(point) && region2(point) } 
}

// 并集
func union(region1: Region, _ region2: Region) -> Region {
  return { point in region1(point) || region2(point) }
}
```

当然，我们更加可以利用这些函数定义更加丰富的区域，如在第一个区域中且不在第二个区域中的点构建成一个新的区域，这个函数实现起来如下：

```swift
func difference(region: Region, minus: Range) -> Range {
  return intersection(region, invert(minus))
}
```

### 总结

通读下来，我认为函数式编程的外在表现是一个一个小函数组成了更大的函数，函数的参数是函数，返回值也是函数，最后，只需要用数据使这些函数运作起来。