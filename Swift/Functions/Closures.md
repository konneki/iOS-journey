# Closures

Closures are a part of code that can have specific functionality. They can be assigned directly to variables and even return functions.

## Copying a function

We can copy a function by doing the below

```swift
func greetings() {
    print("Hello")
}

greetings() // Hello
let greetingsCopy = greetings
greetingsCopy() // Hello
```

## Creating a closure

```swift
let sayHello = {
    print("Hello")
}

sayHello()
```

### Adding parameters to a closure

```swift
let sayHello = { (name: String) -> String in
    "Hello \(name)"
}

sayHello("Adrian")
```

`in` is a keyword that is a marker letting Swift know that there is the end of parameters and return values and start of a functionality of a closure.

Closures does not take in parameter names. They are only used when calling `func` directly.

### Declaring types of closures

```swift
var greetCopy: () -> Void = greetUser
```

`Void` is a keyword that means _literally_ nothing. The above code says that the closure does not accept any parameters and returns nothing at all.

## Why use closures even if we have functions

Closures are used everywhere and there is a reason for it. Instead of declaring function we can use a closure or anonymous function.

Let's say we have a sorting function, but we want a specific user to come first:

```swift
let team = ["Gloria", "Suzanne", "Piper", "Tiffany", "Tasha"]

func captainFirstSorted(name1: String, name2: String) -> Bool {
    if name1 == "Suzanne" {
        return true
    } else if name2 == "Suzanne" {
        return false
    }

    return name1 < name2
}

let captainFirstTeam = team.sorted(by: captainFirstSorted)
print(captainFirstTeam) // ["Suzanne", "Gloria", "Piper", "Tasha", "Tiffany"]
```

A `sorted` function takes in another function that returns a boolean. If it's `true`, then the word comes before next one. The above code can utilize closure by doing the below:

```swift
let captainFirstTeam = team.sorted(by: { (name1: String, name2: String) -> Bool in
if name1 == "Suzanne" {
        return true
    } else if name2 == "Suzanne" {
        return false
    }

    return name1 < name2
})

print(captainFirstTeam) // ["Suzanne", "Gloria", "Piper", "Tasha", "Tiffany"]
```

## Trailing closures

The above code can be simplified. As the `sorted(by: )` function must receive a function with two Strings as parameters and return a Bool, we can get rid of that in the code like so:

```swift
let captainFirstTeam = team.sorted(by: { name1, name2 in
    if name1 == "Suzanne" {
        return true
    } else if name2 == "Suzanne" {
        return false
    }

    return name1 < name2
})

print(captainFirstTeam) // ["Suzanne", "Gloria", "Piper", "Tasha", "Tiffany"]
```

The _trailing closure_ enables us to shorthand that code even more like so:

```swift
let captainFirstTeam = team.sorted { name1, name2 in
    if name1 == "Suzanne" {
        return true
    } else if name2 == "Suzanne" {
        return false
    }

    return name1 < name2
}

print(captainFirstTeam) // ["Suzanne", "Gloria", "Piper", "Tasha", "Tiffany"]
```

## Shorthand syntax

We can also use shorthand syntax provided by Swift language, that automatically names the parameters of the function:

```swift
let captainFirstTeam = team.sorted {
    if $0 == "Suzanne" {
        return true
    } else if $1 == "Suzanne" {
        return false
    }

    return $0 < $1
}

print(captainFirstTeam) // ["Suzanne", "Gloria", "Piper", "Tasha", "Tiffany"]
```

In this case this code can be less readable than it was, so it is better to avoid this when parameters are used multiple times in closure.

If the closure was simpler like reverse sorted it could look like that:

```swift
let captainFirstTeam = team.sorted {
    return $0 > $1
}
```

And if that is the case and there is only one line of code, the `return` keyword can be removed like so:

```swift
let captainFirstTeam = team.sorted { $0 > $1 }
```

## Accepting functions as parameters

To use a function as a parameter we need to specify what the function parameters are an what it returns, like so:

```swift
func makeArray(size: Int, using generator: () -> Int) -> [Int] {
    var numbers = [Int]()

    for _ in 0..<size {
        let newNumber = generator()
        numbers.append(newNumber)
    }

    return numbers
}

let rolls = makeArray(size: 6) {
    Int.random(in: 1...20)
}

print(rolls)
```

In above case `generator` is a passed in function that takes no parameters and returns an Integer. `rolls` is an constant that uses trailing closure to generate a random number between 1 and 20, so it is a `generator` for _makeArray_ function.

The code returns simply an Array of Integers by imitating rolling 20 sided dice 6 times.

We can also passed in function instead of closure to achieve the same goal:

```swift
func generateNumber() -> Int {
    Int.random(in: 1...20)
}

let newRolls = makeArray(size: 6, using: generateNumber)
print(newRolls)
```

## Using multiple functions as parameters (stacking up trailing closures)

This is not uncommon in Swift and is used for example in Buttons. It works as in the example below:

```swift
func doImportantWork(first: () -> Void, second: () -> Void, third: () -> Void) {
    print("About to start first work")
    first()
    print("About to start second work")
    second()
    print("About to start third work")
    third()
    print("Done!")
}

doImportantWork {
    print("This is the first work")
} second: {
    print("This is the second work")
} third: {
    print("This is the third work")
}

// About to start first work
// This is the first work
// About to start second work
// This is the second work
// About to start third work
// This is the third work
// Done!
```

The parameter names have to be provided starting with second one onwards. Without it compiler will complain loudly
