---
title: Trigger UIAlertAction programmatically
date: 2020-05-18 15:53:37
tags: 
- iOS 
- Swift
---

Sometimes you write unit testing, and have to trigger UIAlertAction without user interface.
then just do following:

Extension
```swift
extension UIAlertAction {
    typealias AlertHandler = @convention(block) (UIAlertAction) -> Void
    func trigger() {
        guard let block = value(forKey: "handler") else {
            XCTFail("Should not be here")
            return
        }
        let handler = unsafeBitCast(block as AnyObject, to: AlertHandler.self)
        handler(self)
    }
}
```

Usage
```swift
let alertAction = UIAlertAction(title: "test", style: .default) { (action) in
    print("action triggered!")
}
alertAction.trigger()
```
