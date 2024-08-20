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

## Adding initializers

Class initializers in Swift are more complicated than struct initializers. If a child class has any custom initializers, it must always call the parent’s initializer _after_ it has finished setting up its own properties, if it has any.

```swift
class Vehicle {
    let isElectric: Bool

    init(isElectric: Bool) {
        self.isElectric = isElectric
    }
}

class Car: Vehicle {
    let isConvertible: Bool

    init(isElectric: Bool, isConvertible: Bool) {
        self.isConvertible = isConvertible
        super.init(isElectric: isElectric)
    }
}

let porscheTaycan = Car(isElectric: true, isConvertible: false)
```

**super** is another one of those values that Swift automatically provides for us, similar to **self**: it allows us to call up to methods that belong to our parent class, such as its initializer. You can use it with other methods if you want; it’s not limited to initializers.

If a subclass does not have any of its own initializers, it automatically inherits the initializers of its parent class.

## Copying classes

In Swift, all copies of a class instance share the same data, meaning that any changes you make to one copy will automatically change the other copies. This happens because classes are reference types in Swift, which means all copies of a class all refer back to the same underlying pot of data.

```swift
class User {
    var username = "Anonymous"
}

var user1 = User()
var user2 = user1
user2.username = "Ryan"

print(user1.username) // Ryan
print(user2.username) // Ryan
```

This might seem like a bug, but it’s actually a feature – and a really important feature too, because it’s what allows us to share common data across all parts of our app. As you’ll see, SwiftUI relies very heavily on classes for its data, specifically because they can be shared so easily.

If you want to create a _unique_ copy of a class instance – sometimes called a _deep copy_ – you need to handle creating a new instance and copy across all your data safely.

```swift
class User {
    var username = "Anonymous"

    func copy() -> User {
        let user = User()
        user.username = username
        return user
    }
}

var user1 = User()
var user2 = user1.copy()
user2.username = "Ryan"

print(user1.username) // Anonymous
print(user2.username) // Ryan
```

## Deinitializers

Swift’s classes can optionally be given a deinitializer, which is a bit like the opposite of an initializer in that it gets called when the object is destroyed rather than when it’s created.

This comes with a few small provisos:

1. Just like initializers, you don’t use **func** with deinitializers – they are special.
2. Deinitializers can never take parameters or return data, and as a result aren’t even written with parentheses.
3. Your deinitializer will automatically be called when the final copy of a class instance is destroyed. That might mean it was created inside a function that is now finishing, for example.
4. We never call deinitializers directly; they are handled automatically by the system.
5. Structs don’t have deinitializers, because you can’t copy them.

Exactly when your deinitializers are called depends on what you’re doing, but really it comes down to a concept called scope. Scope more or less means “the context where information is available”, and you’ve seen lots of examples already:

1. If you create a variable inside a function, you can’t access it from outside the function.
2. If you create a variable inside an **if** condition, that variable is not available outside the condition.
3. If you create a variable inside a **for** loop, including the loop variable itself, you can’t use it outside the loop.

When a value exits scope we mean the context it was created in is going away. In the case of structs that means the data is being destroyed, but in the case of classes it means only one copy of the underlying data is going away.

```swift
class User {
    let id: Int

    init(id: Int) {
        self.id = id
        print("User \(id): I'm alive!")
    }

    deinit {
        print("User \(id): I'm dead!")
    }
}

for i in 1...3 {
    let user = User(id: i)
    print("User \(user.id): I'm in control!")
}

//User 1: I'm alive!
//User 1: I'm in control!
//User 1: I'm dead!
//User 2: I'm alive!
//User 2: I'm in control!
//User 2: I'm dead!
//User 3: I'm alive!
//User 3: I'm in control!
//User 3: I'm dead!
```

Now similar code will be run, but data will be stored inside an array and destroyed later.

```swift
class User {
    let id: Int

    init(id: Int) {
        self.id = id
        print("User \(id): I'm alive!")
    }

    deinit {
        print("User \(id): I'm dead!")
    }
}

var users = [User]()

for i in 1...3 {
    let user = User(id: i)
    print("User \(user.id): I'm in control!")
    users.append(user)
}

print("Loop is finished!")
users.removeAll()
print("Array is clear!")

//User 1: I'm alive!
//User 1: I'm in control!
//User 2: I'm alive!
//User 2: I'm in control!
//User 3: I'm alive!
//User 3: I'm in control!
//Loop is finished!
//User 1: I'm dead!
//User 2: I'm dead!
//User 3: I'm dead!
//Array is clear!
```
