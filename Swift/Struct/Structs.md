# Structs

Let us create our own complex data types with their own variables and functions.

Simple struct example:

```swift
struct Album {
    let title: String
    let artist: String
    var year: Int

    func printSummary() {
        print("\(title) (\(year)) by \(artist)")
    }
}
```

From above code we can see a couple of things:

1. The struct name needs to be created with first letter being uppercased
2. We can use constants and variables, which are called _properties_ of a struct
3. Function in a struct is called _method_

To initialize a struct we do:

```swift
let abattoirBlues = Album(title: "Abattoir Blues / The Lyre of Orpheus", artist: "Nick Cave & The Bad Seeds", year: 2004)
```

This creates two things:

1. Swift silently creates initializer for that struct behind the scenes
2. `abattoirBlues` constant (or variable) is now called an _instance_ of a struct

We can create as many instances as we want.

## Manipulating variables inside a struct

We can create variable properties inside structs, but they cannot be simply updated. If we create a `abattoirBlues` as a constant using let, Swift makes the `Album` and all its data constant – we can call functions just fine, but those functions shouldn’t be allowed to change the struct’s data because we made it constant.

If we have a function inside a struct that manipulates the data:

1. It needs a special keyword in front of a function called `mutating`
2. The struct instance needs to be a variable

```swift
struct Employee {
    let name: String
    var vacationRemaining: Int

    mutating func takeVacation(days: Int) {
        if vacationRemaining > days {
            vacationRemaining -= days
            print("I'm going on vacation!")
        } else {
            print("Oops! There aren't enough days remaining.")
        }
        print("Days remaining: \(vacationRemaining)")
    }
}

var archer = Employee(name: "Sterling Archer", vacationRemaining: 14)
archer.takeVacation(days: 5)
print(archer.vacationRemaining)

// I'm going on vacation!
// Days remaining: 9
```

The above code is free from errors and will compile just fine.

## Default values

You can provide default values for properties (constant and variable). To do it just assign a value.

When a value is assigned to a constant you cannot update it while initializing - it is already assigned and updating constants is forbidden. It also is hidden from initializer.
When a value is assigned to a variable it can be updated while initialized.

```swift
struct Employee {
    let name: String = "Anonymous"
    var vacationRemaining: Int = 14

    mutating func takeVacation(days: Int) {
        if vacationRemaining > days {
            vacationRemaining -= days
            print("I'm going on vacation!")
        } else {
            print("Oops! There aren't enough days remaining.")
        }
        print("Days remaining: \(vacationRemaining)")
    }
}

// Below won't work
let kane = Employee(name: "Lana Kane")

// Below works just fine
let poovey = Employee(vacationRemaining: 35)
```

## Compute property values dynamically

There are two kinds of properties:

- a stored property is a variable or constant that holds a piece of data inside an instance of the struct
- a computed property calculates the value of the property dynamically every time it’s accessed

```swift
struct Employee {
    let name: String
    var vacationAllocated = 14 // stored property
    var vacationTaken = 0 // stored property

    // computed property
    var vacationRemaining: Int {
        vacationAllocated - vacationTaken
    }
}

var archer = Employee(name: "Sterling Archer", vacationAllocated: 14)
archer.vacationTaken += 4
print(archer.vacationRemaining) // 10
archer.vacationTaken += 4
print(archer.vacationRemaining) // 6
```

### Getters and setters

At that moment we cannot write directly to _vacationRemaining_ computed property, as Swift would not know how to handle that. For this purpose we can setup `get` and `set`.

- `get` to read data
- `set` to write data

The updated code would look as follows:

```swift
struct Employee {
    let name: String
    var vacationAllocated = 14 // stored property
    var vacationTaken = 0 // stored property

    // computed property
    var vacationRemaining: Int {
        get {
            vacationAllocated - vacationTaken
        }

        set {
            vacationAllocated = vacationTaken + newValue
        }
    }
}
```

`newValue` stores whatever value is provided as a setter. In above case the _vacationAllocated_ property will be updated to make up for it.

```swift
var archer = Employee(name: "Sterling Archer", vacationAllocated: 14)
archer.vacationTaken += 4 // 4
archer.vacationRemaining = 5 // set a newValue as 5, so 4 + 5 = 9
print(archer.vacationAllocated) // 9
```

## Property observers

