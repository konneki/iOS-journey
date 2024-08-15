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
