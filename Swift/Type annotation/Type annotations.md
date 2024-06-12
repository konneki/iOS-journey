# Type annotations

Swift is able to figure out what type of data a constant or variable holds based on what we assign to it. However, sometimes we don’t want to assign a value immediately, or sometimes we want to override Swift’s choice of type, and that’s where type annotations come in.

Normally when you create variables like so:

```swift
var name = "Scooby Doo"
var number = 0
```

Swift in the background assign the type which is the closest to the type used when initiating it, and that is called _type inference_.

Sot the below is exactly the same for Swift:

```swift
var name: String = "Scooby Doo"
var number: Int = 0
```

But what if you'd like to have your `number` variable set as a `Double`?

```swift
var number: Double = 0
```

You can create a constant that doesn’t have a value just yet, later on provide that value, and Swift will ensure we don’t accidentally use it until a value is present. It will also ensure that you only ever set the value once, so that it remains constant.

```swift
let number: Int
// lots of logic
number = 0
// lots of logic
print(number) // 0
```

**Important** although Swift can let us override Swift’s type inference to a degree, our finished code must still be possible. For example, this is not allowed:

```swift
let score: Int = "Zero"
```
