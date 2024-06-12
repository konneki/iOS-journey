# For loop

Loops can be created on arrays, sets, dictionaries or a range and iterate on each item.

```swift
let platforms = ["iOS", "macOS", "tvOS", "watchOS"]

for os in platforms {
    // anything inside braces is called loop body
    print("SwiftUI works great on \(os)!")
}
// SwiftUI works great on iOS
// SwiftUI works great on macOS
// SwiftUI works great on tvOS
// SwiftUI works great on watchOS
```

A range example, when the number of iterations is specified by the length between numbers

```swift
// range from 1 through 5 including 1 and 5
for i in 1...5 {
    print("2 x \(1) is \(2 * i)")
}

// range from 1 up to 5 starting with 1 and excluding 5
for i in 1..<5 {
    print("2 x \(1) is \(2 * i)")
}
```

You can create nested loops (loops inside loops) if needed. You may also create a loop but don't need loop variable. To do so use `_` instead of a loop variable

```swift
for _ in 1...5 {
    print("yo")
}
```
