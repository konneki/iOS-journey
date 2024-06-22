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
