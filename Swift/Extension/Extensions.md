# Extensions

Extensions let us add functionality to any type, whether we created it or someone else created it – even one of Apple’s own types.

Normally we would add a code like this:

```swift
var quote = "   The truth is rarely pure and never simple   "
let trimmed = quote.trimmingCharacters(in: .whitespacesAndNewlines)
```

The **.whitespacesAndNewlines** value comes from Apple’s Foundation API btw.

Instead of doing it like above we can create an extension to the String type, like so:

```swift
extension String {
    func trimmed() -> String {
        self.trimmingCharacters(in: .whitespacesAndNewlines)
    }
}
```

Benefits of using extensions:

1. When you type **quote.** Xcode brings up a list of methods on the string, including all the ones we add in extensions. This makes our extra functionality easy to find.
2. Writing global functions makes your code rather messy – they are hard to organize and hard to keep track of. On the other hand, extensions are naturally grouped by the data type they are extending.
3. Because your extension methods are a full part of the original type, they get full access to the type’s internal data. That means they can use properties and methods marked with **private** access control, for example.

We wrote a **trimmed()** method that returns a new string with whitespace and newlines removed, but if we wanted to modify the string directly we could add this to the extension:

```swift
mutating func trim() {
    self = self.trimmed()
}

quote.trim()
```

Notice how the method has slightly different naming now: when we return a new value we used **trimmed()**, but when we modified the string directly we used **trim()**. This is intentional, and is part of Swift’s design guidelines: if you’re returning a new value rather than changing it in place, you should use word endings like **ed** or **ing**, like **reversed()**.

You can also use extensions to add _properties_ to types, but there is one rule: they must only be _computed_ properties, not stored properties. The reason for this is that adding new stored properties would affect the actual size of the data types – if we added a bunch of stored properties to an integer then every integer everywhere would need to take up more space in memory, which would cause all sorts of problems.

```swift
var lines: [String] {
    self.components(separatedBy: .newlines)
}
```
