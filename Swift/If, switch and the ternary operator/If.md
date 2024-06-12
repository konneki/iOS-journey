# If statements

To check if the condition is true you can use _if statement_.

## How to use

Let's say that we have a variable with number 10. We would like to check if the number is lower than 15 and if so, then do something with it.

```swift
let number = 10
if number < 15 {
    print("\(number) is lower than 15") // 10 is lower than 15
}
```

But let's say that the number is bigger than 15, what should we do then? We can use `else if` or `else` statement.

```swift
let number = 16
if number < 15 {
    print("\(number) is lower than 15")
} else if number > 15 {
    print("\(number) is bigger than 15") // 16 is bigger than 15
} else {
    print("The number is 15")
}
```

You can use multiple `else if` within the condition, but the code becomes unreadable very fast, so then you'd like to switch to `switch` statement (:smile:).
