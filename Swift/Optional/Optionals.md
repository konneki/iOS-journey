# Optionals

Swift likes to be predictable, which means as much as possible it encourages us to write code that is safe and will work the way we expect. Swift has another important way of ensuring predictability called optionals – a word meaning “this thing might have a value or might not.”

```swift
let opposites = [
    "Mario": "Wario",
    "Luigi": "Waluigi"
]

let peachOpposite = opposites["Peach"]
```

Above code did not set the default value for the dictionary, which means that the **peachOpposite** is an **optional String**.

What Swift does underneath is creating an optional type, which look like so:

```swift
let peachOpposite: String? = opposites["Peach"]
```

Optionals are like a box that may or may not have something inside. So, a **String?** means there might be a string waiting inside for us, or there might be nothing at all – a special value called **nil**, that means “no value”. Any kind of data can be optional, including **Int**, **Double**, and **Bool**, as well as instances of enums, structs, and classes.

## Unwrapping optionals using if let

Swift gives us two primary ways of unwrapping optionals, but the one you’ll see the most looks like this:

```swift
if let marioOpposite = opposites["Mario"] {
    print("Mario's opposite is \(marioOpposite)")
} else {
    print("Mario has no opposites")
}
```

**marioOpposite** will contain an unwrapped value of **opposites["Mario"]** only when it detects a value inside.

## Unwrapping optionals using guard let

Guard let us unwrap values in very similar way as **if let** and it's the second most common one to use.

Like **if let**, **guard let** checks whether there is a value inside an optional, and if there is it will retrieve the value and place it into a constant of our choosing.

However, the _way_ it does so flips things around:

```swift
var myVar: Int? = 3

if let unwrapped = myVar {
    print("Run if myVar has a value inside")
}

guard let unwrapped = myVar else {
    print("Run if myVar doesn't have a value inside")
}
```

**guard** provides the ability to check whether our program state is what we expect, and if it isn’t to bail out – to exit from the current function, for example.

This is sometimes called an _early return_: we check that all a function’s inputs are valid right as the function starts, and if any aren’t valid we get to run some code then exit straight away. If all our checks pass, our function can run exactly as intended.

**guard** is designed exactly for this style of programming, and in fact does two things to help:

1. If you use **guard** to check a function’s inputs are valid, Swift will always require you to use **return** if the check fails.
2. If the check passes and the optional you’re unwrapping has a value inside, you can use it after the **guard** code finishes.

```swift
func printSquare(of number: Int?) {
    guard let number = number else {
        print("Missing input")

        // 1: We *must* exit the function here
        return
    }

    // 2: `number` is still available outside of `guard`
    print("\(number) x \(number) is \(number * number)")
}
```

You can use guard with any condition, including ones that don’t unwrap optionals. For example, you might use **guard someArray.isEmpty else { return }**.

## Unwrapping optionals with nil coalescing

Nil coalescing enables us to provide a default value of optionals of any type. This can be easily demonstrated with dictionaries:

```swift
let captains = [
    "Enterprise": "Picard",
    "Voyager": "Janeway",
    "Defiant": "Sisko"
]

let new = captains["Serenity"] ?? "N/A" // N/A
```

The sign **??** is a nil coalescing and it works in a way, that if there is a value on the left side of it, it will be used, but if there is not, then it will use default one provided by us that is on the right side of it.

Different examples:

```swift
struct Book {
    let title: String
    let author: String?
}

let book = Book(title: "Beowulf", author: nil)
let author = book.author ?? "Anonymous"
print(author) // Anonymous

let input = ""
let number = Int(input) ?? 0
print(number) // 0
```

## Optional chaining

Optional chaining is a simplified syntax for reading optionals inside optionals.

Its syntax is pretty straightforward, we use it like so:

```swift
let names = ["Arya", "Bran", "Robb", "Sansa"]

let chosen = names.randomElement()?.uppercased() ?? "No one"
print("Next in line: \(chosen)")
```

If Array of names returns a random element, the code will make it uppercased, but if it return **nil**, then the code will use _No one_ as a result of nil coalescing.

We also can nest things multiple times, for example:

```swift
struct Book {
    let title: String
    let author: String?
}

var book: Book? = nil
let author = book?.author?.first?.uppercased() ?? "A"
print(author) // A
```

So, it reads “if we have a book, and the book has an author, and the author has a first letter, then uppercase it and send it back, otherwise send back A”.
