# Protocol extensions

We can extend a whole protocol to add method implementations, meaning that any types conforming to that protocol get those methods.

We can write a code like this:

```swift
extension Array {
    var isNotEmpty: Bool {
        isEmpty == false
    }
}
```

It would work perfectly fine, but what if we would like to apply the same functionality to a **Set** or a **Dictionary**?

**Array**, **Set**, and **Dictionary** all conform to a built-in protocol called _Collection_, through which they get functionality such as **contains()**, **sorted()**, **reversed()**, and more.

This is important, because **Collection** is also what requires the **isEmpty** property to exist. So, if we write an extension on **Collection**, we can still access **isEmpty** because it’s required. This means we can change **Array** to **Collection** in our code to get this:

```swift
extension Collection {
    var isNotEmpty: Bool {
        isEmpty == false
    }
}
```

Now we can use this method in various places, like so:

```swift
var guests = ["Adrian", "Daniel", "Maja"]
if guests.isNotEmpty {
    print("Guest count: \(guests.count)")
}
```

By extending the protocol we’re adding functionality that would otherwise need to be done inside individual structs. This is really powerful, and leads to a technique Apple calls _protocol-oriented programming_ – we can list some required methods in a protocol, then add default implementations of those inside a protocol extension. All conforming types then get to use those default implementations, or provide their own as needed.

For example, if we had a protocol like this one:

```swift
protocol Person {
    var name: String { get }
    func sayHello()
}
```

That means all conforming types must add a **sayHello()** method, but we can also add a default implementation of that as an extension like this:

```swift
extension Person {
    func sayHello() {
        print("Hi, I'm \(name)")
    }
}
```

And now below code would work just fine:

```swift
struct Employee: Person {
    let name: String
}

let ryan = Employee(name: "Ryan Gosling")
ryan.sayHello() // Hi, I'm Ryan Gosling
```
