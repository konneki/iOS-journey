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