Let us code an action when a particular property changes. Currently, when a property has been updated, a print call should be made like so:

```swift
struct Game {
    var score = 0
}

var game = Game()
game.score += 10
print("Score is now \(game.score)") // Score is now 10
game.score -= 3
print("Score is now \(game.score)") // Score is now 7
```

If we forgot to add print we loose track of the progress, which may lead to issues or problems. To avoid that we can call mentioned print whenever the score changes using `didSet` like so:

```swift
struct Game {
    var score = 0 {
        didSet {
            print("Score is now \(score)")
        }
    }
}

var game = Game()
game.score += 10
game.score -= 3
```

After the property value changes the `didSet` code will run. The `didSet` functionality also enables us to show the `oldValue` of the property.

There is also `willSet` functionality, when the property value is about to change with the `newValue` that contains the data that will be written to the property.

```swift
struct App {
    var contacts = [String]() {
        willSet {
            print("Current value is: \(contacts)")
            print("New value will be: \(newValue)")
        }

        didSet {
            print("There are now \(contacts.count) contacts.")
            print("Old value was \(oldValue)")
        }
    }
}

var app = App()
app.contacts.append("Adrian E")
app.contacts.append("Allen W")
app.contacts.append("Ish S")

// Current value is: []
// New value will be: ["Adrian E"]
// There are now 1 contacts.
// Old value was []
// Current value is: ["Adrian E"]
// New value will be: ["Adrian E", "Allen W"]
// There are now 2 contacts.
// Old value was ["Adrian E"]
// Current value is: ["Adrian E", "Allen W"]
// New value will be: ["Adrian E", "Allen W", "Ish S"]
// There are now 3 contacts.
// Old value was ["Adrian E", "Allen W"]
```

**Important!**
Do not add too much additional logic to `didSet` and `willSet` property observers as this may lead to performance issues when running intensive tasks.

## Creating own initializers

Initializer is a special function running every time a struct is initiated. Its purpose is to assign a value to each property. There is one rule that one should follow **all properties must have a value by the time the initializer ends**, otherwise Swift would refuse to build our code.

To create initializer add `init` function:

```swift
struct Player {
    let name: String
    let number: Int

    init(name: String, number: Int) {
        self.name = name
        self.number = number
    }
}
```

1. There is no `func` keyword. Yes, this looks like a function in terms of its syntax, but Swift treats initializers specially
2. Even though this creates a new `Player` instance, initializers never explicitly have a return type – they always return the type of data they belong to
3. Use `self` to assign parameters to properties to clarify we mean _assign the name parameter to my name property_

Without creating the initializer Swift uses something called _memberwise_ initializer, which is a fancy way of saying an initializer that accepts each property in the order it was defined.

Initializers that we create mustn't have to work as memberwize initializers. We can create them as we please, but we have to stick to the golden rule, that **all properties must have a value by the time the initializer ends**.

```swift
struct Player {
    let name: String
    let number: Int

    init(name: String) {
        self.name = name
        number = Int.random(in: 1...99)
    }
}

let player = Player(name: "Megan R")
print(player.number)
```

**Important:** Although you can call other methods of your struct inside your initializer, you can’t do so before assigning values to all your properties – Swift needs to be sure everything is safe before doing anything else.

You can add multiple initializers to your structs if you want, as well as leveraging features such as external parameter names and default values. However, as soon as you implement your own custom initializers you’ll lose access to Swift’s generated memberwise initializer unless you take extra steps to retain it. This isn’t an accident: if you have a custom initializer, Swift effectively assumes that’s because you have some special way to initialize your properties, which means the default one should no longer be available.

## Access Control

Normally variables inside Structs can be accessed externally. This is not what we'd always want to have, therefore the Access Control keywords may help us.

### private

Meaning, don’t let anything outside the struct use this. Variables inside a struct can be used freely only within the boundaries of a struct.
There is additional one called **_private(set)_**, which is used when the **read** access is available everywhere outside the struct, but **write** access is only available within the struct itself.

### fileprivate

Meaning, don’t let anything outside the current file use this.

### public

Meaning, let anyone, anywhere use this.

Access control is really about limiting what you and other developers on your team are able to do – and that’s sensible! If we can make Swift itself stop us from making mistakes, that’s always a smart move.

**Important:** If you use private access control for one or more properties, chances are you’ll need to create your own initializer.
