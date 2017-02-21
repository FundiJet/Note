使用 UISearchController 时往往会碰到一个警告⚠️ ：

Attempting to load the view of a view controller while it is deallocating is not allowed and may result in undefined behavior (<UISearchController: 0x7fbe0d47cf20>)



解决办法也很简单，在 ViewController 的对应方法中调用一次 `view`：

```swift
class MyViewController: UIViewController {
	private func configureSearchController() {
		let searchController = UISearchController(searchResultsController: nil)
		// Configure UISearchController Code
	
		let _ = searchController.view
	}

	deinit {
		let _ = searchController.view
	}
}
```

再手动调用 searchController 的 view 之后，警告就不会再出现了。