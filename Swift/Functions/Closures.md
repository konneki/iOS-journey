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
