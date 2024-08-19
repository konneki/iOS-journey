# Classes

Swift uses structs for storing most of its data types, including String, Int, Double, and Array, but there is another way to create custom data types called _classes_. These have many things in common with structs, but are different in key places.

### Similarities

- You get to create and name them.
- You can add properties and methods, including property observers and access control.
- You can create custom initializers to configure new instances however you want.

### Differences

- You can make one class build upon functionality in another class, gaining all its properties and methods as a starting point. If you want to selectively override some methods, you can do that too.
- Because of that first point, Swift won’t automatically generate a memberwise initializer for classes. This means you either need to write your own initializer, or assign default values to all your properties.
- When you copy an instance of a class, both copies share the same data – if you change one copy, the other one also changes.
- When the final copy of a class instance is destroyed, Swift can optionally run a special function called a deinitializer.
- Even if you make a class constant, you can still change its properties as long as they are variables.

SwiftUI uses classes extensively, mainly for point 3: all copies of a class share the same data. This means many parts of your app can share the same information, so that if the user changed their name in one screen all the other screens would automatically update to reflect that change.

### Example Swift class

```swift
class Game {
    var score = 0 {
        didSet {
            print("Score is now \(score)")
        }
    }
}

var newGame = Game()
newGame.score += 10
```

## Inheritance

To make one class inherit from another, write a colon after the child class’s name, then add the parent class’s name.

```swift
class Employee {
    let hours: Int

    init(hours: Int) {
        self.hours = hours
    }

    func printSummary() {
    print("I work \(hours) hours a day.")
}
}

class Developer: Employee {
    func work() {
        print("I'm writing code for \(hours) hours.")
    }
}

class Manager: Employee {
    func work() {
        print("I'm going to meetings for \(hours) hours.")
    }
}
```

Notice how those two child classes can refer directly to _hours_ – it’s as if they added that property themselves, except we don’t have to keep repeating ourselves.

```swift
let robert = Developer(hours: 8)
let joseph = Manager(hours: 10)
robert.work() // I'm writing code for 8 hours.
joseph.work() // I'm going to meetings for 10 hours.

let novall = Developer(hours: 8)
novall.printSummary() // I work 8 hours a day.
```

### Changing inherited methods

If a child class wants to change a method from a parent class, you must use **override** in the child class’s version. This does two things:

1. If you attempt to change a method without using **override**, Swift will refuse to build your code. This stops you accidentally overriding a method.
2. If you use **override** but your method doesn’t actually override something from the parent class, Swift will refuse to build your code because you probably made a mistake.

```swift
override func printSummary() {
    print("I'm a developer who will sometimes work \(hours) hours a day, but other times spend hours arguing about whether code should be indented using tabs or spaces.")
}
```

If you know for sure that your class should not support inheritance, you can mark it as **final**. This means the class itself can inherit from other things, but can’t be used to inherit from – no child class can use a final class as its parent.

```swift
final class Employee {
    let hours: Int

    init(hours: Int) {
        self.hours = hours
    }
}
```
