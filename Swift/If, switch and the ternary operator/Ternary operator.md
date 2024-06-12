# Ternary operator

It works like `if else` statement, but in one line. It is very convenient to use.

```swift
let age = 18
let canVote = age >= 18 ? "Yes" : "No"
```

## If statement and ternary operator in comparison

```swift
let hour = 23
print(hour < 12 ? "It's before noon" : "It's after noon")

print(
    if hour < 12 {
        "It's before noon"
    } else {
        "It's after noon"
    }
)

if hour < 12 {
    print("It's before noon")
} else {
    print("It's after noon")
}
```

All of them print exactly the same result
