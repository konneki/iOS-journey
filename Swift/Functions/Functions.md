# Functions

To make code reusable we use functions. These can take parameters and return nothing or a specific type. This ensures us that any data given must send back a specific type of data to ensure that there are no errors along the way.

```swift
func greetings() {
    print("Hello everyone!")
}

greetings()
```

To pass in parameters we use

```swift
func greetings(name: String) {
    print("Hello \(name)!")
}

greetings(name: "Oliwia")
```

## Returning a value

To return a value from function we use

```swift
func rollDice() -> Int {
    return Int.random(in: 1...6)
}

let result = rollDice()
print(result)
```

If there is a one-liner we don't have to use `return` at all

```swift
func areLettersIdentical(string1: String, string2: String) -> Bool {
    string1.sorted() == string2.sorted()
}
```

If your function doesn’t return a value, you can still use `return` by itself to force the function to exit early. For example, perhaps you have a check that the input matches what you expected, and if it doesn’t you want to exit the function immediately before continuing.

## Tuples

Those let us put multiple pieces of data into a single variable, but unlike those other options tuples have a fixed size and can have a variety of data types.

```swift
func getUser() -> (firstName: String, lastName: String) {
    (firstName: "Rocky", lastName: "Balboa")
}

let user = getUser()
print("Name: \(user.firstName) \(user.lastName)")
```

Tuples have a key advantage over dictionaries: we specify exactly which values will exist and what types they have, whereas dictionaries may or may not contain the values we’re asking for.

### Important `tuples` facts

Swift already knows the names you’re giving each item in the tuple, so you don’t need to repeat them when using return. So, this code does the same thing as our previous tuple:

```swift
func getUser() -> (firstName: String, lastName: String) {
    ("Rocky", "Balboa")
}
```

Sometimes you’ll find you’re given tuples where the elements don’t have names. When this happens you can access the tuple’s elements using numerical indices starting from 0, like this:

```swift
func getUser() -> (String, String) {
    ("Rocky", "Balboa")
}

let user = getUser()
print("Name: \(user.0) \(user.1)")
```

If a function returns a tuple you can actually pull the tuple apart into individual values if you want to.

```swift
func getUser() -> (firstName: String, lastName: String) {
    (firstName: "Rocky", lastName: "Balboa")
}

let user = getUser()
let firstName = user.firstName
let lastName = user.lastName

print("Name: \(firstName) \(lastName)")
```

Rather than assigning the tuple to user, then copying individual values out of there, we can skip the first step – we can pull apart the return value from getUser() into two separate constants, like this:

```swift
let (firstName, lastName) = getUser()
print("Name: \(firstName) \(lastName)")

//if we don't care about last name we can use

let (firstName, _) = getUser()
print("Name: \(firstName)")
```

## Parameter naming

There is no need to provide a parameter name always. Let's say that we have a function `hasPrefix()`. We use it like

```swift
let lyric = "I see a red door and I want it painted black"
print(lyric.hasPrefix("I see"))
```

We don't specify the parameter name as it is not needed here. The way it is done is like in the example below

```swift
func isUppercase(string: String) -> Bool {
    string == string.uppercased()
}

let string = "HELLO, WORLD"
let result = isUppercase(string: string)
```

We know that we have to provide string, so why not use it like so?

```swift
func isUppercase(_ string: String) -> Bool {
    string == string.uppercased()
}

let string = "HELLO, WORLD"
let result = isUppercase(string)
```

The word that we provide before the parameter name is used for readability only. So we can use something like

```swift
func printTimesTables(for number: Int) {
    for i in 1...12 {
        print("\(i) x \(number) is \(i * number)")
    }
}

printTimesTables(for: 5)
```
